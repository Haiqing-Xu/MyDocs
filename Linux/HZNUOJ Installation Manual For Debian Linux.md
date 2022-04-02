
# __HZNUOJ Installation Manual For Debian Linux__


### 注意：以下命令成功需要root在普通用户目录下执行为前提


##### 1. 添加环境变量

```bash
# echo 'export PATH=/sbin:/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin' > .bashrc
# echo 'export PATH=/sbin:/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin' > /root/.bashrc
```
需退出当前用户，重新登录后才生效


##### 2. 升级包管理器

```bash
# apt update
```

##### 3. 安装 git gnupg

```bash
# apt install git gnupg
```

##### 4. 添加 MySQL APT Repository

Go to the download page for the MySQL APT repository at [https://dev.mysql.com/downloads/repo/apt/](https://dev.mysql.com/downloads/repo/apt/)

```bash
# wget https://dev.mysql.com/get/mysql-apt-config_0.8.22-1_all.deb
# dpkg -i ./mysql-apt-config_0.8.22-1_all.deb
```

Configuring mysql-apt-config
Select last option 'Ok' to save the configuration

then run 'apt update' to load package list

```bash
# apt-get update
```

##### 5. 安装 Mysql-Server

```bash
~# apt install mysql-server
```
Set root password


Select "Use Legacy Authentication Method (Retain MySQL 5.x Compatibility)" （上面的"Use Strong Password Encryption",虽然官方推荐，但是HZNUOJ不兼容这种加密方式）



```bash
$ sudo systemctl restart ssh
```
