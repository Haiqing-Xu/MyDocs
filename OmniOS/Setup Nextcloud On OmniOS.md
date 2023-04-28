# Setup Nextcloud On OmniOS


## Why do I choose OmniOS+napp-it ?

[![OmniOS](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/OmniOS_logo.png)](https://www.omniosce.org/)

 OmniOS builds on Illumos to make a complete operating system.

current stable 151024
more [http://www.omniosce.org/](http://www.omniosce.org/)

If you use OmniOS CE commercially,
consider to become a Patron, see [https://omniosce.org/patron.html](https://omniosce.org/patron.html)



[![napp-it](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/logo-hp.png)](https://www.napp-it.org/)

 the next generation ZFS-Server advanced and browser managed free internet nas san & backup server.

```
Napp-It’s free extensions include:
    AFP (netatalk)
    Airsonos
    AMP (Apache, MySQL, PHP stack)
    AMPO (obsolet)
    Baikal CalDAV / CardDAV Server
    Logitech MediaServer
    MediaTomb (DLNA / UPnP server)
    Nextcloud
    Owncloud (Dropbox alternative)
    PHPvirtualbox (VirtualBox interface)
    Pydio Sharing
    proFTPD
    Serviio Mediaserver
    Groupware Tine20
```
-----------

## 在 VMware ESXi 中安装 OmniOS 

[![napp-it](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/vmware_logo.png)](https://www.vmware.com/)

ESXi as base for a virtualized NAS/SAN appliance (napp-in-one)  
For napp-in-one, you can use a free or licenced edition of ESXi.

Download ESXi setup ISO and install from CD/DVD to a local disk or USB stick  
[https://www.vmware.com/products/vsphere-hypervisor/](https://www.vmware.com/products/vsphere-hypervisor/)

```
ESXi is a type-1 barebone virtualizer. It runs directly on hardware.
Think of it like a bios extension to run systems like BSD, Linux, OSX, Solaris or Windows side by side.

You can manage single ESXi servers with a free Windows application (vsphere).
After setup, ESXi comes with a 60 day trial of all commercial features.
From your download you can request a final free key for unlimited usage of ESXi without the commercial add-ons.
```

### 升级VMware

步骤1.关闭将升级的ESXi服务器上运行的虚拟机(VMs),并设置交换数据存储位置。

![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/VMware_store.png)

步骤2.设置ESXi服务器进入维护模式。这将帮助关闭hypervisor运行，并且允许服务器升级的所有关键服务。
```Bash
vim-cmd /hostsvc/maintenance_mode_enter
```

步骤3.修改允许流出的超文本传输协议(HTTP)连接的ESXi防火墙。
因为VMware服务器将被查询实际升级文件，必须允许从ESXi服务器的HTTP输出连接。
```Bash
esxcli network firewall ruleset set -e true -r httpClient
```

步骤4.查询可用的VMware服务器升级版本。
这将列出所有可升级的版本。如果vmware工具升级没有要求，可以选择```no-tools```版本，否则，可以选择```standard```版本升级。
```Bash
esxcli software sources profile list -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml | grep ESXi-6.5.0-2017
```

查看本地升级文件
```Bash
esxcli software sources profile list -d /vmfs/volumes/639841cd-ad968008-f160-3868dd48bc78/ISO/VMware-ESXi-8.0b-21203435-depot.zip
```

步骤5.选择版本升级。
```Bash
esxcli software profile update -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml -p ESXi-6.5.0-20171204001-standard
```

本地升级命令
```Bash
esxcli software profile update -d /vmfs/volumes/639841cd-ad968008-f160-3868dd48bc78/ISO/VMware-ESXi-8.0b-21203435-depot.zip -p ESXi-8.0b-21203435-standard
```
步骤6.重新启动服务器。
在升级进程完成后，服务器的重新启动要求为了新版本能生效。运行命令：
```Bash
reboot
```

步骤7.退出维护模式。

一旦ESXi主机回到联机，请从维护模式退出,恢复所有VM操作。
```Bash
vim-cmd /hostsvc/maintenance_mode_exit
```

步骤8.验证
要验证升级是否顺利完成，可以使用vSphere Web Client 连接ESXI服务器查看。或使用命令：
```Bash
vmware -v
```

## Manual Installation:

1. download OmniOS 151024 ce

   [https://www.omniosce.org/download.html](https://www.omniosce.org/download.html)

   1b. create a bootable USB setup stick

   You can setup OmniOS from USB  (use usb-dd file, 1 GB+ USB stick) or CD (.iso)  
   You can use this imager [www.napp-it.org/doc/downloads/usb_image.zip](http://www.napp-it.org/doc/downloads/usb_image.zip)  
   to transfer the usb image to your stick (Windows)

2. Install OmniOS 151024 to a systemdisk, name of the systempool must be rpool !!

   The size of the boot SSD should be at  at least 32 GB and at least 2 x RAM size.
   Due its reliability and powerloss protection I suggest an Intel SSD 35xx 80GB or 120GB

 - Boot the OS Installer from an USB stick
 - Install OmniOS to your bootdisks onto a pool named rpool

3. Configure network

4. Reboot

------------
### 在 VMware ESXi 中安装 OmniOS

#### 一、创建 ESXi 虚拟机

  1、选择`创建新虚拟机`

  ![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/ESXI_Create.PNG)

  2、OS 类型选择 `Oracle Solaris 11 (64 位)`

  ![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/ESXI_OS.PNG)

  3、根据需要调整CPU、内存和硬盘参数，网卡类型选择 `VMXNET3`,并挂载 OmniOS 系统安装光盘镜像。

  ![](https://github.com/Haiqing-Xu/MyDocs/blob/main/Images/ESXI_VMXNET3.PNG)

  虚拟机的创建即告完成。

-------
#### 二、安装 OmniOS

乏善可陈，基本上 F2（下一步）、F2、F2 ... 就完成了。好吧，我们的重点在于后续的 VMXNet3 万兆网卡的配置。

OmniOS 安装完成后，root 密码默认为空，需要你设置密码，及创建新普通用户用于 SSH 登录 balabala...

修改root密码
```Bash
passwd
```
添加用户tom
```Bash
mkdir /export/home
useradd -d /export/home/tom -m -s /usr/bin/bash -g staff tom
passwd tom
```
允许tom ssh登录
```Bash
echo AllowUsers xhq >> /etc/ssh/sshd_config
svcadm restart ssh
```
------------
#### 三、安装 VMware Tools 及 VMXNet3 网卡

OmniOS 没有自带 VMXNet3 网卡驱动，因此当前是没有网络可用的，需要安装相关驱动。

首先，断开安装 OS 时为虚拟机挂接上的光驱设备。

![](https://github.com/tiger2010/SNOO/raw/master/Screenshots/VMware_cd.png)

随后在控制台用以下命令开启 OmniOS 的即插即用服务。
```Bash
svcadm enable hotplug
```

安装 VMware Tools 这个过程，VMware 是以向虚拟机挂载软件光盘，并尝试自动启动安装进程来进行的，遗憾的是，Solaris 类型的客户机并不能自动挂载光驱，还得手动操作一下。

先找出光驱设备名
```Bash
ls –l /dev/dsk  | grep c\*s2
```

上图中的设备节点 c1t0d0s2，就是我们要找的东西，下面的命令则需要根据你的实际情况来调整。挂载光驱设备：
```Bash
mount –F hsfs –o ro /dev/dsk/c1t0d0s2 /media
```

解压展开 VMware Tools 安装文件：
```Bash
cd /tmp
tar zxvf /media/vmware-solaris-tools.tar.gz
cd vmware-tools-distrib
./vmware-install.pl
```

提示的所有问题，都用默认值回答，也就是一路狂按 Enter 键。

VMware Tools 安装完成之后，网卡驱动也随之就绪，我们可以开始配置网卡了。
```Bash
dladm show-link
```

网卡设备 vmxnet3s0 state unknow，为不可用状态。我们为其添加 IP interface
```Bash
ipadm create-if vmxnet3s0
```
删除 IP  interface
```Bash
ipadm delete-if vmxnet3s0
```

接下来为该 Interface 添加 IP 地址等参数
静态IP：
```Bash
ipadm create-addr –T static –a <IP-address>/24 vmxnet3s0/v4
```

动态IP：
```Bash
ipadm create-addr -T dhcp vmxnet3s0/dhcp
```

删除IP地址：
```Bash
ipadm delete-addr vmxnet3s0/v4
```

这时，你再使用 dladm show-link 及 dladm show-phys，应该会得到以下输出：

设置默认网关路由：
```Bash
route –p add default <Gateway-IP-address>
```

指定 DNS 解析服务器并激活：
```Bash
echo 'nameserver <Nameserver-IP-address>' >> /etc/resolv.conf
cp /etc/nsswitch.dns /etc/nsswitch.conf
```

查看IP地址
```Bash
ipadm show-addr
```

查看网关信息
```Bash
netstat -rn
```

create interface 
 list available interfaces and use linkname ex e1000g0:

 optional: enable Jumbo Frames first:
```Bash
dladm set-linkprop -p mtu=9000 e1000g0
```

```Bash
dladm show-link
ipadm create-if e1000g0
```
 Option: use DHCP (or skip to 6.0 to use a static adress)
```Bash
ipadm create-addr -T dhcp e1000g0/dhcp
```

 add nameserver
```Bash
echo 'nameserver 8.8.8.8' >> /etc/resolv.conf
```

 use dns (copy over DNS template)
```Bash
cp /etc/nsswitch.dns /etc/nsswitch.conf
```

If something happens (typo error), retry, opt. delete interface ex ipadm delete-if e1000g0

 Option:  add static IP address
  create static adress
```Bash
  ipadm create-addr -T static -a 192.168.0.1/24 e1000g0/v4
```

  add default route
```Bash
  route -p add default 192.168.0.254
```


OmniOS 网卡安装配置完成。

--------
### 系统升级

#### Configure Publishers

List configured publishers:
    
```Bash
 pkg publisher
```

Add a publisher:
```Bash
pkg set-publisher -g http://pkg.omniti.com/omniti-ms/ ms.omniti.com
```

Remove a publisher:
```Bash
pkg unset-publisher ms.omniti.com
```

You can change the repo URL for a publisher without removing it and re-adding it:
```Bash
pkg set-publisher -G http://old-url -g http://new-url <publisher-name>
```
e.g.
```Bash
pkg set-publisher -G http://pkg.omniti.com/omnios/r151018/ -g http://pkg.omniti.com/omnios/r151022/ omnios
```

#### List

List all installed packages:
```Bash
pkg list
```

Show detailed information on package omniti/runtime/perl:
```Bash
pkg info omniti/runtime/perl
```

If a package is not installed, add option "-r" to query remotely

    List the contents of a package: 
```Bash
    pkg contents omniti/runtime/perl
```

List only regular files (i.e. no dir, link, etc.):
```Bash
pkg contents -t file -o path omniti/runtime/perl
```

List all files with paths matching a pattern:
```Bash
pkg contents -t file -o path -a path=\*.pm omniti/runtime/perl
```

List the dependencies of a given package:
```Bash
pkg contents -t depend -o fmri subversion
```
See the section below on IPS dependencies for details.

#### Search

```Bash
pkg search pkg_name
```

Search terms are expected to be exact matches (there is no implicit substring matching.) Simple globbing with '?' and '*' are permitted, e.g.
```Bash
 pkg search 'git*'
```

The name of the package that provides something (file, dir, link, etc.) called "git":
```Bash
pkg search -p git
```
<-- simple syntax
Packages delivering directory names matching "pgsql*":
```Bash
pkg search -p 'dir::pgsql*'
```
 <-- structured syntax

If your search term is small and/or generic, limiting by action type such as "file" and index "basename" is useful to prune the result set:
```Bash
pkg search 'file:basename:ld'
```
Locally-installed packages that depend on a given package:
```Bash
pkg search -l -o pkg.name 'depend::system/library/gcc-4-runtime'
```
Leave out the "-l" to search remotely and find all available packages that depend on the given one.

#### Install/Update/Remove

Install a package:
```Bash
pkg install omniti/runtime/perl
```

Test to see what would be done:
```Bash
pkg install -nv omniti/runtime/perl
```

Install a specific version of a package:
```Bash
pkg install omniti/runtime/perl@5.16.1
```

Update a package: same as above, substituting "update" for "install"
You can "downgrade" too, just specify the older version you want:
```Bash
pkg update omniti/runtime/perl@5.14.2
```

Apply only the updates from a single publisher:
```Bash
pkg update pkg://mypublisher/*
```

Remove a package: 
```Bash
pkg uninstall omniti/runtime/perl
```

#### Audit

View package change history:
```Bash
pkg history
```

Use "-l" to get verbose info, including package names and versions.

Verify proper package state:
```Bash
pkg verify omniti/library/uuid
```

If a problem is found:
```Bash
pkg fix omniti/library/uuid
```

####Upgrading

In general, updates within stable releases do not require a reboot. Exceptions are made in cases of security vulnerabilities or bugs that affect core functionality.

To see what updates are available, and whether a reboot will be required, run:
```Bash
pkg update -nv
```

At the top of the output will be some summary information similar to:
```
            Packages to update:       311
     Estimated space available: 214.61 GB
Estimated space to be consumed: 518.12 MB
       Create boot environment:       Yes
     Activate boot environment:       Yes
Create backup boot environment:        No
          Rebuild boot archive:       Yes
```

```Bash
pkg install pkg:/package/pkg
```

Upgrade the global zone:
```Bash
pkg update
```

If "Create boot environment" is Yes, then a reboot will be required. Upgrading to a new stable release always requires a reboot.

### 系统调整

#### 修改系统时间

 用root用户登陆后修改。修改时间就用date命令就性，格式为date mmddHHMMYYYY.SS，月日时分年.秒.
```Bash
 date 041610362008.44
```

`2008年04月16日 星期三 10时36分44秒 CST`

### napp-it setup
```Bash
wget -O - www.napp-it.org/nappit | perl

```
除了napp-it，GCC6编译环境也已经安装配置完成。

### AMP setup
```Bash
wget -O - www.napp-it.org/amp-16.4.1  | perl
```

```
Apache 2.4. 
mySQL 5.7 (database)
Redis  (ultrafast in memory database)
PHP 7
PHPMyadmin 4.6.5.2
```

如果要安装最新版软件，可以下载该脚本，编辑版本信息后传到自己的web服务器上再安装

### Proftpd setup

ProFTPD 

ProFTPD is the default ftp Server on Oracle Solaris.
You can install proftpd on OmniOS and OpenIndiana as a SMF service via: 

```Bash
wget -O - www.napp-it.org/proftpd | perl
```

Add ```--with-modules=mod_tls```

comment:
The installer compiles 1.3.6 rc4
There are problems with user authentication that works with 1.3.6 rc1
A compilation of 1.3.6 final failed with an error on the auth module

### Nextcloud

more https://nextcloud.com/

Installer is tested on OmniOS only!
This installer requires to run the amp installer first.

Install: 
```Bash
wget -O - www.napp-it.org/nextcloud-11.0.1a  | perl
```

### 设置SMB共享
Add entry to /etc/pam.conf for pam_smb_passwd
```Bash
# smb settings set by napp-it installer
other   password required   pam_smb_passwd.so.1 nowarn
```

```Bash
zfs set sharesmb=on tank/xhq
zfs set sharesmb=name=xhq$ tank/xhq
passwd xhq
```


### 查看MAC地址
```Bash
netstat -pn |grep SP
```

### OmniOS镜像写入U盘
```Bash
format -e
dd if=/tank/xhq/omniosce-r151030ap.usb-dd of=/dev/rdsk/c6t0d0p0 bs=1M
```
### 添加zlog
```Bash
echo | format
zpool add tank log c6tACE42E000576F4EEd0
```
