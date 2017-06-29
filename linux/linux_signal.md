# kill
用于终止指定的进程（terminate a process），是 Unix/Linux 下进程管理的常用命令。

## 用途
1. 通常在需要终止某个或某些进程时，先使用 ps/pidof/pstree/top 等工具获取进程 pid，然后用 kill 杀掉进程。
2. 向指定的进程或进程组发送信号（The  command kill sends the specified signal to the specified process or process group），或者确定进程号为 pid 的进程是否还在。例如，许多程序都把 HUP 信号作为重新读取配置文件的触发条件。

## 格式
> kill <pid>
> kill -TERM <pid>

发送 TERM 信号到指定进程，如果进程没有捕获该信号，则进程终止（If no signal is specified, the TERM signal is sent.  The TERM signal will kill processes which do not catch this signal.）

kill -l

列出所有信号名称（Print a list of signal names.  These are found in /usr/include/linux/signal.h）。只有第9种信号(SIGKILL)才可以无条件终止进程，其他信号进程都有权利忽略。

# killall

> killall httpd  

杀死同一进程组内的所有进程，允许指定要终止的进程的名称，而非pid



# 信号表

```
user_00@webdev:~> kill -l
 1) SIGHUP	 2) SIGINT	 3) SIGQUIT	 4) SIGILL	 5) SIGTRAP
 6) SIGABRT	 7) SIGBUS	 8) SIGFPE	 9) SIGKILL	10) SIGUSR1
11) SIGSEGV	12) SIGUSR2	13) SIGPIPE	14) SIGALRM	15) SIGTERM
16) SIGSTKFLT	17) SIGCHLD	18) SIGCONT	19) SIGSTOP	20) SIGTSTP
21) SIGTTIN	22) SIGTTOU	23) SIGURG	24) SIGXCPU	25) SIGXFSZ
26) SIGVTALRM	27) SIGPROF	28) SIGWINCH	29) SIGIO	30) SIGPWR
31) SIGSYS	34) SIGRTMIN	35) SIGRTMIN+1	36) SIGRTMIN+2	37) SIGRTMIN+3
38) SIGRTMIN+4	39) SIGRTMIN+5	40) SIGRTMIN+6	41) SIGRTMIN+7	42) SIGRTMIN+8
43) SIGRTMIN+9	44) SIGRTMIN+10	45) SIGRTMIN+11	46) SIGRTMIN+12	47) SIGRTMIN+13
48) SIGRTMIN+14	49) SIGRTMIN+15	50) SIGRTMAX-14	51) SIGRTMAX-13	52) SIGRTMAX-12
53) SIGRTMAX-11	54) SIGRTMAX-10	55) SIGRTMAX-9	56) SIGRTMAX-8	57) SIGRTMAX-7
58) SIGRTMAX-6	59) SIGRTMAX-5	60) SIGRTMAX-4	61) SIGRTMAX-3	62) SIGRTMAX-2
63) SIGRTMAX-1	64) SIGRTMAX	
```

常用信号表
![信号量解释.jpg](http://upload-images.jianshu.io/upload_images/4476075-c6cb50421e1eb16f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---
# 常用信号

## HUP
> kill -s HUP pid
> HUP     1    终端断线

让 Linux 缓和的执行进程关闭，然后重启。在对配置文件修改后需要重启进程时可发送此信号。 退出系统发出的信号

两种解释
1. 被许多守护进程理解为一个重置请求.如果一个进程不用重新启动就能重新读取它的配置文件并调整自己以适应变化的话,那么HUP通常来触发这种行为.
2. 由终端驱动程序生成,试图来"清除"(终止)跟某个特定终端相连的进程.
    例如:
    某个终端会话结束时,或者当调制解调器被挂断时,**shell后台不接受HUP的信号的影响**.有的用户可使用nohup来模仿这种行为.

重启命令
> ExecReload=/bin/kill -s HUP $PID

## INT
> INT       2    程序中断（Ctrl + c 可以触发）

当用户键入时由**终端驱动程序发送的信号**.这是一个终止当前操作的请求.如果捕获了这个信号,一些简单的程序应该退出,或者允许自给被终止,这也是程序没有捕获到这个信号时的默认处理方法.拥有命令行或者输入模式的那些程序应该停止它们在做的事情,清除状态,并等待用户的再次输入.

## QUIT
> QUIT    3    程序退出（同 Ctrl + \）

停止命令
> ExecStop=/bin/kill -s QUIT $PID

会记录日志

**QUIT和TERM类似----不同的是:它会生成内存转储**

## KILL

> kill -s KILL pid
> KILL      9    强制终止

同
> kill -9 pid

结束进程，非阻塞，不可忽略
这个强大和危险的命令迫使进程在运行时突然终止，进程在结束后不能自我清理。

**危害**
导致系统资源无法正常释放，一般不推荐使用，除非其他办法都无效。

**立即把进程无条件的杀掉**

## TERM

> kill -s TERM pid
> TERM    15    终止，阻塞，会被忽略

友好告诉进程退出，进程先保存好数据，再正常退出。
给父进程发送一个 TERM 信号，试图杀死它和它的子进程。
请求彻底终止某项执行操作.它期望接收进程清除自给的状态并退出

## CHLD
> CHLD 17 中断或停止

子进程结束，父进程收到此信号
当一个进程中断或停止，一个 CHLD 信号被发送给父进程
默认情况下这个信号会被忽略，所以父进程必须捕获这个信号，如果他想被通知，无论什么时候一个子进程的状态发生改变。

通常的做法是：在一个信号捕获函数，调用一个wait 函数，去捕获子进程的ID 和结束终止状态


## CONT
> CONT   18    继续（与STOP相反， fg/bg命令）

## STOP
> STOP    19    暂停（同 Ctrl + Z）

## TSTP
> TSTP 停止位

## TRAP

中断或其他自陷指令