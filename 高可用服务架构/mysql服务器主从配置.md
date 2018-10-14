### mysql服务器主从配置

---

* 创建从属数据库服务器app-slave  (2核4gb)

----

* 登录主数据库服务器 app-db

* cd /etc/mysql

* cd mysql.conf.d

* vim mysql.cnf

* /server-id ：搜索该字符串

* 修改以下配置内容

  ```
  server-id            =  1
  log_bin				=  /var/log/mysql/mysql-bin.log
  relay_log			=  /var/log/mysql/mysql-relay-bin.log
  
  ```

* :wq保存。重启mysql   service mysql restart
* mysql -u root -p ：登录mysql,创建从属服务器登录用户

*  create user 'slave_user'@'从属服务器内网ip' identified by '登录密码';
* 分配访问权限

```
grant replication slave on *.* to 'slave_uer从属服务器用户名'@'从属数据服务器内网ip' identified by '访问密码';
flush privileges;
show master status;  # 获得file 信息
```

----

#### 登录app-slave 服务器

* 设置主数据库服务器app-db防火墙访问权限

  ```
  app-mysql规则中
  mysql     加入app-slave(从属数据库服务器)
  ```


* apt update
* apt install mysql-server-5.7 -y
* cd /etc/mysql/mysql.conf.d
* vim mysqld.cnf
* /bind-address  ：搜索注释掉此行数据
* /server-id：搜索此行数据

```
server-id         	 = 2
log_bin				= /var/log/mysql/mysql-bin.log
relay_log			= /var/log/mysql/mysql-relay-bin.log
```

* :wq

* service mysql reload

* mysql -u root -p: 登录mysql

* create database deploylaravel charset utf8mb4;

* 导入主数据库服务器mysql数据

* stop slave;

* 从属数据库连接：

  ```
  CHANGE MASTER TO MASTER_HOST = '主数据库服务器内网地址',MASTER_USER = '主数据服务器设置的从属数据库访问用户名', MASTER_PASSWORD = '主数据服务器设置的从属数据库访问用户密码',MASTER_LOG_FILE = 'mysql-bin.000002',MASTER_LOG_POS = 783;
  
  CHANGE MASTER TO MASTER_HOST = '10.138.40.55',MASTER_USER = 'slave_user', MASTER_PASSWORD = 'My2000163',MASTER_LOG_FILE = 'mysql-bin.000001',MASTER_LOG_POS = 872;
  
  注： 此处MASTER_LOG_FILE、MASTER_LOG_POS 所取值是主数据库服务器执行 show master status;命令获得。
  ```


* start slave;
* SHOW SLAVE STATUS \G
* quit;