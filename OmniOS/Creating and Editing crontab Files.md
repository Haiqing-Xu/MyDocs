# Creating and Editing crontab Files

##### 1、停止crontab服务

```bash
shell> svcadm disable cron
```

##### 2、启动crontab服务

```bash
shell> svcadm enable cron
```

##### 3、编辑crontab

有两种方法:

［第一种］
直接编辑 /var/spool/cron/crontabs/ 下对应用户的crontab文件，没有的就以用户名新建即可。

［第二种］
使用命令 crontab -e <用户名>

```bash
shell> crontab -e root
```

若要20分钟执行一次某个任务，可以这样写：

```
0,20,40 * * * * command
```

若要5分钟执行一次某个任务，可以这样写：

```
0,5,10,15,20,25,30,35,40,4,50,55 * * * * command
```

例如：每半小时自动校时

```
0,30 * * * * ntpdate 6.162.163.101
```

备注：solaris不支持linux下的crontab格式
*/5 表示每5分钟一次
*/20 表示每20分钟一次

##### 4、Solaris下的命令用法

查看root用户的crontab任务列表。

```
crontab -l root
```













