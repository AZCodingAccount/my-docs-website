---
layout:doc
---

# Python

生成随机数：random.randint 是生成整数随机数（左右都包括），random.uniform 是生成小数随机数，random.rand 是 0-1 的随机数（包左不包右）

列表推导式：new_x_list=[x.append(1) for x in x_list]。简化书写，把后面循环的每一个变量传递给前面的变量

判断数据类型：isinstance(my_list , list)。返回一个 bool 类型的值

- 对于 for 循环，while 循环，if 语句，函数等来说，python 使用缩进代表层次关系，如果你在缩进语句下加了一行不符合缩进规则的代码，则解释器会认为这行代码下面的代码不属于上面的层级
- 如果双引号之间包括了双引号，就用单引号来代替。
- 对 if key in dict.keys 是一个判断语句，有这种语法，一个元素是否在元组里面
- 还有 if "/" not in title。这些语句都是数据容器常见的语法，在里面，不在里面

## python 基础语法

### 基础概念

**字面量**：在代码中，被写下来的值，就叫做字面量，用于指示是什么让代码认出来的。例：123，“张晗”，[ ], ( ), { }

**注释：**

- 单行注释：快捷键 ctrl+/，#号加一个空格，例：# 这是单行注释
- 多行注释：pycharm 无快捷键，例：""" 这是多行注释 """

**变量：**记录数据用的量

注意：python 中变量声明方式为 name="张三"，前面没有关键字

**数据类型：**

- 初步的数据类型有 int，string，float，bool
- 查看数据类型使用 type()函数
- 需要转换数据类型时使用 int(a)形式，类似与 Java 的强转但是 Java 是(int)a

**标识符：**

标识符就是变量，方法名等，命名规范跟 Java 基本一致。命名规则：

1. 标识符只能由数字，字母，下划线，中文（不推荐）组成，命名时不能以数字和$开头
2. 对大小写敏感
3. 不能以关键字命名
4. 变量命名规范，见名知意，全部小写，各单词以下划线隔开

**运算符：**

- 算术运算符：+，-，\*，/，//，%，\*\*。其中//是取整除，如 9.0/2.0=4.0；两个星代表的是乘方
- 赋值运算符：=，+=，-=，\*=，/=，//=，%=，\*\*=。用法不再赘述，简化运算
- 比较运算符：==，!=，>，<，>=，<=。
- 逻辑运算符：and，or，not
- 按位运算符：<<，>>等等

**字符串：**

字符串是每个编程语言的重点，python 主要注意：

1. 字符串的定义：单引号，双引号，三引号

2. 字符串的拼接：可以使用+号进行拼接，但是效率不高

3. 字符串在 print 语句中的表现

   - 字符串的格式化方法 format。利用"姓名是{name}，年龄是{age}".format("张晗",19)

   - 使用 print()语句中的逗号，print("str1","str2")。这种方式输出的中间有空格
   - 使用+号拼接的方式，print("str1"+num1+"str2")
   - 使用占位符的形式，类似于 C 语言，print("这是字符串拼接%4.2f"%num)，代表输出占四位，保留两位小数，输出的是 float 类型，常见的类型还有%d(decmical 十进制整数),%s(字符串)。如果值超过 1 个，就用()包裹，中间用,号隔开
   - 使用内嵌变量的方式，类似于 JS，print(f"这是内嵌变量的演示{num2}")，这个是原样输出,num2 也可以替换成表达式

**数据输入：**采用 input 语句，函数内可以放占位文字，需要注意的是，用变量接收的都是字符串，需要手动强转成自己需要的类型

### 判断和循环

#### if 语句

if 语句的结构基本有三种，注意语法跟其他语言不太一样，语法如下

- if-else 语句
- elif 语句
- if 语句的嵌套。我认为这个的应用场景是不用写 elif，层次感强一点，或者说第 n 层的嵌套语句中有很多个执行后也需要一起执行的代码时，我写的猜数字游戏。虽然我感觉还是没啥用。

```python
# if-else语句
a = 5
if a > 10:
    print("a的值大于10")
else:
    print("a的值小于等于10")

# elif语句
if "str1" == "str2":
    print("python亡了")
elif "str1" == "str1 ":
    print("字符串用==号判断时忽略空格")
else:
    print("字符串用==号判断时忽略空格")

# if语句的嵌套
a = 1
b = 2
c = 3
if a < b:
    print("a已经小于b了")
    if a < c:
        print("a也小于c")
else:
    print("a不小于b")
```

#### while 循环

while 循环语句的总体逻辑是跟其他编程语言一样的，就是语法有些差别，并且在构建状态机时，需要注意 python 里面的格式是 while True:，而不是 true。并且 while 语句也可以嵌套，格式就是缩进 4 个空格。

```python
# 猜数字案例，while语句跟if语句的嵌套结合使用
random_num = random.randint(1, 100)
count = 1
guess_num = int(input("请输入一个猜的数字："))
while 1:
    if guess_num == random_num:
        print(f"恭喜你，你在第{count}次猜中了")
        break
    else:
        if guess_num > random_num:
            print("大了")
        else:
            print("小了")
        count += 1
        guess_num = int(input("请输入一个猜的数字："))


# 利用while循环输出九九乘法表，while语句的嵌套
# i控制行，j控制列
i = 1
while i <= 9:
    j = 1
    while j <= i:
        # \t制表符，让前面的对齐
        print(f"{j}*{i}={i * j}\t", end='')
        j += 1
    # 换行，把列值重置为1，把行值加1
    print()
    i += 1
```

#### for 循环

for 循环与 Java 语言不太相同，但是跟增强 for 类似，遍历元素，格式为 for x in str:这个 str 可以是字符串还有 range()函数，range 函数的调用是 range(0,20)，就是 x 的值会一直从 0-19，左闭右开。

**for-in 遍历的时候，那个 data 并不能改变原有的值**。比如说 for data in my_list:这个时候你再做任何操作都不影响 my_list 里面原来的值。

for 循环可以跟 while 循环嵌套使用，对于 continue 和 break 关键字，和 Java 一致，以下是代码示例。

此外就是 print 语句中一些扩展知识：

- 在代码中加入\t 制表符会代表代码自动会缩进一行，对于格式的对齐尤为重要
- 如果 print()函数内不想自动换行，可以使用 print("str1",end='')
- 在代码在可以加入\n 表示自动换行
- print()语句单独使用表示换行

```python
# for循环对字符个数计算
s = "itheima is a brand of itcast"
num_count = 0
for x in s:
    if x == 'a':
        num_count += 1
    else:
        continue
print(num_count)


# for循环和while循环相互嵌套，输出九九乘法表
# 定义控制行的变量
row = 1
while row <= 9:
    # 定义控制列的变量
    column = 1
    # column就代表每一列,row代表第n行
    # 相等于while里面的column<=row
    for column in range(1, row + 1):
        print(f"{column}*{row}={row * column}\t", end='')
    print()
    row += 1
```

### 函数

1. 函数就是为了简化程序代码，便于代码的重用性和简洁性所设计的。函数满足三个条件：提前写好的，可重用的，实现特定功能的代码段。

2. python 中的函数关键字是 def，弱类型语言，传参和返回参数都不用指定类型，由于作用域的关系，在函数体内部如果想要更改外部变量的值或者让外部代码访问到函数内的变量，只需要在函数中用 global 关键字修饰。

3. 对于函数的返回值，如果函数没有写 return 语句，返回值也是可以接收的，值为 None，类型为 NoneType，也可以手动返回 None。在变量初始化的时候，也是可以直接给初值为 None 的。代码如下
4. python 特别喜欢嵌套（if 语句，while 循环，for 循环），函数也可以嵌套，调用方式是在函数内部调用另一个函数是并发执行的，而非并行。调用函数的时候，要注意**函数代码一定要写在调用者上面**，不然程序访问不到函数就会报错。

代码如下：

```python
# 带有参数和返回值的函数调用
status = "张晗单身"


def my_message(sex):
    global status
    if sex == "女生":
        status = "这个女生爱上了张晗"
        print("欢迎来到哈尔滨理工大学！\n您的导游是张晗")
    else:
        print("欢迎来到哈尔滨理工大学，请自行参观")
    # 如果是女生这个也可以不写，status变量已经被改变了；如果是男生就会返回None
    return status


result = my_message("女生")
print(result)
```

#### 函数的拓展知识

python 由于是弱类型加上一些其他的特性，真的是让我蛋疼，python 里面的语法特性简直了，以下是函数拓展的知识

- 函数的多返回值。python 中的函数返回值可以返回多个**不同类型**的数据，不同数据之间用英文逗号分隔，在函数调用处接收的时候，用多个变量接收即可，中间用英文逗号分隔。

- 函数的传参方式。

  - 位置参数。根据参数位置传递参数，就是普通的传参。
  - 关键字参数。根据键值对的形式传递参数，可以把最后一个形参利用 k=v 形式在实参传递时候写成第一个。在跟位置参数混合调用的时候，**一定要写在位置参数后面**，并且不要重复赋值，不会覆盖会直接报错。混合使用时候聊胜于无。
  - 缺省参数。不传递参数值的时候会使用默认的参数值，在函数定义时候，缺省参数一定要定义在最后面。格式为 age=11,gender="男"。
  - 不定长参数。普通不定长定义形参时候是\*args，在函数内想要处理的时候外面传入的参数被保存在元组中了，直接根据下标调用就可以。对于键值对不定长形参是\*\*args，（如 name="张晗"）是存在了一个字典中，调用时候通过键调用就可以了。传递的时候 name="张三",age=19......

- 函数作为参数传递和**lambda**匿名函数。

  - 函数作为参数传递。形参定义时候正常定义，不用加括号。调用时候函数名(args1,args2,...)调用就可以了。不同与传统传参的方式，传的是函数，是一段逻辑，之前传的是数据。

  - lambda 匿名函数是在函数调用时候需要一个函数作为参数传递的时候使用。避免重复定义，应用很广泛。lambda 格式为 lambda 形参 1,形参 2：函数体,会自动 return 值，并且不能换行。

    ```python
    # lambda匿名函数，参数值是函数
    def print_data(compute):
        data = compute(1, 2)
        print(data)


    # lambda格式为lambda 形参1,,形参2：函数体
    # 会自动return，不能换行。
    print_data(lambda x, y: x + y)

    ```

```python
# 1:演示函数的多返回值
def evaluate_looks(name):
    if name == "张晗":
        return "zhanghan is very handsome", "我给他的颜值打10分", 10
    else:
        return "你很丑", "我给你的颜值打0分", 0


# 用三个参数接收数据
is_handsome, evaluate, level = evaluate_looks("张晗")
print(is_handsome)
print(evaluate)
print(level)


# 2:函数的传参方式(关键字参数，缺省参数)
def print_stu_info(name, age, gender, idol="张晗"):
    print(f"这个学生叫{name},性别是{gender},{age}岁了，偶像是{idol}")


# 利用关键字参数给age赋值
print_stu_info("张晗", 19, gender="男")  # 这个学生叫张晗,性别是男,19岁了，偶像是张晗


# 利用不定长参数传参
def compute_add(*args):
    result = 0
    # 打印一下args的值和类型
    print(f"args的值是{args}，数据类型是{type(args)}")
    # 传入的参数不确定，所以直接遍历元组
    for x in args:
        result += x
    return result


# args的值是(1, 2, 3)，数据类型是<class 'tuple'>
# 6
print(compute_add(1, 2, 3))

# args的值是(1, 2, 3, 4, 5)，数据类型是<class 'tuple'>
# 15
print(compute_add(1, 2, 3, 4, 5))

dirt = {"张晗": 18, "李四": 19}
for x in dirt:
    print(x, dirt[x])


# 不定长传键值对
def stu_score(**args):
    # 此时args是一个字典
    for x in args:
        print(f"{x}:{args[x]}")


# 传递三个人的数据，输出结果：
# name:张晗
# age:19
# gender:男
stu_score(name="张晗", age=19, gender="男")
```

#### 递归

递归就是一种函数自己调用自己的语法，在解决复杂问题的时候找到他们的共性，然后分割成子问题进而求解。需要注意的是，递归一定要有初始条件。

以下是代码演示：

```python
# 演示一个简单的递归程序
# 本来是直接遍历文件里面的所有文件的，所以我们要引入python中文件操作的相关方法
import os.path


# os.listdir()是操作系统模块的一个方法，主要返回一个列表关于这个目录下的文件和文件夹
# os.path.exists()是判断路径是否存在
# os.path.isdir()是判断文件是不是个目录
# 此外还有很多对文件操作的方法

def get_files_recursion_from_dir(path):
    file_list = []
    if os.path.exists(path):
        for f in os.listdir(path):
            new_path = path + "\\" + f
            if os.path.isdir(new_path):
                file_list += get_files_recursion_from_dir(new_path)
            else:
                file_list.append(new_path)
    else:
        print(f"指定的目录{path}不存在")
    # 需要注意一定要把这个函数执行完的结果，这个列表返回出去，不然会报对象错误
    # 这个也是递归出口，遍历完所有的文件就会退出
    return file_list


# 调用以下方法，传入d盘的路径
file_list = get_files_recursion_from_dir("D:\\大学课程\\操作系统")
# "D:\大学课程\java"

print(file_list)
listdir = os.listdir("D:\\大学课程\\java")
print(type(listdir))
```

### 数据容器

数据容器就是数据在 python 里面的存放容器，类似于 Java 的集合，数组等等，以下是这几类常见的数据容器的介绍

#### 列表

**两个列表可以直接相加得到一个新列表**

\*把列表中的每个值转成多个参数

列表类似于数组？但是又有很多供他使用的方法。列表的特点是存储很多元素（2^63-1），支持索引（数据有序），允许重复，可以修改，可以存储不同数据类型的数据。当然，在列表中嵌套其他容器也是可行的，比如说列表里面再存列表。

- 定义方式：列表的字面量是[]，定义空列表有两种方式；my_list=list(); my_list=[]
- 列表遍历直接 for x in my_list 即可，x 就代表列表中每一个元素。

常见方法：增删改查

| 调用方法                  | 作用                                                             |
| ------------------------- | ---------------------------------------------------------------- |
| my_list.append(ele)       | 向列表尾部中追加一个元素                                         |
| my_list.extend(container) | 将数据容器中的数据全部追加到列表尾部                             |
| my_list.insert(index,ele) | 在指定下标处，插入指定的元素                                     |
| del my_list[index]        | 删除列表指定下标元素                                             |
| my_list.pop(index)        | 删除下标指定元素，不给下标则默认删除最后一个。会返回被删除的元素 |
| my_list.remove(ele)       | 从前往后，删除此元素第一个匹配项，返回被删除的元素               |
| my_list.clear()           | 清空列表                                                         |
| my_list[index]=ele        | 把指定元素的值更改一下                                           |
| my_list.count(ele)        | 统计此元素在列表中出现的次数                                     |
| my_list.index(ele)        | 查找指定元素的下标，找不到报错 ValueError                        |
| len(my_list)              | 统计容器中有多少元素                                             |

```python
my_list = [2, 4, 5, "张晗", "李四"]
# 增加元素
my_list.append("唱跳rap")
my_list.extend([6, 7, 8])
my_list.insert(1, 3)
# [2, 3, 4, 5, '张晗', '李四', '唱跳rap', 6, 7, 8]
print(my_list)

# 删除元素
# 删除李四这个元素
del my_list[5]
# 删除最后一个元素，他会返回值
pop = my_list.pop()
print(pop)  # 8
# 指定元素的值删除
my_list.remove(2)
# [3, 4, 5, '张晗', '唱跳rap', 6, 7]
print(my_list)

# 更改元素
my_list[4] = "打篮球"
print(my_list)

# 查找元素
# 查找张晗的数量
count = my_list.count("张晗")
# 统计列表的长度
len = len(my_list)
print(f"张晗有{count}个,列表长度是{len}")
# 查找打篮球的下标
my_list_index = my_list.index("打篮球")
print(my_list_index)

```

####

#### 元组

元组跟列表的唯一差别就是不可修改。所以它对应的方法也只有查找的方法。统计数量和找下标，也可以通过下标访问元组中的元素

- 元组的字面量是（），定义空列表有两种方式；my_tuple=tuple(); my_tuple=()

- 列表遍历直接 for x in my_tuple 即可，x 就代表元组中每一个元素。

- **在定义元组的时候，如果元组中只有一个元素，需要在后面加上一个逗号，不然解释器会识别出字符串**

#### 字符串

字符串可以说是数据类型中最重要的一个了。对于 python 中的字符串，特点如下

- 只可以存储字符串
- 长度任意
- 支持下标索引
- 允许重复字符串存在
- 不可以修改（因此每次对字符串操作都是创建一个新串）

对字符串的操作是重中之重，相应的方法有很多，以下只列出几种常见的方法

| 调用方式                    | 作用                                                            |
| --------------------------- | --------------------------------------------------------------- |
| s[index]                    | 根据索引取出相应的字符                                          |
| s.index(little_s)           | 查找小串在大串中的第一个匹配项的下标                            |
| s.replace(str1,str2)        | 将 s 里面的所有 str1 全部替换成 str2，得到一个新的字符串        |
| s.split(str)                | 按照 str 这个字符串进行分割，不会修改原有字符串，得到一个新列表 |
| s.strip()<br />s.strip(str) | 不传参的时候去除空格和换行符，传参的时候去除所有指定字符串      |
| s.count(little_s)           | 统计小串在大串中的出现次数                                      |
| len(s)                      | 统计字符串的字符个数                                            |

#### 序列的切片

序列的切片应该是 python 独有的功能了吧，非常好用。应用场景是截取指定字符串，反转字符串，range 函数里面也用了步长为负数的思想。

语法是：字符串/元组/列表[起始下标：结束下标：步长] 表示从序列中，从指定位置开始，到指定位置结束，得到一个新序列。

起始下标和结束下标可以留空，起始下标留空视为从头开始，结束下标留空表示截取到结尾

步长表示依次取元素的间隔。默认为 1，步长为 N，表示每次跳过 N-1 个元素取。步长为负数，表示反向取（开始下标和结束下标也要反向标记）

```python
# 得到指定数据
s="我是张晗，我很emo，浪费好多时间啊啊啊"

# 得到ome这个数据
# 倒着取的话从右边开始数，默认从0开始，步长为1是连续取
new_s=s[11:8:-1]
print(new_s)

# 实现字符串的倒序
# 需要注意字符串和元组都是不能更改的，所以处理结果是新的字符串
reverse_s=s[::-1]
print(reverse_s)

```

#### 集合

集合跟前面最大的不同是存取无序（不支持下标索引）,集合也不允许重复元素，但是集合可以修改。

集合的字面量是{}，定义空集合的方式是 my_set=set()，没有 my_set={}，这是由于这个定义方式被字典占用了。

集合的常见方法如下：

| 方法调用形式                       | 方法作用                                                      |
| ---------------------------------- | ------------------------------------------------------------- |
| my_set.add(ele)                    | 往集合中添加一个元素                                          |
| my_set.remove(ele)                 | 移除集合内指定的元素                                          |
| my_set.pop()                       | 从集合中随机取出一个元素                                      |
| my_set.clear()                     | 清空集合                                                      |
| my_set1.difference(my_set2)        | **得到一个新集合**，内容是两个集合的差集(A-B)，原集合内容不变 |
| my_set1.difference_update(my_set2) | 操作原理一样，只不过不再生成新集合，而是修改 my_set1(A=A-B)   |
| my_set1.union(my_set2)             | **得到一个新集合**，内容是两个集合的和集，原集合内容不变(A+B) |
| len(my_list)                       | 求集合的元素数量                                              |

代码演示：

```python
# 定义两个集合
my_set1 = {1, 2, 3, 4, 5}
my_set2 = {1, 2, 3}
difference = my_set1.difference(my_set2)
union = my_set1.union(my_set2)
# 求集合一的长度
len = len(my_set1)
# 两集合的差集是{4, 5}，两集合的和集是{1, 2, 3, 4, 5}
print(f"两集合的差集是{difference}，两集合的和集是{union}")
print(f"集合一的长度是{len}")
# for循环遍历数据，遍历出来的是集合的元素
for ele in my_set1:
    print(ele)

```

#### 字典

字典是基于键值对的数据容器。**字典中的 key 不能重复（重复添加会覆盖），并且模式是根据 key 检索 value，所以字典不支持下标索引，但是可以更改。**

由于可以嵌套使用的特点，导致字典的功能特别丰富，字典里面的 key 不能为字典，其余都可以。比如说存储学生信息的时候，用 key 存储学生姓名，对应的 value 存储以列表的形式再存储学生的年龄，成绩等等信息。

字典的字面量是{}，跟集合的一样，但是中间格式需要是 key:value 的形式。定义空字典 my_dict={}，my_dict=dict()。调用的时候用 my_dict[key]调用。

| 操作               | 说明                                                                                     |
| ------------------ | ---------------------------------------------------------------------------------------- |
| my_dict[key]       | 获取指定 key 对应的 value 值                                                             |
| my_dict[key]=value | 添加或更新键值对                                                                         |
| my_dict.pop(key)   | 取出 key 对应的 value 并删除此 key 的键值对                                              |
| my_dict.clear()    | 清空字典                                                                                 |
| my_dict.keys()     | 获取字典中的全部 key，可用于 for 循环遍历字典(存在一个<class 'dict_keys'>数据结构当中了) |
| my_dict.values()   | 获取字典里面的全部值                                                                     |
| len(my_dict)       | 计算字典中的元素数量                                                                     |
| my_dict.get(key)   | 获取对应的 value，如果不存在，就会报错，相当于直接用下标访问                             |

```python
# 定义一个字典，键都是字符串，对应的值一个是列表，一个是字典
my_dict = {"张晗": ["男", 18, "唱跳rap"], "李四": {"性别": "男", "年龄": 18, "爱好": "打篮球"}}

# 添加一个元素，小君君
my_dict["小君君"] = "女"

# 获取字典中的所有key
# dict_keys(['张晗', '李四', '小君君'])
keys = my_dict.keys()
# <class 'dict_keys'>，单独的一个数据结构
print(f"字典中获取的全部key存到了{type(keys)}里面了，内容是{keys}")

# 遍历字典并打印
# 张晗=['男', 18, '唱跳rap']
# 李四={'性别': '男', '年龄': 18, '爱好': '打篮球'}
# 小君君=女
# 遍历出来的是key
for key in my_dict:
    print(f"{key}={my_dict[key]}")
```

###

### 文件的读写

文件的读写操作需要先用 open 函数打开文件，close 函数关闭文件，此外 python 提供了 with-open 语句来自动关闭文件，语法如下

- open(name,mode,buffering,encoding)。

  - name 表示文件名，文件名如果是路径，注意转义字符，如果是单独的文件名，需要在当前包或者模块下存在。如果 mode 指定的是 w 或者 a 模式，会自动创建。
  - mode 参数表示模式，有三种模式 r(read),w(write),a(append)，传递的时候用字符串传递。
  - buffering 参数表示缓冲区大小，初学不用指定用默认就可以。
  - encoding 表示字符编码，默认都是 utf-8，当然有需要也可以使用 GBK 等。如果 buffering 不指定参数，那么 encoding 需要用关键字参数指定。

- close()函数。文件名直接调用就可以，f.close()，一般结合异常处理 try-except-finally 语句使用。

- with-open 语句。避免忘记关闭文件，格式为：with open(name,mode,buffering,encoding) as f:

  ​ 下面要写的逻辑......

  f 是文件对象

#### 文件读取操作

需要注意的是，如果程序刚开始读取了文件，下一个读取方法会从文件结束读取的位置开始读。如果读完了，下一个方法读出来的就是空，就算文件中有多少数据都没用。

常见方法：

- read()方法，一次全部读取，可以传入参数，代表一次读几个字节
- readline()方法，一次读取一行，返回值是读取的内容
- readlines()方法，把文件中数据按行读取并存储在列表中。（注意：\n 也会读取到，一般采用 strip()方法去除）
- for line in 文件对象。按行读取，会读取到换行符。

```python
f = open("a.txt", "r", 1024, encoding="utf-8")

# 先改成写的模式往文件里面写点数据
# f.write("我是张晗\n")
# f.write("我很帅\n")
# f.write("天才少年尘宸")

# reads = f.read()
# 会把换行符一并读取
# print(reads)

# line = f.readline()
# print(line)

# lines = f.readlines()
# print(lines)

# for循环读取
for line in f:
    line = line.strip()
    print(line)

```

#### 文件写入和追加操作

文件写入的时候，如果文件不存在，会重新创建，如果文件不存在，就清空原有内容。因此引入了 append 追加模式。

文件写入的时候写入的方法有：

- write(data)方法：往文件里面写入内容。返回值是写入的字符长度
- flush()方法：把缓冲区的数据刷新到硬盘中。
- close()方法：关闭文件，内置了 flush 功能。

文件追加写入跟文件写入基本一样，创建文件时候把模式设置成追加模式就可以了。追加模式的特点是写入文件时候不会覆盖前面的文件了。

```python
# 往文件里面写点数据
# 打开文件
f = open("张晗.txt", "w", encoding="utf-8")

# 往文件里面写入数据
f.write("张晗是个19岁的帅哥\n")
f.write("身高一米八五\n")
f.write("很多女生都喜欢他")

# 如果文件不刷新，数据不会写入到硬盘中，而是先在缓冲区中
# 但是好像python新版不刷新也能写进去了
f.flush()

# 关闭文件流,内置了flush功能
f.close()

f = open("张晗.txt", "a", encoding="utf-8")
# 这个时候因为前面文件已经关闭了，然后可以用f，再写入一行会追加进去
f.write("\n但是张晗一心向学")

```

### 异常处理

对于异常处理可以说每个编程语言都有的模块了。在实际开发中也是必不可少的模块，因为当出现异常的时候我们往往不希望程序直接停下来，而是想要它们在日志中给出错误信息后还可以继续运行。对于 python 的异常处理如下

- 捕获常规异常：采用 try-except 语句，捕获的是全部异常，但是不能获取异常对象。

- 捕获指定异常：在 except 语句后面指定异常名即可。

- 捕获多个异常：在 except 语句后面指定异常名并且多个异常用小括号包裹。

- 捕获全部异常：指定异常的时候指定一个最大的异常，Exception 异常。

  except 语句后面可以加个 as e 获取异常对象（常规异常不指定异常名不可以）

  try-except-else-finally 语句：else 后面跟的逻辑没有出现异常的时候会执行，finally 后跟的逻辑出不出现异常都会执行

  **出现异常以后 try 后面的语句就执行不了了，如果再有异常也不会被捕获**

代码演示如下：

```python
# 在这个python文件中演示各种异常是如何捕获的

# 捕获常规异常，不指定异常名，不能获取异常对象
try:
    a = 1 / 0
except:
    print("出现异常了")

# 捕获指定异常
try:
    open("a.txt", "r")
except FileNotFoundError as e:
    print(f"出现了文件未找到异常，异常的提示信息是{e}")

# 捕获多个异常
try:
    open("b.txt", "r")
    # ZeroDivisionError
    a = 1 / 0
    # NameError
    print(name)
#     异常用括号包裹，逗号分割
except (FileNotFoundError, ZeroDivisionError) as e:
    # 如果有两个异常，则程序默认捕获到第一个就不执行了（会执行下面的except语句），第二个异常不会捕获
    print(f"出现了文件未找到或者除0异常，异常提示信息是{e}")

# 捕获所有异常
try:
    c = 2 / 0
except Exception as e:
    print(f"出现了未知异常，异常提示信息为{e}")

# try-except-else-finally语句
try:
    # print(1)
    print(sex)
except Exception as e:
    print("出现异常了我会执行")
else:
    print("没有出现异常时候我会执行")
finally:
    print("有没有异常我都会执行")

```

异常具有传递性，函数调用的时候会一直到调用处，如果一直到调用处都没有对异常进行处理的语句，就会报错。代码如下：

```python
# 定义两个函数演示异常的传递性
def method2():
    try:
        print("这是一个method2执行语句")
        # 对method1方法进行异常处理了
        method1()
    except ZeroDivisionError as e:
        print("出现了0除错误")


def method1():
    a = 1 / 0
    print("method1执行语句")

# 最后打印出来了0除错误
method2()

```

### 导入模块和包

​ 导包这个可以说是老生常谈的话题了，一个语言内置的库是远远不够日常使用的，所以需要引入一些第三方包或者自定义的模块和包扩充功能。

​ 格式是 from 包名 import 模块名 as 别名（包名可以嵌套，别名是给下面程序使用的）

#### 自定义模块并导入

​ 如果导入两个模块调用同名功能时候后导的会覆盖前导的

​ 自定义包和模块的目的是因为往往需要自己定义一些工具包用于其他类的正常调用，这些包的命名方式一般都是 my*utils，里面有个* _init_ \_ \_文件时必须的（当你使用 pycharm 工具创建 python 软件包的时候会自动给你创建）。

​ 定义完成以后可以正常调用，调用方式为先导入包里的模块（会先执行这个模块的代码，如果有 print 语句就会直接打印在控制台）。from my_utils import File_util as file，再使用模块名调用对应方法,file.append_data()。

- \_ \_ _name_ \_ \_ _=="_ \_\_ _main_ \_ \_"变量。跟在 if 语句的后面，表示导包的时候 if 后面的语句不会自动执行，只有在程序直接运行的时候才会执行。

  - 格式为 if \_ \_ _name_ \_ \_ _=="_ \_\_ _main_ \_ \_":

        print("这个语句只有在直接运行的时候执行")

- \_ \_ _all_ \_ \_变量。这个变量的内容是一个存储字符串的列表，假如说有的模块有一些内置的方法不想被外界调用，可以采用这个变量，将除了这个方法以外的想被外界调用的方法给添加到 all 变量的列表中。

  - 格式为：_ \_\_all_ \_ \_=["test_A","test_B"] (test_A 和 test_B 都是模块名)
  - 这个时候采用 from 模块名 import\* 的时候就直接导入的是这个列表的元素.

#### 导入第三方包

​ python 的第三方包有很多，有 pyecharts 的画图工具包，还有 pygame 的游戏开发工具包等等包。这个包里也可能嵌套包，比如说 options, charts，Bar，Timeline 等基本的格式就是 from pyecharts.charts import Bar。这种方式的好处是你不用在创建对象时候写 charts.Bar()了，导入 from pyecharts.options import \*写 options 的时候不用每个都得 options.TitleOpts，options.ToolboxOpts 等。

### 对象

- 面向对象这个思想是当代编程的核心，python 也提供类的属性和行为来描述对象，当然 python 里面也继承了传统的对象的三大特性，封装，继承，多态。对于对象比较，对象格式化的有关魔术方法，对于对象的创建，有对应的构造方法。

- 一个特点就是 python 的注解，python 是弱类型语言，因此 python 提供对象注解来告诉解释器这个是什么类型，比如说对变量进行注解，对方法的参数和返回值进行注解，进行详细注解等等。

#### 变量的类型注解

用来指示编译器变量是什么类型，但是注意，这个不是强制要求。需要注意语法中的空格。如果解释器知道该变量的类型，就可以调用对应的方法。

需要注意的是，如果传参时候注解是 int，但是你传入一个 list，这时解释器会提醒你，但是不会标红。如果不违背语法的话可以正常运行，不会报错。

```python
from typing import Union

# 首先演示对于普通字面量的注解

a: int = 1
b: float = 2.2
c: str = "张晗"

# 演示对于数据容器的注解
d: list = [1, 2, 3]
e: dict = {"张晗": 19, "王五": 20}
f: set = {1, 2, 3}


# 演示对于函数参数和返回值的注解
# 对返回值的注解采用->形式，对参数注解采用: 类型名形式
# 这个方法的逻辑是计算传入的一个整数和一个集合的总和，并以int形式返回。
def add_num(x: int, y: list[int]) -> int:
    list_sum = 0
    for ele_list in y:
        list_sum += ele_list
    return list_sum + x


print(add_num(1, [2, 3, 4]))

# 演示详细注解和Union注解
# 在注解过程中，需要对数据容器中的多个数据指示多个不同的数据类型，这个时候需要详细注解
g: list[str, int, float, list] = ["张晗", 18, 15.5, [6, 6, 6]]
# Union注解，使用的时候需要导包
# 对元组类型进行注解的时候需要注意，因为受字面量的干扰。一般对列表进行注解
h: list[Union[int, str]] = [1, 2, 3, "张晗", "李四", "王五"]

```

#### 类和对象

python 类的定义也是使用缩进的方式去定义，采用 class 关键字声明，对属性赋值采用引号赋值或者等号，方法访问类中的属性采用 self 关键字调用。类中方法定义的时候需要传入一个参数 self，没有任何作用，仅仅标识这个方法是属于类中的。调用方法时候也不需要管

对象实例化的时侯不用 new，直接 student=Student()就实例化了一个对象。

```python
# 创建一个简单的person类，实现吃饭喝水等功能
# 演示类和对象的基本语法
class Person():
    name = None
    age = None
    sex = None

    def eat(self):
        print(f"{self.name}在吃饭")

    def drink(self):
        print(f"{self.name}正在喝水")

    def per_info(self):
        print(f"姓名：{self.name}，年龄：{self.age}，性别：{self.sex}")


person1 = Person()
person1.name = "张晗"
person1.age = 19
person1.sex = "男"
# 调用person1的打印方法看看属性是否添加进去
person1.per_info()
# 调用普通成员方法
person1.eat()
person1.drink()

```

#### 构造方法

构造方法类似于 Java 的空参，实参构造等，作用就是在创建类的时候给对象赋值。可以根据实际需求传递参数。

构造方法初始化时候可以直接定义，就不用再属性名: None 了。

```python
# 录入学生的一些信息并保存

# 定义一个类
class Student:
    name: None
    age: None
    addr: None
    count: None

    # 第4个参数是用来标识第几个学生
    def __init__(self, name, age, addr, count):
        self.name = name
        self.age = age
        self.addr = addr
        self.count = count
        print(f"学生{self.count}信息录入完成，信息为【学生姓名：{self.name}，年龄：{age}，地址：{addr}】")


# 采用循环方式创建学生对象录入信息
for s in range(0, 3):
    name = input("请输入学生姓名")
    age = input("请输入学生年龄")
    addr = input("请输入学生地址")
    student = Student(name, age, addr, s + 1)
print("3个学生数据创建成功！")

```

#### 魔术方法

要注意的就是这些魔术方法只实现就行了，调用比较时候直接根据打印对象或者两个对象比较时候用==或者>之类的符号，**不需要调用方法**（想调用的话也可以调用），返回 True 或者 False。

魔术方法就是类里面配备的方法，格式跟* \_\_init* \_ \_方法一样，前面两个下划线，后面两个。常用的方法就是 str，类似于 Java 的 tostring()，eq 方法用于比较对象。还有 lt,le 方法。init 方法也是魔术方法

- _ \_\_str_ \_ \_ 用于对象转字符串的行为。可以按照指定格式输出对象的属性值。如果不重写，输出对象的时候是地址值
- \_ _lt _ \_ 用于实现两个类对象的小于或大于比较的逻辑
- _ \_\_le_ \_ \_ 用于实现两个类对象进行小于等于或大于等于比较的逻辑
- _ \_\_eq_ \_ \_用于实现两个类对象进行相等比较的逻辑

```python
# 演示那4个魔术方法，以student对象演示
class Student():
    name: None
    sex: None
    age: None

    def __init__(self, name: str, sex: str, age: int):
        self.name = name
        self.sex = sex
        self.age = age

    # 以指定格式返回字符串
    def __str__(self):
        return f"姓名是：{self.name}，性别是：{self.sex}，年龄是：{self.age}"

    # 重写eq方法，更新比较逻辑。
    # 按照姓名和性别相等就认为这两个对象相等
    def __eq__(self, other):
        return self.name == other.name and self.age == other.age

    # 重写lt方法
    # 规定按照年龄大小排序
    def __lt__(self, other):
        # 可以返回大于，表示谁年龄大，谁对应的对象就小，当然一般用小于号，指定的属性谁小对应的对象就小
        return self.age < other.age

    # 重写le方法
    # 这个包含相等比较
    def __le__(self, other):
        return self.age < other.age


# 实例化三个学生对象用于演示魔术方法
student1 = Student("张晗", "男", 19)
student2 = Student("李四", "男", 20)
student3 = Student("王五", "男", 21)
student4 = Student("张晗", "女", 19)

# 开始调用魔术方法
# 调用对象转换成字符串，直接打印对象即可
print(student1)

# 判断对象相等，学生1和学生2姓名和年龄都不相等，因此False，学生1跟学生4姓名和年龄都相等，因此True
print(student1 == student2)
print(student1 == student4)

# 判断哪个对象大,学生3最大，因此为True
print(student3 >= student2)

```

#### 封装

封装主要就是私有和公有和保护的区别。

- 对于私有成员，只有类内部可以访问，它的子类和外部均不能访问，不能通过导入的方法来调用保护成员。格式是成员名前面加\_ \_
- 对于保护成员，只有类内部和他的子类可以访问，外部不能访问，并且不能通过导入的方法来调用保护成员。格式是成员名前面加\_
- 对于公有成员，都可以访问。
- 调用父类的方法的时候，一定是 super()再.变量或方法名

代码演示如下：

```python
# 封装基础语法演示
# 定义一个类
class Car():
    # 定义公有属性
    name: None
    price = None
    # 定义保护属性
    _version = "1.1"
    _ui = "红蓝"
    # 定义私有属性
    __os = "鸿蒙"
    __route_scheduling = "优先运行发动机"

    #     定义一些方法
    def drive(self):
        print(f"用户正在开{self.name}车")

    def _extend(self):
        print(f"继承的原来车型的版本号是{self._version},外观设计方案是{self._ui}")

    def __schedule(self):
        print(f"这辆车正在以{self.__os}操作系统运行")


class NewCar(Car):
    def print_str(self):
        # 调用不了上面私有的
        print(super()._extend())


# 实例化这个类
car = Car()
# 这时car只能调用公有的属性。
car.name = "奔驰"

# 实例化继承了Car的这个类
newCar = NewCar()
# 这个时候只能调用公有的方法，受保护的只能在继承的类内部调用
newCar.print_str()

```

#### 继承

​ 继承是子类可以继承父类的属性和方法。格式是定义类的时候在类的括号里面加上父类名。支持多继承。

​ 继承属性的时候会把属性的默认值也给继承下来。可以重写属性和方法，会覆盖的

​ 多继承的一个特性是继承的属性和方法如果有重名的，以括号里面越前面的类的属性为准

​ 需要注意继承的时候哪些变量和方法是直接继承下来的。哪些是可以访问的，哪些是禁止访问的。（私有公有保护的区别）

​ **子类调用的时候采用 super()这个关键字加括号，一定别忘了括号**

```python
# 简单定义一个父亲，一个儿子，一个爷爷
class Dad():
    hobby = None
    wealth = 10000

    def __init__(self, hobby, wealth):
        self.hobby = hobby
        self.wealth = wealth

    def earn(self):
        print("父亲在挣钱")


class GrandFather():
    hobby = "farming"


class Son(GrandFather, Dad):
    def print_data(self):
        # 如果Dad在前面，hobby继承的爱好就是Dad的属性对应的值，即使他的属性初始值为None
        print(f"从父类那里继承的爱好是{super().hobby},继承的财富是{super().wealth}")

    # 可以使用父类的构造方法对本类进行初始化，但是注意需要传参。
    def __init__(self, hobby, wealth):
        super().__init__(hobby, wealth)

    def print_self_data(self):
        print(f"自己的爱好是{self.hobby}，自己的财富是{self.wealth}")


# 实例化这个儿子类，创建的时候需要传递财富和爱好这两参数
son = Son("打篮球", 1000)
# 打印继承来的数据
son.print_data()
# 打印自己的数据（初始化传递的），但是这两个参数就不需要定义了，因为父类有
son.print_self_data()

```

#### 多态

多态是配合继承实现的。这里引入抽象类和 pass 关键字的概念

- 抽象类。只定义规则但是并不具体实现的类叫做抽象类。
- pass 关键字。如果函数体或类体不写逻辑并且需要满足语法规则不报错，写一个 pass。

这个时候就引入了多态的概念，实现这个抽象类的方式就是继承这个类，去重写里面的方法。然后如果方法有参数需要传递这个抽象类的对象的时候，我们并不传递抽象类的对象，而是传入一个抽象类的子类对象，这个现象叫做多态。可以是多个子类对象

```python
# 创建一个跑步的类
class Run():
    def slow_run(self):
        pass

    def fast_run(self):
        pass


# 定义一个人跑步的类
class PersonRun(Run):
    def slow_run(self):
        print("人正在慢跑")

    def fast_run(self):
        print("人正在快跑")


# 定义一个鸡跑步的类
class ChickenRun(Run):
    def slow_run(self):
        print("鸡正在慢跑")

    def fast_run(self):
        print("鸡正在半飞半跑的快跑")


# 定义一个健身函数，里面需要传递一个跑步的对象和当前健身的等级
def fitness(run: Run, level: int):
    if level < 10:
        run.slow_run()
    else:
        run.fast_run()


# 调用这个函数传参使用多态
# 这个时候参数需要Run，然而传的是子类。
fitness(ChickenRun(), 11)
fitness(PersonRun(), 9)

```

## Python 进阶

### 正则表达式

正则表达式是一种字符串验证的规则，通过特殊的规则来过滤出指定的数据

**负向前瞻**，筛选不包含某个字符串的内容`(?!.*指定字符串)` .\*是必须要加的，标识当前位置的任何字符后面都不能包含这个指定字符串，如果仅仅是不以该字符串开头，可以(?!指定字符串)

还有**负向后瞻**，这两个结合使用匹配 0-200 的字符：`count_list = re.findall(r'(?<!\d)(0|[1-9]|[1-9]\d|1[0-9]\d|200)(?!\d)', title)`

python 使用正则表达式之前需要导入一个内置的包，re。以下是正则的三个常用方法

| 方法名                 | 作用                                                                                    |
| ---------------------- | --------------------------------------------------------------------------------------- |
| re.match(regex,s)      | 从头开始匹配，匹配第一个命中项（如果第一个字符不符合第一个正则规则就直接不匹配了）      |
| re.search(regex,s)     | 全局匹配，匹配第一个命中项                                                              |
| re.findall(regex,s)    | 全局匹配，匹配全部命中项（常用）                                                        |
| re.compile(regex,re.S) | 返回一个正则表达式对象，不必重新编译。re.S 使.匹配任意字符，包括换行符。 (.\*?)爬虫常用 |

接下来叙述**如何编写正则表达式**

- 在 python 中需要表示\为一个普通的\字符时，可以采用 r 标记，格式如 regex=r"\d"

**匹配对应字符**

| 字符 | 功能                                    |
| ---- | --------------------------------------- |
| .    | 匹配任意字符（除了\n），\\.匹配点本身   |
| [ ]  | 匹配[ ]里面列举的字符，但是只是一个字符 |
| \d   | 匹配数字，即 0-9                        |
| \D   | 匹配非数字                              |
| \s   | 匹配空格和 tab 键                       |
| \S   | 匹配非空白                              |
| \w   | 匹配单词字符，即 a-z，A-Z，0-9，\_      |
| \W   | 匹配非单词字符                          |

附：需要匹配汉字的时候直接去网上搜范围

**多个字符的规则**（格式如\d{3, }表示数字出现 3-无数次）

| 字符     | 功能                                 |
| -------- | ------------------------------------ |
| \*       | 匹配前一个规则的字符出现 0-无数次    |
| +        | 匹配前一个规则的字符出现 1-无数次    |
| ？       | 匹配前一个规则的字符出现 0 次或 1 次 |
| \{m\}    | 匹配前一个规则的字符出现 m 次        |
| \{m, \}  | 匹配前一个规则的字符出现至少 m 次    |
| \{m, n\} | 匹配前一个规则的字符出现 m-n 次      |

**选择字符的作用**

| 字符 | 功能                                                                 |
| ---- | -------------------------------------------------------------------- |
| \|   | 匹配左右任意一个表达式                                               |
| ( )  | 将括号内字符作为一个分组（分组是可以复用的，具体还有捕获非捕获等等） |

**特殊边界字符**

运用这个要明确是需要匹配子串还是全部，

- 正则加上^和$以后，就匹配的是全部，调用 findall 方法也是如果第一个字符就不符合正则规则就不匹配了。当然，开头结尾也可以出现在正则的任意位置，被包裹起来的正则表示严格匹配。
- \b 的使用则是用一对\b 包裹下来，表示这个里面的内容是一个完整的字符，比如\bPython\b，就会匹配 Python 而不会匹配 Pythonaaa。

| 字符 | 功能               |
| ---- | ------------------ |
| ^    | 匹配字符串开头     |
| $    | 匹配字符串结尾     |
| \b   | 匹配一个单词的边界 |
| \B   | 匹配非单词边界     |

下面是代码演示

```python
# 这里演示正则的特定字符匹配
import re

# 匹配qq号，长度5-11位，纯数字，第一位不为0
# 这里采用findall里面的字符串开头和结尾的两个字符，这样第一个字符不满足直接就不满足了，不会去寻找子串
qq_num_regex = '^[1-9]\d{4,10}$'
qq_num1 = "1592127378"
qq_num2 = '01263990'
qq_num3 = '123456789999'

# 调用findall方法，把正则和字符串传进去判断一下
qq_num1_result = re.findall(qq_num_regex, qq_num1)
qq_num2_result = re.findall(qq_num_regex, qq_num2)
qq_num3_result = re.findall(qq_num_regex, qq_num3)

# 打印一下是不是符合要求的qq号
print(qq_num1_result)  # ['1592127378']
print(qq_num2_result)  # []
print(qq_num3_result)  # []

# 匹配账号，只能由字母和数字组成，长度限制6-10位
# 这个正则使用单字符方括号进行字符类型的限制后使用数量字符控制6-10，注意方括号之间的不同字符正则之间直接写就行，不用加, 号之类的
# 还需要加上字符串开头跟结尾
ID_regex = r'^[a-zA-Z0-9]{6,10}$'
ID_num1 = "hanAlbert666"
ID_num2 = "zhanghan_"
ID_num3 = "zhanghan"

# 调用方法
ID_num1_result = re.findall(ID_regex, ID_num1)
ID_num2_result = re.findall(ID_regex, ID_num2)
ID_num3_result = re.findall(ID_regex, ID_num3)

# 打印结果
print(ID_num1_result)  # []
print(ID_num2_result)  # []
print(ID_num3_result)  # ['zhanghan']

# 匹配邮箱地址，只允许qq,163,gmail这三种邮箱地址，允许这样形式的存在han.123.@qq.com，不以下划线和0开头，不限制长度
# 编写正则
mail_num_regex = '^[1-9a-zA-Z](\w*.)*(@qq.com)|(@163.com)|(@gmail.com)$'

mail_num1 = "hanalbert892577@qq.com"
mail_num2 = "han.albert.892577@qq.com"
mail_num3 = "_hanalbert892577@q1.com"

# 调用findall方法
mail_num1_result = re.findall(mail_num_regex, mail_num1)
mail_num2_result = re.findall(mail_num_regex, mail_num2)
mail_num3_result = re.findall(mail_num_regex, mail_num3)

# 打印输出看看邮箱格式是否符合要求
print(mail_num1_result)  # [('analbert892577', '@qq.com', '', '')]，这个是因为findall会把每一组的数据都单独打印出来
print(mail_num2_result)  # []
print(mail_num3_result)  # []

```

### 多线程

计算机的 cpu 调度主要是通过进程调度来实现的，这个是操作系统的相关知识。而进程又分为多个线程，进程之间并不共享临界资源，而线程之间共享比如说内存空间等。

多线程主要指的是多个程序并行的执行，在这个模块中，一般需要解决线程之间处理共享数据的问题。

多线程当中有很多的知识点，在此只是做一个简单的演示，比如说线程名，线程锁......创建线程时候调用 threading.Thread()方法，方法对应的参数如下

- group:暂时无用，未来功能的预留参数
- target：执行的目标任务名（一般是函数，调用时候不要加小括号（））
- args：以元组的方式给执行任务传参
- kwargs：以字典的方式给执行任务传参
- name：线程名，一般不用设置

创建出来线程以后调用 thread.start()方法就可以了，执行完成后会自动终止，一般方法里面写成循环。

**如果 target 传入的是方法名()，那么开启线程以后就会出现一直执行第一个线程的情况**

```python
""""
python多线程的简单演示
"""
import threading
import time


# # 定义一个吃饭的类和一个喝水的类
# def eat():
#     while True:
#         print("我正在吃饭！！！")
#         time.sleep(1)
#
#
# def drink():
#     while True:
#         print("我正在喝水！！！")
#         time.sleep(1)
#
# # 创建多个线程调用这两个方法
# if __name__ == '__main__':
#     # target是方法名，不要加小括号。
#     thread1 = threading.Thread(target=eat, name="吃饭线程")
#     thread2 = threading.Thread(target=drink, name="喝水线程")
#
#     thread1.start()
#     thread2.start()


# 定义一个带参数的方法让线程调用
class carry_param_methods():
    def eat_param(self, eat_thing: str):
        while True:
            print(f"我正在吃{eat_thing}")
            # 执行完歇一秒
            time.sleep(1)

    def drink_param(self, location, drink_thing):
        while True:
            print(f"我在{location}喝{drink_thing}")
            # 执行完歇一秒
            time.sleep(1)


cpm = carry_param_methods()
# 创建两个线程，一个吃饭，一个喝水
# 演示以元组形式给执行任务传参
thread1 = threading.Thread(target=cpm.eat_param, args=("肯德基",))
# 演示以字典形式给任务传参
thread2 = threading.Thread(target=cpm.drink_param, kwargs={"location": "蜜雪冰城", "drink_thing": "奶茶"})

# 开启线程
thread1.start()
thread2.start()

```

### 网络编程

网络编程又叫 socket 套接字编程，每个编程语言都有的模块，主要就是 C/S 架构客户端和服务端之间进行简单的通信。pythonsocket 编程依托于 socket 包，**使用前先导包**。

客户端的代码相对简单。步骤就是：

1. 创建 socket 对象，采用 socket.socket()的方式，接收返回回来的 socket 对象
2. 连接服务器，输入目标服务器的 ip 地址和端口，采用 client_socket.connect()，注意 connect 传参时候：用元组形式。
3. 发送消息，都是字符串型，采用 client_socket.send(data.encode("UTF-8"))，需要编码成 UTF-8 再往外发。
4. 接收回来的信息，采用 client_socket.recv()接收到数据，再用 decode("UTF-8")方法解出来就可以了。
5. 关闭连接，client_socket.close()

服务端的代码复杂一点，涉及到端口监听和等待的方法。

1. 创建 socket 对象，采用 socket.socket()的方式，接收返回回来的 socket 对象
2. 给当前服务器绑定 ip 和端口，采用 server_client.bind()的方法，注意传参用元组
3. 监听端口，采用 server_client.listen()的方法，传的参数用整数，代表允许几个服务端链接
4. 等待客户端连接，不来就一直阻塞。采用 server_client.accept()的方法，注意返回的参数是一个元组，第一个元素是 conn 表示链接，第二个是 addr 代表客户端地址。这是仅适用于允许一个服务端连接的时候。
5. 接受客户端消息，采用 server_client.recv()的方法，返回的数据用 decode 解码
6. 发送给客户端回应，采用 server_client.send()的方法，传入的数据用 encode 编码
7. 关闭连接，server_client.close()

以下是客户端代码演示：

```python
# 这个python文件里面写客户端的代码
"""
     socket编程大概就几步。
     1. 创建socket对象
     2. 连接服务器
     3. 发送消息（以字节数组形式，注意编码格式）
     4. 接收回来的连接
     5. 关闭连接（这个应该是TCP协议，一直保持连接）
"""
# 导包
import socket

socket_client = socket.socket()

# 连接服务器，注意这个是元组
connect = socket_client.connect(("127.0.0.1", 10086))

# 发送消息并接收，一直发一直接用循环
while True:
    msg=input("请输入给服务端发送的消息：")
    if msg=="exit":
        break
    socket_client.send(msg.encode("UTF-8"))
    # 接收返回的消息,1024是缓冲区的大小
    recv_data = socket_client.recv(1024)
    print(f"服务端发送回来的消息是：{recv_data.decode('UTF-8')}")

# 关闭连接
socket_client.close()
```

接下来是服务端代码演示：

```python
# 这里写服务端的代码
"""
     socket编程大概就几步。
     1. 创建socket对象
     2. 绑定ip和端口
     3. 监听端口
     4. 等待客户端连接
     5. 接收客户端消息
     6. 发送给客户端消息
     7. 关闭连接（这个应该是TCP协议，一直保持连接）
"""
import socket
# 创建socket对象
socket_server = socket.socket()

# 绑定一个ip地址和端口
socket_server.bind(("localhost",10086))
# 监听端口,listen的参数代表接收的连接数量
socket_server.listen(1)

# 等待客户端连接
result = socket_server.accept()
conn=result[0]      # 客户端和服务端的连接对象
address=result[1]   # 客户端的地址信息

while True:
    # 接收客户端消息
    client_data = conn.recv(1024)
    # 打印客户端消息
    print(f'客户端发送过来的消息是：{client_data.decode("Utf-8")}')

    # 发送回复的消息
    response_data=input("请输入一个你要回复给客户端的数据：")
    if response_data=="exit":
        break
    conn.send(response_data.encode("UTF-8"))

# 关闭连接
conn.close()
```

### python 操作数据库

python 对数据库的操作演示简单的增删改查操作，以下并不涉及进阶的事务，数据库安全等模块。主要通过 pymysql 库实现

步骤就是：**导包->创建 MySQL 数据库的连接->得到游标对象->选择数据库->执行 sql 语句->接受返回值并输出->关闭连接**

代码如下：

```python
# 导包
from pymysql import Connection

# 创建MySQL数据库的连接
conn = Connection(
    host="localhost",
    port=3306,
    user="root",
    password="123456"
)

# 得到游标对象，用于选择数据库执行sql语句等操作
cursor = conn.cursor()
# 选择要操作的数据库
conn.select_db("bussiness_system")

# 执行sql
sql_query = "select * from admin"
# 返回的是这个表一共有几行
query_count = cursor.execute(sql_query)
# 调用fetchall方法,返回的是一个元组，每一行数据就是元组的一个元素，元组中嵌套元组。
query_data = cursor.fetchall()

# 打印查询返回的数据，如果需要取出数据直接按照索引取就可以了，也可以循环遍历
print(query_data)

# 其他增删改操作，格式都跟这个差不多，只是不用拿到全部数据了，就是fetchall方法不用执行了。

# 关闭连接
conn.close()

```

### json 数据格式

json 是在各个编程语言里面流通的一个轻量级的数据交互格式，由于各个编程语言的语法和存储方式不尽相同，因此就需要 json 格式，可以让编程语言里面的数据转换成 json 格式，也可以让 json 格式的数据转换到各个编程语言中，比如方言和普通话的区别。

json 在 python 里面有两种对应的格式，一个是列表，比如[1,2,3]，一个是字典，kv 键值对，比如{"name":"zhangsan","sex":"man"}。如果数据里面存在中文，调用 json 里面的 dumps 和 loads 方法的时候加上一个参数，ensure_ascii=False 就可以了。

代码如下：

```python
# 使用之前先导包
import json

# 准备一个列表数据，列表中每一个元素都是字典，将其转换成json格式
data = [{"name": "张三", "age": 13}, {"name": "李四", "age": 26}]

# 采用json里面的dumps方法
json_str = json.dumps(data)

# 现在就从列表转换成字符串了
print(type(json_str))

# 再从json转换成列表
# 调用loads方法
list_json = json.loads(json_str)

# 现在就从json转换成列表了
print(type(list_json))

# 当然也可以字典进行相应转换
data_dict = {"name": "张三", "age": 13}
# 接下来就跟上面的流程一样了

```

### pyecharts 包的使用

### PySpark 框架的简单使用

pyspark 框架是 python 大数据方向的一个框架，实现的功能就是对大量数据的处理。遵循数据输入-数据处理-数据写出的模式。在使用之前需要使用 pip 命令把包下载下来。

**数据处理的相关方法**

| 方法名            | 作用                                                                           |
| ----------------- | ------------------------------------------------------------------------------ |
| map(函数)         | 传入指定的逻辑执行对应的操作，一次只操作一个数据，一般采用 lambda 表达式       |
| flatmap(函数)     | 可以解除数据之间的嵌套，一个一个数据输出                                       |
| reduceByKey(函数) | 用于分组计算                                                                   |
| filter(函数)      | 过滤数据                                                                       |
| distinct()        | 不用传参，自动对 rdd 内数据去重                                                |
| sortBy(函数)      | 函数传入按照什么参数比较，升序还是降序是另一个参数，全局排序需要设置分区数为 1 |

**数据输出的相关方法**

| 方法名        | 作用                                              |
| ------------- | ------------------------------------------------- |
| collect( )    | 将 RDD 内容转换成 list                            |
| reduce(函数 ) | 对 RDD 内容进行自定义聚合（对所有数据自定义操作） |
| take( n )     | 取出 RDD 的前 N 个元素组成 list                   |
| count( )      | 统计 RDD 元素个数                                 |

## python 爬虫

python 爬虫的主要流程就是向指定 URL 发送请求-收到服务器返回的数据-利用第三方库把自己想要的数据解析出来。

所谓的爬虫进阶就是一个攻防对抗的过程，程序模仿浏览器向网站对应的服务器发送请求，为了节省资源，服务器（大多数）需要从各类请求中识别出来爬虫从而拒绝请求，一些恶意爬虫甚至无异于 DDOS 攻击。

python 里面用于发送请求的是 requests 库，用于处理和过滤数据的是 lxml 包和 bs 包，此外，也可以自己指定正则的规则，不过这个比较麻烦。以下是一些关于这两个包的简单示例

#### bs4 代码演示

以下这个代码演示仅仅只是调用了 bs 的 findAll()方法用于查找指定类名标签的值，后续比如说需要查阅特定标签下的元素，特定标签下子类的第几个元素，这种类似于选择器的形式，查阅文档即可 还有 select 选择器里面的采用的 CSS 选择器的语法。

```python
# 我们简单的爬取一些数据用于简单演示解析
# 爬取豆瓣评分前150的电影

# 导包
import requests
from bs4 import BeautifulSoup

# 定义一个变量用于记录评分是第几名
score_level = 1

# 定义请求头用于把请求伪装成正常的浏览器访问，采用键值对的形式
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.46"
}

# 采用response里面的get方法发送一个get请求
# 开始遍历子网页，假如我要得到前150名的数据，需要start开始到100，采用for循环，步长为25，因为一页显示25个，range包头不包尾
for x in range(0, 101, 25):
    response = requests.get(f"https://movie.douban.com/top250?/start={x}", headers=headers)

    # 接收到页面数据后对页面数据进行处理
    html = response.text
    # 得到bs对象，采用bs4自带的解析器的方式
    soup = BeautifulSoup(html, "html.parser")
    # 根据页面检查发现标题的类名都为title，因此采用soup里面的findAll方法
    all_titles = soup.findAll("span", attrs={"class": "title"})

    # 数据容器的类型是一个结果集合，只能遍历了
    # <class 'bs4.element.ResultSet'>
    # print(type(all_titles))
    # 得到数据后for循环遍历
    for title in all_titles:
        # 遍历的是一个标签数据结构，不是字符串，因此需要格式转换
        # 把<class 'bs4.element.Tag'>格式转换成 # <class 'bs4.element.NavigableString'>,这样可以调用字符串特有的方法。
        title = title.string
        # 这个时候会发现打印出来会有原语言的名字，并且发现原名都有一个/，那么就是说可以通过这个共性过滤一下
        if "/" not in title:
            print(f"豆瓣评分第{score_level}名的电影是\t\t{title}")
            score_level += 1  # 每遍历出来一个，就把表示第几名的数字加1

    # 这个数据仅仅是打印在了控制台，有后续需求可以把它存在数据库或者说word，excel文档当中

```

#### lxml 包和 xpath

采用的是 lxml+xpath 的的方式，在采用 lxml 解析过以后，采用 xpath 进行想要的数据的选择，xpath 具体语法直接看 w3school 文档就可以了，主要的就是谓语的正确使用。

以下的代码简单的爬取了一些教务在线的数据，代码演示：

```python
# lxml是xpath的一个解析模块，跟bs4库差不多，都是用于解析爬取下来的数据，常用的方法都是根据特定属性找元素
# 这个是演示lxml+xpath结合
# http://jwzx.hrbust.edu.cn/homepage/infoSingleArticle.do;jsessionid=FD1B2526E3CBC5C86066367F268FB6E1.TH?articleId=4236
# http://jwzx.hrbust.edu.cn/homepage/infoSingleArticle.do;jsessionid=FD1B2526E3CBC5C86066367F268FB6E1.TH?articleId=4231

# 导包
from lxml import etree
import requests

headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36 Edg/112.0.1722.46"
}
response = requests.get(
    "http://jwzx.hrbust.edu.cn/homepage/infoSingleArticle.do;jsessionid=FD1B2526E3CBC5C86066367F268FB6E1.TH?articleId=4231"
    , headers=headers)
html = etree.HTML(response.text)
# 调用xpath的语法
# 以下的表示从全局查找span元素中lang属性为EN-US的元素对象，/text()表示转换成文本数据
xpath_data = html.xpath("//span[@lang='EN-US']/text()", encodings="utf-8")
# 打印数据
print(xpath_data)

```
