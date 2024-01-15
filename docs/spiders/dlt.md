# 某练通逆向（附数据分析结果）

## 背景

背景是23年11月末时候有个人找到我说想打个省标，我问预算多少，我跟他要600，结果他跟我说别人都是200零基础问我200能不能打，我当时就震惊了，一年多没正经打过代练，现在代练行业都这么玩了吗？

正好当时在学APP逆向，由于跟dlt太熟悉了，我就把某代练通的搜索接口和分页查询接口给逆向了，这是当初21年我截的图，当然，现在已经不打单很久了。

|                                                              |                                                              |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| <img src="https://my-picture-bed1-1321100201.cos.ap-beijing.myqcloud.com/mypictures/cc16b2c880d14f3d27144d6a2283e79.jpg" alt="cc16b2c880d14f3d27144d6a2283e79" style="zoom:20%;" /> | <img src="https://my-picture-bed1-1321100201.cos.ap-beijing.myqcloud.com/mypictures/aaf98fcc1d6b50fa5e096dd614fab60.jpg" alt="aaf98fcc1d6b50fa5e096dd614fab60" style="zoom:20%; text-align:center" /> |

但是一次只能爬取3000多条数据。后来想着攒到2万条时候再分析，结果12月事情太多了，各种比赛、考试、毕设中期检查、课设。实在是闲不下来，直到现在元旦才勉强有时间。

在B站也开了个python逆向的小坑，然后就花5个小时左右对所有数据整理了一下用Python的pandas和matplotlib简单分析可视化了一下。接下来就简单说一下逆向的思路和数据分析结果。

## 逆向思路

这个是讲原理的，不感兴趣的直接看数据分析结果就可以啦。

### 环境的选择

首先，搞APP逆向能用真机一定要用真机。原因我初步发现有两个

1. 有的APP会检测你的当前设备，检测到模拟器直接自动退出或者给你一个错误的包（代练通新版本就是直接退出）
2. APP主要设计就是在真机运行的，你模拟器可能别人开发者没有考虑到，会出现意料之外的问题。

但是我用的是模拟器，因为那时候还没买逆向手机，我买的是pixel4，安卓逆向和爬虫都是挺不错的。

这里去豌豆荚下载代练通旧版本就可以了，但是需要注意的是，如果有提示更新的选项。能用老版本就用老版本，我们是搞逆向的，他的新版本**加密肯定更难**了。如果强制让你更新，这个时候有两种情况，

1. 你去逆向这个APP，把这个弹弹窗的这个代码找到，给他删了或者更改掉。说实话，这个也不太简单，还需要过检测、签名绕过，如果不是逆向大佬，不要搞这个。
2. 假设你已经把这个APP给修改了，结果后端接口更新了，你还是拿不到数据，这个时候就只能老老实实更新了。

对于本案例，直接跳过更新就行，他不会让你强制更新。

### 逆向思路

**抓包**

逆向第一步肯定是要抓包了，看看哪个请求是获取数据的。这里dlt有个坑，就是你直接配置代理抓包的时候是抓不到你想要的包的，有一些软件比如使用flutter开发的就不走这个代理，你需要用一些小工具。我这里使用socks droid+charles可以绕过这个检测，使用socks5一步到胃。

❗注意需要登录一下，不登录搜索接口不给你看。

❗对于分页查询接口，你又不能登录了，不然返回给你的数据很少。

**分析请求进而逆向**

逆向一个APP或者网站的时候我们都需要明确自己的目的，这个APP或者网站里面有什么有价值的东西？比如抖音有评论接口，搜索接口，视频下载，得物，识货这种是查询接口。我这里目的是爬取**分页的订单数据**和**根据英雄名称搜索**

这里我就不说具体是哪个URL是发送请求的了，大家感兴趣自己去摸索，多踩踩坑，这样对自己的逆向技术也有帮助。

这个逆向还是非常简单的，非常适合新手小白，只有一个sign需要逆向，找到URL以后其他还有一些查询参数大家多点几下发一下请求就大概能知道这些参数是干嘛的了。

**逆向工具**

我们都知道JS逆向中源代码是基本上都相当于脱光了站在我们面前，基本上不是特别变态的加密我们只要找都可以找到入口。但是APP这里怎么办呢？没有了开发者工具。

我这里采用的解决方案是**jadx**+**frida**直接一把梭。当然像jeb这种动态调试工具也可以用。

拿着我们刚刚找到的参数名和url去全局搜就可以，找到一些可能加密参数的方法用frida进行hook，看看程序过不过这个方法、输入参数和输出参数。就行了。这里分享一个小经验：

💡 `一般我们在开发时候都是会把url这种写成一个常量的，开发的规范要求，搜索的时候对常量注意点。基本没人会把URL写死在程序里面的`

### python完成业务逻辑

经过上面一系列混合双打，基本逆向过程就结束了，接下来就是py模拟发请求。这里就需要处理业务逻辑的东西了

对于根据英雄名称搜索，100多个英雄相信没人想写成一个数组搁那for循环遍历，也不优雅，那我们咋拿呢？直接去王者荣耀官网，这里尝试发一个请求，结果是没有返回完英雄，是动态加载的，使用**selenium**直接滑动一下就可以了

```python
from selenium.webdriver import Chrome
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.options import Options

# 这个类是获取王者荣耀英雄列表的
heroes = []


def get_heros_order():
    opt = Options()
    opt.add_argument("--headless")	# 无头
    opt.add_argument('--disable-gpu')
    opt.add_argument("--window-size=4000,1600")  # 设置窗口大小

    driver = Chrome(options=opt)
    driver.get("https://pvp.qq.com/web201605/herolist.shtml")
    driver.implicitly_wait(10) #等待
    lis = driver.find_elements(By.CSS_SELECTOR, ".herolist li")
    for li in lis:
        # 把数据存储到列表中
        heroes.append(li.text)


get_heros_order()

```

拿到这个heros。其他就完事了。就是两个方法。前面思路都讲得很清楚了，**这个大家自己去逆向就可以了，我这里就不给代码了**。

逆向完毕还有一个存储到数据库的逻辑，当然也可以存储到**csv**文件或者**excel**中，我这里**mysql**用的比较熟就直接**mysql**了。给出我存储数据的逻辑的代码

```python
import time
from datetime import datetime

import pymysql

# 这个类是用来把数据存储到Mysql数据库的
class MyDatabase:

    # 初始化数据库连接环境
    def __init__(self):
        self.db = pymysql.connect(host='localhost', user='root', password='123456', database='spidertestdb')
        self.cursor = self.db.cursor()
        self.create_table()

    # 这个数据库里面主要装爬取的所有数据，重复的也可以装
    def create_table(self):
        create_table_sql = """
              CREATE TABLE IF NOT EXISTS dailiantong_base (
                  id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
                  Title VARCHAR(10000) comment '标题',
                  Price int comment '价格',
                  Ensure1 int comment '安全保证金',
                  Ensure2 int comment '效率保证金',
                  TimeLimit int comment '时间限制',
                  Creater VARCHAR(100) comment '发单人',
                  Stamp DATETIME comment '发布时间',
                  Zone VARCHAR(100) comment '游戏大区',
                  UnitPrice int comment '单价',
                  UserID VARCHAR(100) comment '发单人ID',
                  SerialNo VARCHAR(100) comment '订单ID'
              )
          """
        # 这个数据库主要装按照英雄分类的时候爬取到的数据
        create_table_sql2="""
         CREATE TABLE IF NOT EXISTS heroes_table(
            id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
            hero VARCHAR(100) COMMENT '英雄名称',
            Title VARCHAR(10000) comment '标题',
            Price int comment '价格',
            Ensure1 int comment '安全保证金',
            Ensure2 int comment '效率保证金',
            TimeLimit int comment '时间限制',
            Creater VARCHAR(100) comment '发单人',
            Stamp DATETIME comment '发布时间',
            Zone VARCHAR(100) comment '游戏大区',
            UnitPrice int comment '单价',
            UserID VARCHAR(100) comment '发单人ID',
            SerialNo VARCHAR(100) comment '订单ID'
            
         ) 
        """
        self.cursor.execute(create_table_sql)
        self.cursor.execute(create_table_sql2)
        # 还有个表base_dailiantong用来装清洗过后的数据


    def save_data(self, datas):
        insert_sql = """
        INSERT INTO dailiantong_base (Title, Price, Ensure1, Ensure2, TimeLimit, Creater, Stamp, Zone, UnitPrice,UserID,SerialNo)
        VALUES (%s, %s, %s, %s, %s, %s, %s, %s, %s,%s,%s)
        """
        try:
            for data in datas["LevelOrderList"]:
                values = (
                    data.get("Title", ""),
                    data.get("Price", 0),
                    data.get("Ensure1", 0),
                    data.get("Ensure2", 0),
                    data.get("TimeLimit", 0),
                    data.get("Create", ""),
                    datetime.fromtimestamp(data.get("Stamp", int(time.time())) + 255845872).strftime(
                        '%Y-%m-%d %H:%M:%S'),
                    data.get("Zone", ""),
                    data.get("UnitPrice", 0),
                    data.get("UserID", ""),
                    data.get("SerialNo", "")
                )
                self.cursor.execute(insert_sql, values)
            self.db.commit()
        except Exception as e:
            print("插入数据时候出错了:", e)
            self.db.rollback()

    # 把英雄数据保存到数据库中
    def save_heroes_data(self, datas,search_str):
        insert_sql = """
                INSERT INTO heroes_table (hero,Title, Price, Ensure1, Ensure2, TimeLimit, Creater, Stamp, Zone, UnitPrice,UserID,SerialNo)
                VALUES (%s,%s, %s, %s, %s, %s, %s, %s, %s, %s,%s,%s)
                """
        try:
            for data in datas["LevelOrderList"]:
                values = (
                    search_str,
                    data.get("Title", ""),
                    data.get("Price", 0),
                    data.get("Ensure1", 0),
                    data.get("Ensure2", 0),
                    data.get("TimeLimit", 0),
                    data.get("Create", ""),
                    datetime.fromtimestamp(data.get("Stamp", int(time.time())) + 255845872).strftime(
                        '%Y-%m-%d %H:%M:%S'),
                    data.get("Zone", ""),
                    data.get("UnitPrice", 0),
                    data.get("UserID", ""),
                    data.get("SerialNo", "")
                )
                self.cursor.execute(insert_sql, values)
            self.db.commit()
        except Exception as e:
            print("插入数据时候出错了:", e)
            self.db.rollback()

    # 关闭数据库连接
    def close(self):
        self.cursor.close()
        self.db.close()

```

最后数据分析的代码就不给出了，300多行实在是太多了，感兴趣直接去我Github仓库下就行https://github.com/AZCodingAccount/python-spider

## 数据分析结果

### 关于对英雄的分析结果

最受老板欢迎的10个英雄，瑶瑶公主居然才第8，你敢信？！！！

<img src="https://my-picture-bed1-1321100201.cos.ap-beijing.myqcloud.com/mypictures/image-20240101213204516.png" alt="image-20240101213204516"  />

每单平均价格最高的几个英雄

老虎最高是没啥悬念的，让我没想到的是瑶瑶公主，朵莉亚居然都没上榜，镜第9名......。我猜测可能有人发了国服单一下子就把价格拉上去了？

<img src="https://my-picture-bed1-1321100201.cos.ap-beijing.myqcloud.com/mypictures/image-20240101213416479.png" alt="image-20240101213416479"  />

### 关于对所有订单的分析结果

样本数：**11364**个

#### 游戏大区分布

还是安q傲立群雄，不过安卓微信居然才12.6%。安卓苹果也算是两足鼎立了。

![image-20240101213542268](https://my-picture-bed1-1321100201.cos.ap-beijing.myqcloud.com/mypictures/image-20240101213542268.png)

#### 发单者和订单数的相关关系

这个图很明了了，0.5%的发单者发了50%的单子。我看了排名前几个的，第一个好像是淘宝来的，一共就一万多个单子，他发了1000多个。各种中间商抽成，真的比我国贫富差距还抽象啊

![image-20240101213717192](https://my-picture-bed1-1321100201.cos.ap-beijing.myqcloud.com/mypictures/image-20240101213717192.png)

#### 价格等的描述性统计

简单分析一下下面的图：

- 标准差比平均值大好几倍，我们一般认为波动非常大，数据不稳定，在代练行业这个是正常的
- 价格方面，每个单子的平均价格是在120左右、中位数在32左右，上四分位数是100， 说明低价的订单比较多。
- 保证金方面，都没有太大的浮动。一般发单者也是喜欢设置两个保证金为差不多的价格
- 时间限制方面，平均是在65个小时，上四分位数是72个小时，这个东西我们代练一般也不太看重。

![image-20240101213921817](C:\Users\Albert han\OneDrive\图片\博客图片\代练通逆向特色图.png)

#### 每个段位的价格问题

参与统计的数据集是一万一千多个，还是非常有参考价值的。

- 一切尽在不言中，排位这么多的样本，这个应该是没人反对是偶然性了吧。这个我就不再分析了，都是聪明人。

- 作为一个圈内人，我澄清一下，51星以上的基本就没有参考价值了。样本才53个。50星以下的26-51星，300多个样本，4块钱一颗星，进厂一个小时多少钱啊各位。
- 另外巅峰赛的话现在也已经被各种中介搞的乌烟瘴气了，

最后大家可能会疑惑，为啥我统计了一万个样本，下面的才不到3000个呢。

1. 一个是因为我为了保证结果的准确性是使用正则提取这些星数的，只有符合我的标题格式的要求，我才会提取这个星数来计算平均价格。`宁可放过一千，不会错杀一个`
2. 还有就是我只是提取**固定区间**的数据，比如说有个单子是1-50星的，这个时候计算平均值时候你说把他放在哪个区间里面，所以我就把他舍去了。

![image-20240101215600030](https://my-picture-bed1-1321100201.cos.ap-beijing.myqcloud.com/mypictures/image-20240101215600030.png)

## 写在后面

真的，**做正确的事比正确的做事更重要**。永远都是**选择大于努力**。21年时候随便接个1500的单子一把都10块钱，再看看现在，还好王者荣耀代练我跑路跑的快。