### nginx 定时 任务与日志切割

#### date命令

 1. 昨天的命令

    ​	date -d yesterday

 2. date -s 设置日期时间

    date -s  '2013-09-21 19:00:38'

 3. date -d yesterday +%y   年

 4. date -d yesterday +%m 月

 5. date -d yesterday +%d 日

 6. date -d yesterday +%Y%m%d

#### shell脚本

   	1. 文件命名   xxxx.sh
      	2. 文件执行   sh    xi
         	3. echo 打印
       ​	1.  echo  执行的命令                  执行的命令前后加  `  反引号
       ​	2. echo   $(执行的命令)             将执行的命令包裹在  $() 的 括号中	
 	4. 变量声明

```
# 声明  BASEPATH  变量
BASEPATH=/usr/local/nginx/data
# 声明 bak 变量
bak=$BASEPATH/$(date -d yesterday +%Y%m)/$(date -d yesterday +%H%M).zcom.access.log
# 变量引用 并执行剪切移动命令
mv $LOGPATH $bak
```



#### 定时任务

* crontab -e  修改crontab定时任务文件，如果文件不存在自动创建

```shell
*/1 * * * * sh /usr/local/nginx/data/runlog.sh     // 每分钟定时执行/usr/local/nginx/data/runlog.sh 脚本
```

* crontab  -l  显示crontab 文件
* crontab -r  删除crontab 文件
* crontab -ir：删除crontab文件前提示用户
* crontab 文件格式：

```
以下是 crontab 文件的格式：

{minute} {hour} {day-of-month} {month} {day-of-week} {full-path-to-shell-script} 
* minute: 区间为 0 – 59 
* hour: 区间为0 – 23 
* day-of-month: 区间为0 – 31 
* month: 区间为1 – 12. 1 是1月. 12是12月. 
* Day-of-week: 区间为0 – 7. 周日可以是0或7.

例：
1. 每个工作日(Mon – Fri) 11:59 p.m 都进行备份作业。

59 11 * * 1,2,3,4,5 /root/bin/backup.sh
2. 在 12:01 a.m 运行，即每天凌晨过一分钟。这是一个恰当的进行备份的时间，因为此时系统负载不大。

1 0 * * * /root/bin/backup.sh
```





