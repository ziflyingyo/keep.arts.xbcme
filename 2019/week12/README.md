## ARTS(week12)

### 算法题(Algorithm)

[反转字符串](https://github.com/geekwho11/learn.leetcode.xbcme/tree/master/php/src/344.reverse-string)

### 阅读点评(Review)

#### [The Secret To 10 Million Concurrent Connections -The Kernel Is The Problem, Not The Solution](http://highscalability.com/blog/2013/5/13/the-secret-to-10-million-concurrent-connections-the-kernel-i.html)

#### 阅读笔记

##### 什么是C10M问题？

10M的并发连接挑战意味着什么：

1. 1千万的并发连接数 
2. 100万个连接/秒——每个连接以这个速率持续约10秒 
3. 10GB/秒的连接——快速连接到互联网。 
4. 1千万个数据包/秒——据估计目前的服务器每秒处理50K的数据包，以后会更多。过去服务器每秒可以处理100K的中断，并且每一个数据包都产生中断。 
5. 10微秒的延迟——可扩展服务器也许可以处理这个规模，但延迟可能会飙升。 
6. 10微秒的抖动——限制最大延迟 
7. 并发10核技术——软件应支持更多核的服务器。通常情况下，软件能轻松扩展到四核。服务器可以扩展到更多核，因此需要重写软件，以支持更多核的服务器。 

##### 为什么会有C10M问题？

不远的将来，服务器将要处理数百万的并发连接。IPv6协议下，每个服务器的潜在连接数都是数以百万级的，所以处理规模需要升级。

- 如IDS / IPS这类应用程序需要支持这种规模，因为它们连接到一个服务器骨干网。其他例子：DNS根服务器，TOR节点，互联网Nmap，视频流，银行，Carrier NAT，VoIP PBX，负载均衡器，网页缓存，防火墙，电子邮件接收，垃圾邮件过滤。


- 通常人们将互联网规模问题归根于应用程序而不是服务器，因为他们卖的是硬件+软件。你买设备，并将其应用到你的数据中心。这些设备可能包含一块Intel主板或网络处理器以及用来加密和检测数据包的专用芯片等。

- 截至2013年2月，40gpbs, 32-cores, 256gigs RAM的X86服务器在Newegg网站上的报价是5000美元。该服务器可以处理1万个以上的并发连接，如果它们不能，那是因为你选择了错误的软件，而不是底层硬件的问题。这个硬件可以很容易地扩展到1千万个并发连接。 

面向数据层的系统可以每秒处理1千万个数据包，面向控制层的系统，每秒只能处理1百万个数据包。

这似乎很极端，请记住一句俗语：可扩展性是专业化的。为了做好一些事情，你不能把性能问题依靠操作系统来解决，你必须自己做。 

##### 怎么解决C10M问题？

让Unix处理网络堆栈，但之后的业务由你来处理。

- 网卡
- 问题：通过内核工作效率不高
- 解决方案：使用自己的驱动程序并管理它们，使适配器远离操作系统。

- CPU
- 问题：使用传统的内核方法来协调你的应用程序是行不通的。
- 解决方案：Linux管理前两个CPU，你的应用程序管理其余的CPU。中断只发生在你允许的CPU上。

- 内存
- 问题：内存需要特别关注，以求高效。
- 解决方案：在系统启动时就分配大部分内存给你管理的大内存页

控制层交给Linux，应用程序管理数据。应用程序与内核之间没有交互，没有线程调度，没有系统调用，没有中断，什么都没有。 
然而，你有的是在Linux上运行的代码，你可以正常调试，这不是某种怪异的硬件系统，需要特定的工程师。你需要定制的硬件在数据层提升性能，但是必须是在你熟悉的编程和开发环境上进行。 

#### 参考链接

1. [千万级并发实现的秘密：内核不是解决方案，而是问题所在！](https://www.csdn.net/article/2013-05-16/2815317-the-secret-to-10m-concurrent)
2. [高性能网络编程(三)：下一个10年，是时候考虑C10M并发问题了](http://www.52im.net/thread-568-1-1.html)
3. [DPDK 入门最佳指南](https://www.itcodemonkey.com/article/7930.html)
4. [C10M](http://c10m.robertgraham.com/)

### 技术技巧(Tip)

#### 命令行的艺术

#### grep

| 命令   | 含义          | 备注     |
| ---- | ----------- | ------ |
| -i   | 忽略大小写       |        |
| -o   | 只显示包含搜索内容的行 |        |
| -v   | 匹配不包含搜索内容的行 |        |
| -A   | 显示匹配行的后多少行  | After  |
| -B   | 显示匹配行的后前多少行 | Before |
| -C   | 显示匹配行的前后多少行 |        |

#### 终端

| 命令           | 含义                | 备注                                |
| ------------ | ----------------- | --------------------------------- |
| Tab          | 实现自动补全参数          | man readline 查看手册                 |
| ctrl-r       | 搜索命令行历史记录         | 重复按下 **ctrl-r** 会向后查找匹配项          |
|              |                   | 按下 **Enter** 键会执行当前匹配的命令          |
|              |                   | 按下右方向键会将匹配项放入当前行中                 |
| ctrl-w       | 删除你键入的最后一个单词      |                                   |
| ctrl-k       | 可以删除光标至行尾的所有内容    |                                   |
| ctrl-u       | 可以删除行内光标所在位置之前的内容 | **alt-b** 和 **alt-f**可以以单词为单位移动光标 |
| ctrl-d       | 删除光标处的字符          |                                   |
| ctrl-a       | 可以将光标移至行首         |                                   |
| ctrl-e       | 可以将光标移至行尾         |                                   |
|              |                   |                                   |
| ctrl-l       | 可以清屏              |                                   |
| ctrl-p       | 从命令历史查找上一个命令      |                                   |
| ctrl-n       | 从命令历史查找下一个命令      |                                   |
|              |                   |                                   |
| ctrl-shift-c | 复制                | GNOME Terminal 快捷键                |
| ctrl-shift-v | 粘贴                | GNOME Terminal 快捷键                |
| ctrl-c       | 系统默认中断快捷键         |                                   |

#### 参考链接
1. [The Art of Command Line](https://github.com/jlevy/the-art-of-command-line)

### 分享(Share)

#### [滴滴工程师图文并茂带你深入理解 TCP 握手分手全过程](https://mp.weixin.qq.com/s/n0--UphB4SCFOU3k0cTyRw)

#### 阅读笔记

文章详细解释了什么是TCP的三次握手，四次挥手

#### 三次握手

1. 客户端发送一个SYN段，并指明客户端的初始序列号，即ISN(c).
2. 服务端发送自己的SYN段作为应答，同样指明自己的ISN(s)。为了确认客户端的SYN，将ISN(c)+1作为ACK数值。这样，每发送一个SYN，序列号就会加1. 如果有丢失的情况，则会重传。
3. 为了确认服务器端的SYN，客户端将ISN(s)+1作为返回的ACK数值。

#### 四次挥手
1. 客户端发送一个FIN段，并包含一个希望接收者看到的自己当前的序列号K. 同时还包含一个ACK表示确认对方最近一次发过来的数据。 
2. 服务端将K值加1作为ACK序号值，表明收到了上一个包。这时上层的应用程序会被告知另一端发起了关闭操作，通常这将引起应用程序发起自己的关闭操作。 
3. 服务端发起自己的FIN段，ACK=K+1, Seq=L 
4. 客户端确认。ACK=L+1

#### 为什么建立连接是三次握手，而关闭连接却是四次挥手呢？

这是因为服务端在LISTEN状态下，收到建立连接请求的SYN报文后，把ACK和SYN放在一个报文里发送给客户端。而关闭连接时，当收到对方的FIN报文时，仅仅表示对方不再发送数据了但是还能接收数据，己方是否现在关闭发送数据通道，需要上层应用来决定，因此，己方ACK和FIN一般都会分开发送。

#### 抓包命令

```
tcpdump -w /tmp/a.cap port 6379 -s0
-w把数据写入文件，-s0设置每个数据包的大小默认为68字节，如果用-S 0则会抓到完整数据包

tcpdump -r /tmp/a.cap -n -nn -A -x| vim -
（-x 以16进制形式展示，便于后面分析）
```
