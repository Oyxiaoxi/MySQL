# 纪录一下学习过程

#### MySQL是一个关系型数据库管理系统，由瑞典MySQL AB 公司开发，目前属于 Oracle 旗下产品。MySQL 是最流行的关系型数据库管理系统之一，在 WEB 应用方面，MySQL是最好的 RDBMS (Relational Database Management System，关系数据库管理系统) 应用软件。


###  MyCli 是一个 MySQL 命令行工具，支持自动补全和语法高亮。也可用于 MariaDB 和 Percona。
```bash
brew update && brew install mycli
sudo mycli //输入管理员密码即可登陆 
```

### MySQL服务
```bash
mysql.server start
mysql.server restart
mysql.server stop
```

### mysql 数据类型
#### 1、整型
<table>
    <tr>
        <td>MySQL数据类型</td>
        <td>含义（有符号）</td>
    </tr>
    <tr>
        <td>tinyint(m)</td>
        <td>1个字节  范围(-128~127)</td>
    </tr>
    <tr>
        <td>smallint(m)</td>
        <td>2个字节  范围(-32768~32767)</td>
    </tr>
    <tr>
        <td>mediumint(m)</td>
        <td>3个字节  范围(-8388608~8388607)</td>
    </tr>
    <tr>
        <td>int(m)</td>
        <td>4个字节  范围(-2147483648~2147483647)</td>
    </tr>
    <tr>
        <td>bigint(m)</td>
        <td>8个字节  范围(+-9.22*10的18次方)</td>
    </tr>
</table>

> 取值范围如果加了unsigned，则最大值翻倍，如tinyint unsigned的取值范围为(0~256)。 int(m)里的m是表示SELECT查询结果集中的显示宽度，并不影响实际的取值范围，没有影响到显示的宽度，不知道这个m有什么用。

#### 2、浮点型(float和double)
<table>
    <tr>
        <td>MySQL数据类型</td>
        <td>含义</td>
    </tr>
    <tr>
        <td>float(m,d)</td>
        <td>单精度浮点型    8位精度(4字节)     m总个数，d小数位</td>
    </tr>
    <tr>
        <td>double(m,d)</td>
        <td>双精度浮点型    16位精度(8字节)    m总个数，d小数位</td>
    </tr>
</table>

> 设一个字段定义为float(5,3)，如果插入一个数123.45678,实际数据库里存的是123.457，但总个数还以实际为准，即6位。

#### 3、定点数
> 浮点型在数据库中存放的是近似值，而定点类型在数据库中存放的是精确值。 decimal(m,d) 参数m<65 是总个数，d<30且 d<m 是小数位。

#### 4、字符串(char,varchar,_text)
<table>
    <tr>
        <td>MySQL数据类型</td>
        <td>含义</td>
    </tr>
    <tr>
        <td>char(n)</td>
        <td>固定长度，最多255个字符</td>
    </tr>
    <tr>
        <td>varchar(n)</td>
        <td>固定长度，最多65535个字符</td>
    </tr>
    <tr>
        <td>tinytext</td>
        <td>可变长度，最多255个字符</td>
    </tr>
    <tr>
        <td>text</td>
        <td>可变长度，最多65535个字符</td>
    </tr>
    <tr>
        <td>mediumtext</td>
        <td>可变长度，最多2的24次方-1个字符</td>
    </tr>
    <tr>
        <td>longtext</td>
        <td>可变长度，最多2的32次方-1个字符</td>
    </tr>
</table>

### char和varchar：
+ 1.char(n) 若存入字符数小于n，则以空格补于其后，查询之时再将空格去掉。所以char类型存储的字符串末尾不能有空格，varchar不限于此。 
+ 2.char(n) 固定长度，char(4)不管是存入几个字符，都将占用4个字节，varchar是存入的实际字符数+1个字节（n<=255）或2个字节(n>255)，所以varchar(4),存入3个字符将占用4个字节。 
+ 3.char类型的字符串检索速度要比varchar类型的快。

### varchar和text： 
+ 1.varchar可指定n，text不能指定，内部存储varchar是存入的实际字符数+1个字节（n<=255）或2个字节(n>255)，text是实际字符数+2个字节。 
+ 2.text类型不能有默认值。 
+ 3.varchar可直接创建索引，text创建索引要指定前多少个字符。varchar查询速度快于text,在都创建索引的情况下，text的索引似乎不起作用。


### 5.二进制数据(_Blob)

+ 1._BLOB和_text存储方式不同，_TEXT以文本方式存储，英文存储区分大小写，而_Blob是以二进制方式存储，不分大小写。 
+ 2._BLOB存储的数据只能整体读出。 
+ 3._TEXT可以指定字符集，_BLO不用指定字符集。

### 6.日期时间类型
<table>
    <tr>
        <td>MySQL数据类型</td>
        <td>含义</td>
    </tr>
    <tr>
        <td>date</td>
        <td>日期 '2008-12-2'</td>
    </tr>
    <tr>
        <td>time</td>
        <td>时间 '12:25:36'</td>
    </tr>
    <tr>
        <td>datetime</td>
        <td>日期时间 '2008-12-2 22:06:44'</td>
    </tr>
    <tr>
        <td>timestamp</td>
        <td>自动存储记录修改时间</td>
    </tr>
</table>

> 若定义一个字段为timestamp，这个字段里的时间数据会随其他字段修改的时候自动刷新，所以这个数据类型的字段可以存放这条记录最后被修改的时间。

### 数据类型的属性
<table>
    <tr>
        <td>MySQL数据类型</td>
        <td>含义</td>
    </tr>
    <tr>
        <td>NULL</td>
        <td>数据列可包含NULL值</td>
    </tr>
    <tr>
        <td>NOT NULL</td>
        <td>数据列不允许包含NULL值</td>
    </tr>
    <tr>
        <td>DEFAULT</td>
        <td>默认值</td>
    </tr>
    <tr>
        <td>PRIMARY KEY</td>
        <td>主键</td>
    </tr>
    <tr>
        <td>AUTO_INCREMENT</td>
        <td>自动递增，适用于整数类型</td>
    </tr>
    <tr>
        <td>UNSIGNED</td>
        <td>无符号</td>
    </tr>
    <tr>
        <td>CHARACTER SET name</td>
        <td>指定一个字符集</td>
    </tr>
</table>

## 关键字与函数名称全部大写,数据库名称、表名称、字段名称全部小写

### 登陆数据库
```bash
prompt \u@\h \d> # 修改提示符：prompt \u@\h \d>   当前用户名\本地用户\ 当前使用表名。
mysql -h 127.0.0.1 -u 用户名 -p
mysql -D 所选择的数据库名 -h 主机名 -u 用户名 -p
mysql> mysql -u root -p
mysql> exit # 退出 使用 “quit;” 或 “\q;” 一样的效果
mysql> status;  # 显示当前mysql的version的各种信息
mysql> select version(); # 显示当前mysql的version信息
mysql> show global variables like 'port'; # 查看MySQL端口号
```

### 创建数据库 
```mysql
-- 创建一个名为 samp_db 的数据库，数据库字符编码指定为 gbk
create database samp_db character set gbk;
drop database samp_db; -- 删除 库名为samp_db的库
show databases;        -- 显示数据库列表。
use samp_db;     -- 选择创建的数据库samp_db
show tables;     -- 显示samp_db下面所有的表名字
describe 表名;    -- 显示数据表的结构
delete from 表名; -- 清空表中记录

CREATE DATABASE tables;
```

### 显示数据库编码
```mysql
SHOW CREATE DATABASE tables;
```

### 删除数据库
```mysql
DROP DATABASE tables;
```

### 创建数据库表
```mysql
使用 create table 语句可完成对表的创建, create table 的常见形式:
语法：create table 表名称(列声明);
CREATA TABLE [IF NOT EXISTS] table_name{
  column_name data_type,
  ....
};

CREATE TABLE userInfo(
  username VARCHAR(20),
  age TINYINT UNSIGNED,
  salary FLOAT(8,2) UNSIGNED
);

CREATE TABLE `user_accounts` (
  `id`             int(100) unsigned NOT NULL AUTO_INCREMENT primary key,
  `password`       varchar(32)       NOT NULL DEFAULT '' COMMENT '用户密码',
  `reset_password` tinyint(32)       NOT NULL DEFAULT 0 COMMENT '用户类型：0－不需要重置密码；1-需要重置密码',
  `mobile`         varchar(20)       NOT NULL DEFAULT '' COMMENT '手机',
  `create_at`      timestamp(6)      NOT NULL DEFAULT CURRENT_TIMESTAMP(6),
  `update_at`      timestamp(6)      NOT NULL DEFAULT CURRENT_TIMESTAMP(6) ON UPDATE CURRENT_TIMESTAMP(6),
  -- 创建唯一索引，不允许重复
  UNIQUE INDEX idx_user_mobile(`mobile`)
)
ENGINE=InnoDB DEFAULT CHARSET=utf8
COMMENT='用户表信息';
```

### 查看表结构信息
```mysql
desc table_name;
show columns from table_name;
describe table_name;
```

### 查看数据库列表
```mysql
show tables [from db_name] [like 'pattern' | where expr];
```

### 插入纪录
```mysql
insert into tab_name [(col_name,...)] values(val,...)
INSERT userInfo VALUES('Oyxiaoxi',23,8325.52); //所有的纪录都要赋值
INSERT userInfo(username,salray) VALUES('amMu',7869.23); //指定纪录赋值
```

### 查看纪录
```mysql
SELECT * FROM userInfo; // * 表示字段过滤
```

## 数据库空值与非空值 null ，字段值可以为空 not null ,字段值禁止为空
```mysql
CREATE TABLE userTable(
  username VARCHAR(20) NOT NULL, // username 的纪录值不能为空
  age TINYINT UNSIGNED NULL
);
```

### 纪录值唯一 AUTO_INCREMENT,
+ 1.自动编号，且必须与主键组合使用,默认情况下
+ 2.起始值为1，每次的增量为1
```mysql
CREATE TABLE userTableTwo(
  id SMALLINT UNSIGNED AUTO_INCREMENT,
  username VARCHAR(20) NOT NULL
);

ERROR:Incorrect table definition; there can be only one auto column and it must be defined as a key
```

### PRIMARY KEY 主键约束
+ 1.主键约束
+ 2.每张数据表只能存在一个主键
+ 3.主键保证记录的唯一性
+ 4.主键自动NOT NULL
```mysql
CREATE TABLE userTableTwo(
  id SMALLINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  username VARCHAR(20) NOT NULL
);
```

### 主键可以赋值
```mysql
CREATE TABLE userTableThree(
  id SMALLINT UNSIGNED PRIMARY KEY,
  username VARCHAR(20) NOT NULL
);
```
### UNIQUE KEY 主键唯一约束
+ 1.唯一约束
+ 2.唯一约束可以保证纪录的唯一性
+ 3.唯一约束的字段可以为空值（null）
+ 4.每张数据表可以存在多个唯一约束
```mysql
CREATE TABLE userTableFour(
  id SMALLINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  username VARCHAR(20) NOT NULL UNIQUE KEY,                                 
  age TINYINT unsigned                                                     
);
```

### 默认约束
+ 1.默认值
+ 2.当插入纪录时，如有没有明确为字段赋值，刚自动赋予默认值。
```mysql
create table userTablefive(
  id smallint unsigned auto_increment primary key,
  username varchar(20) not null unique key,
  sex enum('1','2','3') default '3'
);
```

### 约束
+ 1.约束保证数据的完整性和一致性。
+ 2.约束分为表级约束和列级约束。
+ 3.约束类型包括：
>+   NOT NULL 非空约束
>+   PRIMART KEY 主键约束
>+   UNIQUE KEY 唯一约束
>+   DEFAULT 默认约束
>+   FOREIGN KEY 外键约束

### FOREIGN KEY 外键约束
```mysql

// 父表
create table provinces(
  id smallint unsigned primary key auto_increment,
  pname varchar(20) not null
);

// 子表
CREATE TABLE users(
  id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
  username VARCHAR(20) NOT NULL,
  pid SMALLINT UNSIGNED,
  FOREIGN KEY (pid) REFERENCES provinces (id)         
);

得注意子表pid与父表id的约束是否一致！
```

### 显示参照表 users.id 的索引
```mysql
show indexes from provinces;
show indexes from provinces\G;
```

### 外键约束参照操作
+ 1.CASCADE:从父表删除或更新且自动删除或更新子表中匹配的行
+ 2.SET NULL:从父表删除或更新行，并设置子表中的外键列为NUll。如果使用该选项，必须保证子表列没有指定NOT NULL 
+ 3.RESTRICT:拒绝对父表的删除或更新操作。
+ 4.NOT ACTION:标准SQL的关键字，在MYSQL中与RESTRICT相同

```mysql
CREATE TABLE users1( 
  id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
  username VARCHAR(20) NOT NULL,
  pid SMALLINT UNSIGNED,
  FOREIGN KEY (pid) REFERENCES provinces (id) ON DELETE CASCADE
);

delete from provinces where id = 3; // 删除父表中为3的纪录值会同时删除掉子表里为3的pid纪录值。
```

## 多选择逻辑引擎外键约束。不定义物理外键。

### 表级约束与列级约束
+ 1.对一个数据列建立的约束，叫列级约束
+ 2.对多偿数据列建立的约束，叫表级约束
+ 3.列级约束即可以在列定义时声明，也可以在列定义后声明。
+ 4.表约约束只能在列定义后声明。

## 增删改查
### SELECT
> SELECT 语句用于从表中选取数据。 
>+ 语法：SELECT 列名称 FROM 表名称 
>+ 语法：SELECT * FROM 表名称
```mysql
-- 表 station 取个别名叫s，表 station 中不包含 字段id=13或者14 的，并且id不等于4的 查询出来，只显示id
SELECT s.id from station s WHERE id in (13,14) and user_id not in (4);

-- 从表 Persons 选取 LastName 列的数据
SELECT LastName FROM Persons

-- 结果集中会自动去重复数据
SELECT DISTINCT Company FROM Orders 
-- 表 Persons 字段 Id_P 等于 Orders 字段 Id_P 的值，
-- 结果集显示 Persons表的 LastName、FirstName字段，Orders表的OrderNo字段
SELECT p.LastName, p.FirstName, o.OrderNo FROM Persons p, Orders o WHERE p.Id_P = o.Id_P 

-- gbk 和 utf8 中英文混合排序最简单的办法 
-- ci是 case insensitive, 即 “大小写不敏感”
SELECT tag, COUNT(tag) from news GROUP BY tag order by convert(tag using gbk) collate gbk_chinese_ci;
SELECT tag, COUNT(tag) from news GROUP BY tag order by convert(tag using utf8) collate utf8_unicode_ci;
```

### UPDATE
> Update 语句用于修改表中的数据。 
>+ 语法：UPDATE 表名称 SET 列名称 = 新值 WHERE 列名称 = 某值
```mysql
-- update语句设置字段值为另一个结果取出来的字段
update user set name = (select name from user1 where user1 .id = 1 )
where id = (select id from user2 where user2 .name='小苏');
-- 更新表 orders 中 id=1 的那一行数据更新它的 title 字段
UPDATE `orders` set title='这里是标题' WHERE id=1;
```

### INSERT
> INSERT INTO 语句用于向表格中插入新的行。
>+ 语法：INSERT INTO 表名称 VALUES (值1, 值2,....) 
>+ 语法：INSERT INTO 表名称 (列1, 列2,...) VALUES (值1, 值2,....) 

```mysql
-- 向表 Persons 插入一条字段 LastName = JSLite 字段 Address = shanghai
INSERT INTO Persons (LastName, Address) VALUES ('JSLite', 'shanghai');
-- 向表 meeting 插入 字段 a=1 和字段 b=2
INSERT INTO meeting SET a=1,b=2;
-- 
-- SQL实现将一个表的数据插入到另外一个表的代码
-- 如果只希望导入指定字段，可以用这种方法：
-- INSERT INTO 目标表 (字段1, 字段2, ...) SELECT 字段1, 字段2, ... FROM 来源表;
INSERT INTO orders (user_account_id, title) SELECT m.user_id, m.title FROM meeting m where m.id=1;
```

### DELETE
> DELETE 语句用于删除表中的行。
>+ 语法：DELETE FROM 表名称 WHERE 列名称 = 值
```mysql

-- 在不删除table_name表的情况下删除所有的行，清空表。
DELETE FROM table_name
-- 或者
DELETE * FROM table_name
-- 删除 Person表字段 LastName = 'JSLite' 
DELETE FROM Person WHERE LastName = 'JSLite' 
-- 删除 表meeting id 为2和3的两条数据
DELETE from meeting where id in (2,3);
```

### WHERE
> WHERE 子句用于规定选择的标准。 
>+ 语法：SELECT 列名称 FROM 表名称 WHERE 列 运算符 值
```mysql
-- 从表 Persons 中选出 Year 字段大于 1965 的数据
SELECT * FROM Persons WHERE Year>1965
```

### AND 和 OR
>+ AND - 如果第一个条件和第二个条件都成立； 
>+ OR - 如果第一个条件和第二个条件中只要有一个成立；

```mysql
-- 删除 meeting 表字段 
-- id=2 并且 user_id=5 的数据  和
-- id=3 并且 user_id=6 的数据 
DELETE from meeting where id in (2,3) and user_id in (5,6);

-- 使用 AND 来显示所有姓为 "Carter" 并且名为 "Thomas" 的人：
SELECT * FROM Persons WHERE FirstName='Thomas' AND LastName='Carter';
```

```mysql
-- 使用 OR 来显示所有姓为 "Carter" 或者名为 "Thomas" 的人：
SELECT * FROM Persons WHERE firstname='Thomas' OR lastname='Carter'
```

### ORDER BY
> 语句默认按照升序对记录进行排序。 
>+ ORDER BY - 语句用于根据指定的列对结果集进行排序。 
>+ DESC - 按照降序对记录进行排序。
>+ ASC - 按照顺序对记录进行排序。

```mysql
-- Company在表Orders中为字母，则会以字母顺序显示公司名称
SELECT Company, OrderNumber FROM Orders ORDER BY Company

-- 后面跟上 DESC 则为降序显示
SELECT Company, OrderNumber FROM Orders ORDER BY Company DESC

-- Company以降序显示公司名称，并OrderNumber以顺序显示
SELECT Company, OrderNumber FROM Orders ORDER BY Company DESC, OrderNumber ASC
```

### IN
> IN - 操作符允许我们在 WHERE 子句中规定多个值。 
> IN - 操作符用来指定范围，范围中的每一条，都进行匹配。IN取值规律，由逗号分割，全部放置括号中。 
>+ 语法：SELECT "字段名"FROM "表格名"WHERE "字段名" IN ('值一', '值二', ...);
```mysql
-- 从表 Persons 选取 字段 LastName 等于 Adams、Carter
SELECT * FROM Persons WHERE LastName IN ('Adams','Carter')
```

### NOT
> UNION - 操作符用于合并两个或多个 SELECT 语句的结果集。
```mysql
-- 列出所有在中国表（Employees_China）和美国（Employees_USA）的不同的雇员名
SELECT E_Name FROM Employees_China UNION SELECT E_Name FROM Employees_USA

-- 列出 meeting 表中的 pic_url，
-- station 表中的 number_station 别名设置成 pic_url 避免字段不一样报错
-- 按更新时间排序
SELECT id,pic_url FROM meeting UNION ALL SELECT id,number_station AS pic_url FROM station  ORDER BY update_at;
```

 


