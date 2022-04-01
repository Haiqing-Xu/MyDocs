# __Debian Linux 快速安装教程__

#### 1. 启动 Debian 安装程序
选择Graphical install，回车



#### 2. Select a language (选择安装语言)
默认选择English，下面介绍都是以英语安装界面为主

```bash
$ ssh-copy-id username@remote_host
```
or

copy the path (ex: /home/user_name/.ssh/id_rsa.pub) and run the following command: 

```bash
$ ssh-copy-id -i /home/user_name/.ssh/id_rsa.pub username@remote_host
```

##### 3. 选择区域，这里涉及到时区，选择 other->Asia->China

```bash
$ ssh username@remote_host
```

4. ##### Disable Password Authentication on your Server

```bash
$ sudo vim /etc/ssh/sshd_config
```

```
...
PasswordAuthentication no
...
```

```bash
$ sudo systemctl restart ssh
```
