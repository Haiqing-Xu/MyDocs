# __Debian Linux 快速安装教程__

#### 1. 启动 Debian 安装程序
选择Graphical install，回车

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/1_start.png)



#### 2. Select a language （选择安装语言）
默认选择English，下面介绍都是以英语安装界面为主

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/2_localechooser_languagelist_0.png)



#### 3. Select your location （选择区域）
这里涉及到时区，选择 other->Asia->China

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/3.1_localechooser_shortlist_0.png)

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/3.2_localechooser_continentlist_0.png)

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/3.3_localechooser_countrylist_Asia_0.png)



#### 4. Select locales 选择你的语言环境

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/4_localechooser_preferred-locale_0.png)



#### 5. Configure the keyboard (设置键盘映射)

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/5_keyboard-configuration_xkb-keymap_0.png)



#### 6. Configure the network (设置网络）
(1) Enter the hostname for this system （输入系统的主机名）

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/6.1_netcfg_get_hostname_0.png)


(2) Enter the domain （输入该系统的域名）

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/6.2_netcfg_get_domain_0.png)



#### 7. Set up users and passwords (设置用户和密码）
(1) Set a password for root （设置root密码）

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/7.1_passwd_root-password_0.png)


(2) Create a user account, and enter the full name (新建一个普通用户账号,输入新用户的全名)

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/7.2_passwd_user-fullname_0.png)


(3) Select the username for the new account （为新的账号选择用户名）


![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/7.3_passwd_username_0.png)


(4) （为新用户选择一个密码）

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/7.4_passwd_user-password_0.png)



#### 8. Partition disks （磁盘分区）
(1) Use entire disk （使用整个磁盘）

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/8.1_partman-auto_init_automatically_partition_0.png)


(2) Select disk to partition （选择需要分区的磁盘）

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/8.2_partman-auto_select_disk_0.png)


(3) partitioning scheme （选择分区方案）
All files in one partition (recommended for new users) 将所有文件放在同一个分区中（推荐新手使用）

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/8.3_partman-auto_choose_recipe_0.png)


(4) Overview of your currently configured partitions and mount points （目前已配置的分区和挂载点的综合信息）

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/8.4_partman_choose_partition_0.png)


(5) Write the changes to disk （将改动写入磁盘）

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/8.5_partman_confirm_nooverwrite_0.png)



#### 9. Configure the package manager （配置软件包管理器）
(1) Scan extra installation media （扫描额外的安装介质）

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/9.1_apt-setup_cdrom_set-first_0.png)


(2) Debian archive mirror country （Debian 仓库镜像所在的国家）

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/9.2_mirror_http_countries_0.png)


(3) Debian archive mirror （Debian 仓库镜像服务器）

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/9.3_mirror_http_mirror_0.png)


(4) HTTP proxy information （HTTP 代理信息，如果没有请留空）

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/9.4_mirror_http_proxy_0.png)



#### 10. Configureing popularity-contest （参加软件包流行度调查）

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/10_popularity-contest_participate_0.png)



#### 11. Software selection （选择要安装的软件）

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/11_tasksel_first_0.png)



#### 12. Install the GRUB boot loader （安装GRUB启动引导器）
(1) Install the GRUB boot loader to your primary drive （将GRUB启动引导器安装至您的主驱动器）


![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/12.1_grub-installer_only_debian_0.png)

(2) Device for boot loader installation （安装启动引导器的设备）

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/12.2_grub-installer_choose_bootdev_0.png)



#### 13. Finish the installation （结束安装进程）

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/13_finish-install_reboot_in_progress_0.png)


