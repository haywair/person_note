### mysql 命令

----

* truncate table 表名   ：清除表及id的自增 

* 命令行方法

```
方法位置 ：

application/member/service/DataMigrate.php

命令行：

application/command/Customer.php

配置文件：

application/command.php

根目录下 执行命令 ：

php think customer
```



*  mysql 数据库导出

```
/usr/local/mysql/bin/mysqldump -uroot -p dmp shop_notice > /root/databasefiles/shop_notice.sql
```



```
mysql 1449:the user specified as a definer ('root'@'%') does not exist


权限问题，授权给 root 所有sql 权限
grant all privileges on *.* to root@'%' identified by ".";

flush privileges;
```

