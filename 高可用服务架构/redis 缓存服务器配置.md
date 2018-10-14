### redis 缓存服务器配置

----

* 创建redis 服务器，并登录
* apt update
* apt install redis-server
* cd /etc/redis
* vim redis.conf
* /bind   设置并添加内网地址（127.0.0.1不要删除，中间用空格分开）
* service redis-server restart
* 分别在本地、app1、app2中安装  predis类包

```
composer require predis/predis
```



* 分别修改app1、app2的 .env设置

```
BROADCAST_DRIVER=log
CACH_DRIVER=redis
SESSION_DRIVER=redis

REDIS_HOST= app-redis的内网地址（redis服务器的内网地址）

```

* 创建防火墙

​     ssh  

​     custom    端口号：6379   作用域app1、app2

​     规则应用于app-redis（redis服务器）