### mysql服务器配置

----

* 创建ubuntu服务器空间,并执行 apt update
* apt install mysql-server-5.7 -y ：安装mysql
* cd /etc/mysql/mysql.conf.d
* vim mysqld.cnf
* /bind-address： 搜索bind-address位置，将app-db的内网ip地址绑定 (如slave 服务器无法成功调用，此处配置注释)
* service mysql restart：重启mysql
* 设置root用户密码

```
登录mysql    mysql -u root -p 或 mysql 
use mysql; 
update user set authentication_string=PASSWORD("密码") where user='root'; 
update user set plugin="mysql_native_password"; 
flush privileges; 
quit;
重启mysql
```



* mysql -u root -p：登录

* create database deploylaravel charset utf8mb4 :创建数据库

* f分别为app1、app2访问设置密码

  ```
  create user demploylaravel@'app1、app2的内网ip地址' identified by '要设置的密码'
  ```

*  flush privileges ：执行命令

* 分别为app1、app2设置访问权限

```
grant all privileges on deploylaravel.* to deploylaravel@'app1、app2的内网地址' identified by '访问密码';
```

* 分别登录app1、app2，修改env文件，修改DB设置

```
DB_CONNECTION=mysql
DB_HOST= app-db数据库服务器内网地址
DB_PORT=3306
DB_DATABASE=数据库名称
DB_USERNAME=登录名
DB_PASSWORD=登录密码
```

* 设置数据库服务器防火墙

  ssh 所有网络地址

  mysql 请求来源app1、app2

  应用至 app-db	服务器