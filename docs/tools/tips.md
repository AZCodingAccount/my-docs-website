## 小tips

### 包命名

为什么Android或者Java的这些包习惯都叫com.xxx.xxx？

​		因为域名在全球是唯一的，我们常常的命名习惯是域名反写+项目名，这样可以保证包的唯一性，因此maven仓库、发布安卓或者桌面应用不会存在潜在的命名冲突问题，同时也见名知义

为什么包名全小写？

​		第一个原因是命名规范和惯例问题，第二个原因是第一个原因的一个具象化windows大小写不敏感，Linux大小写敏感，如果使用驼峰之类的命名法，会导致命名冲突（比如MySQLwindows就不区分大小写，但是Linux又区分了）

比如我在开发的一个项目叫iTime，我的域名是bugdesigner.cn，可以命名成**cn.bugdesigner.itime**

### 分辨率

在开发时候

### MySQL建表

为什么使用varchar的时候喜欢使用varchar(255)？

​		因为，在Mysql5.0.3之前，varchar的最大长度是255字符，这是由于长度前缀使用了一个字节存储长度信息。但是在5.0.3版本以后，varchar的最大长度是65535字节，如果超过了255，那么长度位就需要两个字节来存储。事实上，在超过255以后，使用65535还是256，对于空间的占用基本没有区别了，但是一般还是2的倍数有助于稍微提升性能。节省空间只需要记住255这个时间点即可

​		需要注意的是，对于Mysql4.1版本以后，varchar(n)中的n代表字符数而非字节数，这就意味着汉字并不需要根据字符集换算。比如：

>假设字段类型为varchar(255)，业务需求需要存储汉字，使用utf8mb4编码且存储满后，占用的空间是255*4+1=1024字节。（Mysql8.0以后默认编码集是utf8mb4）



### Restful命名规则

目前的HTTP API接口设计基本都是Get、Post一把梭。随之衍生的就是不清晰不规范，一般开发的时候还是推荐遵循Restful风格，主要规定是**URL标识资源路径、方法标识操作**。

1. 使用名词而非动词。如添加用户传统方式是`POST`: `/user/update`、restful就是`PUT` : `/users` 

2. 使用复数形式。保持直观性和一致性，如`/users`而非`/user`

3. 使用路径标识资源层级关系。

   >除了对单一资源的简单操作，往往还需要进行一些复杂操作，比如查询某一特定用户的订单、某一用户加入队伍等，用到了层级关系。URL如：
   >
   >- /users/{userId}/orders (GET)    /users/{userId}标明主资源为用户、orders为与用户关联的子资源（订单）、GET表示查询与用户关联的订单
   >- /teams/{teamId}/members (POST)    /teams/{teamId}标明是主资源为队伍、members表示操作的队伍的成员、POST表示添加成员
   >- /users/matches (GET)    匹配推荐的用户伙伴列表

4. 资源列表的查询使用查询字符串。如`/users?role=admin` 表示查询所有管理员用户

5. 尽量保证简单。路径参数{id}在Restful API中非常常用，因为其比路径参数更加清晰。在其他时候如获取个人信息也不用传递个人id，因为这个id一般都是在session|token|jwt中存储

### 单例模式的双检锁实现

为了性能要求，我们常常需要单例模式进行对象的创建。但是在多线情况下，还是会创建多个对象，带来一定程度下的性能损耗，单例模式中**相对高大上**的一个方法双检锁可以解决这个问题

```java
public class MetaManager {
    private static volatile Meta meta;	// 保证线程内存共享，不重复初始化 

    public static Meta getMetaobject() {
        if (meta == null) {
            synchronized (MetaManager.class) {
                if (meta == null) {
                    meta = initMeta(); // 一个初始化方法
                }
            }
        }
        return meta;
    }

    private static Meta initMeta() {
        // 初始化 Meta 对象的逻辑
        return new Meta();
    }
}
```

会有一个疑问，为什么锁不加在外面，还需要外层进行判断呢？

这个是因为性能开销的缘故，在每次调用`getMetaObject`时，如果加在最外层，在已经初始化完成以后，还是变成同步方法，降低性能。在外层判断`meta`对象以后，如果已经被初始化，可以**直接退出而不需要等待锁**
