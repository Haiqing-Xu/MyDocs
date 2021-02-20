# 如何设置和查看 ipfilter 日志文件

缺省情况下，ipfilter 的所有日志信息都由 syslog 记录。较好的做法是创建一个日志文件，用来单独记录 ipfilter的通信信息，将其与可能记录在 syslog 日志文件中的其他数据相区分。

开始之前，必须切换到 root 角色。

##### 1.确定已启用的 system-log 服务

```bash
shell> svcs system-log
STATE          STIME    FMRI
online         18:57:04 svc:/system/system-log:default
```

##### 2.通过添加以下两行来编辑 /etc/syslog.conf 文件

```bash
shell> vim /etc/syslog.conf
## Save IP Filter log output to its own file
local0.debug	/var/log/ipmon.log
```

##### 3.创建新日志文件

```bash
shell> touch /var/log/ipmon.log
```

##### 4. 重新启动system-log服务

```bash
shell>svcadm restart system-log
```

或

##### 刷新 system-log 服务的配置信息

```
shell> svcadm refresh system-log:default
```

##### 5. 查看日志中相应IP的信息

```
cat /var/ipmon.log | grep ip
```



#### 显示 ipmon 进程的信息

##### 1.获得 ipmon 进程的进程 ID

```bash
shell> pgrep ipmon
5336
```

##### 2. 查看 ipmon 进程的当前工作目录

```bash
shell> pwdx 5336

```

##### 3. 查看 ipmon 进程的进程树

```bash
shell> ptree 5336
5336  /usr/sbin/ipmon -Ds
```

##### 4. 显示 fstat 和 fcntl 信息

```
shell> pfiles 5336
5336:   /usr/sbin/ipmon -Ds
  Current rlimit: 65536 file descriptors
   3: S_IFCHR mode:0000 dev:535,0 ino:1722 uid:0 gid:0 rdev:127,95
      O_WRONLY|O_LARGEFILE FD_CLOEXEC
      /devices/pseudo/log@0:conslog
      offset:0
   4: S_IFDOOR mode:0444 dev:546,0 ino:56 uid:0 gid:0 rdev:538,0
      O_RDONLY|O_LARGEFILE FD_CLOEXEC  door to nscd[202]
   5: S_IFCHR mode:0000 dev:535,0 ino:1720 uid:0 gid:0 rdev:107,1
      O_RDONLY|O_LARGEFILE
      /devices/pseudo/ipf@0:ipf
      offset:1640
```



#### 附录：ipmon 命令介绍

监视 /dev/ipl 中记录的包，属于与 Solaris ipfilter关联的一套命令。

ipmon 命令可打开 /dev/ipl 进行读取，等待来自包过滤器的要保存数据。从设备读取的二进制数据以用户可读的格式重新输出。输出转至标准输出（缺省设置）或者文件（如果在命令行上指定了文件名）


支持以下选项：

```
ipmon [-abDFhnpstvxX] [-N device] [ [o] [NSI]] [-O [NSI]] [-P pidfile] [-S device] [-f device] [filename]
```

–a
打开所有设备日志文件读取日志条目。所有条目都显示到相同的输出设备（stderr 或 syslog）。

–b
对于记录包主体的规则，生成表示标头后的包内容的十六进制输出。

–D
导致 ipmon 将自身转变为守护进程。ipmon 将自身转变成孤立进程不需要使用子 shell 或后台处理，因而其可以无限期运行。

–f device
指定其他设备/文件来读取常规 IP 过滤器日志记录的日志信息。

–F
刷新当前的包日志缓冲区。显示刷新的字节数，即使结果为零也显示。

–h
显示用法信息。

–n
在可能的情况下，将 IP 地址和端口号映射回主机名和服务名称。

–N device
设置从 device 读取或向其输出 NAT 日志记录所要打开的日志文件。

–o letter
指定实际从哪些日志文件读取数据。N，NAT 日志文件；S，状态日志文件；I，常规 IP 过滤器日志文件。–a 选项等效于使用 –o NSI。

–O letter
指定不希望从中读取数据的日志文件。此选项最常与 –a 结合使用。可用作参数的字母与用于 –o 的相同。

–p
导致日志消息中的端口号始终输出为编号，并且从不尝试对其进行查找。

–P pidfile
将 ipmon 进程的 PD 写入文件。缺省情况下该文件是 /var/run/ipmon.pid。

–s
读取的包信息将通过 syslogd 发送，而不是保存到文件。编译和安装时的缺省工具为 local0。使用以下级别：

LOG_INFO
使用 log 关键字（而非 pass 或 block）作为操作的已记录的包。

LOG_NOTICE
已记录且已通过的包。

LOG_WARNING
已记录且已阻塞的包。

LOG_ERR
已记录且可视为“短包”的包。

–S device
设置从 device 读取或向其输出状态日志记录所要打开的日志文件。

–t
按照 tail(1) 的执行方式读取输入文件/设备。

–v
显示 TCP window、ack 和 sequence 字段

–x
以十六进制显示包数据。

–X
以十六进制显示日志标头记录数据。