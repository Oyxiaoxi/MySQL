# 纪录一下学习过程

#### MySQL是一个关系型数据库管理系统，由瑞典MySQL AB 公司开发，目前属于 Oracle 旗下产品。MySQL 是最流行的关系型数据库管理系统之一，在 WEB 应用方面，MySQL是最好的 RDBMS (Relational Database Management System，关系数据库管理系统) 应用软件。

### MySQL服务
```bash
mysql.server start
mysql.server restart
mysql.server stop
```

### 登陆数据库
```bash
mysql -u root -p
```

### 修改提示符：prompt \u@\h \d>   当前用户名\本地用户\ 当前使用表名。
```mysql
prompt \u@\h \d>
```

### 显示当前用户
```mysql
select user();
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

## 关键字与函数名称全部大写,数据库名称、表名称、字段名称全部小写
### 创建数据库 
```mysql
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

### 创建表
```mysql
CREATA TABLE [IF NOT EXISTS] table_name{
  column_name data_type,
  ....
};

CREATE TABLE userInfo(
  username VARCHAR(20),
  age TINYINT UNSIGNED,
  salary FLOAT(8,2) UNSIGNED
);
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
#### 1.自动编号，且必须与主键组合使用,默认情况下
#### 2.起始值为1，每次的增量为1
```mysql
CREATE TABLE userTableTwo(
  id SMALLINT UNSIGNED AUTO_INCREMENT,
  username VARCHAR(20) NOT NULL
);

ERROR:Incorrect table definition; there can be only one auto column and it must be defined as a key
```

### PRIMARY KEY 主键约束
#### 1.主键约束
#### 2.每张数据表只能存在一个主键
#### 3.主键保证记录的唯一性
#### 4.主键自动NOT NULL
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
#### 1.唯一约束
#### 2.唯一约束可以保证纪录的唯一性
#### 3.唯一约束的字段可以为空值（null）
#### 4.每张数据表可以存在多个唯一约束
```mysql
CREATE TABLE userTableFour(
  id SMALLINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  username VARCHAR(20) NOT NULL UNIQUE KEY,                                 
  age TINYINT unsigned                                                     
);
```

### 默认约束
#### 1.默认值
#### 2.当插入纪录时，如有没有明确为字段赋值，刚自动赋予默认值。
```mysql
create table userTablefive(
  id smallint unsigned auto_increment primary key,
  username varchar(20) not null unique key,
  sex enum('1','2','3') default '3'
);
```

### 约束
#### 1.约束保证数据的完整性和一致性。
#### 2.约束分为表级约束和列级约束。
#### 3.约束类型包括：
####   NOT NULL 非空约束
####   PRIMART KEY 主键约束
####   UNIQUE KEY 唯一约束
####   DEFAULT 默认约束
####   FOREIGN KEY 外键约束

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
#### 1.CASCADE:从父表删除或更新且自动删除或更新子表中匹配的行
#### 2.SET NULL:从父表删除或更新行，并设置子表中的外键列为NUll。如果使用该选项，必须保证子表列没有指定NOT NULL 
#### 3.RESTRICT:拒绝对父表的删除或更新操作。
#### 4.NOT ACTION:标准SQL的关键字，在MYSQL中与RESTRICT相同

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
#### 1.对一个数据列建立的约束，叫列级约束
#### 2.对多偿数据列建立的约束，叫表级约束
#### 3.列级约束即可以在列定义时声明，也可以在列定义后声明。
#### 4.表约约束只能在列定义后声明。