# Setup Proftpd With TLS

ProFTPD 

ProFTPD is the default ftp Server on Oracle Solaris.
You can install proftpd on OmniOS and OpenIndiana as a SMF service via: 

```Bash
wget -O - www.napp-it.org/proftpd | perl
```

Add ```--with-modules=mod_tls```

下载 [www.napp-it.org/proftpd](http://www.napp-it.org/proftpd) 脚本文件，添加 mod_tls
```Bash
wget www.napp-it.org/proftpd
vi proftpd
```

<del>print "compile now\n";</del><br>
 <del>print "cd /opt/local/share/$ver \n./configure --prefix=/opt/local/ \nmake && make install;\n\nplease wait some minutes..\n";</del>

<del># compile</del><br>
<del>$r=\`cd /opt/local/share/$ver; ./configure --prefix=/opt/local/; make && make install;\`;</del>

```perl
print "compile now\n";
print "cd /opt/local/share/$ver \n./configure --prefix=/opt/local/ \--with-modules=mod_tls \nmake && make install;\n\nplease wait some minutes..\n";

 # compile
$r=`cd /opt/local/share/$ver; ./configure --prefix=/opt/local/ \--with-modules=mod_tls; make && make install;`;
```

脚本自动安装：
```Bash
perl proftpd
```

comment:
The installer compiles 1.3.6 rc4
There are problems with user authentication that works with 1.3.6 rc1
A compilation of 1.3.6 final failed with an error on the auth module

### Proftpd.conf.example

```
# This is a basic ProFTPD configuration file (rename it to 
# 'proftpd.conf' for actual use.  It establishes a single server
# and a single anonymous login.  It assumes that you have a user/group
# "nobody" and "ftp" for normal operation and anon.

ServerName			 "ProFTPD Default Installation"
ServerType			  standalone
DefaultServer			on
UseReverseDNS			off

MasqueradeAddress 192.168.1.1

# Port 21 is the standard FTP port.
Port				21
PassivePorts                    1234 1235
# Don't use IPv6 support by default.
UseIPv6				off

# Umask 022 is a good standard umask to prevent new dirs and files
# from being group and world writable.
Umask				022

# To prevent DoS attacks, set the maximum number of child processes
# to 30.  If you need to allow more than 30 concurrent connections
# at once, simply increase this value.  Note that this ONLY works
# in standalone mode, in inetd mode you should use an inetd server
# that allows you to limit maximum number of processes per service
# (such as xinetd).
MaxInstances			30

# Set the user and group under which the server will run.
User				nobody
Group				nogroup

# To cause every FTP user to be "jailed" (chrooted) into their home
# directory, uncomment this line.
DefaultRoot /tank	

# Normally, we want files to be overwriteable.
AllowOverwrite		on

# Bar use of SITE CHMOD by default
<Limit SITE_CHMOD>
  DenyAll
</Limit>

#Include /opt/local/etc/proftpd-tls.conf

<IfModule mod_lang.c>
    UseEncoding utf8 zh_CN
</IfModule>

# A basic anonymous configuration, no upload directories.  If you do not
# want anonymous users, simply delete this entire <Anonymous> section.
<Anonymous ~ftp>
  User				ftp
  Group				ftp

  # We want clients to be able to login with "anonymous" as well as "ftp"
  UserAlias			anonymous ftp

  # Limit the maximum number of anonymous logins
  MaxClients			10

  # We want 'welcome.msg' displayed at login, and '.message' displayed
  # in each newly chdired directory.
  DisplayLogin			welcome.msg
  DisplayChdir			.message

  # Limit WRITE everywhere in the anonymous chroot
  <Limit WRITE>
    DenyAll
  </Limit>
</Anonymous>

<Directory /tank/tom>
  HideFiles "(.$EXTEND|.DS_Store)$"
  <Limit ALL>
   Order Allow,Deny
   AllowUser tom
   DenyAll
  </Limit>
HideUser john
</Directory>
```

编辑 proftpd.conf，并测试

```Bash
vi /opt/local/etc/proftpd.conf
svcadm disable proftpd
svcadm enable proftpd
```

查看proftpd服务的状态

```Bash
svcs -l proftpd
```

### 生成TLS证书

```Bash
mkdir /opt/local/etc/ssl-proftpd
openssl req -new -x509 -days 3650 -nodes -out /opt/local/etc/ssl-proftpd/proftpd.cert.pem -keyout /opt/local/etc/ssl-proftpd/proftpd.key.pem
mkdir /var/log/proftpd
touch /var/log/proftpd/tls.log
```

编辑 proftpd.conf，去掉注释。<br>
<del>#Include /opt/local/etc/proftpd-tls.conf</del>

```
Include /opt/local/etc/proftpd-tls.conf
```

### proftpd-tls.conf
```
<IfModule mod_tls.c>
    TLSEngine on
    TLSLog /var/log/proftpd/tls.log

    # Support both SSLv3 and TLSv1
    TLSProtocol SSLv3 TLSv1

    # Are clients required to use FTP over TLS when talking to this server?
    TLSRequired off

    # Server's RSA certificate
    TLSRSACertificateFile      /opt/local/etc/ssl-proftpd/proftpd.cert.pem
    TLSRSACertificateKeyFile   /opt/local/etc/ssl-proftpd/proftpd.key.pem

    # Server's EC certificate
    #TLSECCertificateFile /etc/ftpd/server-ec.cert.pem
    #TLSECCertificateKeyFile /etc/ftpd/server-ec.key.pem

    # CA the server trusts
    #TLSCACertificateFile /etc/ftpd/root.cert.pem

    # Authenticate clients that want to use FTP over TLS?
    TLSVerifyClient off

    # Allow SSL/TLS renegotiations when the client requests them, but
    # do not force the renegotations.  Some clients do not support
    # SSL/TLS renegotiations; when mod_tls forces a renegotiation, these
    # clients will close the data connection, or there will be a timeout
    # on an idle data connection.
    TLSRenegotiate none
 #  RequireValidShell off
  </IfModule>
```

```Bash
vi /opt/local/etc/proftpd-tls.conf
svcadm disable proftpd
svcadm enable proftpd
```

查看proftpd服务的状态

```Bash
svcs -l proftpd
```

用 FileZilla 测试。







