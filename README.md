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