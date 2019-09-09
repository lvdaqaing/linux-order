## 简介
```
crontab 是一个用于设置周期性被执行的任务工具。
周期性执行的任务列表称为Cron Table
```

## crontab(选项)(参数)
```
-e：编辑该用户的计时器设置；
-l：列出该用户的计时器设置；
-r：删除该用户的计时器设置；
-u<用户名称>：指定要设定计时器的用户名称。
```

## 检查Crontab 服务
```
service crond status
```

## 列出服务项需要执行的操作
```
[root@xuexi-001 ~]# service crond
The service command supports only basic LSB actions 
(start, stop, restart, try-restart, reload, force-reload, status). 
For other actions, please try to use systemctl.
```

## 安装crontab
```
-yum install -y vixie-cron
-yum install -y crontabs
```

## crontab 的配置文件格式
```
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ... mon月 1-12
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat 星期 1-6 0表示星期天
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed
```

```
星号         表示任意值，比如在小时部分填写*代表任意小时（每小时）
 
逗号         可以允许在一个部分中填写多个值，比如在分钟部分填写1,3表示一分钟或三分钟
 
斜线         一般配合 *使用，代表每隔多长时间，比如在小时部分填写*/2代表每隔两分钟。所以 */1和 *没有区别
```

## 案例
```
每晚的21:30重启apache
30 21 * * * service httpd restart

每月1、10、22日的4:45重启apache
45 4 1,10,22 * * service httpd restart

每月1到10日的4:45重启apache
45 4 1-10 * * service httpd restart

每隔两分钟重启apache
*/2 * * * * service httpd restart
1-59/2 * * * * service httpd restart

晚上11点到早上7点之间，每隔一小时重启apache
0 23-7/1 * * * service httpd restart

每天18:00至23:00之间每隔30分钟重启apache
0，30 18-23 * * * service httpd restart
0-59 18-23 * * * service httpd restart

* * * * *      date >> /home/postgres/time.log           # 每隔一分钟执行一次任务 
0 * * * *      date >> /home/postgres/time.log         # 每小时的0点执行一次任务，比如6:00，10:00 
6,10 * 2 * *    date >>/home/postgres/time.log      # 每个月2号，每小时的6分和10分执行一次任务 
*/3,*/5 * * * *  date >> /home/postgres/time.log       # 每隔3分钟或5分钟执行一次任务，比如10:03，10:05，10:06

#每晚的21:30重启apache。
30 21 * * * /usr/local/etc/rc.d/lighttpd restart

#每月1、10、22日
4 5 4 1,10,22 * * /usr/local/etc/rc.d/lighttpd restart

#每天早上6点10分
10 6 * * * date

#每两个小时
0 */2 * * * date

#晚上11点到早上8点之间每两个小时，早上8点
023-7/2，8 * * * date

#每个月的4号和每个礼拜的礼拜一到礼拜三的早上11点
0 11 4 * mon-wed date

#1月份日早上4点
0 4 1 jan * date


案例： crontab 中最小只能设置到每分钟执行一个命令，如果想每半分钟执行某个命令怎么做到呢？

通过shell脚本的sleep 命令配合crontab 即可完成这一功能
date && sleep 0.5s && date
[root@xuexi-001 ~]# crontab -e
*/1 * * * * date >> date.log
*/1 * * * * sleep 30s; date >> date.log
[root@xuexi-001 ~]# cat  date.log 
2018年 06月 18日 星期一 00:34:01 CST
2018年 06月 18日 星期一 00:34:32 CST
2018年 06月 18日 星期一 00:35:01 CST
2018年 06月 18日 星期一 00:35:31 CST
```

## 小结：
```
表示任何时候都匹配
用”A,B,C“表示A或者B或者C时执行命令
“A-B”表示A-B之间时执行命令
“*/A”表示每A分钟（小时等）执行一次命令

crontab 配置文件 /etc/crontab
crontab 默认的保存的日志文件 /var/log/cron
```