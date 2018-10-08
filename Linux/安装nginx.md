### 安装nginx

#### 编译安装

1. ​        ./configure --prefix='要编译的地址'
2. ​       缺少库  yum install (devel库的意义)
3.   make && make install
4.  在  /usr/local/nginx目录下     
   1. config 配置文件
   2. html 网页文件
   3. logs 日志文件
   4. sbin 进程命令

5.  启动   ./sbin/nginx
6. 查看运行的端口号：   netstat -ant
7. 查看当前的进程： ps aux|grep 80
8. 杀掉进程   kill -9 主进程pid(pid)
9. 强制杀掉进程: pkill -9 http

    ```shell
yum install pcre pcre-devel
cd /usr/local/src
wget http://nginx.org/download/nginx-1.4.2.tar.gz
tar zxvf nginx-1.4.2.tar.gz
cd nginx-1.4.2
./configure --prefix=/usr/local/nginx
make && make install

启动：
cd /usr/local/nginx
    ```



#### nginx 信号量

1. wiki.niginx.org/CommandLine 命令介绍

   ​	kill + 信号(signal）+ 主进程(pid）

   ​	master process：主进程

   ​	work process: 工作进程

   ​	kill -INT/QUIT/HUP (主进程号)

   ​	ps aux|grep nginx：查找ngingx进程

   ​	tail -10  文件名：查看文件的末尾10行

#### nginx 虚拟主机配置 

 1. ​	server {

    ​		信息

    ​	}

 #### location命中过程

 	1.    = 精准命中
 	2.    ~ 正则命中
 	3.    普通命中

   命中过程：

 	1.   先判断精准命中，如果命中 ，立即返回结果并 结束解析的过程
 	2.   判断普通命中，如果有多个命中，记录下来  最长  的命中结果（注：记录但不结束，最长的为准）
 	3.   继续判断正则表达式的解析结果，按配置的正则表达式顺序为准，由上到下开始匹配，一旦匹配成功一个，立即返回结果，并结束解析过程

   延伸分析： 

​	a. 普通命中，顺序无所谓，是因为按命中的 长短来确定的

​	b. 正则命中，顺序有所谓，因为是从前往后命中的

​		 

