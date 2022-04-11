---
layout:     post
title:     PROJECT-JOB DAY8
description:     Project JOB
date:     2022-03-19
author:     POt
header-img:     img/post-bg-kuaidi.jpg
catalog: true
category: project
tags:     
    -   
        -   

    -   

---
## 数据库复习
https://github.com/wolverinn/Waking-Up/blob/master/Database.md
## 操作系统
### Q：进程和线程有什么区别
A：
* 进程是系统资源的基本单位，线程是CPU调度的基本单位
* 线程是依赖于进程存在的，一个进程至少有一个线程
* 进程是拥有系统资源的独立单位，而线程几乎不占用系统资源
* 进程在切换时的开销比线程大
* 线程之间通信更方便，进程之间用IPC（7种）通信
* 多线程程序只要有一个线程崩溃那么程序崩溃，而多进程程序一个进程崩溃并不会对其他进程产生影响

#### Q：统一进程中的线程可以共享那些数据？
A： 进程进程代码段，进程的公有变量， 进程的文件文件描述符， 进程的当前目录， 信号处理器/信号处理函数：对收到的信号的处理方式， 进程ID与进程组ID

#### Q：线程独占哪些资源？
A： 线程ID， 一组寄存器的值， 线程本身的栈， 错误返回码， 信号掩码/信号屏蔽字

### 进程间通信的方式（IPC）7种
1. 匿名管线 (Unix无名，半双工，只能用于亲缘进程之间， 缓冲区有限)
2. 有名管线 （有名，先进先出，名字存在文件系统，内容存在内存）
3. 信号（Linux， 无需知道进程状态，直接发信息）
4. 消息
5. 共享内存
6. 信号量
7. 套接字

### 进程同步问题
#### 管程(Monitor)
管程，将共享变量以及对这些共享变量的操作封装起来，形成一个具有一定接口的功能模块，这样只能通过管程提供的某个过程才能访问管程中的资源。进程只能互斥地使用管程，使用完之后必须释放管程并唤醒入口等待队列中的进程。有wait（唤醒队列第一个，自己排最后）和signal（唤醒队列第一个，自己进入紧急队列）方法
（**人话：管程就像一座房子，有两扇门：紧急队列和普通队列。房子里面装着互斥的方法和资源，房子里只能有一个人进入，出去后唤醒排在队列第一个的进程**）
#### 生产者消费者问题
伪代码：
```
// 伪代码描述 
// 定义信号量 full记录缓冲区物品数量 empty代表缓冲区空位数量 mutex为互斥量
semaphore full = 0, empty = n, mutex = 1;

// 生产者进程
void producer(){
	do{
   	  P(empty);
	  P(mutex);

     // 生产者进行生产
   	
   	  V(mutex);
   	  V(full);
 	} while(1);
}

void consumer(){
	do{
	  P(full);
	  P(mutex);

    	// 消费者进行消费

	  V(mutex);
	  V(empty);
 	} while(1);
}
```

#### 临界区
临界区是进程**访问临界资源**（互斥资源/共享变量）进行操作的**代码块**
#### 同步与互斥的概念（多个进程）
同步：**多个进程**因为合作而使得进程根据一定顺序先后执行，待执行的进程顺序等待
互斥：**多个进程**，同一时间内只能有一个进程访问临界区
#### 并发、并行、异步的区别
并发：**同一进程**在宏观上看是多个程序在运行，但其实在任意时刻，宏观上的并发是由于进程不断切换实现的，本质上同一时间只有一个进程在执行。
并行（和**串行**相比）：在宏观和微观上同一时间都是有多个程序同时运行
异步（和**同步**相比）：进程不老实，在等待资源的时候不老实等待，干自己的其他事情

### Q：进程有几种状态？
A：就绪态，运行态，阻塞态
### Q：进程调度策略有哪些
#### 批处理系统
* 先来先服务（First Come First Serve）
按照请求的顺序进行调度。非抢占式，开销小，无饥饿问题，响应时间不确定（可能很慢）；
对短进程不利，对IO密集型进程不利。
* 最短作业优先（Shortest Job First）
按估计运行时间最短的顺序进行调度。非抢占式，吞吐量高，开销可能较大，可能导致饥饿问题；
对短进程提供好的响应时间，对长进程不利。
* 最短剩余时间优先（Shortest Remaining Time Next）
按剩余运行时间的顺序进行调度。(最短作业优先的抢占式版本)。吞吐量高，开销可能较大，提供好的响应时间；
可能导致饥饿问题，对长进程不利。
* 最高响应比（Highest Response Ratio Next）
响应比 = 1+ 等待时间/处理时间。同时考虑了等待时间的长短和估计需要的执行时间长短，很好的平衡了长短进程。非抢占，吞吐量高，开销可能较大，提供好的响应时间，无饥饿问题。


#### 可交互式系统
* 时间片轮转
维护一个先来先服务的队列，用完时间片的进程排队到最后。
* 优先级调度
为每个进程分配一个优先级，按优先级进行调度。为了防止低优先级的进程永远等不到调度，可以随着时间的推移增加等待进程的优先级。
* 多级反馈队列调度算法
设置多个就绪队列1、2、3...，优先级递减，时间片递增。只有等到优先级更高的队列为空时才会调度当前队列中的进程。如果进程用完了当前队列的时间片还未执行完，则会被移到下一队列。

抢占式（时间片用完时），开销可能较大，对IO型进程有利，可能会出现饥饿问题。

### Q：什么是僵尸进程
A：父进程没等待子进程，导致子进程运行结束后会变成僵尸进程，僵尸进程是一个已经死亡的进程，但是并没有真正被销毁。它已经放弃了几乎所有内存空间，没有任何可执行代码，也不能被调度，仅仅在进程表中保留一个位置，记载该进程的进程ID、终止状态以及资源利用信息(CPU时间，内存使用量等等)供父进程收集，除此之外，僵尸进程不再占有任何内存空间。这个僵尸进程可能会一直留在系统中直到系统重启。
**危害：占用进程号，而系统所能使用的进程号是有限的；占用内存。**
以下情况不会产生僵尸进程
* 该进程的父进程先结束了。每个进程结束的时候，系统都会扫描是否存在子进程，如果有则用Init进程接管，成为该进程的父进程，并且会调用wait等待其结束。
* 父进程调用wait或者waitpid等待子进程结束（需要每隔一段时间查询子进程是否结束）。wait系统调用会使父进程暂停执行，直到它的一个子进程结束为止。waitpid则可以加入WNOHANG(wait-no-hang)选项，如果没有发现结束的子进程，就会立即返回，不会将调用waitpid的进程阻塞。同时，waitpid还可以选择是等待任一子进程（同wait），还是等待指定pid的子进程，还是等待同一进程组下的任一子进程，还是等待组ID等于pid的任一子进程；
* 子进程结束时，系统会产生SIGCHLD(signal-child)信号，可以注册一个信号处理函数，在该函数中调用waitpid，等待所有结束的子进程（注意：一般都需要循环调用waitpid，因为在信号处理函数开始执行之前，可能已经有多个子进程结束了，而信号处理函数只执行一次，所以要循环调用将所有结束的子进程回收）；
* 也可以用signal(SIGCLD, SIG_IGN)(signal-ignore)通知内核，表示忽略SIGCHLD信号，那么子进程结束后，内核会进行回收。

#### 什么是孤儿进程
在子进程结束之前，父进程已经结束了，那么这些子进程将成为孤儿进程。孤儿进程会被Init（进程ID为1）接管，当这些孤儿进程结束时由Init完成状态收集工作。

### Q：线程同步的方式有哪些？
* 互斥量（mutex）
互斥量是内核对象，只有拥有互斥对象的线程才有访问互斥资源的权限。因为互斥对象只有一个，所以可以保证互斥资源不会被多个线程同时访问；当前拥有互斥对象的线程处理完任务后必须将互斥对象交出，以便其他线程访问该资源；
* 信号量（Semaphore）
信号量是内核对象，它允许同一时刻多个线程访问同一资源，但是需要控制同一时刻访问此资源的最大线程数量。信号量对象保存了最大资源计数和当前可用资源计数，**每增加一个线程对共享资源的访问，当前可用资源计数就减1**，只要当前可用资源计数大于0，就可以发出信号量信号，如果为0，则将线程放入一个队列中等待。线程处理完共享资源后，**应在离开的同时通过ReleaseSemaphore函数将当前可用资源数加1**。如果信号量的取值只能为0或1，那么信号量就成为了互斥量；
* 事件（Event）
允许一个线程在处理完一个任务后，主动唤醒另外一个线程执行任务。事件分为手动重置事件和自动重置事件。手动重置事件被设置为激发状态后，会唤醒所有等待的线程，而且一直保持为激发状态，直到程序重新把它设置为未激发状态。自动重置事件被设置为激发状态后，会唤醒一个等待中的线程，然后自动恢复为未激发状态。
* 临界区（Critical Section）
任意时刻只允许一个线程对临界资源进行访问。拥有临界区对象的线程可以访问该临界资源，其它试图访问该资源的线程将被挂起，直到临界区对象被释放。


#### 互斥量和临界区有什么区别？
互斥量是可以命名的，可以用于不同进程之间的同步；而临界区只能用于同一进程中线程的同步。创建互斥量需要的资源更多，因此临界区的优势是速度快，节省资源。

### Q：什么是协程
**轻量级线程**，协程的调度完全由用户控制。协程拥有自己的寄存器上下文和栈。**协程调度切换时，将寄存器上下文和栈保存到其他地方，在切回来的时候，恢复先前保存的寄存器上下文和栈**，直接操作栈则基本没有内核切换的开销，可以不加锁的访问全局变量，所以**上下文的切换非常快**。


#### 协程和线程比较
* 一个线程可以拥有多个协程，一个进程也可以单独拥有多个协程，这样python中则能使用多核CPU。
* 线程进程都是同步机制，而协程则是异步
* 协程能保留上一次调用时的状态，每次过程重入时，就相当于进入上一次调用的状态

### 进程的异常控制流：陷阱、中断、异常和信号
* 陷阱
陷阱是系统故意而为之，为的是实现系统调用。比如从用户态切换到内核态
* 中断
中断是由外部设备引发的，不是执行某条指令的结果，也无法预测发生时机。
* 异常
异常是一种错误情况，异常是同步的。这里特指因为执行当前指令而产生的错误情况，比如除法异常、缺页异常等。有些书上为了区分，也将这类“异常”称为**“故障”**。
* 信号
信号是一种更高层的软件形式的异常，同样会中断进程的控制流，可以由进程进行处理。
### Q：什么是IO多路复用？怎么实现？
IO多路复用（IO Multiplexing）是指单个进程/线程就可以同时处理多个IO请求。
（**人话：机场塔台调度**）
#### select/poll/epoll三者的区别？
* select
将文件描述符放入一个集合中。缺点：每次都要复制，有空间限制（1024），轮询的方式效率较低
* poll
是select的链式存储改进版，取消了空间限制
* epoll
通过内核与用户空间共享内存，采用回调机制，支持的同时连接数上限很高
#### 什么时候使用select/poll，什么时候使用epoll？
连接数较多 epoll， 连接数较少 select/poll
#### 什么是文件描述符？
实际上，它是一个索引值，指向内核为每一个进程所维护的该进程打开文件的记录表。

### 什么是用户态和内核态？

为了限制不同程序的访问能力，防止一些程序访问其它程序的内存数据，CPU划分了用户态和内核态两个权限等级。

- 用户态只能受限地访问内存，且不允许访问外围设备，没有占用CPU的能力，CPU资源可以被其它程序获取；
- 内核态可以访问内存所有数据以及外围设备，也可以进行程序的切换。

所有用户程序都运行在用户态，但有时需要进行一些内核态的操作，比如从硬盘或者键盘读数据，这时就需要进行系统调用，使用**陷阱指令**，CPU切换到内核态，执行相应的服务，再切换为用户态并返回系统调用的结果。

##### 为什么要分用户态和内核态？
<details>
<summary>展开</summary>

（我自己的见解：）

- 安全性：防止用户程序恶意或者不小心破坏系统/内存/硬件资源；
- 封装性：用户程序不需要实现更加底层的代码；
- 利于调度：如果多个用户程序都在等待键盘输入，这时就需要进行调度；统一交给操作系统调度更加方便。
</details>

##### 如何从用户态切换到内核态？
<details>
<summary>展开</summary>

- 系统调用：比如读取命令行输入。本质上还是通过中断实现
- 用户程序发生异常时：比如缺页异常
- 外围设备的中断：外围设备完成用户请求的操作之后，会向CPU发出中断信号，这时CPU会转去处理对应的中断处理程序
</details>

### 什么是死锁？

在两个或者多个并发进程中，每个进程持有某种资源而又等待其它进程释放它们现在保持着的资源，在未改变这种状态之前都不能向前推进，称这一组进程产生了死锁(deadlock)。

### 死锁产生的必要条件？
- **互斥**：一个资源一次只能被一个进程使用；
- **占有并等待**：一个进程至少占有一个资源，并在等待另一个被其它进程占用的资源；
- **非抢占**：已经分配给一个进程的资源不能被强制性抢占，只能由进程完成任务之后自愿释放；
- **循环等待**：若干进程之间形成一种头尾相接的环形等待资源关系，该环路中的每个进程都在等待下一个进程所占有的资源。

### 死锁有哪些处理方法？
<details>
<summary>鸵鸟策略</summary>

直接忽略死锁。因为解决死锁问题的代价很高，因此鸵鸟策略这种不采取任务措施的方案会获得更高的性能。当发生死锁时不会对用户造成多大影响，或发生死锁的概率很低，可以采用鸵鸟策略。
</details>

<details>
<summary>死锁预防</summary>

基本思想是破坏形成死锁的四个必要条件：
- 破坏互斥条件：允许某些资源同时被多个进程访问。但是有些资源本身并不具有这种属性，因此这种方案实用性有限；
- 破坏占有并等待条件：
    - 实行资源预先分配策略（当一个进程开始运行之前，必须一次性向系统申请它所需要的全部资源，否则不运行）；
    - 或者只允许进程在没有占用资源的时候才能申请资源（申请资源前先释放占有的资源）；
    - 缺点：很多时候无法预知一个进程所需的全部资源；同时，会降低资源利用率，降低系统的并发性；
- 破坏非抢占条件：允许进程强行抢占被其它进程占有的资源。会降低系统性能；
- 破坏循环等待条件：对所有资源统一编号，所有进程对资源的请求必须按照序号递增的顺序提出，即只有占有了编号较小的资源才能申请编号较大的资源。这样避免了占有大号资源的进程去申请小号资源。
</details>

<details>
<summary>死锁避免</summary>

动态地检测资源分配状态，以确保系统处于安全状态，只有处于安全状态时才会进行资源的分配。所谓安全状态是指：即使所有进程突然请求需要的所有资源，也能存在某种对进程的资源分配顺序，使得每一个进程运行完毕。

> 银行家算法
</details>

<details>
<summary>死锁解除</summary>

> 如何检测死锁：检测有向图是否存在环；或者使用类似死锁避免的检测算法。

死锁解除的方法：
- 利用抢占：挂起某些进程，并抢占它的资源。但应防止某些进程被长时间挂起而处于饥饿状态；
- 利用回滚：让某些进程回退到足以解除死锁的地步，进程回退时自愿释放资源。要求系统保持进程的历史信息，设置还原点；
- 利用杀死进程：强制杀死某些进程直到死锁解除为止，可以按照优先级进行。
</details>

### 分页和分段有什么区别？

- 页式存储：用户空间划分为大小相等的部分称为页（page），内存空间划分为同样大小的区域称为页框，分配时以页为单位，按进程需要的页数分配，逻辑上相邻的页物理上不一定相邻；（**人话：将进程和内存空间均分成若干大小相等的页面，站在操作系统的防止碎片化**）
- 段式存储：用户进程地址空间按照自身逻辑关系划分为若干个段（segment）（如代码段，数据段，堆栈段），内存空间被动态划分为长度不同的区域，分配时以段为单位，每段在内存中占据连续空间，各段可以不相邻；（人话：按照代码段进行分段，站在用户的角度上满足用户需求）
![](https://s2.loli.net/2022/03/20/N7wedIGSDlKh6PM.jpg)- 段页式存储：用户进程先按段划分，段内再按页划分，内存划分和分配按页。
![](https://s2.loli.net/2022/03/20/N7wedIGSDlKh6PM.jpg)
- 分段与分页二者对比
![](https://s2.loli.net/2022/03/20/RbnqioXMcA2kUG4.jpg)

区别：
- 目的不同：分页的目的是管理内存，用于虚拟内存以获得更大的地址空间；分段的目的是满足用户的需要，使程序和数据可以被划分为逻辑上独立的地址空间；
- 大小不同：段的大小不固定，由其所完成的功能决定；页的大小固定，由系统决定；
- 地址空间维度不同：分段是二维地址空间（段号+段内偏移），分页是一维地址空间（每个进程一个页表/多级页表，通过一个逻辑地址就能找到对应的物理地址）；
- 分段便于信息的保护和共享；分页的共享收到限制；
- 碎片：分段没有内碎片，但会产生外碎片；分页没有外碎片，但会产生内碎片（一个页填不满）

### 什么是虚拟内存？
![](https://s2.loli.net/2022/03/20/1T6rkUgzWR89BVH.jpg)
（游戏例子，64G游戏但是内存只有16G，这时通过虚拟内存，使用时从虚拟内存搬入内存，不用时搬出）
每个程序都拥有自己的地址空间，这个地址空间被分成大小相等的页，这些页被映射到物理内存；但不需要所有的页都在物理内存中，当程序引用到不在物理内存中的页时，由操作系统将缺失的部分装入物理内存。这样，对于程序来说，逻辑上似乎有很大的内存空间，只是实际上有一部分是存储在磁盘上，因此叫做虚拟内存。

虚拟内存的优点是让程序可以获得更多的可用内存。

虚拟内存的实现方式、页表/多级页表、缺页中断、不同的页面淘汰算法：[答案](https://imageslr.github.io/2020/07/08/tech-interview.html#virtual-memory)。

##### 如何进行地址空间到物理内存的映射？
<details>
<summary>展开</summary>

**内存管理单元**（MMU）管理着逻辑地址和物理地址的转换，其中的页表（Page table）存储着页（逻辑地址）和页框（物理内存空间）的映射表，页表中还包含包含有效位（是在内存还是磁盘）、访问位（是否被访问过）、修改位（内存中是否被修改过）、保护位（只读还是可读写）。逻辑地址：页号+页内地址（偏移）；每个进程一个页表，放在内存，页表起始地址在PCB/寄存器中。
</details>

### 有哪些页面置换算法？
在程序运行过程中，如果要访问的页面不在内存中，就发生缺页中断从而将该页调入内存中。此时如果内存已无空闲空间，系统必须从内存中调出一个页面到磁盘中来腾出空间。页面置换算法的主要目标是使页面置换频率最低（也可以说缺页率最低）。

- **最佳页面置换算法**OPT（Optimal replacement algorithm）：置换以后不需要或者最远的将来才需要的页面，是一种理论上的算法，是最优策略；
- **先进先出**FIFO：置换在内存中驻留时间最长的页面。缺点：有可能将那些经常被访问的页面也被换出，从而使缺页率升高；
- **第二次机会算法**SCR：按FIFO选择某一页面，若其访问位为1，给第二次机会，并将访问位置0；
- **时钟算法** Clock：SCR中需要将页面在链表中移动（第二次机会的时候要将这个页面从链表头移到链表尾），时钟算法使用环形链表，再使用一个指针指向最老的页面，避免了移动页面的开销；
- **最近未使用算法**NRU（Not Recently Used）：检查访问位R、修改位M，优先置换R=M=0，其次是（R=0, M=1）；
- **最近最少使用算法**LRU（Least Recently Used）：置换出未使用时间最长的一页；实现方式：维护时间戳，或者维护一个所有页面的链表。当一个页面被访问时，将这个页面移到链表表头。这样就能保证链表表尾的页面是最近最久未访问的。
- **最不经常使用算法**NFU：置换出访问次数最少的页面

<details>
<summary>局部性原理</summary>

- 时间上：最近被访问的页在不久的将来还会被访问；
- 空间上：内存中被访问的页周围的页也很可能被访问。
</details>

<details>
<summary>什么是颠簸现象</summary>

颠簸本质上是指频繁的页调度行为。进程发生缺页中断时必须置换某一页。然而，其他所有的页都在使用，它置换一个页，但又立刻再次需要这个页。因此会不断产生缺页中断，导致整个系统的效率急剧下降，这种现象称为颠簸。内存颠簸的解决策略包括：

- 修改页面置换算法；
- 降低同时运行的程序的数量；
- 终止该进程或增加物理内存容量。
</details>

### 缓冲区溢出问题

<details>
<summary>什么是缓冲区溢出？</summary>
C 语言使用运行时栈来存储过程信息。每个函数的信息存储在一个栈帧中，包括寄存器、局部变量、参数、返回地址等。C 对于数组引用不进行任何边界检查，因此**对越界的数组元素的写操作会破坏存储在栈中的状态信息**，这种现象称为缓冲区溢出。缓冲区溢出会破坏程序运行，也可以被用来进行攻击计算机，如使用一个指向攻击代码的指针覆盖返回地址。
</details>

<details>
<summary>缓冲区溢出的防范方式</summary>

防范缓冲区溢出攻击的机制有三种：随机化、栈保护和限制可执行代码区域。
- 随机化：包括栈随机化（程序开始时在栈上分配一段随机大小的空间）和地址空间布局随机化（Address-Space Layout Randomization，ASLR，即每次运行时程序的不同部分，包括代码段、数据段、栈、堆等都会被加载到内存空间的不同区域），但只能增加攻击一个系统的难度，不能完全保证安全。
- 栈保护：在每个函数的栈帧的局部变量和栈状态之间存储一个**随机产生的**特殊的值，称为金丝雀值（canary）。在恢复寄存器状态和函数返回之前，程序检测这个金丝雀值是否被改变了，如果是，那么程序异常终止。
- 限制可执行代码区域：内存页的访问形式有三种：可读、可写、可执行，只有编译器产生的那部分代码所处的内存才是可执行的，其他页限制为只允许读和写。

</details>

更详细的可以参考：https://imageslr.github.io/2020/07/08/tech-interview.html#stackoverflow

### 磁盘调度
过程：磁头（找到对应的盘面）；磁道（一个盘面上的同心圆环，寻道时间）；扇区（旋转时间）。为减小寻道时间的调度算法：

- 先来先服务
- 最短寻道时间优先
- 电梯算法：电梯总是保持一个方向运行，直到该方向没有请求为止，然后改变运行方向。