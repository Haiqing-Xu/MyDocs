# Installing Lighttpd, PHP 7.3 on Debian 10

In this guide, we are going to explain the steps in installing Lighttpd, PHP, and SQLite3 on Debian 10.

#### 1. Updating System Packages

It is recommended that you update the system to the latest packages before beginning any major installations. Issue the command below:

```bash
shell> sudo apt update && sudo apt upgrade
```

#### 2. Installing Lighttpd

```bash
shell> sudo apt install lighttpd
```

Ensure that Lighttpd is running by checking the status with the following command:

```bash
shell> sudo systemctl status lighttpd
```

#### 3. Install PHP

Issue the command below to install PHP and its related packages from the apt repositories:

```bash
shell> sudo apt install php-fpm php php7.3-sqlite3
```

#### 4. Configure Lighttpd

After installing Lighttpd, PHP 7.3, there are some configurations which we have to carry out to correctly combine the packages to produce the desired results.

First, we must modify /etc/php/7.3/fpm/php.ini and uncomment the line cgi.fix_pathinfo=1:

```bash
shell> sudo vim /etc/php/7.3/fpm/php.ini
```

```
;cgi.fix_pathinfo=1			=>		cgi.fix_pathinfo=1
```

Next, make the following changes:

```bash
shell> cd /etc/lighttpd/conf-available/
shell> sudo cp 15-fastcgi-php.conf 15-fastcgi-php.conf.bak
shell> sudo vim 15-fastcgi-php.conf
```

Comment out the following lines:

```
#"bin-path" => "/usr/bin/php-cgi",
#"socket" => "/var/run/lighttpd/php.socket",
```


Add the following line:

```
"socket" => "/var/run/php/php7.3-fpm.sock",
```


Lastly, to enable the fastcgi configuration, run the following commands:

```bash
shell> sudo lighttpd-enable-mod fastcgi
shell> sudo lighttpd-enable-mod fastcgi-php
```

```
This creates the symlinks /etc/lighttpd/conf-enabled/10-fastcgi.conf which points to /etc/lighttpd/conf-available/10-fastcgi.conf 
and 
/etc/lighttpd/conf-enabled/15-fastcgi-php.conf which points to 
/etc/lighttpd/conf-available/15-fastcgi-php.conf:
```

Next, reload Lighttpd:

```
shell> sudo service lighttpd force-reload
```

The next step is to check that Lighttpd and PHP are working correctly. We can test this using a PHPInfo file.

```
shell> sudo vim /var/www/html/info.php
```


Enter the following content:

```php
<?php phpinfo(); ?>
```

Next, visit your server's IP address on the browser or your domain name if you have one followed by /info.php

#### Setup TPB's Server

```bash
shell> sudo cp tpb.db /var/www
shell> sudo chgrp www-data /var/www/tpb.db
shell> sudo chmod -R 640 /var/www/tpb.db

shell> sudo cp htdocs/* /var/www/html
shell> sudo chgrp -R www-data /var/www/html
shell> sudo chmod -R 750 /var/www/html
```

modify /etc/lighttpd/lighttpd.conf

```bash
shell> sudo vim  /etc/lighttpd/lighttpd.conf
```

```
server.modules += ("mod_rewrite",)

# These are my mod_rewrite rules:
url.rewrite = (
        "^/browse$" => "/cats.php",
        "^/browse/(\d+)$" => "/browse.php?cat=$1",
        "^/browse/(\d+)/(\d+)$" => "/browse.php?cat=$1&page=$2",
        "^/recent$" => "/browse.php",
        "^/recent/(\d+)$" => "/browse.php?page=$1",
        "^/search/([^/]+)$" => "/browse.php?q=$1",
        "^/search/([^/]+)/(\d+)$" => "/browse.php?q=$1&page=$2",
        "^/s\?(.+)$" => "/browse.php?$1&redir",
        "^/details/(\d+)$" => "/details.php?id=$1",
        "^/details/(\d+)/(.+)$" => "/details.php?id=$1&q=$2",
        "^/torrent/(\d+)$" => "/torrent.php?id=$1",
        "^/about$" => "/about.php",
        "^/about\?(.+)$" => "/about.php?$1",
        "^/searchplugin$" => "/searchplugin.php",
)
```

modify /var/www/html/config.php

```bash
shell> sudo vim /var/www/html/config.php
```

```php
#DB
$dbfile = "/var/www/tpb.db";
```

