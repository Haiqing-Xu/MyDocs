How to Set Up SSH Keys on Debian 10

##1. Create the RSA Key Pair
   
```bash
~$ mkdir .ssh 
~$ cd .ssh
$ ssh-keygen
```

##2. Copy the Public Key to Debian Server
   
```bash
$ ssh-copy-id username@remote_host
```
	or

copy the path (ex: /home/user_name/.ssh/id_rsa.pub) and run the following command: 
```bash
$ ssh-copy-id -i /home/user_name/.ssh/id_rsa.pub username@remote_host
```

##3. Authenticate to Debian Server Using SSH Keys
   
```bash
$ ssh username@remote_host
```

##4. Disable Password Authentication on your Server
   
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