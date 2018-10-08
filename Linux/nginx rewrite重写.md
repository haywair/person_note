### nginx rewrite重写

#### 常用命令：

1. if 语法格式：
   * 判断规则    作用于 ：server，location

```
= ,!= 变量比较
~ 区分大小写字母匹配
~* 不区分大小写字母匹配
!~ 区分大小写字母不匹配
!~* 不区分大小写字母不匹配
-f,!-f 检查一个文件是否存在
-d,!-d 检查一个目录是否存在
-e ,!-e 检查一个文件、目录、符号链接是否存在
x ,!-x 检查一个文件是否可执行
```

​	If 空格 (条件) {

​		重写模式

​	}

* 代码块

```
if ($remote_addr = 222.173.29.50) {
    return 403;
}

if ($http_user_agent ~ Edge) {
    return ^.*$ /ie.html;
    break;   # 如不加break,会死循环，无限重定向
}
```

2. set 变量

   ```
   
              if ($http_user_agent ~* Edge) {
                   set $isedge 1;    // 设置变量值
              }
              if ($fastcgi_script_name = error_now.html) {
                   set $isedge 0;
              }
              if ($isedge = 1) {   // if判断
                   rewrite ^.*$ /error_now.html;
                   break;   // break跳出循环定向
              }
   
   ```

   3. break  跳出 rewrite
   4. return 返回状态码
   5. rewrite 重写规则
      * rewrite常用全局 变量

   ```
   $args：变量中存放了URL中的指令。
   $content_length:保存了请求报文头部中的content-lenght字段。
   $content_type:保存了请求头部中的content-type字段。
   $document_root:保存了针对当前资源的请求的系统根目录。
   $document_uri：保存了当前请求中不包含指令的URI，主注意是不包含请求的指令。
   $host:存放了请求的服务器名称。
   $http_user_agent：客户端浏览器的详细信息。
   $http_cookie:客户端的cookie信息。
   $limit_rate：如果nginx服务器使用limit_rate配置了显示网络速率，则会显示，如果没有设置， 则显示0。
   $remote_addr:存放了客户端的地址，注意是客户端的公网IP，也就是一家人访问一个网站，则会显示为路由器的公网IP。
   $remote_port:客户端请求Nginx服务器时随机打开的端口，这是每个客户端自己的端口。
   $remote_user:已经经过Auth Basic Module验证的用户名。
   $request_body_file:做反向代理时发给后端服务器的本地资源的名称。
   $request_method:请求资源的方式，GET/PUT/DELETE等。
   $request_filename:当前请求的资源文件的路径名称，由root或alias指令与URI请求生成。
   $request_uri：包含请求参数的原始URI，不包含主机名。
   $squery_string:保存了URL请求的指令，与 $args相同。
   $scheme:请求的协议，如ftp，https，http等。
   $server_protocpl：保存了客户端请求资源使用的协议的版本，如HTTP/1.0，HTTP/1.1，HTTP/2.0等。
   $server_addr:保存了服务器的IP地址。
   $server_name:服务器的主机名。
   $server_port:服务器的端口号。
   $uri：与$document_uri相同,是一个不包含指令的uri地址。
   
   ```
