# 使用管理

![control](https://user-gold-cdn.xitu.io/2018/12/25/167e430f2c5fb533?w=684&h=348&f=png&s=24600)

## 用户与用户组

linux 中涉及到用户信息的主要有三个文件

| 文件 | 作用 |
| ---------| ---------- |
| /etc/passwd | 账号 | 
| /etc/shadow | 密码  |
| /etc/group  | 用户组 |


/etc/passwd

登陆 Linux 主机的时候，输入的是我们的账号。但其实Linux并不认识账号，而是使用的与其对应的ID 
ID 与账号的对应就在 /etc/passwd 当中
每个登陆的使用者至少都会取得两个 ID ，一个是使用者 ID ( UID)、一个是群组 ID (GID)

/etc/group    用于展示用户群信息


* 登录Linux  在输入账号和密码时候    系统会发生什么

1 先找寻 /etc/passwd 里面是否有你输入的账号
如果没有则跳出，如果有的话则将该账号对应的 UID 与 GID (在 /etc/group 中) 读出来，另外，该账号的主文件夹与 shell 配置也一并读出

2 核对密码表
  Linux 会进入 /etc/shadow 里面找出对应的账号与 UID，然后核对一下你刚刚输入的密码与 /etc/shadow的密码是否相符

3  验证通过  就进入 Shell 管理的阶段


### 用户管理（增加 删除 管理）

新增一个用户 usersadd newName
删除一个用户  userdel  -r newName

新增一个用户组  groupadd newName
删除一个用户组 groupdel newName

将用户加入到组和从组中删除
usermod -a -G  boys b2  // 将已有用户添加到组中
gpasswd –a 用户名  组名        //添加用户
gpasswd –d 用户名  组名        //删除用户

查看用户属于某组  groups  用户名
新建用户加入某组  useradd –g  某组名  用户

* usersadd (新建用户)的时候Linux会发生什么

系统主要会帮我们处理几个项目

1 在 /etc/passwd 里面创建一行与账号相关的数据，包括创建 UID/GID 等

2 在 /etc/shadow 里面将此账号的密码相关参数填入，但是尚未有密码  (此时在 /etc/shadow 内仅会有密码参数但没有密码数据，需要使用命令『 passwd 账号 』来输出密码)

3 在 /etc/group 里面加入一个与账号名称一模一样的组名

4 在 /home 下创建一个与账号同名的目录作为用户主目录，且权限为 700  （ r4w2x1）




  
## crontab

定义 —  可以理解为node中的定时任务（node-schedule）
crontab —— 用于处理周期性的工作，循环工作

Linux常见的例行性工作调度  比如日志文件的轮替  临时文件的删除等
某些软件在运行中会产生一些缓存文件，但是当这个软件关闭时，这些缓存文件可能并不会主动的被移除
系统透过例行性工作来删除这些缓存文件

```js
编写  crontab -e     
查看  crontab -l
```

## 进程


触发任何一个事件时，系统都会将其定义为一个进程，并给与此进程一个ID，为PID. 同时依据触发这个程序的使用者与相关属性关系，给予这个 PID 一组有效的权限配置

* 程序
系统需要启动的那个二进制的文件
> 通常为二进制程序放置在存储媒介中，以物理文件形式存在

* 进程
程序触发之后，被加载到内存中成为一个个体，这就是进程
> 程序被触发后，执行者的权限与属性，程序的程序代码与所需数据会被加载到内存，操作系统会给与这个内存单元一个标识符PID


![pid](https://user-gold-cdn.xitu.io/2018/12/20/167ca41e31e5af01?w=422&h=284&f=png&s=145771)

## Linux的多用户 多任务

我们一直在说Linux是一个多用户 多任务的环境 这个是什么意思呢

多用户： 为什么不同身份下开启的进程，另一个用户看不到呢

在登录的时候，这个登录进程会有一个PID  A用户和B用户是两个不同的登录进程 （每个人登录后shell的pid不同）

 在同一台服务器登录的两个用户彼此之间是没有关系的两个进程，所以没有什么关系。不过会共享一些文件或者内存等

```js
// 文件目录
const dir = {}
// 一些内存
const Memory = {}

function linux1 () {
	// 用户1登录的环境
	function crontab(){...}
	function pm2(){...}
}

function linux2 () {
	// 用户2 登录的环境
	function crontab(){...}
	function pm2(){...}
}
```


多任务：“同步” 的真正含义

目前的 CPU 速度可高达几个 GHz。 这代表 CPU 每秒钟可以运行多次命令。

Linux 可以让 CPU 在各个工作间进行切换， 也就是说，其实每个工作都仅占去 CPU 的几个命令次数，所以 CPU 每秒就能够在各个程序之间进行切换


## job control

> 当登录后同时进行多个工作的行为管理

前端：可以出现提示字节让你操作的环境

后台：看不到但是仍在执行的环境


& 直接将命令丢到后台中『运行』

jobs 查询目前在后台的工作

fg 后台工作拿到前台处理   —-   fg %1

kill  杀掉进程     ——-   kill -9 pid  强制删除某一个进程

ps  监控某一个时间点的进程状态

top 可以监测整个系统的进程工作状态

## 系统资源查看

free 查看内存占用  free -mh

* 处理端口占用

lsof -i tcp:9997

du -h --max-depth=1 | sort -n   查询当前目录下磁盘占用量（子目录大小）

*  查询当前进程信息，比如node的：ps -ef | grep -v grep | grep node  










