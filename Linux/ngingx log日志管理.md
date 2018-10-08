### ngingx log日志管理

#### ngingx server段 如下信息

 	1. #access_log logs/host.access.log main;
 	2. 释义：这说明 该server 它的访问日志的文件是 logs/host.access.log ,使用的格式是 main 格式。处了main 格式，可以自定义其他格式

#### main格式

```shell
#log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

```

​	main 格式是我们定义好的 一种格式，并起个名字，便于引用。以上面的例子，main类型的日					志，记录的 remote_addr......http_x_forwarded_for等选项

 	1. $remote_addr      客户端 ip
 	2. $remote_user   客户端用户名称 
 	3. $request     请求的URI和HTTP协议
 	4. $status   http请求状态
 	5. $body_bytes_sent   发送给客户端文件内容大小
 	6. $http_referer   url跳转来源
 	7. $http_user_agent    用户终端浏览器信息 
 	8. $http_x_forwarded_for 客户使用代理服务器时，获得代理服务器代理访问的真实 ip地址

#### 配置日志

```shell
log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
// 该 代码块前的注释符 '#' 需要去掉
```



```shell
server {
        listen 8080;
        server_name 123.56.218.60;

        location / {
          root  html/z.com;
          index index.html;
        }
        access_log logs/z.com.access.log main;   // 此处为配置日志路径及格式
    }

```



















