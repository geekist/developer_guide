
## 一、Yum介绍

YUM，全称Yellow dog Updater, Modifier，是一个自由、开源的命令行软件包管理工具，运行在基于RPM包管理的的Linux操作系统（例如RedHat、CentOS、Suse等）。

基于 RPM 包管理，能够从指定的服务器自动下载 RPM 包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包，无须繁琐地一次次下载、安装。

yum 主要是更方便的添加、删除、更新RPM包，自动解决软件包之间的依赖关系，方便系统更新及软件管理。yum 通过软件仓库（repository）进行软件的下载、安装等，软件仓库可以是一个 HTTP 或 FTP 站点，也可以是一个本软件池，资源仓库也可以是多个，在 /etc/yum.conf 文件中进行相关配置即可。在yum的资源库中，会包括 RPM 的头信息（header），头信息中包括了软件的功能描述、依赖关系等。通过分析这些信息，yum 计算出依赖关系并进行相关的升级、安装、删除等操作。


## 二、Yum命令

### 2.1 语法：

```
yum [options] [command] [package ...]

options：可选，选项包括-h（帮助），-y（当安装过程提示选择全部为"yes"），-q（不显示安装的过程）等等。
command：要进行的操作。
package操作的对象。

```

常用选项(options)：

```
-h, --help         #显示帮助信息
-t, --tolerant     #容错
-C, --cacheonly    #完全从系统缓存中运行，不更新缓存
-c [config file], --config=[config file]      #本地配置文件
-R [minutes], --randomwait=[minutes]          #命令最大等待时间
-d [debug level], --debuglevel=[debug level]  #设置调试级别
-e [error level], --errorlevel=[error level]  #设置错误等级
-q, --quiet        #退出运行
-v, --verbose      #详细模式
-y, --assumeyes    #对所有交互提问都回答 yes

```
命令列表：

```
check         check         #检测 rpmdb 是否有问题
check-update  #检查可更新的包
clean         #清除缓存的数据
deplist       #显示包的依赖关系
distribution-synchronization  #将已安装的包同步到最新的可用版本
downgrade     #降级一个包
erase         #删除包
groupinfo     #显示包组的详细信息
groupinstall  #安装指定的包组
grouplist     #显示可用包组信息
groupremove   #从系统删除已安装的包组
help          #删除帮助信息
history       #显示或使用交互历史
info          #显示包或包组的详细信息
install       #安装包
list          #显示可安装或可更新的包
makecache     #生成元数据缓存
provides      #搜索特定包文件名
reinstall     #重新安装包
repolist      #显示已配置的资源库
resolvedep    #指事实上依赖
search        #搜索包
shell         #进入yum的shell提示符
update        #更新系统中的包
upgrade       #升级系统中的包
version       #显示机器可用源的版本

```


### 2.2 yum查询

yum [list | info | search | provides | whatprovides] 参数

```
$ yum [option] [查询工作项目] [相关参数]
选项与参数：
[option]：主要的选项，包括有：
  -y ：当 yum 要等待使用者输入时，这个选项可以自动提供 yes 的回应；
  --installroot=/some/path ：将该软件安装在 /some/path 而不使用默认路径
[查询工作项目] [相关参数]：这方面的参数有：
  search  ：搜寻某个软件名称或者是描述 (description) 的重要关键字；
  list    ：列出目前 yum 所管理的所有的软件名称与版本，有点类似 rpm -qa；
  info    ：同上，不过有点类似 rpm -qai 的运行结果；
  provides：从文件去搜寻软件！类似 rpm -qf 的功能！

范例一：搜寻git相关的软件有哪些？
$ yum search git

范例二：找出 git 这个软件的功能为何
$ yum info git

范例三：列出 yum 服务器上面提供的所有软件名称
$ yum list

范例四：列出目前服务器上可供本机进行升级的软件有哪些？
$ yum list updates  <==一定要是 updates 喔！

范例五：列出提供 passwd 这个文件的软件有哪些
$ yum provides passwd

```

### 2.3 yum安装升级功能

yum [install | update | groupinstall | groupupdate] 软件

```
$ yum [option] [查询工作项目] [相关参数]
选项与参数：
  install ：后面接要安装的软件！
  groupinstall : 组包安装，后面接软件包组
  update  ：后面接要升级的软件，若要整个系统都升级，就直接 update 即可
  groupupdate ： 组包升级

范例一：安装git
$ yum install git

范例二：升级整个系统的软件
$ yum update

```



### 2.4 yum删除功能

yum [remove | groupremove] 软件


```
$ yum remove git

```

### 2.5 yum清除缓存

yum 会把下载的软件包和header存储在cache中，而不会自动删除。如果我们觉得它们占用了磁盘空间，可以使用yum clean指令进行清除，更精确的用法是：

yum clean headers 清除header

yum clean packages 清除下载的rpm包

yum clean all 清除所有

yum [clean]

```
1.清除缓存目录(/var/cache/yum)下的软件包
$ yum clean packages

2.清除缓存目录(/var/cache/yum)下的 headers
$ yum clean headers

3.清除缓存目录(/var/cache/yum)下旧的 headers
$ yum clean oldheaders

4.清除缓存目录(/var/cache/yum)下的软件包及旧的headers
$ yum clean, yum clean all (= yum clean packages; yum clean oldheaders)

```

## 三、Yum配置

yum 的配置文件分为main 和repository ：

>main:定义了全局配置选项，该文件只有一个。通常位于 /etc/yum.conf
>
>repository:定义了源服务器的具体配置，可能是一或多个。通常位于 /etc/yum.repo.d 目录

### 3.1 配置文件main

可以通过以下命令查看yum的配置：

```
$ cat /etc/yum.conf

```
主要配置如下：

```
[main]
# yum 的缓存目录，用于存储下载的 RPM 包和数据库
cachedir=/var/cache/yum/$basearch/$releasever

# 安装完成后是否保留软件包，0为不保留（默认为0），1为保留
keepcache=0
   
# Debug 信息输出等级，范围为0-10，默认为2
debuglevel=2

# yum 日志文件位置，用户通过该文件查询做过的更新   
logfile=/var/log/yum.log
   
# 是否只安装和系统架构匹配的软件包。可选项为：1､0，默认为1
# 设置为 1 时不会将 i686 的软件包安装在适合i386的系统中
exactarch=1

# update 设置，是否允许更新陈旧的 RPM 包，相当于 upgrade
obsoletes=1
   
# 是否进行 GPG(GNU Private Guard) 校验，以确定 RPM 包的来源是有效和安全
# 当在这个选项设置在[main]部分，则对每个 repository 都有效
plugins=1

# 是否启用插件，默认1为允许，0表示不允许
gpgcheck=1
   
# 排除某些软件在升级名单之外，可以用通配符，各个项目用空格隔开
exclude=*.i?86 kernel kernel-xen kernel-debug

# 可同时安装多少程序包   
installonly_limit=5

# Bug 追踪路径   
bugtracker_url=http://bugs.centos.org/set_project.php?project_id=16&ref=http://bugs.centos.org/bug_report_page.php?category=yum

# 当前发行版版本号   
distroverpkg=centos-release

```

### 3.2 配置目录repository

在yum.repos.d 目录下存放的就是yum源的设定文件。

查看对应目录下的文件内容

```
$ cat /etc/yum.repos.d/CentOS-Base.repo
[base]
name=CentOS-$releasever - Base
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

...

```

说明：

>[base]：代表容器的名字！中刮号一定要存在，里面的名称则可以随意取。但是不能有两个相同的容器名称， 否则 yum 会不晓得该到哪里去找容器相关软件清单文件。
>
>name：只是说明一下这个容器的意义而已，重要性不高！
>
>mirrorlist=：列出这个容器可以使用的映射站台，如果不想使用，可以注解到这行；
>baseurl=：这个最重要，因为后面接的就是容器的实际网址！ mirrorlist 是由 yum 程序自行去捉映射站台， baseurl 则是指定固定的一个容器网址！我们刚刚找到的网址放到这里来啦！
>
>enable=1：就是让这个容器被启动。如果不想启动可以使用 enable=0 喔！
>
>gpgcheck=1：还记得 RPM 的数码签章吗？这就是指定是否需要查阅 RPM 文件内的数码签章！
>
>gpgkey=：就是数码签章的公钥档所在位置！使用默认值即可


注意：手工修改repo文件后，需要更新缓存，命令如下

```
yum clean all

```


