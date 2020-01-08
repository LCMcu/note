# 			 文件IO

## 一、系统调用IO

### **access**

```c
#include <unistd.h>
int access(const char* pathname, int mode);
```

参数：

pathname: 是文件的路径名+文件名。

mode:

F_OK 值为0，判断文件是否存在

X_OK 值为1，判断对文件是可执行权限

W_OK 值为2，判断对文件是否有写权限

R_OK 值为4，判断对文件是否有读权限

注：后三种可以使用或“|”的方式，一起使用，如W_OK|R_O

返回值：成功返回0； 错误返回-1。



## 二、标准IO

字符串操作函数:

| 函数    | 原函数                                                    | 返回值                                                       | 功能                                                         |
| ------- | --------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| strlen  | size_t strlen(const char *str)                            | 返回长度                                                     | 计算字符串长度，并返回　直到遇到’\0’ ,`不包括`　’\0’，       |
| strcpy  | char *strcpy(char *a, const char *b)                      | 返回ａ                                                       | 把ｂ指向的字符串，复制到ａ,返回ａ，包括’\0’．                |
| strncpy | char  *strncpy(char *s1, const char *s2, size_t n)        | 返回s1                                                       | 把 s２ 所指向的字符串复制到s１，最多复制 n 个字符。当 s２ 的长度小于 n 时，s１ 的剩余部分将用空字节填充 |
| strcat  | char *strcat(char *s1, const char *s2)                    | 返回一个指向最终的目标字符串 s1 的指针。                     | 把s2指向的字符串，追加到s1所指的内容结尾；覆盖s1的结尾‘\0‘   |
| strncat | char *strncat(char *s1, const char *s2, size_t n)         | 返回一个指向最终的目标字符串 s1 的指针.                      | 把s2指向的字符串，追加ｎ个s1所指向的内容结尾’\0’;            |
| strcmp  | int strcmp(const char *s1, const char *s2)                | 返回值 < 0， str1 小于 str2。** 返回值 > 0，** str2 小于 str1。 **返回值 = 0， str1 等于 str2。 | 把 str1 所指向的字符串和 str2 所指向的字符串进行比较。       |
| strncmp | Int　strncmp(const char *s1, const char *s2, size_t n)    | 返回值同上                                                   | 把 str1 和 str2 进行比较，最多比较前 n 个字节。              |
| strchr  | char *strchr(const char *str, int c)                      | 返回在字符串 str 中第一次出现字符 c 的位置，如果未找到该字符则返回 NULL。 | 在参数 str 所指向的字符串中搜索第一次出现字符 c（一个无符号字符）的位置。 |
| strrchr | char *strrchr(const char *str, int c)                     | 返回同上                                                     | 在参数 str 所指向的字符串中搜索最后一次出现字符 c（一个无符号字符）的位置。 |
| strstr  | char *strstr(const char *s1, const char *s2)              | 该函数返回在 ｓ２ 中第一次出现 s2 字符串的位置。未找到则返回NULL | 在字符串 s1 中查找第一次出现字符串 s2 的位置，不包含终止符 '\0'。位置。 |
| getline | ssize_t getline(char **lineptr, size_t *n, FILE *stream); |                                                              |                                                              |
| puts    |                                                           |                                                              |                                                              |
| atoi    |                                                           |                                                              | 将字符串转换为整型值。                                       |
|         |                                                           |                                                              |                                                              |
| strtok  | char * strtok (char *str, const char * delimiters)        |                                                              | 原字符串将被改变，分隔符被替换为‘\０’，如果分隔符连续的话，只替换连续分隔符中的第一个。分割结束时　返回值=NULL |
| strsep  | char *strsep(char **s, const char *delim)                 |                                                              | 原字符串将被改变,分隔符被替换为‘\０’，如果分隔符连续的话，分隔符都被替换。                分割结束时＊ｓ=NULL |
|         | 堆空间:                                                   |                                                              |                                                              |
| malloc  | void *malloc(size_t size)                                 | 分配成功则返回指向被分配内存的[指针](https://baike.baidu.com/item/%E6%8C%87%E9%92%88)(此存储区中的初始值不确定)，否则返回空指针[NULL](https://baike.baidu.com/item/NULL)。 | 当内存不再使时，应使用free()函数将内存块释放。函数返回的指针一定要适当对齐，使其可以用于任何[数据对象](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E5%AF%B9%E8%B1%A1)。 |
| calloc  | void *calloc(size_t n, size_t size)                       | 回一个指向分配起始地址的[指针](https://baike.baidu.com/item/%E6%8C%87%E9%92%88/2878304)；如果分配不成功，返回NULL。 | 在内存的[动态存储](https://baike.baidu.com/item/%E5%8A%A8%E6%80%81%E5%AD%98%E5%82%A8/10370297)区中分配n个长度为size的连续空间,不再使用时，应使用free() |
| realloc | void *realloc(void *mem_address, unsigned int newsize);   | 如果重新分配成功则返回指向被分配内存的[指针](https://baike.baidu.com/item/%E6%8C%87%E9%92%88)，否则返回空指针NULL。 | 动态内存调整,先判断当前的指针是否有足够的连续空间，如果有，扩大mem_address指向的地址，并且将mem_address返回，如果空间不够，先按照newsize指定的大小分配空间，将原有数据从头到尾拷贝到新分配的内存区域，而后释放原来mem_address所指内存区域,不再使用时，应使用free() . |
|         | 初始化                                                    |                                                              |                                                              |
| memset  | void *memset(void *s,int c,size_t n)                      | 返回值为指向S的指针。                                        | 内存做初始化，将已开辟内存空间 s 的首 n 个字节的值设为值 c。 |
| bzero   | void bzero（void *s, int n）                              | 无                                                           | 内存做初始化，将字符串s的前n个字节置为0，一般来说n通常取sizeof(s),将整块空间清零。 |
| free    |                                                           |                                                              | 释放内存，要重复防止释放，第一次释放后                       |





## 三、高级IO

### IO多路转接

​	先构造一张有关描述符的列表，然后调用一个函数，如select、pselect、poll，直到这些描述符中的一个已准备好进行IO操作时，该函数才返回，返回时，它告诉进程哪些描述符已准备好可以进行IO操作了。IO多路转接可以避免阻塞IO的弊端，因为有时候需要在多个描述符上读read、写write，如果使用阻塞IO，就有可能长时间阻塞在某个描述符上而影响其它描述符的使用。

**fcntl**

fcntl() :打开一个已打开的文件属性。

```
#include <unistd.h>
#include <fcntl.h>
int fcntl(int fd, int cmd, ... /* arg */ );
```



1. 

### **select**(5)　

查询文件的可读性、可写性及错误状态信息

```c
  #include <sys/select.h>
  #include <sys/time.h>
  #include <sys/types.h>
  #include <unistd.h>
  int select(int nfds, fd_set *readfds, fd_set *writefds,fd_set *exceptfds, struct timeval 　*timeout);
　void FD_CLR(int fd, fd_set *set);　//清除集合中相关的fd
  int  FD_ISSET(int fd, fd_set *set);　//监测集合set中的相关fd的位
  void FD_SET(int fd, fd_set *set);　　//设置集合set中相关fd的位
  void FD_ZERO(fd_set *set);　　　　　　//清除集合set 中左右位
```

参数：

　1、**nfds**是需要监听的所有文件描述符中最大的文件描述符+1，+1的目的是为了限定遍历区间 
　2、**readfds,　writefds,　exceptfds**分别对应所要检测的可读文件描述符的集合，可写文件描述符的集合，异常文件描述符　的集合 ，监测结果也会返回到对应的集合中。
　3、**timeout**为结构timeval，用来设置select()等待的时间 
　　timeout的取值： 
　　NULL:表示select()没有timeout,select将一直被阻塞，知道某个文件描述符就绪 
　　0：仅检测文件描述符集合的状态，然后立即返回，并不等待外部事件的发生 

​	特定的时间值：如果在指定时间里没有发生，select将超时返回 

返回值：　成功　０；　错误　－１，设置errno.

实例：

```c
fd_set rset, wset;
FD_ZERO(&rset);  //集合清空
FD_ZERO(&wset);  //集合清空
FD_SET(fd1,&rset);
FD_SET(fd1,&wset);
FD_SET(fd2,&rset);
FD_SET(fd2,&wset);
/*fd2>fd1*/ // 最大fd +1
ret=select(fd2+1, &rset, &wset, NULL, NULL);  //没有可读或可写时一直阻塞
if(ret==-1)
{
	if(errno==EINTR)
		continue;
	perror("select()");
	goto ERROR;
}
```



### **poll(3)** :

```c
 #include <poll.h>
 int poll(struct pollfd *fds, nfds_t nfds, int timeout);
```

参数：fds　　结构体数组

```c
struct pollfd
{
　int fd； // 文件描述符
　short event；// 请求的事件
　short revent；// 返回的事件
}
```

**nfds**: 要监视的描述符的数目。

**timeout**: 

​	**timeout>0** 的毫秒数，无论I/O是否准备好，poll都会返回。  

​	**timeout<0** 表示无限超时,阻塞。

​	**timeout=0** 指示poll调用立即返回并列出准备好I/O的文件描述符，但并不等待其它的事件。这种情况下，poll()就像它的名字那样，一旦选举出来，立即返回。

**返回值**：poll()返回结构体中revents域不为0的文件描述符个数；如果在超时前没有任何事件发生，poll()返回0；失败时，poll()返回-1，并设置errno。

**实例**：

```c
struct pollfd pfd[2]={};
pfd[0].fd=fd1;
pfd[1].fd=fd2;

pfd[0].events=0;
pfd[0].revents=0;
pfd[1].events=0;
pfd[1].revents=0;

pfd[0].events|=POLLIN; //添加要监测的时间　　read()
pfd[1].events|=POLLIN;　//************    write() 
pfd[0].events|=POLLOUT;
pfd[0].events|=POLLOUT;

ret=poll(pfd,2,-1);  //
if(ret==-1)
{
 	if(errno==EINTR)
    	continue;
    goto ERROR;
 }
```



### **epoll**

epoll_create()  创建epoll实例

```c
#include <sys/epoll.h>
int epoll_create(int size);
```

参数: size 忽略，但必须大于０．

返回值：成功：epoll描述符。失败：－１。设置errno.



#### **epoll_ctl**

epoll_ctl()  操作epoll实例。

```c
#include <sys/epoll.h>
 int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);
```

参数: 

​	epfd:　epoll实例描述符。

​	op:     操作指令：EPOLL_CTL_ADD　添加一个文件描述符fd到实例。	

​				　EPOLL_CTL_MOD 　调整一个fd 的 evebt.

​     			       EPOLL_CTL_DEL     删除一个fd.

​	event:   fd对应的相关参数。

```c
typedef union epoll_data {
    void        *ptr;
    int          fd;
    uint32_t     u32;
    uint64_t     u64;
} epoll_data_t;

struct epoll_event {
    uint32_t     events;      /* 要监测的事件，位图 */
    epoll_data_t data;        /* 返回的相关标志 */
};
/*
events可以是以下几个宏的集合：
!!EPOLLIN ：表示对应的文件描述符可以读（包括对端SOCKET正常关闭）；
!!EPOLLOUT：表示对应的文件描述符可以写；
EPOLLPRI：表示对应的文件描述符有紧急的数据可读（这里应该表示有带外数据到来）；
EPOLLERR：表示对应的文件描述符发生错误；
EPOLLHUP：表示对应的文件描述符被挂断；
EPOLLET： 将EPOLL设为边缘触发(Edge Triggered)模式，这是相对于水平触发(Level Triggered)来说的。
EPOLLONESHOT：只监听一次事件，当监听完这次事件之后，如果还需要继续监听这个socket的话，需要再次把这个socket加入到EPOLL队列里
*/
```

返回值：成功：０。失败：－１。设置errno.



#### **epoll_wait**

epoll_wait()  等待有fd 的event 发生变化。

```c
#include <sys/epoll.h>
int epoll_wait(int epfd, struct epoll_event *events,　int maxevents, int timeout);
```

参数: 

​	epfd:　epoll实例描述符。

​	event：回填的事件。可按位与判断相应位是否置１．

​	maxevents:

​	timeout:等待时间。<0阻塞，＝０不等待；>0 等待时间。

返回值：返回需要处理的事件数目，如返回0表示已超时。

**epolll**实例

```c
int ret;
int epfd;
struct epoll_event evt, revt;

epfd=epoll_create(1);
if(epfd==-1)
{
	perror("epll_create()");
	goto ERROR;
}　
evt.data.fd=fd1;
evt.events=0;
epoll_ctl(epfd,EPOLL_CTL_ADD,fd1, &evt);//将ｆｄ添加到实例
evt.events |=EPOLLIN;　　　
evt.events |=EPOLLOUT;

ret=epoll_wait(epfd,&revt,1,-1);  //等待事件到来。
if(ret==-1)
{
      if(errno==EINTR)
             continue;
         goto ERROR;
 }
//判断是哪个事件
if(revt.events & EPOLLIN ||revt.events & EPOLLOUT)
	  op;	

```



### **fcntl**



### 状态机

**poll版本**

```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>
#include <string.h>
#include <errno.h>
#include <sys/select.h>
#include <poll.h>

#define BUFSIZE	100
#define TTY1	"/dev/tty8"
#define TTY2	"/dev/tty9"

enum {
	FSM_R,
	FSM_W,
	FSM_E,
	FSM_T
};

// 状态机数据类型抽象
struct fsm_st {
	int rfd;
	int wfd;
	int status;
	char buf[BUFSIZE];
	int cnt;
	int pos;
	char *errmsg;
};

// 推动状态机的运行
static int fsm_drive(struct fsm_st *fsm)
{
	int ret;

	switch (fsm->status) 
    {
		case FSM_R:
			fsm->cnt = read(fsm->rfd, fsm->buf, BUFSIZE);
			if (fsm->cnt == -1) 
			{
				if (errno != EAGAIN)
				 {
					fsm->status = FSM_E;
					fsm->errmsg = "read()";
				}
			}
			else if (fsm->cnt == 0) 
			{
				fsm->status = FSM_T;
			} else 
			{
				fsm->pos = 0;
				fsm->status = FSM_W;	
			}
			break;
		case FSM_W:
			ret = write(fsm->wfd, fsm->buf+fsm->pos, fsm->cnt);
			if (ret == -1) 
			{
				if (errno != EAGAIN)
				 {
					fsm->errmsg = "write()";
					fsm->status = FSM_E;
				}
			} else 
			{
				fsm->cnt -= ret;
				if (fsm->cnt > 0) 
				{
					fsm->pos += ret;
				} 
                else 
				{
					// 写完了
					memset(fsm->buf, '\0', BUFSIZE);
					fsm->status = FSM_R;
				}
			}
			break;
		case FSM_E:
			perror(fsm->errmsg);
			fsm->status = FSM_T;
			break;
		case FSM_T:
			abort();
			break;
	}
}

static int max2num(int fd1,int fd2)
{
	return fd1>fd2 ? fd1 : fd2;
}

// 构建两个状态机
static void create_fsm(int fd1, int fd2)
{
	struct fsm_st fsm12, fsm21;
	int status_fd1, status_fd2;
	int ret;
	struct pollfd pfd[2]={};

	// 获取之前的状态
	status_fd1 = fcntl(fd1, F_GETFL);
	// 加上非阻塞选项
	fcntl(fd1, F_SETFL, status_fd1 | O_NONBLOCK);
	status_fd2 = fcntl(fd2, F_GETFL);
	fcntl(fd2, F_SETFL, status_fd2 | O_NONBLOCK);

	fsm12.rfd = fd1;
	fsm12.wfd = fd2;
	memset(fsm12.buf, '\0', BUFSIZE);
	fsm12.cnt = 0;
	fsm12.pos = 0;
	fsm12.errmsg = NULL;
	fsm12.status = FSM_R;

	fsm21.rfd = fd2;
	fsm21.wfd = fd1;
	memset(fsm21.buf, '\0', BUFSIZE);
	fsm21.cnt = 0;
	fsm21.pos = 0;
	fsm21.errmsg = NULL;
	fsm21.status = FSM_R;

    pfd[0].fd=fd1;
    pfd[1].fd=fd2;

	while (fsm12.status != FSM_T || fsm21.status != FSM_T) 
	{
		if (fsm12.status == FSM_E)
		{
			fsm_drive(&fsm12);
			continue;
		}
		if (fsm21.status == FSM_E)
		{
			fsm_drive(&fsm21);
			continue;
		}
        
        pfd[0].events=0;
        pfd[0].revents=0;
        pfd[1].events=0;
        pfd[1].revents=0;
        
        if(fsm12.status==FSM_R)
        {
            pfd[0].events|=POLLIN;
        }
        else if(fsm12.status==FSM_W)
        {
            pfd[1].events|=POLLOUT;/* code */
        }

        if(fsm21.status==FSM_R)
        {
            pfd[1].events|=POLLIN;
        }
        else if(fsm21.status==FSM_W)
        {
            pfd[0].events|=POLLOUT;/* code */
        }
    
        while(1)
        {
            ret=poll(pfd,2,-1);
            if(ret==-1)
            {
                if(errno==EINTR)
                    continue;
                goto ERROR;
            }
            break;

        }


		if(fsm12.status!=FSM_T&&(pfd[0].revents & POLLIN || pfd[1].revents & POLLOUT))
			fsm_drive(&fsm12);
		if(fsm21.status!=FSM_T&&(pfd[0].revents & POLLOUT || pfd[1].revents &POLLIN ))
			fsm_drive(&fsm21);
		
	}
ERROR:
	fcntl(fd1, F_SETFL, status_fd1);
	fcntl(fd2, F_SETFL, status_fd2);
}

int main(void)
{
	int fd1, fd2;

	fd1 = open(TTY1, O_RDWR | O_NONBLOCK);
	if (fd1 == -1) {
		perror("open()");
		exit(1);
	}

	write(fd1, "[tty8]", 6);

	fd2 = open(TTY2, O_RDWR);
	if (fd2 == -1) {
		perror("open()");
		close(fd1);
		exit(1);
	}

	write(fd2, "[tty9]", 6);

	create_fsm(fd1, fd2);

	exit(0);
}

```



# 进程



## 进程状态

```
进程状态:
 　　1、S 可终止的睡眠
	２、R 运行
	３、Z 退出状态，进程成为僵尸进程
	４、D 不可终止睡眠
    ５、T 暂停状态或跟踪状态
	６、X 退出状态，进程即将被销毁。
进程优先级:
	+  前台
	s  会话的组长
	l 
	< 高优先级
查看进程状态
	ps axj
		PPID PID PGID PSID
	   aux
	   	USER PID STAT TTY
```
​	进程:内核调度的最小单位,资源分配的最小单位	
​	进程标识:
​		pid_t ---> 0 内核调用进程
​				   1 init进程
​		getpid(2);
​		getppid(2);



### 一、进程创建

子进程与父进程的　PID . PPID 不同

函数：vfork

```c
pid_t vfork(void)
```

头文件：

```c
#include <sys/types.h>
#include <unistd.h>
```

返回值：子进程的PID

功能：创建子进程后，父进程进入阻塞状态， 等待子进程终止后，父进程才会继续执行；

​	   创建的子进程与父进程使用同一页表，子或父进程中的数据改变，相应的子或父进程中的数据也会改变。

函数：fork

```c
pid_t fork(void)
```

头文件：

```c
#include <sys/types.h>
#include <unistd.h>
```

返回值：子进程的PID

功能：子进程复制父进程(缓存区，虚拟地址空间，进程表项....)；

应用：多应用于shell 、网络模型

​	 

### 二、exec族函数（进程内容替换）

函数：execl

```c
int execl(const char *path, const char *arg, ... /* (char  *) NULL */)
```

参数：path 要替换程序路径和文件名

​	   arg 参数选项    ...........NULL    必须以NULL结尾

![1553047633728](/home/lc/.config/Typora/typora-user-images/1553047633728.png)



### 三、进程终止

![1553046434487](/home/lc/.config/Typora/typora-user-images/1553046434487.png)

函数：exit

```c
 void exit(int status)
```

头文件：

```c
 #include <stdlib.h>
```

参数：status子进程的返回值 。

功能：终止进程，使用此函数，终止时会调用标准IO清理程序、终止处理程序。



函数：_exit

```c
 void _exit(int status)
```

头文件：

```c
#include <unistd.h>
```

参数：status子进程的返回值 。

功能：终止进程，使用此函数，直接返回内核，不会会调用标准IO清理程序、终止处理程序。

![1553047270109](/home/lc/.config/Typora/typora-user-images/1553047270109.png)

### 四、注册终止处理程序

函数：atexit

```c
int atexit(void (*function)(void));
```

头文件：

```c
#include <stdlib.h>
```

参数：终止处理程序的函数指针

返回值 ：成功返回0，失败返回非0。

功能：



### 五、收尸函数

函数：wait

```c
 pid_t wait(int *status)
```

头文件：

```c
#include <sys/types.h>
#include <sys/wait.h>
```

参数：status子进程的返回值回填，需使用宏 WEXITSTATUS() 获取

返回值 ：子进程的PID

功能：为任意子进程收尸，子进程不结束，父进程一直等待。



函数：waitpid

```c
pid_t waitpid(pid_t pid, int *status, int options)
```

头文件：

```c
#include <sys/types.h>
#include <sys/wait.h>
```

参数：

PID：

1. pid>0时，只等待进程ID等于pid的子进程，不管其它已经有多少子进程运行结束退出了，只要指定的子进程还没有结束,waitpid就会一直等下去。
2. pid=-1时，等待任何一个子进程退出，没有任何限制，此时waitpid和wait的作用一模一样。 　
3. pid=0时，等待同一个进程组中的任何子进程，如果子进程已经加入了别的进程组，waitpid不会对它做任何理睬。
4. pid<-1时，等待一个指定进程组中的任何子进程，这个进程组的ID等于pid的绝对值。
5. options: options提供了一些额外的选项来控制waitpid，目前在Linux中只支持WNOHANG和WUNTRACED两个选项，这是两个常数，可以用"|"运算符把它们连接起来

status: 子进程的返回值回填，需使用宏 WEXITSTATUS() 获取

返回值 ：为指定子进程的PID

功能：子进程收尸。

waitpid与wait区别: 

1.waitpid等待特定的子进程, 而wait则返回任一终止状态的子进程; 

2.waitpid提供了一个wait的非阻塞版本。



## 进程关系

![1553085873338](/home/lc/.config/Typora/typora-user-images/1553085873338.png)

### 一、进程

函数：getpid

```c
 pid_t getpid()   
 pid_t getppid()  
```

头文件：

```c
  #include <sys/types.h>
  #include <unistd.h>
```

返回值 ：PID

功能：getpid 获取调用进程的PID,

​	   getppid获取调用进程的父进程的PID



### 二、进程组

进程组ID为每个进程组组长的ID

函数：getpgid 、getpgrp

```c
 #include <sys/types.h>
 pid_t getpgid(pid_t pid)
 pid_t getpgrp(void)    
```

返回值 ：进程组ID

参数：getpgid 的参数pid=0时，与getpgrp的功能相同，获取调用进程的进程组ID     getpgid(0)=getpgrp()。

功能：getpid 获取调用进程的PID,

​	   getppid获取调用进程的父进程的PID



函数：setpgid

```c
 #include <unistd.h>
 int  setpgid(pid_t pid，pid_t pgid)
```

返回值 ：成功 0 ；出错 -1。

参数：pid 要被设置进程组的 “进程ID”，pgid 要被设置的进程组  “组ID”。

功能：将pid进程的进程组ID 设置为pgid，

1. pid=pgid, pid指定的进程作为进程组组长。
2. pid=0,则使用调用者的ID。
3. gpid=0, 则由pid 指定的进程作为进程组ID



### 三、会话

函数：getsid

```c
 #include <unistd.h>
 pid_t getdid(pid_t pid)
```

返回值 ：getsid :成功 返回会话首进程的进程组ID，出错,返回 -1。

功能：pid为０时返回调用进程的会话首进程；pid 与调用者不在同一会话，不能返回。



函数：setsid

```c
 #include <unistd.h>
 pid_t setid(void)
```

返回值 ：setsid: 成功返回 进程组ＩＤ，  出错　-1.

参数：

功能：如果调用此函数的进程不是一个进程组的组长，则创建一个新会话，以该进程为会话首进程。

​	　如果调用此函数的进程是一个进程组的组长，则出错返回。

当进程是会话的领头进程时setsid()调用失败并返回（-1）。
setsid()调用成功后，返回新的会话的ID，调用setsid函数的进程成为新的会话的领头进程，并与其父进程的会话组和进程组脱离。
由于会话对控制终端的独占性，进程同时与控制终端脱离。





## 守护进程

### 一、守护进程

必要条件：

​	pid == pgid == sid 脱离控制终端---->setsid(2) 非进程组组长

非必要条件：

　　umask(0);

​	chdir("/");

　　文件描述符　0, 1, 2 　重定向到 "/dev/null"

​	

#### daemon(2)

函数：daemon

```c
 #include <unistd.h>
 int daemon(int nochdir, int noclose);
```

返回值 ：成功 返回０，出错,返回 -1,**设置errno**.

参数：nochdir=0时，进工作目录变为　‘ / ’，否则不变。

​	   noclose=0时，文件描述符　０　１　２　重定向到“/dev/null”

功能：创建守护进程，使一个进程在后台运行，并关闭其父进程。



### 二、日志文件

**日志文件路径：/var/log/syslog**

守护进程无法将错误信息输出到终端。所以要写到日志文件中。

普通用户没有写日志文件的权限，执行写日志文件的进程是需用 root 用户权限。

函数：openlog

```c
  #include <syslog.h>
  void openlog(const char *ident, int option, int facility);
```

参数：ident 将加至每则日志消息中。ident一般为进程的名称。ident=NULL，默认为进程名。

功能：openlog是可选择的，不调用的话，则在第一次调用syslog时调用openlog。守护进程无法将错误信息输出到终端。所以要写到日志文件中。

​	 option: 常用　LOG_PERROR | LOG_PID

![1553090227359](/home/lc/.config/Typora/typora-user-images/1553090227359.png)



facility：守护进程调用openlog时　facility=LOG_DAEMON

![1553090767222](/home/lc/.config/Typora/typora-user-images/1553090767222.png)





#### syslog( , , ...)

函数：syslog  将信息提交到日志文件

```c
  #include <syslog.h>
  void syslog(int priority, const char *format, ...)
```

参数：priority：

![1553091115639](/home/lc/.config/Typora/typora-user-images/1553091115639.png)

format、"...."　格式化字符串，类似printf("%s",s)

实例：

```c
 syslog(LOG_ERR,"foped():%s",strerror(errno));
```



函数：closelog 　关闭打开的日志文件

```c
void closelog(void);
```

可不调用。





### 三、单实例守护进程

单实例守护进程：文件和记录锁提供互斥机制，使同一个进程，在同一时刻只能运行一次，该进程终止时，锁会自动删除。

#### lockf

```c
   #include <unistd.h>
   int lockf(int fd, int cmd, off_t len)
```

参数：

fd: 是打开文件的文件描述符，存放锁的文件，　"/var/run/daemon.pid"　。为通过此[函数调用](https://baike.baidu.com/item/%E5%87%BD%E6%95%B0%E8%B0%83%E7%94%A8)建立锁定，[文件描述符](https://baike.baidu.com/item/%E6%96%87%E4%BB%B6%E6%8F%8F%E8%BF%B0%E7%AC%A6)必须使用只写权限（O_WRONLY）或读写权限（O_RDWR）打开。

cmd:

1. F_ULOCK 0 //解锁
2. F_LOCK 1 //互斥锁定
3. F_TLOCK 2 //测试互斥锁定   锁定返回－１
4. F_TEST 3 //测试

len:  是要锁定或解锁的连续字节数,如果 len 为零，则锁定从当前偏移量到文件结尾的区域.

返回值：成功　０；　失败　－１，　设置errno。

功能：openlog是可选择的，不调用的话，则在第一次调用syslog时调用openlog。守护进程无法将错误信息输出到终端。所以要写到日志文件中。

实例

```c
//单实例进程
static int single_instance(void)
{
	int fd;
	char buf[BUFSIZE] = {};

	fd = open(DAEMON_FILE, O_RDWR | O_CREAT, 0666);
	if (fd < 0) {
		syslog(LOG_ERR, "open():%s", strerror(errno));			
		return -1;
	}
//判断是否上锁
	if (lockf(fd, F_TLOCK, 0) < 0) {
		if (errno == EACCES || errno == EAGAIN) {
			// 已被占用
			close(fd);
			return -1;
		}
        //写日志
		syslog(LOG_ERR, "lockf():%s", strerror(errno));
		exit(1);
	}
	//截断文件为0
	ftruncate(fd, 0);	
	snprintf(buf, BUFSIZE, "%d", getpid());
	write(fd, buf, strlen(buf));	

	// close(fd);
}
```



## 信号

### 一、信号概述

​	信号(signal)，又称为软中断信号，用来通知进程发生了什么异步事件。进程间可以互相发送信号，内核也可以因为内部事件给进程发送信号。   信号仅仅是同时进程发生了什么事件，并不向进程传递任何数据。

#### 信号分类：

​	标准信号(1  - 31)       有默认行为，会丢失

​	实时信号(34 - 64)     没有默认行为，排队的，不会丢失。

标准默认信号行为：

1.    Term   Default action is to terminate the process.                                                   **默认终止进程**。
2.    Ign      Default action is to ignore the signal.                                                           **默认忽略信号**。
3.    Core   Default action is to terminate the process and  dump  core  (see core(5)). **默认终止进程**，**产生**core文件。

4.    Stop   Default action is to stop the process.                                                            **默认停止进程**。

5.    Cont   Default  action  is  to  continue the process if it is currently stopped.           **继续运行当前停止的进程**。

常用的信号：在头文件 <signal.h>中



​         Signal     Value     Action   Comment
​       ──────────────────────────────────────────────────────────────────────
​       SIGHUP         1       Term    Hangup detected on controlling terminal
​                                     or death of controlling process
​       SIGINT           2       Term    Interrupt from keyboard         【Ctrl +c】
​       SIGQUIT        3       Core    Quit from keyboard                【Ctrl +\ 】
​       SIGILL            4       Core    Illegal Instruction       
​       SIGABRT       6       Core    Abort signal from abort(3)    
​       SIGFPE         8       Core    Floating point exception
​       SIGKILL         9       Term    Kill signal
​       SIGSEGV      11       Core    Invalid memory reference
​       SIGPIPE       13       Term    Broken pipe: write to pipe with no  readers
​       SIGALRM      14       Term    Timer signal from alarm(2)
​       SIGTERM      15       Term    Termination signal
​       SIGUSR1    30,10,16    Term    User-defined signal 1
​       SIGUSR2    31,12,17    Term    User-defined signal 2
​       SIGCHLD    20,17,18    Ign     Child stopped or terminated
​       SIGCONT   19,18,25    Cont    Continue if stopped
​       SIGSTOP   17,19,23    Stop    Stop process
​       SIGTSTP    18,20,24    Stop    Stop typed at terminal
​       SIGTTIN     21,21,26    Stop    Terminal input for background process
​       SIGTTOU   22,22,27    Stop    Terminal output for background process

![1553320486925](/home/lc/.config/Typora/typora-user-images/1553320486925.png)

查看信号：

​	命令：kill -l   查看所有信号

​	man 7 signal

### 二、注册信号行为

#### siaction(3)

​	函数：sigaction 为制定信号注册新行为， 新行为相当于MCU中断处理函数。

```c
   #include <signal.h>
   int sigaction(int signum, const struct sigaction *act, struct sigaction *oldact);
```

参数：

signum：要改变行为的信号标号。

act :要注册的新信号行为。

oldact: 注册前的信号行为。

act 结构：

struct sigaction {
	void (*sa_handler)(int);     //一参数的信号处理函数
	void (*sa_sigaction)(int, siginfo_t *, void *);  两参数的信号处理函数
	sigset_t sa_mask;         //信号屏蔽集   ,指定信号函数执行期间要屏蔽的信号。
	int  sa_flags;               //指定信号的处理行为 ,可按位或使用。
	void (*sa_restorer)(void);   //无用
			};

sa_flags:

1.  SA_RESTART:使被信号打断的系统调用自动重新发起;

2.  SA_NOCLDSTOP:使父进程在它的子进程暂停或继续运行时不会收到SIGCHLD 信号;

3. ** SA_NOCLDWAIT:使父进程在它的子进程退出时不会收到 SIGCHLD 信号,这时子进程如果退出也不会成为僵尸程;**

4.  SA_NODEFER:使对信号的屏蔽无效,即在信号处理函数的执行期间仍能发出这个信号;

5.  SA_RESETHAND:信号被处理后重新设置处理方式到默认值;

6.  SA_SIGINFO:使用 sa_sigaction 成员而不是 sa_handler 作为信号处理函数。

   

返回值：成功 0， 出错-1；设置error.

### 三、信号产生

#### 	1、按键输入

#### 	2、函数

   函数：kill  向指定进程发送信号。

```c
   #include <sys/types.h>
   #include <signal.h>
   int kill(pid_t pid, int sig)
```

参数

pid:

pid>0: 发送给进程ＩＤ为ＰＩＤ的进程。

pid==0: 向与该进程同一进程组，且有权限发送的　所有进程发送信号。

pid<0  ：向与ＰＩＤ绝对值相等的进程组，且有权限的所有进程发送信号。

pid==-1: 向该进程有权限发送的所有进程发送信号。

sig: 要发送信号的进程ＩＤ。

返回值：成功　０；　失败　-1.  设置errno.



函数alarm: 一个进程只能有一个闹钟时间，如果在调用alarm之前已设置过闹钟时间，则任何以前的闹钟时间都被新值所代替。

```c
 #include <unistd.h>
 unsigned int alarm(unsigned int seconds);
```

参数: seconds 闹钟秒数。

返回值：成功返回０　或以前闹钟剩余秒数。



函数：setitimer 　比alarm功能强大，调用一次可一直定时。

```c
 #include <sys/timer.h>
 int setitimer(int which, const struct itimerval *new_value, struct itimerval *old_value)
```

参数: 

which:定时器方式。

**ITIMER_REAL**: 以系统真实的时间来计算，它送出SIGALRM信号。

**ITIMER_VIRTUAL**: -以该进程在[用户态](https://baike.baidu.com/item/%E7%94%A8%E6%88%B7%E6%80%81/9548791)下花费的时间来计算，它送出SIGVTALRM信号。

**ITIMER_PROF**: 以该进程在用户态下和内核态下所费的时间来计算，它送出SIGPROF信号。

new_value: 定时时间。

old_value:回填的旧的定时时间。

返回值：成功返回０；错误返回－１；设置erron。



函数：abort 　向调用进程发送SIGABRT信号。进程收到SIGABRT，会终止。　如果SIGABRT信号行为被注册，则执行完注册的函数后，进程仍会终止。 

```c
#include <stdlib.h>
void abort(void);
```

### 四、等待信号

#### pause()

函数：pause 　 等待信号到来。pause()会令目前的进程暂停(进入睡眠状态), 直到被信号(signal)所中断

```c
#include <unistd.h>
int pause(void);
```

返回：当有信号到来时只返回-1.

#### sigsuspend(1)

函数: sigsuspend 函数接受一个信号集指针，将信号屏蔽字设置为信号集中的值，在进程接收到一个信号之前，进程会挂起，当捕捉到一个信号，首先执行信号处理程序，然后从sigsuspend返回，最后将信号屏蔽字恢复为调用sigsuspend之前的值。

```c
 #include <signal.h>
 int sigsuspend(const sigset_t *mask);
```

返回值：总是返回-1，并将errno设置为EINTR（表示一个被中断的系统调用）。

### 五、信号屏蔽

添加前必须能初始化，置空或置满。

```c
#include <signal.h>
int sigemptyset(sigset_t *set);　　　//信号集置空
int sigfillset(sigset_t *set);　　　//信号集置满
int sigaddset(sigset_t *set, int signum);　//添加一个信号集
int sigdelset(sigset_t *set, int signum);　//删除一个信号集
int sigismember(const sigset_t *set, int signum);　//查找一个信号集
				以上均成功返回0 出错返回-1
```


sigpromask()

### 六、应用

​	流量控制

​		漏桶

​		令牌桶

​			令牌

​			速率

​			上限



满足下列条件的函数多数是不可重入的：

 


（1）函数体内使用了静态的数据结构；

（2）函数体内调用了malloc()或者free()函数；

（3）函数体内调用了标准I/O函数。



## 进程间通信

### 一、管道

​	管道是一个允许单向信息传递的通信设备。从管道“写入端”写入的数据可以从“读取端”读回。管道是一个串行设备;从管道中读取的数据总保持它们被写入时的顺序。一般来 说,管道通常用于一个进程中两个线程之间的通信,或用于父子进程之间的通信。

**匿名管道**：**匿名管道需要注意的问题**
1、读空管道，read调用会阻塞；写满管道，write调用会阻塞。
2、当管道所有读端关闭时，read调用会返回0
3、匿名管道通信，进程间必须有亲缘关系。

４、不能用lseek()



pipe(1)　创建管道文件

```c
 #include <unistd.h>
 int pipe(int pipefd[2]);
```

参数：pipefd[2],回填的管道读写端文件描述符。　pipe[0]读端，　pipe[1]写端。



不能用lseek()

匿名

### IPC

​	每个内核中的 I P C结构（消息队列、信号量或共享存储段）都用一个非负整数的标识符
( i d e n t i f i e r )加以引用。 

　　无论何时创建I P C结构（调用m s g g e t、 s e m g e t或s h m g e t） ,都应指定一个关键字（k e y），关
键字的数据类型由系统规定为 k e y _ t，通常在头文件< s y s / t y p e s . h >中被规定为长整型。关键字由
内核变换成标识符。

ftok(２):将文件路径和计划代号转换为System V IPC key

```c
#include <sys/types.h>
#include <sys/ipc.h>
key_t ftok(char *pathname, char proj)
```

参数：　pathname 文件，　项目ID，非0整数(只有低8位有效)

返回值：成功返回key_t ;　　错误：-1. 设置erron.



### 二、消息队列	

msgget(2) 获取System V 消息队列id.

```c
 #include <sys/types.h>
 #include <sys/ipc.h>
 #include <sys/msg.h>
 int msgget(key_t key, int msgflg);
```

参数：　key :信号识别码。

​	      msgflg：标识函数的行为：IPC_CREAT(创建)或IPC_EXCL(如果已经存在则返回失败)。

返回值：成功：消息队列id, 失败-1; 　　设置errno.

msgctl(3) 控制消息队列的运作

```c
#include <sys/types.h>
#include <sys/ipc.h>
#include <sys/msg.h>
int msgctl(int msqid, int cmd, struct msqid_ds *buf)
```

参数：　msqid　消息队列id

​		cmd:

​			IPC_RMID：删除由msqid指示的消息队列，将它从系统中删除并破坏相关数据结构。

​			IPC_STAT：将msqid相关的数据结构中各个元素的当前值存入到由buf指向的结构中。

​			IPC_SET：将msqid相关的数据结构中的元素设置为由buf指向的结构中的对应

​	    buf: 用来存放或更改消息队列的属性。  删除消息队列时填NULL.



msgrcv(5)：从消息队列读取信息。

```c
#include <sys/types.h>
#include <sys/ipc.h>
#include <sys/msg.h>
ssize_t msgrcv(int msqid, void *msgp, size_t msgsz, long msgtyp, int msgflg);
```

参数：　msqid:消息队列　id;

msgp: 存放消息

```c
	struct msgbuf 
		  {
           	long mtype;       /* 消息类型  > 0*/
           	char mtext[1];    /* 存放消息，变长结构体*/
    	   };
```
msgsz: 信息数据的长度，即mtext参数的长度。

msgtyp: 指定所要读取的信息种类。

​		msgtpy=0 返回队列内第一项信息。

​		msgtpy>0 返回队列　内第一项　msgtyp与mtype 相同的信息。

​		msgtpy<0 反回队列内第一项mtype小于或等于mtyoe绝对值的信息。

msgflg:

​	0：msgrcv调用阻塞直到接收消息成功为止。

​	MSG_NOERROR:若返回的消息字节数比nbytes字节数多,则消息就会截短到nbytes字节,且不通知消息发送进程。

​	IPC_NOWAIT:调用进程会立即返回。若没有收到消息则立即返回-1。

返回值：成功：读取消息的长度，失败：-1。设置errno。

实例

```c
   cnt=msgrcv(msg_id,&rcv_buf,sizeof(struct stu_t),0,0);
   if(cnt==-1)
   {
         if(errno==EINTR)
        {
           　continue;
    	}
    perror("msgrcv()");
    exit(1);
   }    /* code */
  printf("rcv type :%ld, date: %d %s\n", rcv_buf.mtype, rcv_buf.stu.id, rcv_buf.stu.name);
```



msgsnd(5)：将信息存入消息队列。

```c
#include <sys/types.h>
#include <sys/ipc.h>
#include <sys/msg.h>
int msgsnd(int msqid, const void *msgp, size_t msgsz, int msgflg);
```

参数：　msqid:消息队列　id;

msgp: 存放消息

```c
	struct msgbuf 
		  {
           	long mtype;       /* 消息类型  > 0*/
           	char mtext[1];    /* 存放消息，变长结构体*/
    	   };
```

msgsz 为信息数据长度，即mtext参数长度。

msgflg: 可设为IPC_NOWAIT,若队列已满，或有其他情况无法马上写入消息，则立即返回EAGAIN。

返回值：成功　０；错误：-1;　　 设置　errno.

### 三、信号量

semget(3) 创建一个新信号量或取一个已有信号量。

```c
#include <sys/types.h>
#include <sys/ipc.h>
#include <sys/sem.h>
int smeget(key_t key, int num_sems, int sem_flags)
```

参数：key 信号识别码，若为IPC_PRIVATE则建立新的信号队列

num_sems 信号集合的数目。

sem_flags:  取值IPC_CREAT 、IPC_EXCL 。也用来决定信号队列的存取权限。

返回值：成功信号队列识别代码；错误　－１；设置　errno. 

错误代码：。。。。。。

实例

```c
    semid=semget(IPC_PRIVATE,1,IPC_CREAT | 0600);
	if(semid==-1)
    {
        perror("semget()");
        exit(1);
    }
```



semctl(3)　得到一个信号量集标识符或创建一个信号量集对象并返回信号量集标识符

```c
#include <sys/types.h>
#include <sys/ipc.h>
#include <sys/sem.h>
int semctl(int semid, int semnum, int cmd, union semun arg)
```

参数：　

semid: 信号队列识别码。

semnum:信号量集数组上的下标，表示某一个信号量

cmd:

​	IPC_RMID:从内核中删除信号量集合.

​	SETVAL:用联合体中val成员的值设置信号量集合中单个信号量的值

返回值：成功：０；　错误：errno。

实例：

```c
    semctl(semid,0,SETVAL,1);
```

semop(3)  信号量操作函数

```c
#include <sys/types.h>
#include <sys/ipc.h>
#include <sys/sem.h>
int smeop(int semid, struct sembuf *sops, size_t nsops)
```

参数：　semid　信号队列识别码

​	　　sops       

```c
 struct sembuf
 {
   short int sem_num;   //信号量在信号量集合中的索引，第一个为0；
   short int sem_op;　　// －1 获取信号量，　１归还信号量
   short int sem_flg;　//操作标志，取值IPC_NOWAIT：对信号的操作不能满足时，semop()不会阻塞，并立即返回，同时设定错误信息。IPC_UNDO:程序结束时（无论正常或不正常）释放信号量，这样做的目的在于避免程序在异常情况下结束时未将锁定的资源解锁，造成资源永远锁定。
 }
```

nsops 代表　sop结构个数。

返回值：成功　０；错误　－１；　设置　errno

错误代码：。。。。。

实例：

```c
static int P(void)　　　　//取信号量
{
    struct sembuf buf;
    buf.sem_num=0;
    buf.sem_op=-1;
    buf.sem_flg=0;

    if(semop(semid,&buf,1)==-1)
    {
        perror("semop()");
        return -1;
    }
    return 0;
}

static int V(void)　//还信号量
{
    struct sembuf buf;
    buf.sem_num=0;
    buf.sem_flg=0;
    buf.sem_op=1;

    if(semop(semid,&buf,1)==-1)
    {
        perror("semop()V");
        return -1;
    }
    return 0;
}

```



### 四、共享内存

#### mmap(6)

mmap(6)  mmap是一种内存映射文件的方法，即将一个文件或者其它对象映射到进程的地址空间，实现文件磁盘地址和进程虚拟地址空间中一段虚拟地址的一一对映关系。

```c
#include <sys/mman.h>
void *mmap(void *addr, size_t length, int prot, int flags,int fd, off_t offset);
```

参数：　

​	**addr** :将要共享的内存地址，若为空，则函数自动分配，并返回。

​	**length** :共享的内存大小（字节）。

​	**prot** : 共享内存权限。

1. ​			PROT_EXEC //内容可以被执行

2. ​			PROT_READ //内容可以被读取

3. ​			PROT_WRITE //可以被写入

4. ​			PROT_NONE //不可访问

   **flags**: 指定对象的类型，选项和是否可以共享.

   ​	 MAP_SHARED: 内存共享。

   ​	 MAP_PRIVATE：内存私有。

   　　　MAP_ANONYMOUS：共享内存区不关联任何文件。

   **fd**: 有效的文件描述词。如果MAP_ANONYMOUS被设定，为了兼容问题，其值应为-1。

   **offset**: 文件偏移量。

   

返回值：成功：返回被映射区的指针。失败时，mmap()返回MAP_FAILED[其值为(void *)-1]。　设置errno。



munmap(2):清除共享内存。

```c
 #include <sys/mman.h>
 int munmap(void *addr, size_t length);
```

参数：addr:共享内存的地址。　length:共享内存字节个数。

返回值：成功：０；出错　－１；　设置erron。



#### shmat(3)

shmat(3) 把共享内存区对象映射到调用进程的地址空间

```c
 #include <sys/ipc.h>
 #include <sys/shm.h>
void *shmat(int shmid, const void *shmaddr, int shmflg);
```

参数：shmid:  共享内存标识符。

​	　shmaddr: 指定共享内存出现在进程内存地址的什么位置，直接指定为NULL让内核自己决定一个合适的地址位置.

​	   shmflg:SHM_RDONLY：为只读模式，其他为读写模式.

返回值：成功：共享内存地址。

实例

```c
void *p=shmat(shm_id,NULL,0);
memcpy(p,"plan",4);
```



**shmdt** 断开共享内存连接

```c
 #include <sys/ipc.h>
 #include <sys/shm.h>
　int shmdt(const void *shmaddr);
```

参数：shmaddr: 共享内存地址.

返回值：成功：０，失败　－１。　设置error.



# 线程

多线程：同一个进程的　**多个子函数**　可以在　**同一时刻**同时运行。

多ＣＰＵ:

UCOS的多任务，单个CPU上多线程，但是各线程不是同时运行。（LC个人见解）

ps axm  -L　查看线程

**在编译 Pthread 程序时在编译和链接过程中需要加上-pthread 参数:**

```
LDFLAGS += -pthread
```



## 线程属性

```c
typedef struct
{ 
   int                   detachstate;   //线程的分离状态
   int                   schedpolicy;   //线程调度策略
   structsched_param     schedparam;    //线程的调度参数
   int                   inheritsched;  //线程的继承性
   int                   scope;         //线程的作用域
   size_t                guardsize;     //线程栈末尾的警戒缓冲区大小
   int                   stackaddr_set; //线程的栈设置
   void*                 stackaddr;     //线程栈的位置
   size_t                stacksize;     //线程栈的大小
}pthread_attr_t;
```



## 线程安全

### **不安全的操作**

- a, 函数中访问全局变量和堆。
- b, 函数中分配，重新分配释放全局资源。
- c, 函数中通过句柄和指针的不直接访问。
- d, 函数中使用了其他线程不安全的函数或者变量。

　　　　								**非线程安全函数**

![1553734311871](/home/lc/.config/Typora/typora-user-images/1553734311871.png)



## 创建线程

#### pthread

pthread_create(4)　创建线程。

```c
#include <pthread.h>
int pthread_create(pthread_t *thread, const pthread_attr_t *attr, 
                   void *(*start_routine)(void *), void *arg);
```

参数：thread: 新创建的线程标识符。

​	　atttr: 线程属性。

​	　void *(*start_routine)(void *)：线程对应的函数。

​	   arg: 线程对应函数的返回值。

返回值：成功：０；　出错：返回erron的值。**不设置erron** 。



#### pthread_exit(1)

线程终止

```c
#include <pthread.h>
void pthread_exit(void *retval);
```

retval:　线程返回值。



## 线程终止

１、从启动 例程返回，返回值是例程的退出码。----> return .

２、线程被同一进程中的其他线程取消。 

３、线程调用pthread_exit。



## 线程收尸

任意线程都可为已知线程标示的其他线程收尸

#### **pthread_join(２)**

pthread_join(２): 线程收尸。

```c
#include <pthread.h>
int pthread_join(pthread_t thread, void **retval);
```

参数：thread 线程描述符。

​	　retval:线程返回值。





## 线程取消

whether 
			pthread_setcancelstate(3) --->enable(default) / diabled
		when
			asyn / delay(default, 到取消点函数)
		发送取消请求	
			pthread_cancel(3);
		设置取消点
			pthread_testcancel(3);

## 线程清理

pthread_cleanup_push(3);
		pthread_cleanup_pop(3);
		when:
			<1>线程被取消
			<2>pthread_cleanup_pop(not zero);
			<3>pthread_exit(3);

## 线程同步

**多线程调用同一函数问题**：

​	**同一进程或不同进程多个线程**中的函数的**局部变量**由于是保存在不同的线程中，因此不需要进行互斥处理。

​	**需要互斥处理**的，一般是函数中有**全局变量，有动态申请的空间，有静态局部变量，有需要进程数据循环发送**之类的操作需要进行互斥处理。

### 互斥量

​	互斥量(Mutex),又称为互斥锁,是一种用来保护临界区的特殊变量,它可以处于锁
定(locked)状态,也可以处于解锁(unlocked)状态:
​	　 如果互斥锁是锁定的,就是某个特定的线程正持有这个互斥锁;
​	 　如果没有线程持有这个互斥锁,那么这个互斥锁就处于解锁状态。
​	每个互斥锁内部有一个线程等待队列,用来保存等待该互斥锁的线程。当互斥锁处于解
​	锁状态时,如果某个线程试图获取这个互斥锁,那么这个线程就可以得到这个互斥锁而不会
​	阻塞;当互斥锁处于锁定状态时,如果某个线程试图获取这个互斥锁,那么这个线程将阻塞

​	在互斥锁的等待队列内。
​	互斥量是最简单也是最有效的线程同步机制。程序可以用它来保护临界区,以获得对排
它性资源的访问权。另外,互斥量只能被短时间地持有,使用完临界资源后应立即释放锁。

**创建互斥量**：

静态初始化：	pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;

动态初始化：     　可改变默认属性

#### **thread_mutex_init**

```c
#include <pthread.h>
//静态初始化
pthread_mutex_t fastmutex = PTHREAD_MUTEX_INITIALIZER;　　　//默认属性
pthread_mutex_t recmutex = PTHREAD_RECURSIVE_MUTEX_INITIALIZER_NP;
pthread_mutex_t   errchkmutex   =   PTHREAD_ERRORCHECK_MUTEX_INITIAL‐IZER_NP;
//动态初始化
int  pthread_mutex_init(pthread_mutex_t  *mutex, const pthread_mutex‐attr_t *mutexattr);
```

参数：mutex:互斥量。

​	　mutexattr: 互斥量属性.

返回值：成功：０　；错误：errno.



加锁：

#### **pthread_mutex_lock**()

```c
#include <pthread.h>
int pthread_mutex_lock(pthread_mutex_t *mutex);// pthread_mutex_lock()函数会一直阻塞到互斥量可用为止。
int pthread_mutex_trylock(pthread_mutex_t *mutex);//尝试加锁,但通常会立即返回。
```



解锁:

#### pthread_mutex_unlock(1)

```c
#include <pthread.h>
int pthread_mutex_unlock(pthread_mutex_t *mutex);
```

是线程将互斥量由锁定状态变为解锁状态。
pthread_mutex_unlock()函数用来释放指定的互斥量。

销毁锁：

#### pthread_mutex_destroy(1)

```c
int pthread_mutex_destroy(pthread_mutex_t *mutex);
```


参数 mutex 指向要销毁的互斥量。

**实例：**

```c
pthread_mutex_t mylock = PTHREAD_MUTEX_INITIALIZER;
pthread_mutex_lock(&mylock);
/*临界区代码*/
pthread_mutex_unlock(&mylock);
pthread_mutex_destroy(&mylock);
```



### 条件变量

初始化：

#### pthread_cond_init 

```c
#include <pthread.h>
pthread_cond_t cond = PTHREAD_COND_INITIALIZER;   //静态初始化　默认属性
//动态初始化
int pthread_cond_init(pthread_cond_t   *cond,   pthread_condattr_t　*cond_attr);
int pthread_cond_wait(pthread_cond_t *cond, pthread_mutex_t *mutex);
```

**通知**

#### signal & **broadcast**

```c
#include <pthread.h>
int pthread_cond_signal(pthread_cond_t *cond); //唤醒一个在条件变量等待队列等待的线程
int pthread_cond_broadcast(pthread_cond_t *cond); // 通知所有在条件变量等待队列等待的线程。
```



**等待**

#### pthread_cond_wait

​	条件变量是与条件测试一起使用的,通常线程会对一个条件进行测试,如果条件不满足
就会调用条件等待函数来等待条件满足。

```c
#include <pthread.h>
int pthread_cond_wait(pthread_cond_t *restrict cond, pthread_mutex_t *restrict mutex);
int pthread_cond_timedwait(pthread_cond_t *restrict cond,pthread_mutex_t *restrict mutex, const struct timespec *restrict abstime);
```



## 线程和信号

新创建的线程继承创建者的信号屏蔽集。

### pthread_sigmask(3)　

为一个线程设置信号屏蔽集。

```c
 #include <signal.h>
 int pthread_sigmask(int how, const sigset_t *set, sigset_t *oldset);
```

**用法同sigprocmask(2)**

返回值：成功　０；　错误：erron。



### sigwait(3) 

线程等待信号集中的任信号到来。

```
#include <signal.h>
int sigwait(const sigset_t *set, int *sig);
```

参数：set 要等待的信号集。　

​	  sig: 回填接收到的信号。

### pthread_kill(3)

向指定线程发送信号。

```c
#include <signal.h>
int pthread_kill(pthread_t thread, int sig);
```

用法同：kill().

返回值：成功　０，错误：erron.

## 线程和fork()

pthread_atfork(3)

```c
#include <pthread.h>
int  pthread_atfork(void  (*prepare)(void),  void (*parent)(void), void (*child)(void));
```



## 线程和IO

pread(2)

pwrite(2);



int  pthread_once(pthread_once_t  *once_control,  void  (*init_routine)
       (void));





# 网络基础

网关:

子网掩码：

​	表示子网络特征的一个参数。它在形式上等同于IP地址，也是一个32位二进制数字，它的网络部分全部为1，主机部分全部为0。比如，IP地址172.16.10.1，如果已知网络部分是前24位，主机部分是后8位，那么子网络掩码就是11111111.11111111.11111111.00000000，写成十进制就是255.255.255.0。 

知道”子网掩码”，我们就能判断，任意两个IP地址是否处在同一个子网络。方法是将两个IP地址与子网掩码分别进行AND运算（两个数位都为1，运算结果为1，否则为0），然后比较结果是否相同，如果是的话，就表明它们在同一个子网络中，否则就不是。ｉｐ

内

外

端口

网络基础：<https://www.cnblogs.com/linhaifeng/articles/5937962.html>



### TCP三次握手

三次握手建立连接：**

**第一次握手：**客户端发送syn包(seq=x)到服务器，并进入SYN_SEND状态，等待服务器确认;

**第二次握手：**服务器收到syn包，必须确认客户的SYN(ack=x+1)，同时自己也发送一个SYN包(seq=y)，即SYN+ACK包，此时服务器进入SYN_RECV状态;

**第三次握手：**客户端收到服务器的SYN+ACK包，向服务器发送确认包ACK(ack=y+1)，此包发送完毕，客户端和服务器进入ESTABLISHED状态，完成三次握手。

握手过程中传送的包里不包含数据，三次握手完毕后，客户端与服务器才正式开始传送数据。理想状态下，TCP连接一旦建立，在通信双方中的任何一方主动关闭连接之前，TCP 连接都将被一直保持下去。

### TCP 四次挥手

![/home/lc/桌面/927356481.png](/home/lc/桌面/927356481.png)

# 网络编程

## socket

socket(3)创建套接字

```c
#include <sys/types.h>          /* See NOTES */
#include <sys/socket.h>
int socket(int domain, int type, int protocol);
```

参数:

**domain**: 通讯协议:

**AF_UNIX, AF_LOCAL**   Local communication              unix(7)     本地进程间通讯
**AF_INET**   	             IPv4 Internet protocols            ip(7)	 IPV4讯协议
**AF_INET6**       	       IPv6 Internet protocols            ipv6(7)      IPV6通讯协议

**type**:通讯类型:

 SOCK_STREAM :创建链接,可靠地,字节流.		   流式

 SOCK_DGRAM   :无连接不可靠.   				报式

**protocol**: 用来指定传输协议的编号,通常填写0.

返回值:成功: 套接字. 失败-1; 设置erron.

## bind

**bind(3)**的作用是将参数sockfd和addr绑定在一起，使sockfd这个用于网络通讯的文件描述符监听addr所描述的地址和端口号

```c
 #include <sys/types.h>          /* See NOTES */
 #include <sys/socket.h>
 int bind(int sockfd, const struct sockaddr *addr,　socklen_t addrlen);
```

参数:

sockfd: 创建好的套接字.

addr:

  struct sockaddr {
               sa_family_t sa_family;      //通讯协议
               char        sa_data[14];
           }

addrlen: addr结构体长度.

返回值: 成功:0.  错误:-1 .设置errno.

## connect

connet(3):创建流式socket与网络地址的链接, 链接建立成功前一直阻塞,直至链接成功或出错.

```c
#include <sys/types.h>          /* See NOTES */
#include <sys/socket.h>
int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
```

参数: 

sockfd: 以创建好的流式套接字;

addr:要连接的网络地址.

addrlen: 地址长度.

## listen

listen(2):设置套接字为监听模式.

```c
 #include <sys/types.h>          /* See NOTES */
 #include <sys/socket.h>
 int listen(int sockfd, int backlog);
```

socket: 要设置的套接字

backlog: 套接字最大连接个数.

返回值: 成功返回0, 失败 -1, 设置errno.

## accept

accept(2) 连接一个请求连接的套接字. 并返回一个新的套接字.

```c
 #include <sys/types.h>          /* See NOTES */
 #include <sys/socket.h>
 int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
```

参数: 

返回值:成功:返回新的套接字,用于之后的数据传输, 失败: -1; 设置errno.

## 字节序

多字节传输时需注意大小端问题。

**端口号需进行字节序转换。**

```c
 #include <arpa/inet.h>
 uint32_t htonl(uint32_t hostlong);
 uint16_t htons(uint16_t hostshort);
 uint32_t ntohl(uint32_t netlong);
 uint16_t ntohs(uint16_t netshort);
```

## setsockopt

setsockopt(4):设置套接字属性

```c
 #include <sys/types.h>          /* See NOTES */
 #include <sys/socket.h>
 int getsockopt(int sockfd, int level, int optname, void *optval, socklen_t *optlen);
 int setsockopt(int sockfd, int level, int optname, const void *optval, socklen_t optlen);
```

参数:

sockfd:已创建好的套接字.

level: 将要设置的网络层, 一般选SOL_SOCKET socket层, 或 IPPROTO_IP 层.

optname: 

​	socket层选项

​		SO_BROADCAST:  使用广播方式传送.

​		SO_REUSEADDR 允许本地地址多次bind().

​	IP层选项:

​		IP_ADD_MEMBERSHIP :加入多播组.

实例:加入多播组.

```c
struct ip_mreqn mreq;
char buf[MAX_LENGTH] = {};
int len;
socklen_t addr_len;

mreq.imr_multiaddr.s_addr=inet_addr(MULTI_ADDR);	//MULIT_ADDR 多播组IP宏定义
mreq.imr_address.s_addr=inet_aton("10.0.19.115", &local_addr.sin_addr);
mreq.imr_ifindex=if_nametoindex("enp2s0");			//网卡名转换为索引号
len=sizeof(struct ip_mreqn);
//加入多播组
setsockopt(sd, IPPROTO_IP, IP_ADD_MEMBERSHIP,&mreq, len);
```

optval:欲设值的值.

oplen: optval 的长度.

返回值: 成功 0, 失败-1. 设置errno.

## 字节对齐

不同的编译器有不同的对齐方式

```c
		struct {
		
		}__attribute__((packed));   //不进行字节对齐
		
		#pragma pack(1)    //指定字节对齐  
```

domain:
	AF_INET --->ipv4 
		地址类型 man 7 ip
	AF_UNIX / AF_LOCAL --->本地
		地址类型 man 7 unix
type: 

## UDP

​	SOCK_DGRAM
​		无连接的，不可靠 (效率高)
​		

## TCP

​	主动端(先发)		    	      被动端(先收)--->先运行
​	socket(2);				       socket(2);
​	//bind(2);				         bind(2);
​	sendto(2); / recvfrom(2)		 recvfrom(); / sendto();
​	close(2);					 close(2);

##  组播/多播

​		接收---》加入多播组
​		setsockopt(2);
​		IPPROTO_IP(IP层) 	IP_ADD_MEMBERSHIP / IP_DROP_MEMBERSHIP(加入/退出多播组) (man 7 ip)



## 广播

​		setsockopt(2);
​			SOL_SOCKET (SOCKET层)	SO_BROADCAST (广播方式)  (man 7 socket)					

​	SOCK_STREAM
​		创建连接的，可靠有序

​		socket(2);			socket(2);
​		// bind(2);			bind(2);
​							listen(2);
​		connect(2);(创建)			accept(2);
​		send(2); / write(2);		recv(2) / read(2);
​		close(2);			close(2);

​		三次握手
​		四次挥手 

​		动态进程池--->服务器



# 数据库



# Linux C函数库参考手册



目录

第1章 字符测试函数

isalnum(测试字符是否为英文字母或数字)

isalpha(测试字符是否为英文字母)

isascii(测试字符是否为ascii码字符)

isblank(测试字符是否为空格字符)

iscntrl(测试字符是否为ascii码的控制字符)

isdigit(测试字符是否为阿拉伯数字)

isgraph(测试字符是否为可打印字符)

islower(测试字符是否为小写英文字母)

isprint(测试字符是否为可打印字符)

isspace(测试字符是否为空格字符)

ispunct(测试字符是否为标点符号或特殊符号)

isupper(测试字符是否为大写英文字母)

isxdigit(测试字符是否为16进制数字)

第2章 数据转换函数

atof(将字符串转换成浮点型数)

atoi(将字符串转换成整型数)

atol(将字符串转换成长整型数)

ecvt(将浮点型数转换成字符串，取四舍五入)

fcvt(将浮点型数转换为字符串，取四舍五入)

 

.gcvt(将浮点型数转换为字符串，取四舍五入)

strtod(将字符串转换成浮点型数)

strtol(将字符串转换成长整型数)

strtoul(将字符串转换成无符号长整型数)

toascii(将整型数转换成合法的ascii码字符)

tolower(将大写字母转换成小写字母)

toupper(将小写字母转换成大写字母)

第3章 内存配置函数

alloca(配置内存空间)

brk(改变数据字节的范围)

calloc(配置内存空间)

free(释放原先配置的内存)

getpagesize(取得内存分页大小)

malloc(配置内存空间)

mmap(建立内存映射)

munmap(解除内存映射)

realloc(更改己配置的内存空间)

sbrk(增加程序可用的数据空间)

第4章 时间函数

asctime(将时间和日期以字符串格式表示)

clock(取得进程占用cpu的大约时间)

ctime(将时间和日期以字符串格式表示)

difftime(计算时间差距)

ftime(取得目前的时间和日期)

gettimeofday(取得目前的时间)

gmtime(取得目前的时间和日期)

localtime(取得当地目前的时间和日期)

mktime(将时间结构数据转换成经过的秒数)

settimeofday(设置目前的时间)

strftime(格式化日期和时间)

time(取得目前的时间)

tzset(设置时区以供时间转换)

第5章 字符串处理函数

bcmp(比较内存内容)

bcopy(拷贝内存内容)

bzero(将一段内存内容全清为零)

ffs(在一整型数中查找第一个值为真的位)

index(查找字符串中第一个出现的指定字符)

memccpy(拷贝内存内容)

memchr(在某一内存范围中查找一特定字符)

memcmp(比较内存内容)

memcpy(拷贝内存内容)

memfrob(对内存区域编码)

memmove(拷贝内存内容)

memset(将一段内存空间填入某值)

rindex(查找字符串中最后一个出现的指定字符)

strcasecmp(忽略大小写比较字符串)

strcat(连接两字符串)

strchr(查找字符串中第一个出现的指定字符)

strcmp(比较字符串)

strcoll(采用目前区域的字符排列次序来比较字符串)

strcpy(拷贝字符串)

strcspn(返回字符串中连续不含指定字符串内容的字符数)

strdup(复制字符串)

strfry(随机重组字符串内的字符)

strlen(返回字符串长度)

strncasecmp(忽略大小写比较字符串)

strncat(连接两字符串)

strncmp(比较字符串)

strncpy(拷贝字符串)

strpbrk(查找字符串中第一个出现的指定字符)

strrchr(查找字符串中最后一个出现的指定字符)

strspn(返回字符串中连续不合指定字符串内容的字符数)

strstr(在一字符串中查找指定的字符串)

strtok(分割字符串)

第6章 数学计算函数

abs(计算整型数的绝对值)

acos(取反余弦函数值)

asin(取反正弦函数值)

atan(取反正切函数值)

atan2(取得反正切函数值)

ceil(取不小于参数的最小整型数)

cos(取余弦函数值)

cosh(取双曲线余弦函数值)

div(取得两整型数相除后的商及余数)

exp(计算指数)

fabs(计算浮点型数的绝对值)

frexp(将浮点型数分为底数与指数)

hypot(计算直角三角形斜边长)

labs(计算长整型数的绝对值)

ldexp(计算2的次方值)

ldiv(取得两长整数相除后的商及余数)

log(计算以e为底的对数值)

log10(计算以10为底的对数值)

modf(将浮点型数分解成整数与小数)

pow(计算次方值)

sin(取正弦函数值)

sinh(取双曲线正弦函数值)

sqrt(计算平方根值)

tan(取正切函数值)

tanh(取双曲线正切函数值)

第7章 用户和组函数

cuserid(取得用户帐号名称)

endgrent(关闭组文件)

endpwent(关闭密码文件)

endutent(关闭utmp文件)

fgetgrent(从指定的文件来读取组格式)

fgetpwent(从指定的文件来读取密码格式)

getegid(取得有效的组识别码)

geteuid(取得有效的用户识别码)

getgid(取得真实的组识别码)

getgrent(从组文件文件中取得帐号的数据)

getgrgid(从组文件中取得指定gid的数据)

getgrnan(从组文件中取得指定组的数据)

getgroups(取得组代码)

getlogin(取得登录的用户帐号名称)

getpw(取得指定用户的密码文件数据)

getpwent(从密码文件中取得帐号的数据)

getpwnam(从密码文件中取得指定帐号的数据)

getpwuid(从密码文件中取得指定uid的数据)

getuid(取得真实的用户识别码)

getutent(从utmp文件中取得帐号登录数据)

getutid(从utmp文件中查找特定的记录)

getutline(从utmp文件中查找特定的记录)

initgroups(初始化组清单)

logwtmp(将一登录数据记录到wtmp文件)

pututline(将utmp记录写入文件)

setegid(设置有效的组识别码)

seteuid(设置有效的用户识别码)

setfsgid(设置文件系统的组识别码)

setfsuid(设置文件系统的用户识别码)

setgid(设置真实的组识别码)

setgrent(从头读取组文件中的组数据)

setgroups(设置组代码)

setpwent(从头读取密码文件中的帐号数据)

setregid(设置真实及有效的组识别码)

setreuid(设置真实及有效的用户识别码)

setuid(设置真实的用户识别码)

setutent(从头读取utmp/文件中的登录数据)

updwtmp(将一登录数据记录到wtmp文件)

utmpname(设置utmp文件路径)

第8章 数据[加密](http://www.2cto.com/Article/jiami/)函数

crypt(将密码或数据编码)

getpass(取得一密码输入)

第9章 数据结构函数

bsearch(二元搜索)

hcreate(建立哈希表)

hdestory(删除哈希表)

hsearch(哈希表搜索)

insque(加入一项目至队列中)

lfind(线性搜索)

lsearch(线性搜索)

qsort(利用快速排序法排列数组)

rremque(从队列中删除一项目)

tdelete(从二叉树中删除数据)

tfind(搜索二叉树)

tsearch(二叉树)

twalk(走访二叉树)

第10章 随机数函数

drand48(产生一个正的浮点型随机数)

erand48(产生一个正的浮点型随机数)

initstate(建立随机数状态数组)

jrand48(产生一个长整型数随机数)

lcong48(设置48位运算的随机数种子)

lrand48(产生一个正的长整型随机数)

mrand48(产生一个长整型随机数)

nrand48(产生一个正的长整型随机数)

rand(产生随机数)

random(产生随机数)

seed48(设置48位运算的随机数种子)

setstate(建立随机数状态数组)

srand(设置随机数种子)

srand48(设置48位运算的随机数种子)

srandom(设置随机数种子)

第11章 初级i／o函数

close(关闭文件)

creat(建立文件)

dup(复制文件描述词)

dup2(复制文件描述词)

fcntl(文件描述词操作)

flock(锁定文件或解除锁定)

fsync(将缓冲区数据写回磁盘)

lseek(移动文件的读写位置)

mkstemp(建立唯一的临时文件)

open(打开文件)

read(由己打开的文件读取数据)

sync(将缓冲区数据写回磁盘)

write(将数据写入已打开的文件内)

第12章 标准i／o函数

clearerr(清除文件流的错误旗标)

fclose(关闭文件)

fdopen(将文件描述词转为文件指针)

feof(检查文件流是否读到了文件尾)

fflush(更新缓冲区)

fgetc(由文件中读取一个字符)

fgetpos(取得文件流的读取位置)

fgets(由文件中读取一字符串)

fileno(返回文件流所使用的文件描述词)

fopen(打开文件)

fputc(将一指定字符写入文件流中)

fputs(将一指定的字符串写入文件内)

fread(从文件流读取数据)

freopen(打开文件)

fseek(移动文件流的读写位置)

fsetpos(移动文件流的读写位置)

ftell(取得文件流的读取位置)

fwrite(将数据写至文件流)

getc(由文件中读取一个字符)

getchar(由标准输入设备内读进一字符)

gets(由标准输入设备内读进一字符串)

mktemp(产生唯一的临时文件文件名)

putc(将一指定字符写入文件中)

putchar(将指定的字符写到标准输出设备)

puts(将指定的字符串写到标准输出设备)

rewind(重设文件流的读写位置为文件开头)

setbuf(设置文件流的缓冲区)

setbuffer(设置文件流的缓冲区)

setlinebuf(设置文件流为线性缓冲区)

setvbuf(设置文件流的缓冲区)

tmpfile(建立临时文件)

ungetc(将一指定字符写回文件流中)

第13章 进程及流程控制

abort(以异常方式结束进程)

assert(若测试的条件不成立则终止进程)

atexit(设置程序正常结束前调用的函数)

execl(执行文件)

execle(执行文件)

execlp(从path环境变量中查找文件并执行)

execv(执行文件)

execve(执行文件)

execvp(执行文件)

exit(正常结束进程)

_exit(结束进程执行)

fork(建立一个新的进程)

getpgid(取得进程组识别码)

getpgrp(取得进程组识别码)

getpid(取得进程识别码)

getppid(取得父进程的进程识别码)

getpriority(取得程序进程执行优先权)

longjmp(跳转到原先setjmp保存的堆栈环境)

nice(改变进程优先顺序)

on＿exit(设置程序正常结束前调用的函数)

ptrace(进程追踪)

setjmp(保存目前堆栈环境)

setpgid(设置进程组识别码)

setpgrp(设置进程组识别码)

setpriority(设置程序进程执行优先权)

siglongjmp(跳转到原先sigsetjmp保存的堆栈环境)

sigsetjmp(保存目前堆栈环境)

system(执行shell命令)

wait(等待子进程中断或结束)

waitpid(等待子进程中断或结束)

第14章 格式化输人输出函数

fprintf(格式化输出数据至文件)

fscanf(格式化字符串输入)

printf(格式化输出数据)

scanf(格式化字符串输入)

snprintf(格式化字符串复制)

sprintf(格式化字符串复制)

sscanf(格式化字符串输入)

vfprintf(格式化输出数据至文件)

vfcanf(格式化字符串输入)

vprintf(格式化输出数据)

vscanf(格式化字符串输入)

vsnprintf(格式化字符串复制)

vsprintf(格式化字符串复制)

vsscanf(格式化字符串输入)

第15章 文件及目录函数

access(判断是否具有存取文件的权限)

alphasort(依字母顺序排序目录结构)

chdir(改变当前的工作目录)

chmod(改变文件的权限)

chown(改变文件的所有者)

chroot(改变根目录)

closedir(关闭目录)

fchdir(改变当前的工作目录)

fchmod(改变文件的权限)

fchown(改变文件的所有者)

fstat(由文件描述词取得文件状态)

ftruncate(改变文件大小)

ftw(遍历目录树)

get_current_dir_name(取得当前的工作目录)

getcwd(取得当前的工作目录)

getwd(取得当前的工作目录)

lchown(改变文件的所有者)

link(建立文件连接)

lstat(由文件描述词取得文件状态)

nftw(遍历目录树)

opendir(打开目录)

readdir(读取目录)

readlink(取得符号连接所指的文件)

realpath(将相对目录路径转换成绝对路径)

remove(删除文件)

rename(更改文件名称或位置)

rewinddir(重设读取目录的位置为开头位置)

scandir(读取特定的目录数据)

seekdir(设置下回读取目录的位置)

stat(取得文件状态)

symlink(建立文件符号连接)

telldir(取得目录流的读取位置)

truncate(改变文件大小)

+-umask(设置建立新文件时的权限遮罩)

unlink(删除文件)

utime(修改文件的存取时间和更改时间)

utimes(修改文件的存取时间和更改时间)

第16章　信号函数

alarm(设置信号传送闹钟)

kill(传送信号给指定的进程)

pause(让进程暂停直到信号出现)

psignal(列出信号描述和指定字符串)

raise(传送信号给目前的进程)

sigaction(查询或设置信号处理方式)

sigaddset(增加一个信号至信号集)

sigdelset(从信号集里删除一个信号)

sigemptyset(初始化信号集)

sigfillset(将所有信号加入至信号集)

sigismember(测试某个信号是否已加入至信号集里)

signal(设置信号处理方式)

sigpause(暂停直到信号到来)

sigpending(查询被搁置的信号)

sigprocmask(查询或设置信号遮罩)

sigsuspend(暂停直到信号到来)

sleep(让进程暂停执行一段时间)

isdigit(测试字符是否为阿拉伯数字)

第17章 错误处理函数

ferror(检查文件流是否有错误发生)

perror(打印出错误原因信息字符串)

streror(返回错误原因的描述字符串)

第18章 管道相关函数

mkfifo(建立具名管道)

pclose(关闭管道i／o)

pipe(建立管道)

popen(建立管道i/o)

第19章socket相关函数

accept(接受socket连线)

bind(对socket定位)

connect(建立socket连线)

endprotoent(结束网络协议数据的读取)

endservent(结束网络服务数据的读取)

gethostbyaddr(由ip地址取得网络数据)

gethostbyname(由主机名称取得网络数据)

getprotobyname(由网络协议名称取得协议数据)

getprotobynumber(由网络协议编号取得协议数据)

getprotoent(取得网络协议数据)

getservbyname(依名称取得网络服务的数据)

getservbyport(依port号码取得网络服务的数据)

getservent(取得主机网络服务的数据)

getsockopt(取得socket状态)

herror(打印出网络错误原因信息字符串)

hstrerror(返回网络错误原因的描述字符串)

htonl(将32位主机字符顺序转换成网络字符顺序)

htons(将16位主机字符顺序转换成网络字符顺序)

inet_addr(将网络地址转成网络二进制的数字)

inet_aton(将网络地址转成网络二进制的数字)

inet_ntoa(将网络二进制的数字转换成网络地址)

listen(等待连接)

ntohl(将32位网络字符顺序转换成主机字符顺序)

ntohs(将16位网络字符顺序转换成主机字符顺序)

recv(经socket接收数据)

recvfrom(经socket接收数据)

recvmsg(经socket接收数据)

send(经socket传送数据)

sendmsg(经socket传送数据)

sendto(经socket传送数据)

setprotoent(打开网络协议的数据文件)

setservent(打开主机网络服务的数据文件)

setsockopt(设置socket状态)

shutdown(终止socket通信)

socket(建立一个socket通信)

第20章 进程通信(ipc)函数

ftok(将文件路径和计划代号转为system vipckey)

msgctl(控制信息队列的运作)

msgget(建立信息队列)

msgrcv(从信息队列读取信息)

msgsnd(将信息送入信息队列)

semctl(控制信号队列的操作)

semget(配置信号队列)

semop(信号处理)

shmat(attach共享内存)

shmctl(控制共享内存的操作)

shmdt(detach共享内存)

shmget(配置共享内存)

第21章 记录函数

closelog(关闭信息记录)

openlog(准备做信息记录)

syslog(将信息记录至[系统](http://www.2cto.com/os/)日志文件)

第22章 环境变量函数

getenv(取得环境变量内容)

putenv(改变或增加环境变量)

setenv(改变或增加环境变量)

unsetenv(清除环境变量内容)

第23章 正则表达式

regcomp(编译正则表达式字符串)

regerror(取得正则搜索的错误原因)

regexec(进行正则表达式的搜索)

regfree(释放正则表达式使用的内存)

第24章 动态函数

dlclose(关闭动态函数库文件)

dlerror(动态函数错误处理)

dlopen(打开动态函数库文件)

dlsym(从共享对象中搜索动态函数)

第25章 其他函数

getopt(分析命令行参数)

isatty(判断文件描述词是否是为终端机)

select(i/o多工机制)

ttyname(返回一终端机名称)

附录a 编译程序-gcc

附录b 宏与函数

附录c 不定参数

附录d linux信号列表

附录e 常见错误代码及原因0