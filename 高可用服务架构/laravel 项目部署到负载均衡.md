### laravel 项目部署到负载均衡

1. 本地创建laravel项目：composer create-project --prefer-dist laravel/laravel deploylaravel

2. github 创建deploylaravel仓库

3.  进入本地项目文件：

   * git init

   * git add .

   * git commit -m 

   * ```
     git remote add origin https://github.com/haywair/deploylaravel.git
     ```

* ```git
  git push -u origin master
  ```

4. 将项目部署到应用app1 、app2

   * 使用ssh 方式克隆，需要生成key

   * 分别在app1、app2上生成ssh  key。命令：ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

   * 分别查看key值，在github上分别添加ssh key。存储位置：/root/.ssh/id_rsa.pub--->查看key命令：cat /root/.ssh/id_rsa.pub

   * 进入app1、app2网站目录，进行clone（目录地址：/var/www/）

     ```
     git clone git@github.com:haywair/deploylaravel.git
     ```

   * 进入app1、app2中网站项目目录deploylaravel中，执行命令

     ```
     composer install --no-dev
     ```

   * app1、app2中分别执行 

   ```
   cp .env.example .env
   ```

   注意：两个应用 .env 中app_key必须保持一致

   * 分别进行app1、app2中 /etc/nginx/sites-available/default  进行nginx配置。配置文件地址：jellybool/laravel-server-script/nginx-laravel.conf

   ```
   注意修改网站路径(必须定位到public目录下)
   nginx -t 
   sudo service nginx reload
   ```

   * 修改app1 app2应用拥有者权限

   ```
   sudo chown -R www-data:www-data /var/www/deploylaravel
   ```
