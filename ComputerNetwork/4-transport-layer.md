# 第五章 传输层

## 导读

### 【考纲内容】

1.   传输层提供的服务
     +   传输层的功能
     +   传输层寻址与端口
     +   无连接服务和面向连接服务
2.   $UDP$
     +   $UDP$数据报
     +   $UDP$校验
3.   $TCP$
     +   $TCP$段
     +   $TCP$连接管理
     +   $TCP$可靠传输
     +   $TCP$流量控制与拥塞控制

### 【知识导图】

![179b476cc0644174be2706d0be5520ab~tplv-k3u1fbpfcp-zoom-in-crop-mark_1512_0_0_0](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307271126730.webp)

### 【复习提示】

+   传输层是整个网络体系结构中的关键层次
+   要求掌握传输层在计算机网络中的地位、功能、 工作方式及原理等，掌握$UDP$及$TCP $
    +   如首部格式、可靠传输、流量控制、拥塞控制、连接管理等
+   其中，==$TCP$报文分析、流量控制与拥塞控制机制，出选择题、综合题的概率均较大，因此要将其工作原理透彻掌握，以便能在具体的题目中灵活运用。==

## 基本概念

传输层只有主机才有，而路由器这种中间设备至多只有物理层、数据链路层和网络层三层架构。

+   即在通信子网中没有传输层，传输层只存在于通信子网以外的主机中

传输层是面向通信的最高层，也是用户功能的最底层。

+   它使应用进程看见的是在两个传输层实体之间好像有一条端到端的逻辑通信信道，这条逻辑通信信道对上层的表现却因传输层协议不同而有很大的差别
    1.   当传输层采用面向连接的$TCP$时，尽管下面的网络是不可靠的（只提供尽最大努力的服务），这种逻辑通信信道就相当于一条全双工的可靠信道。
    2.   但当传输层采用无连接的$UDP$时，这种逻辑通信信道仍然是一条不可靠信道，如果使用$UDP$进行数据传输，那么必须在应用层提供可靠性方面的全部工作


![image-20230802140019654](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202308021400772.png)

### 传输层的功能

1. 传输层提供进程与进程之间的逻辑通信。
    +   即==端到端的通信==
    +   与网络层的区别是，网络层提供的是主机之间的逻辑通信
        +   从网络层来说，通信的双方是两台主机，IP数据报的首部给出了这两台主机的IP地址。 
        +   但==“两台主机之间的通信”实际上是两台主机中的应用进程之间的通信，应用进程之间的通信又称端到端的逻辑通信==
        +   这里“逻辑通信”的意思是：==传输层之间的通信好像是沿水平方向传送数据，但事实上这两个传输层之间并没有一条水平方向的物理连接==
2. 复用与分用。

    1.   复用：应用层所有的应用进程都可以通过传输层再传输到网络层
    2.   分用：传输层从网络层收到数据后交付指明的应用进程

    >   网络层也有复用分用的功能，但网络层的复用是指发送方不同协议的数据都可以封装成IP数据报发送出去，分用是指接收方在网络层剥去首部后把数据交付给相应的协议
3. 对收到的报文（包括首部和数据部分）进行差错检测。
    +   网络层只检查IP数据报的首部，不检验数据部分是否出错。
4. 传输层提供两种不同的传输协议，即面向连接的$TCP$和无连接的$UDP$。

    +   而网络层无法同时实现两种协议：虚电路（面向连接，可靠）或数据报（无连接服务，不可靠），而**不可能在网络层同时存在这两种方式**

### 寻址与端口

#### 端口的作用

+ 端口（逻辑端口/软件端口）：是传输层服务访问点$TSAP$（传输层$T(Transport)$的服务访问点$SAP$），标识主机中的应用进程。
    + 在传输层的作用类似于$IP$地址在网络层的作用或$MAC$地址在数据链路层的作用
        + 只不过$IP$地址和$MAC$地址标识的是主机，而端口标识的是主机中的应用进程。 

    + 数据链路层的$SAP$是$MAC$地址，网络层的$SAP$是$IP$地址，传输层的$SAP$是端口。
    + 在协议栈层间的抽象的协议端口是软件端口，它与路由器或交换机上的硬件端口是完全不同的概念
        + 硬件端口是不同硬件设备进行交互的接口，而软件端口是应用层的各种协议进程与传输实体进行层间交互的一种地址
        + 传输层使用的是软件端口

+ 每一个端口有用于区分的端口号，只有本地意义，因特网中不同主机的相同端口号无联系。

#### 端口号

+ 端口号长度为$16bit$，能标识$65536=2^{16}$个不同的端口号。
+ 端口号按范围划分可以分为两类：
    + 服务端使用的端口号：
        + 熟知端口号（$0-1023$）：给$TCP/IP$最重要的一些应用程序，让所有用户都知道。
        + 登记端口号（$1024-49151$）：为没有熟知端口号的应用程序使用。
    + 客户端使用的端口号：仅在客户进程运行时才系统动态分配。

一些常用的熟知端口号如下：

|  应用程序   | 熟知端口号 |
| :---------: | :--------: |
| FTP数据连接 |     20     |
| FTP控制连接 |     21     |
|   TELNET    |     23     |
|    SMTP     |     25     |
|     DNS     |     53     |
|    TFTP     |     69     |
|    HTTP     |     80     |
|    POP3     |    110     |
|    SNMP     |    161     |
|    HTTPS    |    443     |

>   ~~Tomcat端口8080~~
>
>   ~~SSH端口22~~
>
>   ~~mongodb端口27017~~
>
>   ~~mysql端口3306~~
>
>   ~~redis端口6379~~
>
>   ~~顺便补充一个小知识，网页服务部署在本地80端口(HTTP)上可以浏览器打开localhost/启动项目~~

#### 套接字

在网络中通过$IP$地址来标识和区别不同的主机，通过端口号来标识和区分一台主机中的不同应用进程，端口号拼接到$IP$地址即构成套接字$Socket$在网络中采用发送方和接收方的套接字来识别端点。套接字，实际上是一个通信端点，即
$$
Socket=(IP_{Address}:Port)
$$
唯一标识了网络中的一个主机和它上面的一个进程。

如果有新的同样套接字的连接请求建立，则建立失败，不影响原有连接。

## UDP协议

用户数据报协议$UDP(User \;Datagram \;Protocol)$只在$IP$数据报服务之上添加了两个功能，即只有复用分用（接收方的传输层剥去报文首部后，能把这些数据正确交付到目的进程。通过目的端口实现）与差错检测功能。

### 主要特点

1. 无须建立连接，没有建立连接的时延

    +   $HTTP$使用$TCP$而非$UDP$，是因为对于基于文本数据的$Web$网页来说，可靠性是至关重要的

2. 无连接状态

    +   $UDP$不维护连接状态，也不跟踪这些参数
        +   因此，某些专用应用服务器使用$UDP$时，一般都能支持更多的活动客户机
    +   $TCP$需要在端系统中维护连接状态
        +   此连接状态包括接收和发送缓存、拥塞控制参数和序号与确认号的参数

3. 使用最大努力交付，而非保证可靠交付。所以不会对报文编号。

    +   ==因为$UDP$不保证可靠性，所以其可靠性由应用层完成。同时，应用开发者可根据应用的需求来灵活设计自己的可靠性机制。==

4. 面向报文（不对报文拆分，应用层给多长报文，$UDP$就会照样一次发送一个完整报文），适合一次性传输少量数据的网络应用。

    +   发送方$UDP$对应用层交下来的报文，在添加首部后就向下交付给$IP$层，==一次发送一个报文，既不合并，也不拆分，而是保留这些报文的边界==
    +   接收方$UDP$对$IP$层交上来$UDP$数据报，在去除首部后就==原封不动地交付给上层应用进程，一次交付一个完整的报文==
        +   ==因此报文不可分割，是$UDP$数据报处理的最小单位==
    +   因此，应用程序必须选择合适大小的报文
        +   若报文太长，$UDP$把它交给$IP$层后，可能会导致分片
        +   若报文太短，$UDP$把它交给IP层后，会使IP数据报的首部的相对长度太大，两者都会降低IP层的效率

5. ==无拥塞控制==，适合很多实时应用，实时应用延迟要求高，需要立即响应。
    +   应用层能更好地控制要发送的数据和发送时间
    +   网络中的拥塞不会影响主机的发送效率
        +   这意味着在网络中发生拥塞时，$UDP$并不会自动调整发送速率或进行流量控制，它会尽可能快地将数据包发送出去，无论网络的拥塞情况如何。
    
6. $UDP$支持一对一、一对多、多对一、多对多的交互通信。

7. ==首部开销小$UDP$只需要$8B$，而$TCP$需要$20B$。==

8. $UDP$的应用

    +   $UDP$常用于一次性传输较少数据的网络应用，如$DNS$、$SNMP$等
        +   因为对于这些应用，若采用$TCP$，则将为连接创建、维护和拆除带来不小的开销

    +   $UDP$也常用于多媒体应用（如$IP$电话、实时视频会议、流媒体等）
        +   显然，可靠数据传输对这些应用来说并不是最重要的，但$TCP$的拥塞控制会导致数据出现较大的延迟，这是它们不可容忍的。

### UDP数据报格式

![UDP数据报格式](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202308021348723.png)

$UDP$数据报包含两部分：

1.   $UDP$首部
2.   用户数据

$UDP$首部有$8B$，由$4$个字段组成，每个字段的长度都是$2B$，各字段意义如下：

+ 源端口号：需要对方回应时选用
    + 如果不需要回应，==可以不填，即是全$0$的。==
+ 目的端口号：==是必填的==
    + 分用时，如果找不到对应的目的端口号就丢弃该报文，并向发送方发送$ICMP$``端口不可达``差错报告报文。
+ $UDP$长度：整个$UDP$用户数据报的长度，==首部加上数据部分==
    + 最小值为$8$
    + 以字节为单位。不包括伪首部。
+ $UDP$检验和：检测整个$UDP$数据报是否有错（伪首部+首部+数据，而$IP$只检测首部），错就丢弃
    + 若不想校验则是全$0$
    + 若计算结果为全$0$则置为全$1$。

### UDP协议校验和

具体的$UDP$数据报格式如下：

![UDP具体格式](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202308021348527.png)

+ 伪首部只有在计算检验和时才出现，不向下传达也不向上提交，而只是为了计算校验和。
    + ==在计算校验和时，要在$UDP$数据报之前增加$12B$的伪首部，伪首部并不是$UDP$的真正首部。==
    + 只是在计算校验和时，临时添加在$UDP$数据报的前面，得到一个临时的$UDP$数据报

+ 其中的$17$代表封装$UDP$报文的$IP$数据报首部协议字段是$17$。
+ $UDP$长度是$UDP$首部$8B$加上数据长度，不包括伪首部。
+ $UDP$校验和的计算方法和$IP$数据报首部校验和的计算方法相似
    + 但不同的是，$IP$数据报的校验和只检验$IP$数据报的首部，但$UDP$的校验和则检查首部和数据部分。

+ 通过伪首部，还可以检查$IP$数据报的源$IP$地址和目的地址

发送端：

如果不使用校验和字段，则字段全部填充$0$。

1. 填上$12B$的伪首部。
2. $UDP$数据报要看成许多$16$位的字符串连接一起，全$0$填充数据部分末尾，使数据报成为偶数个字节。
    +   填充字节不发送
3. 采用二进制反码计算伪首部+首部+数据部分的总和。
4. 将此和的二进制取反码写入校验和字段。
5. 去掉伪首部进行发送。

接收端：

如果校验和字段计算结果恰好为$0$，则表示错误，字段全部填充$1$。

1. 填上伪首部，若不是偶数个字节还要在末尾加$0$。
2. 采用二进制反码计算伪首部+首部+数据部分的总和。
3. 如果结果全为$1$则无差错，否则出错，丢弃或交给上层并附上出错的警告。

![UDP校验](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202308021348766.png)

>   这种简单的差错校验方法的校错能力并不强，但它的好处是简单、处理速度快

## TCP协议

传输控制协议$TCP(Transmission \;Control\; Protocol)$对比用户数据报协议更复杂。

### TCP协议主要特点

1. $TCP$是面向连接的传输层协议，==$TCP$连接是一条逻辑连接==。
2. 每一条$TCP$连接只能有两个端点，所以连接是一对一的。
    +   相较而言，$UDP$支持一对一、一对多、多对一、多对多的交互通信。
3. $TCP$提供可靠有序的服务，保证传送的数据无差错、不丢失、不重复且有序
    +   使用确认机制实现
4. $TCP$提供全双工通信，允许通信双方的应用进程在任何时候都能发送数据，包含发送缓存，连接的两端都设有发送缓存和接收缓存，用来临时存放双向通信的数据
    +   发送缓存用来暂时存放以下数据
        +   发送应用程序传送给发送方$TCP$准备发送的数据
        +   $TCP$已发送但尚未收到确认的数据
    +   接受缓存用来暂时存放以下数据
        +   按序到达但尚未被接收应用程序读取的数据
        +   不按序到达的数据
5. $TCP$是面向字节流的
    +   虽然应用程序和$TCP$的交互是一次一个大小不等的数据块，但$TCP$把应用程序交下来的数据仅视为一连串的无结构的字节流

>   +   $TCP$和$UDP$在发送报文时所采用的方式完全不同
>       +   $UDP$报文的长度由发送应用进程决定
>       +   而$TCP$报文的长度则根据接收方给出的窗口值和当前网络拥塞程度来决定
>           +   如果应用进程传送到$TCP$缓存的数据块太长，$TCP$就把它划分得短一些再传送
>           +   如果太短，$TCP$也可以等到积累足够多的字节后再构成报文段发送出去

### TCP数据报格式

$TCP$传输的数据单位称为报文段。可以用来传输数据，也可以用来建立连接、释放连接、应答。首部长度为$4B$整数倍，默认最短为$20B$，报头最长为$60B$。

![TCP报文段格式](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202308021348020.jpg)

$TCP$的全部功能体现在其首部的各个字段中，各字段意义如下

+ 源端口和目的端口：各占用$2B$
    + 端口是传输层与应用层的服务接口
    + 传输层的复用和分用功能都要通过端口实现

+ 序号：占用$4B$
    + 范围为$0\sim2^{32}-1$，共$2^{32}$个序号
    + 在一个$TCP$连接中传送的字节流中的每一个字节都按顺序编号，序号字段的值表示本报文段所发送数据的第一个字节的序号
    + 例如
        + 一报文段的序号字段值是`301`，而携带的数据共有$100B$，表明本报文段的数据的最后一个字节的序号是`400`（序号`301〜400`）
        + 因此下一个报文段的数据序号应从`401`开始。

+ 确认号：占用$4B$
    + 期望收到对方下一个报文段的第一个数据字节的序号
    + ==若确认号为$N$，则证明到序号$N-1$为止的所有数据都已正确收到。==
    + 例如
        + $B$正确收到了 $A$发送过来的一个报文段，其序号字段是`501`，而数据长度是$200B $（序号`501〜700`）
        + 这表明$B$正确收到了$ A$发送的到序号`700`为止的数据
        + 因此$B$期望收到$A$的下一个数据序号是`701`，于是$B$在发送给$A$的确认报文段中把确认号置为`701`

+ 数据偏移/**首部长度**：占用$4B$
    + 这里不是$IP$数据报分片的那个数据偏移，而是表示首部长度
        + $TCP$首部中还有长度不确定的选项字段

    + $TCP$报文段的数据起始处距离$TCP$报文段的起始处有多远，即$TCP$报头的长度
    + 以$4B$为单位，即$1$个数值是$4B$，最大值为$15$，达到$TCP$首部的最大值$60B$。

+ 保留：占用$4B$，保留为今后使用，但目前应置为$0$。
+ 六个控制位，见下
+ 窗口：占用$2B$
    + 范围$0\sim2^{16}-1$
    + 指的是发送本报文段的一方的接收窗口，即现在允许对方还可以发送的数据量，防止对方发送过多数据导致自己无法接受。
    + 例如
        + 设确认号是`701`，窗口字段是`1000`
        + 这表明，从`701`号算起，发送此报文段的一方还有接收$1000$字节数据（字节序号为`701〜1700`）的接收缓存空间

+ 检验和：占用$2B$
    + 在计算校验和时，和$UDP$一样，要在$TCP$报文段的前面加上$12B$的伪首部
        + 只需将$UDP$伪首部的协议字段的`17`改成`6`， $UDP$长度字段改成$TCP$长度，其他的和$UDP $一样

+ 紧急指针： 占用$2B$
    + $URG=1$时才有意义，指出本报文段中数据部分的紧急数据的字节数。

+ 选项：占用长度可变
    + 最大报文段长度$MSS$、窗口扩大、时间戳、选择确认……
    + $TCP$最初只规定了一种选项，即最大报文段长度$MSS(Maximum\; Segment\; Size)$
    + $MSS$是$TCP$报文段中的数据字段的最大长度
        + 仅仅是数据字段

+ 填充：当首部长度不为$4$的整数倍就由填充部分填充$0$，到$4$字节的整数倍。

还有六个控制位，除了$PSH$和$RST$位都较重要：

1. 紧急位$URG$：$URG=1$时， 表明此报文段中有紧急数据，是高优先级的数据，应尽快传送，不用在缓存里排队。
    +   紧急数据都在数据报最前面，配合紧急指针字段使用，即==数据从第一个字节到紧急指针所指字节之间的数据就是紧急数据。==
2. 确认位$ACK$：$ACK=1$时确认号字段有效，在连接建立后所有传送的报文段都必须把$ACK$置为$1$。
3. 推送位$PSH(Push)$：$PSH=1$时，接收方尽快交付接收应用进程，不再等到缓存填满再向上交付
    +   如果没有$PSH$，一般都是接收方缓存满了之后再将数据发送到主机。
4. 复位位$RST(Reset)$：==$RST=1$时， 表明$TCP$连接中出现严重差错，必须释放连接，然后再重新建立传输链接。==
5. 同步位$SYN$：$SYN=1$时，表明是一个连接请求/连接接受报文
    +   此时若$ACK=0$代表这是一个**连接请求报文**
    +   若$ACK=1$代表这是一个**连接接收报文**。
6. 终止位$FIN(Finish)$：$FIN=1$时，表明此报文段文送方数据已发完，要求释放连接。

### TCP协议连接管理

TCP建立连接采用客户/服务器($C/S$)方式。但是实际上任何一台计算机都可能做服务器也可能做客户端。

TCP是面向连接的协议，因此每个TCP连接都有三个阶段：连接建立、数据传送和连接释放

在TCP连接建立的过程中，要解决以下三个问题

1.   要使每一方能够确知对方的存在。
2.   要允许双方协商一些参数（如最大窗口值、是否使用窗口扩大选项、时间戳选项及服务质量等）。
3.   能够对运输实体资源（如缓存大小、连接表中的项目等）进行分配

#### TCP三次握手（建立连接）

>   谢希仁---电子工业出版社---《计算机网络(第八版)》(408参考教材)中提到`three way handshake`(原译名`三次握手`)翻译为"`三报文握手`"更为准确(`P247`注)，因为这其实是==一次握手过程中的三次报文交换==
>
>   本笔记参考王道教材，这里选用`三次握手`译名，下`四次挥手（四报文握手）`同

![TCP建立连接](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202308021349354.png)

连接建立前，服务器进程处于`LISTEN `（收听）状态，等待客户的连接请求

1. 客户端的$TCP$先向服务器的$TCP$发送**连接请求报文段**，无应用层数据，然后$TCP$客户端进入同步已发送`SYN-SENT`状态。
    +   这个特殊报文段的首部中的同步位`SYN=1`，同时选择一个初始序号`seq=x`
    +   ==$TCP$规定，`SYN`报文段不能携带数据，但要消耗掉一个序号==
2. 服务端$TCP$接受到连接请求报文段后同意建立链接进入同步收到入`SYN-RCVD`状态，服务器端为该$TCP$连接分配缓存和变量，并向客户端返回**确认报文段**，允许连接，无应用层数据。
    +   在确认报文段中，把`SYN`位和`ACK`位都置$1$，确认号是`ack=x + 1`，同时也为自己选择一个初始序号`seq=y`
    +   ==确认报文段不能携带数据，但也要消耗掉一个序号==
3. 客户端接受确认报文段后变成已建立连接`ESTABLISHED`状态，为该$TCP$连接分配缓存和变量，并向服务器端返回**确认报文段的确认**，可以携带数据。
    +   确认报文段的`ACK`位置$1$，确认号`ack=y+1`，序号`seq=x+1`
    +   ==该报文段可以携带数据， 若不携带数据则不消耗序号。==
4. 服务端接受到报文段后变成已建立连接状态。
    +   接下来就可以传送应用层数据
    +   $TCP$提供的是全双工通信，因此==通信双方的应用进程在任何时候都能发送数据==

>   +   其中$seq$表示序号，指本报文的随机编号；$ack$表示确认号，指期待对方发送的报文的第一个序号。
>
>   +   确认位$ACK$：$ACK=1$时确认号字段有效，在连接建立后所有传送的报文段都必须把$ACK$置为$1$。
>   +   同步位$SYN$：$SYN=1$时，表明是一个连接请求/连接接受报文
>       +   此时若$ACK=0$代表这是一个**连接请求报文**
>       +   若$ACK=1$代表这是一个**连接接收报文**。

![TCP建立连接](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202308021349354.png)

若不指出为$1$，则代表其值为$0$。

1. 第一部分：
   + $SYN=1$：主机$A$要建立连接了。
   + $seq=x$（客户机指定）：后面没有数据。
2. 第二部分：
   + $SYN=1$：主机$B$同意主机$A$建立连接。
   + $ACK=1$：连接确认建立了，之后的$ACK$必须都置为$1$，表示开始同步。
   + $seq=y$（服务器指定）：设置初始序号，后面没有数据。
   + $ack=x+1$：表示期待对方放松的报文段的第一个字节，之前发送方$A$说发送的是第$x$位数据（虽然发送方是任意给出的），所以主机$B$要的是$x+1$位数据。
3. 第三部分：
   
   +   $SYN=0$：$SYN$只有在建立连接时才为$1$，其他时候均设为$0$
   
   + $ACK=1$：连接建立了，之后的$ACK$必须都置为$1$。
   + $seq=x+1$：主机$A$发送的报文段的第一个字节就是$x+1$。
   + $ack=y+1$：之前接收方$B$发送的是第$y$位数据（虽然接收方是任意给出的），所以主机$A$要的是$y+1$位数，对其确认。

值得注意的是$seq$的值是随机的，所以客户端的和服务器端的序列值可能相同

#### SYN洪泛攻击*

$SYN$洪泛攻击（$SYN Flood Attack$）是一种常见的网络拒绝服务（$DDoS$）攻击手法，旨在使目标服务器的$TCP$连接资源耗尽，从而导致正常用户无法建立新的连接或访问服务器。

由于三次握手时，服务器的资源在第二次握手完成时分配，而客户端的资源第三次握手完成时分配，可能导致反复确认与占用，产生$SYN$洪泛攻击。

==$SYN$洪泛攻击发生在$OSI$第四层。==记得这个就行

攻击原理：
1. 在TCP三次握手建立连接的过程中，客户端向服务器发送一个SYN（同步）包，服务器收到后会回复一个SYN+ACK（同步+确认）包，然后客户端再回复一个ACK（确认）包，完成连接的建立。
2. 在SYN洪泛攻击中，攻击者发送大量伪造的SYN包，但不回复服务器的SYN+ACK包。这使得服务器在等待客户端回复的ACK包的过程中，资源被耗尽，无法处理正常的连接请求。

攻击过程：
1. 攻击者发送大量的伪造IP地址的SYN包到目标服务器，每个SYN包都需要占用服务器一定的连接资源。
2. 服务器收到SYN包后，会为每个未建立的连接分配一些资源来处理可能的连接建立。
3. 由于攻击者发送大量的SYN包，服务器的连接资源被迅速耗尽，无法处理更多的连接请求。
4. 正常用户尝试建立连接时，服务器无法回复SYN+ACK包，导致连接超时或无法建立成功，从而造成服务不可用的情况。

防御措施：
1. SYN Cookies：服务器使用SYN Cookies技术，将一部分连接信息编码到SYN响应包中，从而避免建立连接时分配大量资源。只有在客户端回复ACK包时，服务器才会还原连接信息并建立连接。
2. 过滤恶意流量：使用防火墙或入侵防御系统（IDS/IPS）等设备，对入站流量进行过滤和监测，过滤掉可能是攻击流量的请求。
3. 流量限制和限速：限制来自单个IP地址的连接数，限制每个连接的请求速率，以减缓攻击者对服务器的影响。
4. CDN和反向代理：使用内容分发网络（CDN）或反向代理，将流量分散到多个服务器上，以分担攻击的影响。
5. 加强服务器硬件和网络设备：增加服务器的连接资源，使用更高性能的网络设备来处理大量的连接请求。

#### TCP四次挥手（连接释放）

每一条$TCP$连接的两个进程中的任何一个都能终止连接，连接结束后主机的资源将被释放。

![TCP释放连接](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202308021349416.png)

参与TCP连接的两个进程中的任何一个都能终止该连接

1. 客户端发送连接**连接释放报文段**，停止发送数据，主动关闭$TCP$连接，进入终止等待$1$(`FIN-WAIT-1` )状态。

    +   连接释放报文段的终止位`FIN = 1`，序号`seq = u`
        +   其中`u`等于客户端前面己传送过的数据的最后一个字节的序号加1
    +   ==连接释放报文段即使不携带数据，也消耗掉一个序号==
    +   $TCP$是全双工的，即可以想象为一条$TCP$连接上有两条数据通路， 发送连接释放报文段的一端不能再发送数据，即关闭了其中一条数据通路，但对方还可以发送数据

2. 服务端收到连接释放报文段后会回送一个**确认报文段**，此时服务器端进入关闭等待`CLOSE-WAIT`状态，客户到服务器这个方向的连接就释放了，不允许客户端再发送数据给服务器。

    +   确认位`ACK = 1`，确认号`ack = u+1`，序号`seq = v`
        +   其中`v`等于服务端前面己传送过的数据的最后一个字节的序号加1
    +   从客户机到服务器这个方向的连接释放了，==但服务器若发送数据，客户机仍要接收==，即从服务器到客户机这个方向的连接并未关闭

    +   客户端接受报文段后进入终止等待$2$(`FIN-WAIT-2` )状态。

3. 服务器发完数据，如果没有要向服务器发送的数据，就发出**连接释放报文段**，主动关闭$TCP$连接，进入最后确认`LAST-ACK`阶段。

    +   确认位`ACK = 1`，终止位`FIN = 1`，确认号`ack = u + 1`，序号`seq = w`
        +   由于在半关闭状态服务器可能又发送了一些数据，`v`等于服务端前面己传送过的数据的最后一个字节的序号加1

4. 客户端回送一个**确认报文字段**，服务器端接收后进入关闭状态。客户端等到时间等待计时器设置的$2MSL$（最长报文段寿命）后关闭服务器到客户这个方向，彻底关闭连接，进入关闭`CLOSED`状态。

    +   确认位`ACK = 1`，确认号`ack = u + 1`，序号`seq = w + 1`

注释：

1. 第一部分：
   + $FIN=1$：主机$A$要释放连接。
   + $seq=u$（随机）：后面可以有数据也可以没有数据。
2. 第二部分：
   + $ACK=1$：连接建立了，之后的$ACK$必须都置为$1$。
   + $seq=v$（随机）：$v=u+$第一部分数据长度$+1$，如果第一部分的确认报文没有数据就是$v=u+1$。
   + $ack=u+1$：之前发送方$A$发送的是第$u$位数据，所以主机$B$要的是$u+1$位数据（尽管此时$A$已经决定释放连接了）。
3. 第三部分：
   + $FIN=1$：主机$B$要释放连接。
   + $ACK=1$：连接建立了，之后的$ACK$必须都置为$1$。
   + $seq=w$（随机）：$w=v+$第二部分数据长度$+1$，如果第二部分的确认报文没有数据就是$w=v+1$。
   + $ack=u+1$：之前发送方$A$说发送的是第$u$位数据，所以主机$B$要的是$u+1$位数据（因为A直接不发数据了，所以第二段第三段的$ack$都是$u+1$）。
4. 第四部分：
   + $ACK=1$：连接建立了，之后的$ACK$必须都置为$1$。
   + $seq=u+1$：之前发的数据时第$u$位数据，$B$也要第$u+1$位数据，所以我发第$u+1$位数据。
   + $ack=w+1$：之前发送方$B$说发送的是第$w$位数据，所以主机$A$要的是$w+1$位数据。

>   ==关于连接和释放的题目，ACK、SYN、FIN 一定等于1==

为什么要等待$2MSL$时间？

1. 保证$A$发送的最后一个$ACK$报文段能发送到$B$，否则$B$服务器会接收不到$A$确认的信息，而$A$已经关闭无法重发确认报文段，从而$B$无法正常关闭。
2. 防止已失效的连接请求报文段传输到下一次的连接请求，干扰下一次的连接服务。

### TCP协议可靠传输

传输层使用的是$GBN$与$SR$的混合。

#### 校验

通过校验的方式来保证数据一致，其方式也是如$UDP$校验一样增加伪首部与校验和。

#### 序号

![image-20230802222526872](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202308022225980.png)

$TCP$报文传输时每个字节都会编上序号，一个字节占用一个序号，并按报文段的形式一起发送，报文段长度不定，根据$MTU$来定。

==序号字段指一个报文段第一个字节的序号。==

==序号建立在传送的字节流上，而不是报文段。==

虽然$TCP$面向字节，但是不是每个字节都要发回确认，而是在==发送一个报文段后才发回一个确认，确认号为报文段第一个字节的序号==，所以$TCP$是对**报文段**的确认机制。

#### 确认

==确认号是期望收到的下一个报文段数据的第一个字节的序号。==

>   例如，在图`5.9`中， 如果接收方B己收到第一个报文段，此时B希望收到的下一个报文段的数据是从第3个字节开始的，那么B发送给A的报文中的确认号字段应为3。

$TCP$缓存中的字节流按序传输后不会立刻在缓存中清除，而会等待接收方的确认字段，可以是单独确认也可以携带确认。

默认使用累积确认，即$TCP$只确认数据流中至第一个丢失字节为止的字节。

>   例如，在图`5.9`中，接收方B收到了 A发送的包含字节0〜2及字节6〜7的报文段。由于某种原因，B还未收到字节3〜5的报文段，此时B仍在等待字节3 (和其后面的字节)，因此B到A的下一个报文段将确认号字段置为3。

#### 重传

有两种事件会导致TCP对报文段进行重传：超时和冗余ACK

超时：

+ $TCP$每发送一个报文段就会设置一次计时器，$TCP$在重传时间内未收到确认就需要重传已发送的报文段。
+ 由于$TCP$下层互联网环境复杂，每次路由选择可能变化从而带来时延方差也很大，所以采用自适应算法：
  + 记录报文段发出时间和收到响应确认时间，称其差为报文段的往返时间$RTT$。
  + 根据$RTT$的测量值动态改变重传时间$RTT_s$（加权平均往返时间）。
      + $RTT_s$会随新测量$RTT$样本值的变化而变化
  + 从而超时计时器设计的超时重传时间$RTO$应该略大于$RTT_S$。
      + 但也不能大太多，否则当报文段丢失时，$TCP$不能很快重传，导致数据传输时延大
      + 新估计$RTT=(1-\alpha)\times$旧$RTT+\alpha\times$新$RTT$样本。

----

冗余$ACK$：

+ 超时触发重传存在的一个问题是超时周期往往太长。所幸的是，发送方通常可在超时事件发生之前通过注意所谓的冗余$ACK$来较好地检测丢包情况
+ 为了加快发现需要重传的报文段，可以采用冗余$ACK$（冗余确认/快重传），每当比期望序号大的失序报文段到达时，发送一个冗余$ACK$，指明下一个期待字节的序号。

>   例如，发送方A发送了序号为1、2、3、4、5的TCP报文段，其中2号报文段在链路中丢失，它无法到达接收方B。因此3、4、5号报文段对于B来说就成了失序报文段。TCP规定每当比期望序号大的失序报文段到达时，就发送一个冗余ACK， 指明下一个期待字节的序号。
>
>   在本例中，3、4、5号报文到达B，但它们不是B所期望收到的下一个报文，于是B就发送3个对1号报文段的冗余ACK，表示自己期望接收2号报文段。TCP规定当发送方收到对同一个报文段的3个冗余ACK时，就可以认为跟在这个被确认报文段之后的报文段已经丢失。
>
>   就前面的例子而言，当A收到对于1号报文段的3个冗余ACK时，它可以认为2号报文段巳经丢失，这时发送方A可以对2号报文执行重传，这种技术通常称为快速重传。

### TCP协议流量控制

$TCP$使用滑动窗口机制来完成流量控制，与数据链路层的滑动窗口类似。单位为字节。

在通信过程中，接收方会根据接收缓存的大小，动态调整发送方发送窗口的大小，即接收窗口$rwnd$（接受方设置确认报文段的窗口字段，将$rwnd$通知给发送方），发送方的发送窗口取接收窗口$rwnd$和拥塞窗口$cwnd$（根据当前网络拥塞程度而由发送发确定的窗口值，与网络带宽与时延相关）的最小值。

$A$向$B$发送数据，连接建立时，$B$告诉$A$：$B$的$rwnd=400B$，设每一个报文段$100B$，报文段序号初始值为$1$。

![TCP流量控制](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202308021349374.png)

$B$只有处理完接收窗口中的数据才能继续接收$A$的数据，发送$A$一个$rwnd$不为$0$的报文。

而如果这个告诉$A$接收窗口$rwnd$不为$0$的报文丢失了，$A$就一直会等待发送，$B$就会一直等待接收，从而产生死锁般的情况。

$TCP$为每一个连接设有一个持续计时器，只要$TCP$连接的一方收到对方的零窗口通知（即$rwnd=0$的通知）就启动持续计时器。

如果计时器设置的时间到期，$A$就会发送一个零窗口探测报文段，接收方收到探测报文段就会给出现在的窗口值。

如果窗口仍然是$0$，那么发送方就重新设置持续计时器。

>   传输层和数据链路层的流量控制的区别是：传输层定义端到端用户之间的流量控制，数据链路层定义两个中间的相邻结点的流量控制。另外，数据链路层的滑动窗口协议的窗口大小不能动态变化，传输层的则可以动态变化。

### TCP协议拥塞控制

出现拥塞条件：需求>可用资源。

当网络中有许多资源同时呈现供应不足时网络性能变坏，网络吞吐量将随输入负荷增大而下降。

拥塞控制与流量控制的区别

+   拥塞控制是让网络能够承受现有的网络负荷，是一个全局性的过程，涉及所有的主机、所有的路由器，以及与降低网络传输性能有关的所有因素
+   相反，流量控制往往是指点对点的通信量的控制，是个端到端的问题（接收端控制发送端），它所要做的是抑制发送端发送数据的速率，以便使接收端来得及接收
+   当然，拥塞控制和流量控制也有相似的地方，即它们都通过控制发送方发送数据的速率来达到控制效果

>   例如，某个链路的传输速率为10Gb/s，某大型机向一台PC以IGb/s的速率传送文件，显然网络的带宽是足够大的，因而不存在拥塞问题，但如此高的发送速率将导致PC可能来不及接收，
>
>   因此必须进行流量控制。但若有100万台PC在此链路上以IMb/s的速率传送文件，则现在的问题就变为网络的负载是否超过了现有网络所能承受的范围

因特网建议标准定义了进行拥塞控制的4种算法：慢开始、拥塞避免、快重传和快恢复

拥塞控制：防止过多的数据注入网络中。与流量控制不同的是它是面向全局的，是因为网络堵塞。形象来说拥塞控制就是为了控制路上堵车，而流量控制就是降低发车率。单位为$MSS$。

+ 接收窗口$rwnd$指接收方能接收缓存设置的值，并告知给发送方，反映接收方容量。
+ 拥塞窗口$cwnd$指发送方根据自己估算的网络拥塞程度而设置的窗口值，反映网络当前容量。

拥塞控制的假定：

1. 数据单方向发送，而另一个方向只发送确认，而不会捎带确认。
2. 接收方总是有足够大的缓存空间，因而发送窗口大小取决于拥塞程度。发送窗口=$\min$(接收窗口$rwnd$，拥塞窗口$cwnd$)。

>   这里假设接收方总是有足够大的缓存空间，因而发送窗口大小由网络的拥塞程度决定， 也就是说，可以将发送窗口等同为拥塞窗口

#### 慢开始与拥塞避免

![慢开始与拥塞避免](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202308021350127.png)

$cwnd$初始值是$1$，指一个最大报文段长度$MSS$。

传输轮次指发送了==一批报文段==并收到其确认的时间，一般指一个往返时延$RTT$。可能一次性传输多个报文。

1. 最开始是慢开始算法，一步步试探网络拥塞，开始时以$2$的指数形式增长。
2. $ssthresh$的意思是慢开始门限，代表从这个地方注入的报文段就比较多了，需要开始慢速增加了。
3. 拥塞窗口超过慢开始门限后进行拥塞避免算法，之后一段都是线性增长，每次增加$1$，直至达到网络拥塞状态。
4. 当网络开始拥塞时，进行乘法减小，瞬间将$cwnd$设置为$1$，同时调整原来的$ssthresh$的值到之前达到网络拥塞状态前值的$1/2$，（这里是$24$降到$12$），但是不能小于$2$。这样就能让拥塞的路由器能快速把队列中积压的分组处理完。
5. 重复以上步骤，但是注意此时$ssthresh$变了之后线性增长的转折点也变了。所以最后拥塞窗口会波动逼近适合当前网络拥塞状态的窗口值。

根据cwnd的大小执行不同的算法，可归纳如下：

+   当`cwnd<ssthresh`时，使用慢开始算法。 
+   当`cwnd>ssthresh时`，停止使用慢开始算法而改用拥塞避免算法。
+   当`cwnd = ssthresh`时，既可使用慢开始算法，又可使用拥塞避免算法（通常做法）。

<span style="color:orange">注意：</span>当慢开始进行指数增长时，当$2cwnd>ssthresh$时，则一个$RTT$后$cwnd=ssthresh$，不会让慢开始的拥塞窗口超过阈值。

>   拥塞避免并不能完全避免拥塞。利用以上措施要完全避免网络拥塞是不可能的。拥塞避免是指在拥塞避免阶段把拥塞窗口控制为按线性规律增长，使网络比较不容易出现拥塞

#### 快重传与快恢复

快重传（冗余$ACK$）在$TCP$协议可靠传输中已经提到过。

![快重传与快恢复](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202308021350372.png)

这里和上面的慢开始和拥塞避免的一开始步骤差不多，都是先指数增长再转变为线性增长。

不同的点是快重传和快恢复算法是在收到连续的$ACK$确认之后执行，这里的$ACK$就是冗余$ACK$，冗余$ACK$的特点是如果多次对某一段请求的数据没有被收到，达到一定数目，一般为三个冗余$ACK$之后就会立即执行重传。

但是此时只是降到现在$cwnd$的一半，再重新线性增长。而不是像慢开始和拥塞避免的从头开始，这就是快恢复。

一般而言$TCP$建立连接和网络超时时使用慢开始和拥塞避免算法；当发送方接收到冗余$ACK$时使用快重传和快恢复算法。
