
# __HZNUOJ Installation Manual For Debian Linux__

##### 1. 升级包管理器

```bash
~# apt update
```

##### 2. 安装 git gnupg

```bash
~# apt install git gnupg
```

##### 3. 添加 MySQL APT Repository

Go to the download page for the MySQL APT repository at [https://dev.mysql.com/downloads/repo/apt/](https://dev.mysql.com/downloads/repo/apt/)

```bash
~# wget https://dev.mysql.com/get/mysql-apt-config_0.8.22-1_all.deb
~# dpkg -i ./dev.mysql.com/get/mysql-apt-config_0.8.22-1_all.deb
~# apt update
```

4. ##### 安装 Mysql-Server

```bash
~# apt install mysql-server
```

```
...
PasswordAuthentication no
...
```

```bash
$ sudo systemctl restart ssh
```
