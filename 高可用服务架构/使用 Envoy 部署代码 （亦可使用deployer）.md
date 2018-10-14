### 使用 Envoy 部署代码 （亦可使用deployer）

---

* app1、app2、本地安装envoy

  ```
  composer global require laravel/envoy
  ```

  注意：~/.composer/vendor/bin必须加入环境变量PATH中

  ```
  如～目录下没有 .compoer 文件夹，卸载composer,并执行 sudo apt install composer
  1) vim /etc/environment
  在PATH="/usr/local/sbin:/usr/sbin:/usr/bin:/sbin:/bin"中加入 ~/.composer/vendor/bin
  2)shutdown -r now 立刻重启生效
  ```


* 在项目根目录创建 Envoy.blade.php 文件, 并写入以下内容

```
@servers(['web-1' => 'root@192.168.1.1', 'web-2' => 'root@192.168.1.2'])   # root@后填写对应的应用地址

@task('deploy', ['on' => ['web-1', 'web-2'], 'parallel' => true])
    cd /var/www.deploylaravel  # 对应的网站根目录
    git pull origin master    #  对应分支
    composer install --no-dev
    php artisan migrate --force
@endtask
```

* 修改项目入口文件测试

* git add .

  git commit -m ''

  git push

* 执行 envoy run deploy