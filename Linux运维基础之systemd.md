#### systemd基础

**systemd的简介**
​	计算机在启动一个操作系统必须加载并初始化操作系统，方能运行其他的应用程序，这是计算机初始化必不可少的一个启动过程，也就是说计算机启动需要一款初始化系统。systemd是Red Hat软件开发工程师Lennart Poettering以及Kay Sievers最初研发的一套Linux系统中最新的初始化系统，systems旨在能提高系统的启动效率与质量，一方面尽可能地减少进程已经将进程并行启动，另一方面就是更好地守护init进程，减少系统内存的不必要的开销。当然在systemd诞生之前呢，还有两个系统初始化的系统：systemvinit以及upstart，systemvinit是一套传统的初始化系统，已经逐渐地淡出了Linux历史舞台，然而被systemd和upstart取而代之，systemd和upstart它们有着各自的特点，不过目前已经有绝大多数的Linux发行版都采取了systemd，比如Fedora、openSUSE、Ubuntu、Gentoo、Arch Linux等一系列Linux发行版。
​	Linux系统启动要执行的程序是非常多的，比如挂载文件系统、加载硬件设备、启动后台服务、激活交换分区、启动用户程序等。每一个执行任务被systemd称为一个配置单元(uint)，换句话来说呢，挂在一个文件系统被称为一个配置单元、加载硬件设备被称为一个配置单元、启动系统后台服务被称为一个配置单元，等等。systemd包含了很多种不同类型的配置单元，在这里为什么写明具体有多少种类型的配置单元呢，很简单：因为当前systemd被诸多的Linux发行版系统推上了发展的热潮并一直在更新以及开发新的配置单元。以下就是目前systemd所包含的一些配置单元。

|    类型     | 说明                                       |
| :-------: | :--------------------------------------- |
|  service  | 服务配置单元，它代表一种服务进程，比如Apache2、Nginx、MySQL   |
|  device   | 设备配置单元，此类型的配置单元位于系统的设备树中                 |
|   mount   | 此类配置单元封装文件系统结构层次中的一个挂载点。Systemd 将对这个挂载点进行监控和管理 |
| automount | 该配置单元与上一个相似，automount为自动挂载点，但是当该自动挂载点被访问时，systemd 会执行挂载点中自定义的挂载行为 |
|   swap    | 交换配置单元，此类交换配置目的是用来管理交换分区。系统启动时激活用户在系统自定义的交换分区 |
|  target   | 逻辑配置单元，此类配置单元为其他配置单元进行逻辑分组。旨在引用其他配置单元    |
|   timer   | 定时器配置单元，它用来定时触发用户定义的操作，该类配置单元已经取代了传统的定时服务atd以及crond等 |
| snapshot  | 快照配置单元，顾名思义：用来保存系统运行的状态                  |
|  socket   | 套接字配置单元，此类配置单元封装系统和互联网中的一个套接字 。就目前来说，systemd 支持流式、数据报和连续包的AF_INET、AF_INET6、AF_UNIX socket 。 |

![systemd架构图](http://upload-images.jianshu.io/upload_images/1678789-e939e998bc54614b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

​	注意，每一个配置单元(uint)都有彼此对应的配置单元文件，比如Apache有对应的一个配置单元文件apache2.service、MysQL有对应的一个配置单元msqld.service。此类型的配置单元文件的编写即简单又简洁。便于Linux管理者或运维者编辑以及维护这些配置单元。如下是systemd中的一个系统日志服务的配置单元syslog.service文件内容：

```
[Unit]
Description=System Logging Service        #描述信息
Requires=syslog.socket          #指定依赖
Documentation=man:rsyslogd(8)         
Documentation=http://www.rsyslog.com/doc/

[Service]
Type=notify        #服务类型
ExecStart=/usr/sbin/rsyslogd -n
StandardOutput=null
Restart=on-failure        #指定失败时重启

[Install]
WantedBy=multi-user.target
Alias=syslog.service       #别名
```

​	我们简单分析systemd的配置单元文件可以归纳为三个部分组成，分别为Uint、Service、Install。Unit区块通常是配置文件的第一个区块，用来定义 Unit 的元数据，以及配置与其他 Unit 的关系。它的主要字段如表1-1-2；Install通常是配置文件的最后一个区块，用来定义如何启动，以及是否开机启动。它的主要字段如表1-1-3；[Service]区块用来 Service 的配置，只有 Service 类型的 Unit 才有这个区块。它的主要字段如表1-1-4。

表1-1-2

|      字段       | 描述                                       |
| :-----------: | ---------------------------------------- |
|  Description  | 描述信息                                     |
| Documentation | 文档URL等地址                                 |
|   Requires    | 当前 Unit 依赖的其他 Unit，如果它们没有运行，当前 Unit 会启动失败 |
|     Wants     | 与当前 Unit 配合的其他 Unit，如果它们没有运行，当前 Unit 不会启动失败 |
|    BindsTo    | 与Requires类似，它指定的 Unit 如果退出，会导致当前 Unit 停止运行 |
|    Before     | 如果该字段指定的 Unit 也要启动，那么必须在当前 Unit 之后启动     |
|     After     | 如果该字段指定的 Unit 也要启动，那么必须在当前 Unit 之前启动     |
|   Conflicts   | 这里指定的 Unit 不能与当前 Unit 同时运行               |
| Condition...  | 当前 Unit 运行必须满足的条件，否则不会运行                 |
|   Assert...   | 当前 Unit 运行必须满足的条件，否则会报启动失败               |

表1-1-3

|     字段     | 描述                                       |
| :--------: | ---------------------------------------- |
|  WantedBy  | 它的值是一个或多个 Target，当前 Unit 激活时（enable）符号链接会放入/etc/systemd/system目录下面以 Target 名 + .wants后缀构成的子目录中 |
| RequiredBy | 它的值是一个或多个 Target，当前 Unit 激活时，符号链接会放入/etc/systemd/system目录下面以 Target 名 + .required后缀构成的子目录中 |
|   Alias    | 当前 Unit 可用于启动的别名                         |
|    Also    | 当前 Unit 激活时，会被同时激活的其他 Unit               |

表1-1-4

|      字段       | 描述                                       |
| :-----------: | ---------------------------------------- |
|     Type      | 定义启动时的进程行为。它有以下几种值                       |
|  Type=simple  | 默认值，执行ExecStart指定的命令，启动主进程               |
| Type=forking  | 以 fork 方式从父进程创建子进程，创建后父进程会立即退出           |
| Type=oneshot  | 一次性进程，Systemd 会等当前服务退出，再继续往下执行           |
|   Type=dbus   | 当前服务通过D-Bus启动                            |
|  Type=notify  | 当前服务启动完毕，会通知Systemd，再继续往下执行              |
|   Type=idle   | 若有其他任务执行完毕，当前服务才会运行                      |
|   ExecStart   | 启动当前服务的命令                                |
| ExecStartPre  | 启动当前服务之前执行的命令                            |
| ExecStartPost | 启动当前服务之后执行的命令                            |
|  ExecReload   | 重启当前服务时执行的命令                             |
|   ExecStop    | 停止当前服务时执行的命令                             |
| ExecStopPost  | 停止当其服务之后执行的命令                            |
|  RestartSec   | 自动重启当前服务间隔的秒数                            |
|    Restart    | 定义何种情况 Systemd 会自动重启当前服务，可能的值包括always（总是重启）、on-success、on-failure、on-abnormal、on-abort、on-watchdog |
|  TimeoutSec   | 定义 Systemd 停止当前服务之前等待的秒数                 |
|  Environment  | 指定环境变量                                   |



​	那么systemd的配置单元文件位于什么地方呢，由于一个Linux操作系统有很多systemd的配置单元文件，并且不同的配置单元文件加载的顺序自然也是不一样的，也就是说，一个Linux操作系统有多个存放systemd配置单元的目录。以Ubuntu系统为例，systemd的配置单元文件位于如下的目录：

```
# find / -name  systemd
/sys/fs/cgroup/systemd
/bin/systemd
/etc/systemd
/etc/xdg/systemd
/run/user/1000/systemd
/run/systemd
/run/udev/tags/systemd
/lib/systemd
/lib/systemd/systemd
/usr/share/doc/systemd
/usr/share/doc/rygel/examples/service/service/systemd
/usr/share/systemd
/usr/share/bug/systemd
/usr/share/lintian/overrides/systemd
/usr/share/lintian/data/systemd
/usr/share/pam-configs/systemd
/usr/lib/python3/dist-packages/systemd
/usr/lib/systemd
/usr/lib/x86_64-linux-gnu/switchboard/hardware/pantheon-power/systemd
/var/lib/systemd
```

**systemd的原理 **
在systemd体系框架中，所有的服务程序都是可以并发进行启动的。比如 Avahi、D-Bus、livirtd、X11、HAL可以同时启动。当然，我们仔细思考会发现这样一个问题：Avahi需要syslog的服务，Avahi和syslog同时启动，倘若假设Avahi的启动比较快，syslog还没来得及启动，然而Avahi却需要记录日志，那么这种情况系统的启动岂不是有问题的吗？当然是的。Systemd系统采取了各个服务之间互相依赖的解决方案，这种解决方案的相互依赖具体分类成三种关系类型：socket 依赖、D-Bus 依赖以及文件系统依赖，然而每一个类型的依赖都是可以通过相应的技术解除依赖关系，从而解决了所有服务程序可以并发启动的问题。

systemd并发解决socket依赖
​	socket是套接字配置单元，几乎所有的服务依赖是套接字依赖。假设系统在初始化中有服务A、B以及一个套接字端口S，在启动中，服务A需要依赖套接字端口S，同时服务B依赖套接字端口S才能成功启动，倘若在初始化是套接字S没有成功启动端口时，在传统的启动逻辑思维中是这样的：首先需要服务A启动套接字端口S，等待它进入就绪状态之后再一一启动其他的服务A、B。但是 systemd 与此不一样，我们需要将套接字端口S预先启动，因此服务B就可以同时启动而无需等待服务 A 来创建 S 了，即使服务A没有成功启动，服务B也是可以正常启动的，那么其他进程向 S1 发送的服务请求实际上会被 Linux 操作系统缓存，其他进程会在这个请求的地方等待。一旦服务 A 启动就绪，就可以立即处理缓存的请求，一切都开始正常运行。
那么服务如何使用由 init 进程创建的套接字呢？Linux操作系统有一个特性，当进程调用 fork 或者 exec 创建子进程之后，所有在父进程中被打开的文件句柄 (file descriptor) 都会被子进程所继承。当然，套接字也是一种文件句柄，进程 A 可以创建一个套接字，此后当进程 A 调用 exec 启动一个新的子进程时，只要确保该套接字的 close_on_exec 标志位被清空，那么新的子进程就可以继承这个套接字。子进程看到的套接字和父进程创建的套接字是同一个系统套接字。

systemd并发解决D-Bus 依赖
​	D-Bus 是 desktop-bus 的简称，它是一个低延迟、低开销、高可用性的进程间通信机制。它越来越多地用于应用程序之间通信，同时也用于应用程序和操作系统内核之间的通信。很多系统服务进程都使用D-Bus 取代套接字作为进程间通信机制，对外提供服务。
假设如果服务 A 需要使用服务 B 的 D-Bus 服务，但是服务 B 并没有运行，则 D-Bus 可以在服务 A 请求服务 B 的 D-Bus 时自动启动服务 B。而服务 A 发出的请求会被 D-Bus 缓存，服务 A 会等待服务 B 启动就绪。这样，systemd依赖 D-Bus 的服务就可以实现并行启动。

systemd并发解决文件系统依赖
​	我们知道，Linux系统在启动的过程中，文件系统的启动是最耗时的，或者说依赖文件系统的进程的启动是相当地耗时的，比如挂载文件系统、各个磁盘配额的检测等与文件系统相关的进程执行是相当耗时的。在systemd启动系统框架中是可以解决这种不良的逻辑依赖，Linux的服务不需要等到文件系统初始化完成才可以成功启动。systemd是如何做到的呢，它集成了AutoFS的设计思路模式。AutoFS是一个程序，它使用Linux内核自动挂载器根据需要自动挂载文件系统，它通过内核 automounter 模块可以监测到某个文件系统挂载点真正被访问到的时候才触发挂载操作。
 	比如目录/home，当系统启动的时候，systemd 为其创建一个临时的自动挂载点。在这个时刻/home 真正的挂载设备尚未启动好，真正的挂载操作还没有执行，文件系统检测也还没有完成。可是那些依赖该目录的进程已经可以并发启动，他们的 open()操作被内建在 systemd 中的 autofs 捕获，将该 open()调用挂起（可中断睡眠状态）。然后等待真正的挂载操作完成，文件系统检测也完成后，systemd 将该自动挂载点替换为真正的挂载点，并让 open()调用返回。由此，实现了那些依赖于文件系统的服务和文件系统本身同时并发启动。注意对于"/"根目录的依赖实际上一定还是要串行执行，因为 systemd 自己也存放在/之下，必须等待系统根目录挂载检查好。这种并发可以提高系统的启动速度，尤其是当/home 是远程的 NFS 节点，或者是加密盘等，需要耗费较长的时间才可以准备就绪的情况下，因为并发启动，这段时间内，系统可以利用这段空余时间启动其他服务，总的来说就大大缩短了系统启动时间。

------

systemd的启动顺序

​	在了解了systemd原理之后，我们可以知道：在systemd初始化系统机制中，不管程序的依赖关系，全部可以并行启动，调用的服务程序存在依赖关系，则自动激活其他程序。
以Ubuntu系统为例，systemd的启动顺序如下：
(1) Boot Sequence ，启动顺序，比如硬盘启动、软盘启动、u盘启动等
(2) Bootloader ，引导加载
(3) kernel + initramfs(initrd) ， 加载内核以及initramfs或initrd
(4) rootfs ， 文件系统
(5) /sbin/init ， 启动init进程
​	对与systemd的所有详细启动顺序情况，我们可以使用systemd本身自带的systemd-analyze，它是一个分析启动性能的工具，用于分析启动时服务时间消耗。默认显示启动是内核和用户空间的消耗时间，下面简单介绍systemd-analyze的基本用法。

```
# 查看启动耗时
$ systemd-analyze                                                                                       
# 查看每个服务的启动耗时
$ systemd-analyze blame
# 显示瀑布状的启动过程流
$ systemd-analyze critical-chain
# 显示指定服务的启动流
$ systemd-analyze critical-chain atd.service
# 将systemd启动的顺序以及消耗的时间详细信息生成svg
$ systemd-analyze plot > message.svg
```

​	以Ubuntu为例，执行`systemd-analyze plot > message.svg`命令生成的矢量图部分如下，充分直观地显示了systemd启动顺序以及消耗的时间。
![systemd](http://upload-images.jianshu.io/upload_images/1678789-e93c54328cd439db.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

------

 systemd管理与命令
​	Systemctl是一个systemd工具，主要负责控制systemd系统和服务管理器，Systemctl工具集成了诸多的命令，使得管理员更好地控制管理Linux系统，首先我们来查看Systemctl的版本。

```
➜  ~ systemctl --version
systemd 229
+PAM +AUDIT +SELINUX +IMA +APPARMOR +SMACK +SYSVINIT +UTMP +LIBCRYPTSETUP +GCRYPT +GNUTLS +ACL +XZ -LZ4 +SECCOMP +BLKID +ELFUTILS +KMOD -IDN
```

`systemctl list-units`命令可以查看当前系统的所有 Unit

```
# 列出正在运行的Uint
systemctl list-units
# 列出所有的Uint
systemctl list-uints --all
# 列出所有没有运行的Uint
systemctl list-units -all --state=inactive
# 列出所有加载失败的Uint
systemctl list-units --failed
# 列出所有正在运行的、类型为service的Unit
systemctl list-units --type=service
```

`systemctl status`命令用于查看系统状态和单个 Unit 的状态

```
# 显示系统状态
$ systemctl status
# 显示单个 Unit 的状态，以mysql为例
$ systemctl status mysql.service
# 显示远程主机的某个 Unit 的状态，以mysql为例
$ systemctl -H {user}@{ip} status mysql.service
```

`systemctl {action}`对于用户来说，是常使用的命令，用于启动(start)、停止(stop)、重加载(reload)以及重启(restart) Unit（主要是 service），下面主要以以mysql为例。

```
# 启动一个服务
$  systemctl start mysql.service
# 停止一个服务
$ systemctl stop mysql.service
# 重启一个服务
$ systemctl restart mysql.service
# 杀死一个服务的所有子进程
$ systemctl kill mysql.service
# 重新加载一个服务的配置文件
$ systemctl reload mysql.service
# 重载所有修改过的配置文件
$ systemctl daemon-reload
# 显示某个 Unit 的所有底层参数
$ systemctl show mysql.service
# 显示某个 Unit 的指定属性的值
$ systemctl show -p CPUShares mysql.service
# 设置某个 Unit 的指定属性
$ sudo systemctl set-property mysql.service CPUShares=500
```

`systemctl list-dependencies`命令，用于查看systemctl配置单元的依赖关系，以mysql为例

```
# 列出一个 mysql Unit 的所有依赖。
$ systemctl list-dependencies mysql.service
# 如果要展开 Target，就需要使用--all参数。
$ systemctl list-dependencies --all mysql.service
```

`systemctl enable/disable`命令，用于将systemd配置单元激活或撤销开机启动，以mysql为例

```
# 激活开机启动mysql服务
$ systemctl enable mysql.service
# 撤销开机启动mysql服务
$ systemctl disable mysql.service
```

`systemctl list-unit-files`命令，用于查看配置单元的所有文件(Unit File)以及配置单元的状态(Status)情况

```
# 列出所有配置文件
$ systemctl list-unit-files
# 列出指定类型的配置文件
$ systemctl list-unit-files --type=service
```

对于Unit的状态包含如下四中情况：

- enabled：已建立启动链接，已激活即开机启动
- disabled：没建立启动链接，已撤销开机启动
- static：该配置文件没有[Install]部分，即无法被执行，只能作为其他配置文件的依赖
- masked：该配置文件被禁止建立启动链接

systemd电源管理命令

```
# 重启机器	
$ systemctl reboot	
# 关机
$ systemctl poweroff	
#挂起	
$ systemctl suspend	
# 休眠
$ systemctl hibernate		
# 混合休眠模式
$ systemctl hybrid
```

查看target中Unit组的状态及切换

```
# 切换target
$ systemctl isolate multi-user.target
$ systemctl isolate graphical.target
# 获取默认的target
$ systemctl get-default 
# 设置默认的target
$ systemctl set-default multi-user.target
```

`hostnamectl`命令查看与设置主机信息

```
# 查看主机信息
$ hostnamectl
# 设置主机信息，以设置hostname为例
$ hostnamectl set-hostname {$hostname}
```

`timedatectl`命令管理时区

```
# 查看时区日期等信息
$ timedatectl
# 设置主机的时区
$ timedatectl set-timezone Asia/Shanghai
# 将硬件时钟配置为地方时
$ timedatectl set-local-rtc true
```

注意，在执行上述的命令中，*.service的后缀.service是可以省略不写的，其执行的结果是一样的。对于更多的systemd内置命令请到systemd官方wiki查阅。