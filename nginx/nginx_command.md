
## nginx常用命令

### 运行nginx

如果nginx没有运行，则运行nginx

start nginx 

如果nginx已经运行，则重新启动nginx

nginx -s reload

### 终止nginx

快速停止nginx

nginx -s stop

完整有序的关闭

nginx -s quit

### 检查配置文件

nginx -t -c /nginx-1.15.2/conf/nginx.conf

* pid文件的作用

打开系统(Linux) 的 "/var/run/" 目录可以看到有很多已 ".pid" 为结尾的文件，如
/var/run 目录下的 pid 文件。这些文件只有一行，它记录的是相应进程的 pid，即进程号。所以通过 pid 文件可以很方便的得到一个进程的 pid，然后做相应的操作，比如检测、关闭。

pid 文件是不是只是存储呢？当然不是！它还有另一个更重要的作用，那就是防止进程启动多个副本。通过文件锁，可以保证一时间内只有一个进程能持有这个文件的写权限，所以在程序启动的检测逻辑中加入获取pid 文件锁并写pid文件的逻辑就可以防止重复启动进程的多个副本了。

下面是实现这个逻辑的一段 c 代码，在程序的启动检测逻辑中调用这个函数即可保证程序唯一启动。

```c
void writePidFile(const char *szPidFile)
{
    /* 获取文件描述符 */
    char str[32];
    int pidfile = open(szPidFile, O_WRONLY|O_CREAT|O_TRUNC, 0600);
    if (pidfile < 0) {
        printf("pidfile is %d", pidfile);
        exit(1);
    }
   
    /* 锁定文件，如果失败则说明文件已被锁，存在一个正在运行的进程，程序直接退出 */
    if (lockf(pidfile, F_TLOCK, 0) < 0) {
        fprintf(stderr, "File locked ! Can not Open Pid File: %s", szPidFile);
        exit(0);
    }

    /* 锁定文件成功后，会一直持有这把锁，知道进程退出，或者手动 close 文件
       然后将进程的进程号写入到 pid 文件*/
    sprintf(str, "%d\n", getpid()); // \n is a symbol.
    ssize_t len = strlen(str);
    ssize_t ret = write(pidfile, str, len);
    if (ret != len ) {
        fprintf(stderr, "Can't Write Pid File: %s", szPidFile);
        exit(0);
    }
}

```