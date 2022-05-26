
## 开机和关机

>linux系统中，一切皆文件。所有的操作都是文件操作。

>开机会启动许多程序。它们在Windows叫做"服务"（service），在Linux就叫做"守护进
程"（daemon）。

### 开机

当计算机打开电源后，首先是BIOS开机自检，按照BIOS中设置的启动设备（通常是硬盘）来启动。

阿里云服务器可以在阿里云的后台管理系统

### 关机前同步数据 ---sync

 sync 将内存中的数据同步到硬盘中。

### 关机

#### shutdown

```

hutdown # 关机指令，你可以man shutdown 来看一下帮助文档。例如你可以运行如下命令关机：

shutdown –h 10 # 这个命令告诉大家，计算机将在10分钟后关机

shutdown –h now # 立马关机

hutdown –h 20:25 # 系统会在今天20:25关机

shutdown –h +10 # 十分钟后关机

```

#### poweroff



#### halt


### 重新启动

* shutdown –r now # 系统立马重启

* shutdown –r +10 # 系统十分钟后重启

* reboot # 就是重启，等同于 shutdown –r now



