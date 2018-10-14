### 负载均衡配置 ssl

---

* 登录负载均衡服务器

* freenom网站购买了域名： tech-live.tk

* 登录 https://letsencrypt.org

* 脚本命令文件地址 ：jelloybool/laravel-server-script/nginx-ssl.sh

  ```
  sudo apt-get update
  sudo LC_ALL=en_US.UTF-8 add-apt-repository ppa:certbot/certbot -y
  sudo apt-get install python-certbot-nginx -y
  sudo certbot --nginx -d tech-live.tk -d www.tech-live.tk  # 绑定域名
  ```
