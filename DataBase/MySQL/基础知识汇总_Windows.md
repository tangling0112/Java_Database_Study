# 第一部分

## `MySQL`安装好后在`Windows`下的存在情况与特点

- `Data`存储文件夹

> 其中存储了``MySQL``中我们创建的的数据库，表，配置文件等。当我们删除`MySQL`后,这个文件并不会被删除.

- `Application`存储文件夹

> 其中存储着我们的`MySQL`的应用

- 自启动服务

> 存在于我们的任务管理器的服务选项中.

- `Path`环境变量

> 存在于`系统-高级设置-环境变量-系统变量-Path`中,值得注意的是,我们的`MySQL`安装器不会自动配置环境变量,而是我们需要自己将`MySQL`的`bin`目录添加为系统变量

## 多版本`MySQL`共存

> 多个不同版本的`MySQL`是可以同时存在于我们的系统中的,当然我们应该注意不同版本的`MySQL`应该使用不同的端口号(这里我们的`MySQL 8.0 = 3306,MySQL 5.7 = 3307`)

### 不同版本`MySQL`登录

- 首先我们在命令行中登录`MySQL`是需要使用到`MySQL`的登陆代码的,因此我们在环境变量中是`MySQL 8.0`的`bin`目录在前还是`MySQL 5.7`的`bin`目录在前是有不同的.如果是前者,那么使用不带端口号的登陆就会登入`MySQL 8.0`,相反后者则会进入`MySQL 5.7`.
- 若我们的`MySQL 8.0`与`MySQL 5.7`的端口号分别为`3306/3307`,那么就可以通过指定端口号来进行不同版本`MySQL`服务的登陆
  - `mysql -u root -p -P 3306`:非明文密码登陆`MySQL 8.0`
  - `mysql -u root -p -P 3307`:非明文密码登陆`MySQL 8.0`
  - `mysql -u root -p123456 -P 3306`:明文密码登陆`MySQL 8.0`
  - `mysql -u root -p123456 -P 3307`:明文密码登陆`MySQL 8.0`

> **问题**:不同版本`MySQL`的密码不同的情况下,使用`8.0`版本的登陆代码,输入`5.7`版本的登陆密码与用户名,是否可以正确登陆`5.7`的`MySQL`服务?

## `MySQL`服务的启动与停止

> 服务的作用是用于连接我们的`MySQL`客户端,如果我们将服务关闭,那么我们便无法在命令行中连接到我们的`MySQL`
>
> 前提:以管理员权限打开命令行

- `net stop mysql2022_6_29`
- `net start mysql2022_6_29`

## `MySQL`的登陆与退出

### 登陆

> 命令:`mysql -u <用户名> -p<密码> -h <需要连接的MySQL服务器的IP> -P <MySQL服务进程的端口号>`
>
> 注意:`-p`之后如果要输入密码则不应该接空格

- `-h`:默认情况下为`localhost`,即我们当前主机.当我们需要远程连接其他主机上的`MySQL`时就需要指定这一属性为目标主机的`IP`
- `-P`:默认情况下为``3306``计算机的每一个进程都具备一个唯一的端口号,我们需要通过指定端口号来连接到目标主机指定的`MySQL`服务进程上.比如目标主机上有多个不同版本的`MySQL`,分别占据不同的端口,那么我们就可以通过这个属性来指定连接到哪个版本的`MySQL`服务上
- 实例
  - `mysql -u root -p<密码> -P 3306`:连接到`MySQL 8.0`
  - `mysql -u root -p<密码> -P 3307`:连接到`MySQL 5.7`

### 退出

> `quit/exit`

## `MySQL`数据库中文存储问题的解决(`MySQL 5.7`)

> 问题原因:`MySQL`的数据表的默认字符集为`latin`字符集,而这个字符集是无法表示中文字符的,因此当我们在创建数据表时没有显式地指明其要使用的字符集,那么我们的数据表就会默认使用`Latin`字符集

### 当前默认字符集查询

- `show variables like 'character_%'`
- `show variables like 'collation_%'`

### 默认字符集修改

- 我们需要在`MySQL`的`Data File`的`mysql.ini`文件中找到`default-character-set=`选项,将其修改成我们需要的默认字符集即可(如`utf8,utf16,gbk,gbk2312`等)
- <img src="F:\A_Java_DataBase_Study_FIle\DataBase\MySQL\基础知识汇总_Windows.assets\image-20220629230227930.png" alt="image-20220629230227930" style="zoom:80%;" />

### 显式地指明数据库/数据表使用的字符集

#### 创建

- `create databese <数据库名> charset '<Latin1|utf8|gbk2312...>'`
- `create table <表名>(...) charset '<Latin1|utf8|gbk2312...>'`

#### 修改

- `alter databese <数据库名> charset '<Latin1|utf8|gbk2312...>'`
- `alter table <表名> charset '<Latin1|utf8|gbk2312...>' `

> **注意**:
>
> - 我们修改了默认字符集后,`MySQL`并不会自动改变我们在修改之前创建的数据表使用的默认字符集,因此我们应该在创建数据表之前养成判断其需要使用的字符集的习惯.
> - 在修改好默认字符集后,务必要通过`net stop mysql2022_6_29和net start mysql2022_6_29`来重启服务
>
> - 在`MySQL 8.0`中默认使用的为`utf8`

## `MySQL`图形化管理工具

> 图形化管理工具能够让我们通过图形界面来操作我们的`MySQL`服务

### `MySQL Workbench`

### `Navicat Preminum`

> [点击下载](https://www.52pojie.cn/thread-1117892-1-1.html)

### `SQLyog`

> [点击下载](https://www.52pojie.cn/thread-1066141-1-1.html)

### `dbeaver`

## `MySQL`默认的自带数据库介绍

### `information_schema`

> 保存数据库服务系统的基本信息(包括但不限于)
>
> - 数据库名称
> - 表名称
> - 字段名称
> - 存储权限

### `mysql`

> 保存数据库运行时所需的系统信息
>
> - 数据库文件夹
> - 当前系统字符集

### `performance_schema`

> 存储``MySQL``服务的各个系统性能指标

### `sys`

> 存储`MySQL`服务的各个系统性能指标

# 第二部分

## `SQL`基本介绍

> 美国国家标准局(`ANSI`)制定了统一的`SQL`语言标准,但是不同的`DBMS`大多都具备不同的`SQL`,并没有严格地遵守`SQL`语言的标准.

### `SQL`语言的分类

- `DDL`:**数据定义语言**
  - `CREATE`:创建
  - ``ALTER``:修改
  - ``DROP``:丢弃
  - ``RENAME``:重命名
  - ``TRUNCATE``:清空
- `DML`:**数据操作语言**
  - `INSERT`:插入
  - `DELETE`:删除
  - `UPDATE`:更新
  - `SELECT`:查找
- `DCL`:**数据控制语言**
  - `COMMIT`:提交
  - `ROLLBACK`:回滚
  - `SAVEPOINT`:保存点
  - `GRANT`:授权
  - `REVOKE`:收回权限

### `SQL`语言的基本规则规范

- `SQL`语言借助分号来表示一个语句的结束,因此我们可以将一个语句任意拆分为多行,只要保证在结束的时候添加分号,就能保证`SQL`解析器能够正确解析我们的`SQL`语句

  ```SQL
  SELECT * FROM student;
  等价于
  SELECT *
  FROM student;
  ```

- `SQL`语言在`Windows`环境下是**大小写不敏感**的,但是在`Linux`环境下是**大小写敏感**的

- 推荐将`数据库名`,`表名`,`表别名`,`字段名`,`字段别名`都使用**小写**,`关键字`,`函数名`,`绑定变量`都使用**大写**

- `SQL`语言的单行注释使用`#或-- `,多行注释使用`/**/`

- 我们在创建``数据库``,``数据表``,``字段``等自命名成员时,应该保证我们的命名不与数据库系统的关键字相互冲突.**当已经相互冲突时我们可以通过使用着重号`<字符串>`的方式来告诉系统这不是一个关键字**

- `SQL`语言的不区分大小写指的是我们的`SQL`**指令不区分**大小写,而**数据表中的记录的属性的值是区分大小写的**.

## `MySQL`基础命令

- `show databases`:呈现所有现存的数据库
- `create databese <数据库名>`:创建数据库
- `delete database <数据库名>`:删除数据库
- `use <数据库名>`:使用指定数据库
- `show tables`:呈现当前数据库内的所有数据表
- `SOURCE <SQL文件>`:执行我们指定的`SQL`脚本
- `DESCRIBE <Table_Name> 或 DESC <Table_Name>` :显示指定表中的字段的**详细信息**(如`数据类型`,`是否可为空`,`是否主键`等)

## `MySQL`中的单引号与双引号

### 单引号

- 字符串
- 日期

### 双引号

- 别名

## ==`MySQL`的显式转换与隐式转换==

> **隐式转换**
>
> - **整数与整数**做``加减乘以及比较``,**不会**发生类型转换
> - **整数与整数**``相除以及比较``**会**被转换为浮点数相除或比较
> - **整数与浮点数**做``加减乘除以及比较``**会**被转换为浮点数的加减乘除以及比较
> - **整数与字符,字符串**的``加减乘以及比较``**会**被转换为整数与整数的加减乘以及比较
> - **整数与字符,字符串**``相除以及比较``**会**被转换为浮点数相除以及比较
> - **浮点数与浮点数**的``加减乘除以及比较``**不会**发生类型转换
> - **浮点数与字符,字符串**的``加减乘除以及比较``**会**被转换为浮点数与浮点数的加减乘除以及比较
> - **字符,字符串与字符,字符串**做``加减乘除以及比较``**不会**发生类型转换
> - 任何数据类型与``NULL``做``加减乘除以及比较(除安全等于以外)``的结果都必定为`NULL`

- `SELECT 1 + '1'`:结果为2
- `SELECT 1 + 'a'`:结果为1
- `SELECT 1 + 'ab'`:结果为1
- `SELECT 1 + NULL`:结果为``NULL``

## `MySQL`数据表的创建

## `MySQL`给数据表起别名

- `SELECT * FROM Student as S`
- `SELECT * FROM Student S`

## `MySQL`的查询初级

### 基础查询

- `SELECT * FROM <Table_Name>`
- ``SELECT 字段1,字段2… FROM <Table_Name>``

****

### 常量查询

- `SELECT 12 FROM <Table_Name>`
- `SELECT '汤凌' FROM <Table_Name>`
- `SELECT '汤凌',字段1 FROM <Table_Name>`

****

### 别名查询(`AS`)

> - **别名查询**时会按照字段原名查询我们指定的字段,但是在呈现结果时,设置了别名的字段就会以别名呈现出来,而非原名
>
> - **双引号**:我们可以使用`""`将别名包裹起来,其作用为使得`"别 名"`可以实现,即使得别名中可以带有空格.当然如果我们的别名中不带空格,那么可以不用添加`""`

- `SELECT 字段1 as <别名> 字段2,字段3 as <别名> FROM <Table_Name>`
- `SELECT 字段1 <别名> 字段2,字段3 <别名> FROM <Table_Name>`
- `SELECT 字段1 "<别名>" 字段2,字段3 <别名> FROM <Table_Name>`

****

### 去重查询(``DISTINCT``)

> 去重查询会将我们查询得到的记录中,对于我们指定的字段相同的记录,我们的查询语句最终只会呈现其中一个

- `SELECT DISTINCT 字段1 FROM <Table_Name>`:根据**字段1**进行去重
- `SELECT DISTINCT 字段1,字段2 FROM <Table_Name>`:根据**字段1**与**字段2**进行去重
- `SELECT 字段1,DISTINCT 字段2 FROM <Table_Name>`:根据**字段2**进行去重,这样的去重查询**一般都会报错**

****

### `MySQL`运算符

> **注意**
>
> - 用于`运算符`的字段并不一定是我们查询的字段之一,也可以是数据表中其他任意未被查询的字段

####  算术运算符

| **+** | 加         |
| ----- | ---------- |
| **-** | **减**     |
| ***** | **乘**     |
| **/** | **除**     |
| **%** | **取余数** |

#### 数学比较运算符

> `MySQL`中非零表示真,0表示假

| **>**      | **大于**                                                     |
| ---------- | ------------------------------------------------------------ |
| **<**      | **小于**                                                     |
| =****      | **等于,注意`1=NULL`的结果为``NULL``,`NULL=NULL`的结果为``NULL``** |
| **<=>**    | **安全等于,其与普通等于的区别在于,``2<=>NULL``的结果为0,`NULL<=>NULL`结果为1** |
| **!=或<>** | **不等于**                                                   |
| **<=**     | **小于等于**                                                 |
| **>=**     | **大于等于**                                                 |

#### 特殊比较运算符

| 运算符                   | 名称       | 作用                                 | 使用实例                                                  |
| ------------------------ | ---------- | ------------------------------------ | --------------------------------------------------------- |
| **IS NULL**              | 为空       | 判断值,字符串,表达式的结果是否为NULL | `SELECT * FROM students WHERE age IS NULL`                |
| **IS NOT NULL**          | 不为空     | 判断值,字符串,表达式结果是否不为NULL | `SELECT * FROM students WHERE age IS NOT NULL`            |
| **ISNULL**               | 为空       | 判断值,字符串,表达式结果是否为NULL   | `SELECT * FROM students WHERE ISNULL(age)`                |
| **LEAST**                | 最小值     | 返回多个值中的最小值                 | ``SELECT LEAST(字段1,字段2) FROM students``               |
| **GREATEST**             | 最大值     | 返回多个值中的最大值                 | ``SELECT GREATEST(字段1,字段2) FROM students``            |
| **BETWEEN  A AND B**     | 两值之间   | 判断一个值是否在[A,B]区间内          | `SELECT * FROM students WHERE age BETWEEN 25 AND 30`      |
| **NOT BETWEEN  A AND B** | 非两值之间 | 判断一个值是否不在[A,B]区间内        | `SELECT * FROM students WHERE age NOT BETWEEN 25 AND 30`  |
| **IN**                   | 属于       | 判断一个值是否属于给出的值中的一个   | `SELECT * FROM students WHERE age IN(20,25,23,36)`        |
| **NOT IN**               | 不属于     | 判断一个值是否不属于给出的值中的一个 | `SELECT * FROM students WHERE age NOT IN(20,25,23,36)`    |
| **LIKE**                 | 模糊匹配   | 判断一个值是否符合模糊匹配规则       | `SELECT * FROM students WHERE age LIKE('<模糊匹配式>')`   |
| **REGEXP**               | 正则匹配   | 判断一个值是否符合正则匹配规则       | `SELECT * FROM students WHERE age REGEXP('<正则匹配式>')` |
| **RLIKE**                | 正则匹配   |                                      | `SELECT * FROM students WHERE age RLIKE('<正则匹配式>')`  |

#### 逻辑运算符

> 逻辑运算符优先级排序为`NOT>AND>OR=XOR`

| **运算符**     | **作用**     |
| -------------- | ------------ |
| **NOT 或 !**   | **逻辑非**   |
| **AND 或 &&**  | **逻辑与**   |
| **OR 或 \|\|** | **逻辑或**   |
| **XOR**        | **逻辑异或** |

#### 位运算符

> 位运算是基于我们**给定的值的二进制数**来进行运算

| **运算符** | **作用**     | **实例**           |
| ---------- | ------------ | ------------------ |
| **&**      | **按位与**   | **`SELECT A & B`** |
| **\|**     | **按位或**   | **`SELECT A | B`** |
| **^**      | **按位异或** | **`SELECT A ^ B`** |
| **~**      | **按位取反** | **`SELECT ~A`**    |
| **>>**     | **按位右移** | **`SELECT A>>2`**  |
| **<<**     | **按位左移** | **`SELECT A<<2`**  |

> **注意**
>
> - **:red_circle: 空值`NULL`在`SQL`中并不等于0,当`NULL`参与运算时无论参与的是``加减乘除``中的任何一个运算,都会导致最后的结果为`NULL`,``NULL``参与比较运算的结果也为`NULL`**

****

### **条件查询(`WHERE`)**

> **条件查询通过借助比较运算符，逻辑运算符以及`WHERE`关键字的组合来实现数据记录的过滤**
>
> **注意**
>
> - 用于`WHERE`的字段并不一定是我们查询的字段之一,也可以是数据表中其他任意未被查询的字段

- **`SELECT 字段1,字段2 FROM <Table_Name> WHERE 字段1 = '汤凌'`**
- **`SELECT 字段1,字段2 FROM <Table_Name> WHERE 字段2 > 50 `**

****

### **模糊查询(`WHERE+LIKE`)**

#### **模糊查询运算符**

> **注意:模糊查询以包含模糊查询表达式的左边的单引号为开始标志,右边的单引号为结束标志**

| **符号** | **作用**                                            |
| -------- | --------------------------------------------------- |
| **%**    | **匹配不确定个数的任意字符**                        |
| **_**    | **匹配一个任意字符**                                |
| **\或$** | **转义字符,用于将用于匹配的专用字符转换为普通字符** |

****

### **排序查询(`ORDER BY`)**

> **注意**
>
> - 我们的字段的别名**只可以**在`ORDER BY`子句中使用,而`WHERE`等其他任何关键字的子句中是**无法使用**字段别名的.
> - 用于`ORDER BY`的字段并不一定是我们查询的字段之一,也可以是数据表中其他任意未被查询的字段
> - 当语句中使用到`WHERE`时`ORDER BY`必须使用在`WHERE`关键字之后
> - 无论使用升序还是降序排列,`NULL`永远在前
>
> **原因**:其原因涉及到排序与条件过滤对于数据表中记录的操作.

#### `MySQL`默认排序规则

- 当我们在查询时未指定查询结果的排序规则时,`MySQL`使用其默认的排序规则,**即按照我们在向数据表中插入数据时的各个记录插入的先后顺序来进行排序.先插入的记录排在前,后插入的记录排在后**

#### `MySQL`排序查询实例

##### 基础排序

- `SELECT 字段1,字段2,字段3 FROM <Table_Name> ORDER BY 字段3 `
  - 默认为``ASC``
- `SELECT 字段1,字段2,字段3 FROM <Table_Name> ORDER BY 字段3 DESC `
  - `DESC`:降序
- `SELECT 字段1,字段2,字段3 FROM <Table_Name> ORDER BY 字段3 ASC `
  - `ASC`:升序

##### 别名排序

- `SELECT 字段1,字段2,字段3 别名 FROM <Table_Name> ORDER BY 别名 `
- `SELECT 字段1,字段2,字段3*100 别名 FROM <Table_Name> ORDER BY 别名 `

##### 二级排序

> - 当某些记录的字段3取值相等时,那么这些记录就会按照字段2进行`ASC`排序
>
> - 参考二级排序,我们还可以设计**多级排序**

- `SELECT 字段1,字段2,字段3 FROM <Table_Name> ORDER BY 字段3 DESC,字段2 ASC `
- `SELECT 字段1,字段2,字段3 FROM <Table_Name> ORDER BY 字段3,字段2 ASC`

****

### **分页查询(`LIMIT`)**

> **注意**
>
> - 我们应该保证在查询语句中`LIMIT`关键字对应的子句位于`WHERE`,`ORDER BY`对应的子句之后(**实际上我们更进一步地,`LIMIT`关键字必须处于任意查询语句的最后**)
> - `LIMIT a,b`中``a``为起始索引,`b`为偏移量
> - **分页区间计算公式`区间=[a,a+b)`**
> - 在`MySQL`中第一条记录的索引为`0`
>
> **原因**:

#### 作用与应用背景

- **作用**
  - 分页即将我们的查询结果分成多个页,每个页由多个记录组成.我们每一次只能查看到某一页中的记录.而剩余的记录则需要涉及到对页的切换.

- **应用背景**
  - 当我们的查询结果由巨量的记录组成时,我们可以通过分页使得每一次只获取查询结果的指定条记录

#### 分页的实现

> **分页区间计算公式`区间=[a,a+b)`**

- `SELECT * FROM <Table_Name> LIMIT(0,20)`:返回$[0,20)$区间内的记录
- `SELECT * FROM <Table_Name> LIMIT(20,40)`:返回$[20,60)$区间内的记录
- `SELECT * FROM <Table_Name> LIMIT(0,10)`:返回$[0,10)$区间内的记录
- `SELECT * FROM <Table_Name> LIMIT(10,20)`:返回$[10,30)$区间内的记录

#### `MySQL 8.0`分页新特性(`LIMIT <a> OFFSET <b>`)

> **分页区间计算公式`区间=[b,b+a)`**
>
> - `b`为起始索引,`a`为偏移量.与纯`LIMIT`正好相反

- `SELECT * FROM <Table_Name> LIMIT 40 OFFSET 20`:返回$[20,60)$区间内的记录
- `SELECT * FROM <Table_Name> LIMIT 2 OFFSET 20`:返回$[20,22)$区间内的记录
- `SELECT * FROM <Table_Name> LIMIT 88 OFFSET 2`:返回$[2,90)$区间内的记录

#### 拓展(不同的`DBMS`如何实现分页查询)

- 在`MySQL`,`PostgreSQL`,`MariaDB`,`SQLite`中使用`LIMIT`关键字实现分页

- 在`SQL Server`与`Access`中使用`TOP`关键字

  ```SQL
  SELECT TOP 5 字段1,字段2 FROM <Table_Name> ORDER BY 字段3 DESC;
  ```

- 在`DB2`中使用`FETCH FIRST 5 ROWS ONLY`关键字

  ```SQL
  SELECT 字段1,字段2 FROM <Table_Name> ORDER BY 字段3 DESC FETCH FIRST 5 ROWS ONLY;
  ```

- 在`Oracle`中不具备分页关键字,我们只能通过`Oracle`给每个数据表自动维护的`rownum`字段来进行分页

  ```sql
  SELECT Rownum,字段1,字段2 FROM <Table_Name> WHERE Rownum<5 ORDER BY 字段3 DESC;
  ```

****

### **多表查询**(内连接,外连接,等值连接,非等值连接,自连接,非自连接)

> 多表查询即使用一个`SQL`语句查询多个不同的数据表中的数据,并在结果呈现时,将多个表的查询结果有机结合起来进行呈现.
>
> ==**当涉及到三个及以上的表的查询时最好不要使用`JOIN`**==
>
> **注意**
>
> - 不同与给**字段**起别名,在`SQL`语句中给**表**起了别名,那么其原名在改`SQL`语句中便不再可用
> - **当涉及到$$n$$个表的多表查询时,那么我们至少需要有$n-1$个连接条件**

#### 多表查询实例

- `SELECT 字段1,字段2 FROM <Table1_Name>,<Table2_Name>`:在字段1属于表1,字段2属于表2时	会返回字段1与字段2的笛卡尔积
- ``SELECT 字段1,字段2 FROM <Table1_Name> CROSS JOIN<Table2_Name>``:等价于上面的查询语句
- `SELECT 字段1,字段2 FROM <Table1_Name>,<Table2_Name> WHERE <Table1_Name>.字段3= <Table2_Name>.字段4 `
- `SELECT 字段1,字段2 FROM <Table1_Name>,<Table2_Name> WHERE <Table1_Name>.'字段3'= <Table2_Name>.'字段4' `:与上一个查询等价
  - 这里的`WHERE`是作为多表查询的表间连接条件出现的
- ``SELECT 字段1,字段2,<Table1_Name>.字段5 FROM <Table1_Name>,<Table2_Name> WHERE <Table1_Name>.'字段3'= <Table2_Name>.'字段4' ``
  - 当字段5在表1与表2中都出现时,我们就需要在查询时指明我们要查询的是哪个表中的字段5
  - **实际上我们推荐在使用到多表查询时,对于每一个待查询的字段都指定好其表的归属**
- ``SELECT <Table1_Name>.字段1,<Table2_Name>.字段2,<Table1_Name>.字段5 FROM <Table1_Name>,<Table2_Name> WHERE <Table1_Name>.'字段3'= <Table2_Name>.'字段4' ``
- ``SELECT T1.字段1,T2.字段2,T1.字段5 FROM <Table1_Name> T1,<Table2_Name> as T2 WHERE T1.'字段3'= T2.'字段4' ``:与上一个查询相同,只不过通过对表起别名的方法增加了`SQL`语句的可读性
- ``SELECT T1.字段1,T2.字段2,T1.字段5 FROM <Table1_Name> T1,<Table2_Name> as T2 WHERE <Table1_Name>.'字段3'= <Table2_Name>.'字段4' ``:与上一个查询相同,但这个语句是错误的,因为一旦在`SQL`语句中给表起了别名,那么其原名便不再可用

#### 多表查询的分类

- **等值连接与非等值连接**

  - **等值连接**
    - `SELECT s1.age,s2.address,s2.height FROM students1 s1,students2 s2 WHERE(s1.StudentID=s2.StudentID)  `
  - **非等值连接** 
    - `SELECT s1.name,s1.age,s1.height,s2.salary,s2.mother FROM students1 s1,students2 s2 WHERE(s1.age BETWEEN s2.least_age AND s2.greatest_age`

- **自连接与非自连接**

  > 即我们的表与表间联系的**自我引用**

  - **自连接**
    - `SELECT emp.employeeid,emp.last_name,mgr.employeeid,mgr.last_name FROM employees as emp,employees as mgr WHERE(emp.manager_id=mgr.employeeid)`
  - **非自连接**
    - `SELECT emp.employeeid,emp.last_name,mgr.employeeid,mgr.last_name FROM employees as emp,managers as mgr WHERE(emp.manager_id=mgr.employeeid)`

- **内连接与外连接**

  > **注意**
  >
  > - `MySQL`不支持`SQL92`标准,部分支持`SQL99`标准
  > - `Oracle`同时支持`SQL92`标准与`SQL99`标准
  > - `SQL92`与`SQL99`的内外连接我们可以按照相同的方式去理解

  - **内连接**

    - 一般用于合并具有**相同字段的多个表时使用**,其会将该相同字段上取值相同的各个表的记录拼接起来,然后根据我们的需要进行查询结果的返回.**那些该字段取值不相等的记录则会被不会被拼接,也就不会呈现在结果中**

    - `SQL92`

      - **单级内连接**
        - `SELECT s1.age,s1.name,s2.age,s2.name FROM students1 s1,students2 s2 WHERE(s1.height = s2.height)`
      - **多级内连接**
        - `SELECT s1.age,s1.name,s2.age,s2.name,s3.age,s3.name FROM students1 s1,students2 s2,students s3 WHERE(s1.height = s2.height AND s2.weight=s3.weight)`

    - `SQL99`

      - **单级`INNER JOIN`的使用**

        - `SELECT s1.age,s1.name,s2.age,s2.name FROM students1 s1 JOIN students2 s2 ON(s1.height = s2.height)`

          > **原理**就相当于先把`students2`中符合`ON`关键字指示的相等条件的记录合并到到`students1`的对应记录上,然后再对这个合并后的虚拟数据表进行查询

      - **多级`INNER JOIN`的使用**

        - `SELECT s1.age,s1.name,s2.age,s2.name,s3.age,s3.name FROM students1 s1 JOIN students2 s2 ON(s1.height = s2.height) JOIN students3 s3 ON(s2.weight=s3.weight)`

          > **原理**就相当于在得到第一个`JOIN`的虚拟表后,再将第三个表`JOIN`到我们的虚拟表中,形成一个新的虚拟表后再对这个新的虚拟表进行查询操作

  - **外连接**

    - **左外连接**$(A ∩ B)∪(A-B)$

      - 在**内连接**的基础上,还会返回**左表**中没有被呈现的记录的对应的被查询字段

      - **`SQL92`**

        - `SELECT s1.age,s1.name,s2.age,s2.name FROM students1 s1,students2 s2 WHERE(s1.height = s2.height(+))`

      - **`SQL99`**:**LEFT OUTER JOIN** 

        - `SELECT s1.age,s1.name,s2.age,s2.name FROM students1 s1 LEFT OUTER JOIN students2 s2 ON(s1.height = s2.height)`

          > **原理**就相当于首先把`students1`与`students2`中符合`ON`关键字指示的相等条件的记录拼接起来
          >
          > **然后**再在拼接好后的虚拟数据表中**自动添加**上**左表**中没有被添加到该虚拟数据表中的记录添加进来(**当然这会出现此时插入的记录的字段数与前面的记录字段数不匹配的情况,系统会自动补齐字段数,并将用于补齐该记录字段数的字段设置为`NULL`**)
          >
          > **然后**再对这个合并后的虚拟数据表进行查询

    - **右外连接**($(A ∩ B)∪(B-A)$)

      - 在**内连接**的基础上,还会返回**右表**中没有被呈现的记录的对应的被查询字段

      - **`SQL92`**

        - `SELECT s1.age,s1.name,s2.age,s2.name FROM students1 s1,students2 s2 WHERE(s1.height(+) = s2.height)`

      - **`SQL99 `****RIGHT OUTER JOIN**

        - `SELECT s1.age,s1.name,s2.age,s2.name FROM students1 s1 RIGHT OUTER JOIN students2 s2 ON(s1.height = s2.height)`

          > **原理**就相当于首先把`students1`与`students2`中符合`ON`关键字指示的相等条件的记录拼接起来
          >
          > **然后**再在拼接好后的虚拟数据表中**自动添加**上**右表**中没有被添加到该虚拟数据表中的记录添加进来(**当然这会出现此时插入的记录的字段数与前面的记录字段数不匹配的情况,系统会自动补齐字段数,并将用于补齐该记录字段数的字段设置为`NULL`**)
          >
          > **然后**再对这个合并后的虚拟数据表进行查询

    - **满外连接(全外连接)**($A∪B$)

      - 在**内连接**的基础上,还会返回**左表与右表**中没有被呈现的记录的对应的被查询字段
      - **`SQL92`**
      - **`SQL99`** **FULL OUTER JOIN**
        - `SELECT s1.age,s1.name,s2.age,s2.name FROM students1 s1 FULL OUTER JOIN students2 s2 ON(s1.height = s2.height)`
      - **`MySQL`**(**不支持满外连接**)

<img src="F:\A_Java_DataBase_Study_FIle\DataBase\MySQL\基础知识汇总_Windows.assets\image-20220630211243062.png" alt="image-20220630211243062" style="zoom:80%;" />

****

### `MySQL`使用`UNION`与`UNION ALL`实现满外连接

> **注意**:这两个关键字都是用于对两个`SELECT`查询的结果进行拼接,但是他们的拼接效果不同.这里我们假设第一个`SELECT`查询有$$10$$条查询结果,第二条查询有$$20$$条查询结果,并且两个查询的查询结果中有$$5$$条记录是重复的
>
> - `UNION`:拼接后会返回一个$(10-5)+5+(20-5)=25$条记录的结果
>
> - `UNION ALL`:拼接后会返回一个$[(10-5)+5]+[5+(20-5)]=30$条记录的结果下`下`

![image-20220630211718491](F:\A_Java_DataBase_Study_FIle\DataBase\MySQL\基础知识汇总_Windows.assets\image-20220630211718491.png)

<img src="F:\A_Java_DataBase_Study_FIle\DataBase\MySQL\基础知识汇总_Windows.assets\image-20220630212408645.png" alt="image-20220630212408645" style="zoom:67%;" />

****

### **`SQL99`新特性(自然连接`NATURAL JOIN`与`USING`连接)**

#### 自然连接`NATURAL JOIN`

> `NATURAL JOIN`会自动寻找两个待连接的表中**字段名相同的所有字段**，并且按照这些字段对我们的两个表进行等值连接

- `SELECT s1.age,s1.name,s2.age,s2.name FROM students1 s1 NATURAL JOIN students2 s2`

#### `USING`连接

> 我们可以通过`USING`**指定一个或多个两个表中都有的字段**，那么我们的两个表就会根据指定的这一个或多个字段对两个表进行等值连接

- `SELECT s1.age,s1.name,s2.age,s2 .name FROM students1 s1  JOIN students2 s2 USING(height)`

****

### ==`MySQL`的多表外连接的字段补齐问题==

> - 我们容易知道多表之间的外连接，由于会将左表或右表或左右表中没有符合连接条件的记录也会最终拼接到我们的待呈现虚拟表上，因此这就会带来两个个问题
>   - **问题1**：拼接的不符合条件的记录的字段数与符合连接条件的合并记录的字段数是不同的，那么如何让字段数不相同的记录共存在同一个虚拟表中呢？
>   - **问题2**：对于左外连接，是不是左表中所有不符合连接条件的记录都会被拼接到虚拟表中符合连接条件的合并记录之后呢？那右外连接和全外连接又是什么样的顺序呢？

****

### ==`MySQL`的多表连接的`WHERE`子句条件过滤问题==

> 我们可以对这个过滤做出如下简易化理解，但不代表这是`MySQL`的内部实现方式

- **首先**`MySQL`会对我们要连接的两个表进行笛卡尔积拼接操作(如左表$$10$$条记录,右表$$10$$条记录,那么拼接好后的虚拟表中则有$100$条合并记录)
- **然后**`MySQL`就会对我们上一步得到的共$100$条记录的虚拟数据表进行`WHERE`过滤操作
- **然后**通过上一步,$100$条合并记录中那些不符合连接条件的记录就会被我们的`MySQL`从虚拟数据表中剔除.
- **然后**`MySQL`就会对最终的虚拟数据表进行`SELECT`查询操作,并将得到的查询结果返回给用户

****

### ==`SQL`查询的简易化理解==

> 我们可以将`SQL`查询的过程理解为一个对数据表中每一个记录进行遍历筛选的过程.因此我们可以将`SQL`查询中的字段名理解为一个用于遍历的变量,其会在遍历数据表的每一条记录时跟随记录的变化而变化.**例如当遍历到第`t`个记录时,那么该`SQL`语句中的那些`WHERE`子句中的字段都可以认为是改记录中该字段所对应的值.**
>
> - 如第`t`条记录为
>
> | 姓名 | 年龄 | 身高 |
> | ---- | ---- | ---- |
> | 汤凌 | 20   | 181  |
>
> - 查询语句为`SELECT 姓名 FROM students WHERE 身高=181`
> - 那么查询到该条数据时查询语句实际为`SELECT 姓名 FROM students WHERE 181=181`

#### 单表查询

- 遍历到第`t`个记录,发现改记录符合`WHERE`表示的过滤条件.将该记录加入到待呈现记录集合
- 遍历完整张数据表,得到完整的待呈现数据表
- 将待呈现数据表按照`ORDER BY`子句进行排序
- 按照`LIMIT`子句提取待呈现数据表中的指定个记录组成预备数据表
- 将我们`SELECT`中指定的要呈现的字段从排序好,并提取好的预备数据表提取出来
- 按顺序将我们上一步提取得到的数据呈现出来

#### 多表查询

> 多表查询涉及到多个表的记录的遍历问题.实际上我们可以理解为一个二重`for`循环,第一个循环的循环元素为第一个表的记录,第二个循环的循环元素为第二个表的记录

```python
虚拟数据表=[]
for 记录1 in Table1:
	for 记录2 in Table2:
		合并记录 = [记录1,记录2]
        if 合并记录 符合 WHERE过滤条件:
            虚拟数据表.append(合并记录)

SELECT 字段1,字段2... FROM 虚拟数据表
		
```



****

## 单行函数与聚合函数的区别

> - **单行函数**:即一次接收数据表的**一个**记录,字段,常量等,并返回一个结果
> - **聚合函数**:即一次可以接收数据表的**多个**记录,字段,常量等,返回一个结果

## **单行函数**

不同`DBMS`的函数的差异

### 数值函数

### 字符串函数

### 日期与时间函数

### 流程控制函数

### 加密与解密函数

### `MySQL`信息函数

### 其他函数

****

## **聚合函数**

****

## 子查询(查询进阶)

- `SELECT city FROM department WHERE(departmentid = (SELECT departmentid FROM employees WHERE last_name='Abel'))`

****

## **`MySQL`增删改**

### **增**

### **删**

### **改**