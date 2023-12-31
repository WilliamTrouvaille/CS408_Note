# EX-CN相关总结

## 强化梳理

### 考频

![image-20230926220311520](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202309262203795.png)

大题主要考点

![23September26-220943-1695737383-db2c2dfc-7533-464f-a211-b9ae04b94958](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202309262210682.png)

可见现在主要考查各层之间的混合

### 第一章 计算机网络体系结构

主要靠选择题

选择题考点分布：

+   【2010】网络体系结构所描述的内容
+   【2009】【2011】【2013】【2014】【2016】【2019】【2021】【2022】层次顺序及层次功能
+   【2017】PDU=SDU+PCI，求应用层数据传输效率
+   【2020】协议三要素

>   基本都在考体系结构与参考模型，计算机网络概述还没考过

### 第二章

选择题考点分布：

+   【2011】【2013】【2015】【2021】编码与调制
+   【2009】【2014】【2017】【2016】【2023】奈氏准则和香农定理
+   【2010】【2013】【2020】【2023】数据交换方式（常考数据传输总用时）
+   【2012】【2018】接口特性
+   【2019】传输介质（双绞线）
+   【2020】不同层次设备是否能隔离冲突域/广播域

### 第三章 数据链路层

选择题考点分布：

+   【2023】差错控制
+   【2009】【2012】【2011】【2014】【2015】【2018】【2019】【2020】【2023】滑动窗口（GBN：3）
    （SR：1）（停等：2）
+   【2009】【2011】【2013】【2014】【2015】【2017】【2018】【2019】【2020】【2023】介质访问控制
    （CSMA：1）（802.3的CSMA/CD：3）（802.11的CSMA/CA：4）（CDMA：1）
+   【2016】最短帧长
+   【2012】以太网的MAC协议
+   【2013】HDLC协议
+   【2009】【2013】【2014】【2015】【2016】链路层设备

>   近几年常考无线局域网

### 第四章 网络层

选择题考点分布：

+   【2016】【2010】【2017】路由算法与路由协议
+   【2011】【2010】【2017】【2012】【2016】【2019】【2021】【2022】【2023】子网及子网划分
+   【2016】【2017】特殊IP地址
+   【2021】IP分组分片
+   【2012】【2010】协议：ARP、ICMP
+   【2018】路由聚合
+   【2011】【2015】【2018】路由选择与转发（拓扑/路由表）
+   【2010】【2012】网络层设备
+   【2022】 SDN
+   【2023】IPv6

>   IP组播、移动IP没考过

### 第五章 传输层

选择题考点分布：

+   【2014】UDP协议
+   【2021】UDP与TCP报文段
+   【2018】端口与寻址
+   【2010】【2011】【2013】【2019】【2020】【2021】【2022】连接管理
+   【2009】【2010】【2015】【2017】【2020】【2022】拥塞控制
+   【2009】【2011】【2013】【2019】可靠传输（考查序号确认序号）

### 第六章 应用层

选择题考点分布：

+   【2019】网络应用模型
+   【2010】【2016】【2018】【2020】DNS
+   【2009】【2017】FTP
+   【2012】【2013】【2015】【2018】电子邮件中的协议（主要考SMTP）
+   【2014】【2015】【2022】WWW与HTTP

>    WWW与HTTP需要熟练掌握，常会放在大题里考查

## 相关总结

### 计算公式

>   需先注意是否是电气属性：一般带电气属性的传输单位中$1K=1000$，而计算机的存储单位中$1K=2^{10}=1024$

1.   信道利用率
     $$
     U=\dfrac{T_D}{T_D+2RTT+T_A}
     $$
     其中，$T_D$表示发送周期中有效地发送数据所占时间，$RTT$表示单程往返时间，$T_A$表示确认帧传输时间

     当忽略确认帧传输时间$T_A$时，计算公式为
     $$
     U=\dfrac{L/C}{T}\\
     T=L/C+2RTT
     $$
     其中，$L$表示发送周期中发送$L$比特的数据(==帧长==)，$C$表示发送方数据传输率，$T$表示发送周期(发送时延+2*单程往返时间,其中发送时延可以使用$L/C$计算)，$RTT$表示单程往返时间

2.   奈式准则
     $$
     B=2W\\
     R=2W\log_2V
     $$

3.   香农$Shannon$​定理
     $$
     R=Wlog_2{(1+S/N)}
     $$

4.   海明码中确定校验码位数r的海明不等式：
     $$
     2^r\geqslant k+r+1
     $$

5.   GBN窗口长度
     $$
     1\leqslant W_T\leqslant 2^n-1
     $$
     

### 报文段

![image-20230926221201259](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202309262212457.png)

1.   以太网的MAC帧：以太网帧附加信息$18B$，规定帧最短为$64B$，所以数据最短为$46B$，规定数据部分最长为$1500B$，所以以太网帧最长为$1518B$。

     ![image-20230730121337884](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307301213026.png)

     + 物理层会插入前导码，用于接收端与发送端时钟同步
         + 包括==前同步码（用于快速实现$MAC$帧的比特同步）==与==帧开始定界符（表示后面的信息就是$MAC$帧）=两个部分。=

     + 目的地址和源地址均为$MAC$地址。
         + 通常使用$6B=48bit$

     + 类型表示$IP$数据报的类型，表示数据应该交给哪个协议实体处理。
         + 固定$2B$

     + 数据包括高层的协议信息
         + $46\sim 1500\; bit$
         + 最小值为$46$是因为==$CSMA/CD$算法规定以太网最小帧长为$64$字节==，所以数据最小就是$64-6-6-2-4=46$字节
         + 同样规定最长为$1500$字节。

     + 填充用于帧长太短来填充到$64$字节
         + 长度为$0\sim46$字节。

     + 校验码$FCS$：校验范围从目的地址到数据端末尾，采用$32$位循环冗余码$CRC$，但是不用校验$MAC$帧的前导码。
         + 固定$4B$

     + 以太网帧没有结束定界符是因为以太网采用曼彻斯特编码，如果与数据必然是电压变化的，而如果电压没有变化就代表已经结束，所以不需要结束定界符
         + 所以图`3.26`只有帧开始定界符

     + 但是$MAC$帧也要加尾部。
         + 在数据链路层上，帧既要加首部，也要加尾部

2.   $IEEE802.11$的$MAC$帧格式

     ![56226958-dc43-48e7-bda4-ae8d06c88ebb](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202309291639618.jpg)

     只需要掌握$4$个地址字段，其他字段不是考试重点

     地址1是直接接收数据帧的结点地址，地址2是实际发送数据帧的结点地址

3.   IP数据报

     ![IP数据报具体格式](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307311056592.png)

     |              名称               |                             注释                             |            大小             |
     | :-----------------------------: | :----------------------------------------------------------: | :-------------------------: |
     |          版本 Version           |                         ipv4或者ipv6                         |             4位             |
     |          首部长度 IHL           | ==单位为4字节==(即首部总长=首部长度字段值$\times4B$,下同)，同时因为IP数据报固定长度（最小值）为20字节，所以此处最小值为5，即二进制的0101 |             4位             |
     |       区分服务 DSCP + ECN       |                 希望获得哪种服务，用的比较少                 |             8位             |
     |       总长度 Total Length       | 首部和数据之和的长度，==单位为1字节==，最大为$2^{16}-1=65535B$，但是实际上永远不会达到该长度，因为有$MTU$的限制 |            16位             |
     |       标识 Identification       | 就是一个计数器，用来表示是哪一个数据报的分片，==同一个数据报的分片标识相同== |             8位             |
     |           标志 Flags            | 3bit，用来表示是否分片和分片是否结束；中间位DF（Don't Fragment）为1表示禁止分片，如果是0代表允许分片；最低位MF（More Fragment）为1表示后面还有分片，如果为0表示最后一片分片 | 3位，但实际有用的只有后两位 |
     |     片偏移 Fragment Offset      | 用来标记分片之后，该分片在原来的数据报的位置，==以8字节为单位== |            13位             |
     |      生存时间 Time To Live      | 即TTL，每经过一个路由器TTL-1，等于0时自动放弃，根据系统不同默认的TTL不同，为了防止无法传输的数据报在链路中无限传输 |             8位             |
     |          协议 Protocol          | 用来标记数据部分协议名的字段值，如ICMP：1；IGMP：2；==TCP：6==；EGP：8；IGP：9；==UDP：17==；IPv6：41；ESP：50；OSPF：89 |             8位             |
     |   首部检验和 Header Checksum    |  检验首部的字段是否出错，不包括数据部分，出错就丢弃此数据报  |            16位             |
     |    源地址 Source IP Address     |                         发送方ip地址                         |            32位             |
     | 目的地址 Destination IP Address |                         接收方ip地址                         |            32位             |
     |        可选字段 Options         |                      用来排错等安全检测                      |      可在0-40字节之间       |
     |              填充               |           将数据报对齐成4字节的整数倍，数值全部为0           |   未知，根据可选字段来定    |

     ==数据部分永远在 4 字节的整数倍开始==，这样在实现 IP 协议时较为方便。

     >   在IP数据报首部中有三个关于长度的标记，==首部长度、总长度、片偏移，基本单位分别为4B、1B、8B== （需要记住）。题目中经常会出现这几个长度之间的加减运算。另外，读者要熟悉IP数据报首部的各个字段的意义和功能，但不需要记忆IP数据报的首部，正常情况下如果需要参考首部，题目都会直接给出。第5章学到的TCP、UDP的首部也是一样的

4.   UDP数据报

     ![UDP数据报格式](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202308021348723.png)

     $UDP$首部有$8B$，由$4$个字段组成，每个字段的长度都是$2B$，各字段意义如下：

     + 源端口号：需要对方回应时选用
         + 如果不需要回应，==可以不填，即是全$0$的。==
     + 目的端口号：==必填==
         + 分用时，如果找不到对应的目的端口号就丢弃该报文，并向发送方发送$ICMP$``端口不可达``差错报告报文。
     + $UDP$长度：整个$UDP$用户数据报的长度，==首部加上数据部分==
         + 最小值为$8$
         + 以字节为单位。不包括伪首部。
     + $UDP$检验和：检测整个$UDP$数据报是否有错（伪首部+首部+数据，而$IP$只检测首部），错就丢弃
         + 若不想校验则是全$0$
         + 若计算结果为全$0$则置为全$1$。

     具体的$UDP$数据报格式如下：

     ![UDP具体格式](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202308021348527.png)

5.   TCP报文段

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
         + 以$4B$位单位，即$1$个数值是$4B$，最大值为$15$，达到$TCP$首部的最大值$60B$。

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

     1. 紧急位$URG$：$URG=1$时， 标明此报文段中有紧急数据，是高优先级的数据，应尽快传送，不用在缓存里排队。
         +   紧急数据都在数据报最前面，配合紧急指针字段使用，即==数据从第一个字节到紧急指针所指字节之间的数据就是紧急数据。==
     2. 确认位$ACK$：$ACK=1$时确认号字段有效，在连接建立后所有传送的报文段都必须把$ACK$置为$1$。
     3. 推送位$PSH(Push)$：$PSH=1$时，接收方尽快交付接收应用进程，不再等到缓存填满再向上交付
         +   如果没有$PSH$，一般都是接收方缓存满了之后再将数据发送到主机。
     4. 复位位$RST(Reset)$：==$RST=1$时， 表明$TCP$连接中出现严重差错，必须释放连接，然后再重新建立传输链接。==
     5. 同步位$SYN$：$SYN=1$时，表明是一个连接请求/连接接受报文
         +   此时若$ACK=0$代表这是一个**连接请求报文**
         +   若$ACK=1$代表这是一个**连接接收报文**。
     6. 终止位$FIN(Finish)$：$FIN=1$时，表明此报文段文送方数据已发完，要求释放连接。

6.   HTTP报文

     $HTTP$报文分为请求报文和响应报文，因为其面向文本$Text-Oriented$，所以报文中的每一个字段都是$ASCII$码串。

     ![image-20230803220326089](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202308032203189.png)

     HTTP请求报文和响应报文都由三个部分组成。从图6.14可以看出，这两种报文格式的区别就是开始行不同

     + 请求报文与响应报文的第一行叫做开始行，用于互相区分是请求报文还是响应报文。

         + 在请求报文中的开始行称为请求行，而在响应报文中的开始行称为状态行

         + 开始行的三个字段之间都以空格` `分隔，最后的`CR`和`LF`分别代表`回车`和`换行`。

         + 请求报文中的`方法`字段是指命令，就是对所请求的对象进行什么操作，如获取/删除等等。

             ![image-20230803222313833](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202308032223905.png)

         + $URL$就是资源标识符。

         + `状态码`，表明服务器当前的状态:

             + $1xx$表示通知信息的，如请求收到了或正在处理。
             + $2xx$表示成功，如接受或知道了。
             + $3xx$表示重定向，如要完成请求还必须采取进一步的行动。
             + $4xx$表示客户的差错，如请求中有错误的语法或不能完成。
             + $5xx$表示服务器的差错，如服务器失效无法完成请求。

         + `版本`是指使用的是什么版本的$HTTP$协议。

         + $CRLF$标识一行的结束，分别为回车和换行。同时，在整个首部行结束时，为了区别首部行和实体主体还会有一行单独的$CRLF$。

     + 首部行用于说明浏览器、服务器和报文主体的一些信息。

         + 首部可以有几行，但也可以不使用
         + 在每个首部行中都有首部字段名和它的值，每一行在结束的地方都需要`回车`和`换行`
         + 整个首部行结束时，还有一个`换行`空行将首部行和后面的实体主体分开

     + 实体柱体：请求报文一般不用，响应报文也可能没有。

### 特殊IP地址

![image-20230926221218392](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202309262212564.png)

### 重要协议

![23July31-213458-1690810498-1ba23eeb-5227-4873-b012-efb0d89c90a9](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202309262218142.png)

1.   物理层：往往容易忽视
     1.  **EIA-232C（也称为RS-232C）**：
         +   **定义**：用于定义计算机和通信设备之间的串行通信接口。
         +   **特点**：EIA-232C使用串行通信，通过一对线（发送线和接收线）在设备之间传输数据。它定义了电压级别、数据位数、停止位等通信参数。
     2.  **EIA/TIA RS-449（也称为EIA-449或TIA-449）**：
         +   **定义**：用于定义数据通信设备之间的串行通信接口。
         +   **特点**：RS-449是RS-232的后继标准，提供了更高的数据传输速率和更远的通信距离。它使用差分信号传输，减少了电磁干扰的影响，因此适用于长距离通信。
     3.  **CCITT的X.21**：
         +   **定义**：用于定义数字通信接口。
         +   **特点**：X.21标准通常用于连接数字通信设备，例如调制解调器和数据终端设备。它规定了物理、电气和功能特性，以便实现可靠的数字通信。X.21接口通常采用15针或25针连接器。
2.   数据链路层：封装成帧、差错控制、流量控制和传输管理
     1.   停止-等待协议$SW$
     2.   后退$N$帧协议$GBN$
     3.   选择重传协议$SR$
     4.   $ALOHA$协议
          1.   纯$ALOHA$协议
          2.   时隙$ALOHA$协议
     5.   $CSMA$协议
          1.   坚持$CSMA$
          2.   非坚持$CSMA$
          3.   $p-$坚持$CSMA$
     6.   $CSMA/CD$协议
     7.   $CSMA/CA$协议
     8.   $HDLC$协议
     9.   轮询协议
     10.   令牌传递协议
     11.   $MAC$协议：特点是无连接，不可靠，不确认，不对帧编号
     12.   广域网$PPP$协议
3.   网络层：把网络层的协议数据单元(分组)从源端传到目的端，为分组交换网上的不同主机提供通信服务
     1.   路由信息协议$RIP$：属于内部网关协议$IGP$，是一种分布式的基于**距离-向量路由算法**的路由选择协议，最大的优点是简单。
          +   在距离-向量路由算法中，所有结点都定期地将它们的整个路由选择表传送给所有与之直接相邻的结点。
              + 要求网络中的每一个路由器都维护从它到其他每一个目的网络的唯一最佳路径（即一组距离）。
              + 这种路由选择表包含：
                  + 每条路径的目的地（另一结点）。
                  + 路径的代价（也称距离）
                      + 距离通常指跳数，即从源端口到目的端口所经过的路由器个数，经过一个路由器跳数$+1$，与路由器直接连接的网络距离为$1$。$RIP$允许一条路最多只能包含$15$个路由器，所以$16$表示网络不可达。
          +   ==$RIP$协议是应用层协议，使用$UDP$传送数据。==
     2.   开放最短路径优先协议$OSPF$：属于内部网关协议$IGP$，是基于**链路状态路由算法**的路由选择协议。
          +   $OSPF$协议是网络层（或传输层）协议，使用$IP$数据报传输。
     3.   IP协议
     4.   网络地址转换协议$NAT$：将专用网络地址（如内部网络$Intranet$）转换为公用地址（如因特网$Internet$），从而对外隐藏内部管理的$IP$地址
     5.   虚拟专用网$VPN$：利用公用的互联网作为本地各专用网之间的通信载体
     6.   地址解析协议$ARP$：完成主机或路由器$IP$地址到$MAC$地址的映射
     7.   动态主机配置协议$DHCP$：用于给主机动态地分配IP地址，它提供了即插即用的联网机制，这种机制允许一台计算机加入新的网络和获取IP地址，而不用手工参与
          +   ==$DHCP$是应用层协议==，它是基于$UDP$的。
     8.   网际控制报文协议$ICMP$：提高IP数据报交付成功的机会提高IP数据报交付成功的机会
     9.   边界网关协议$BGP$：属于外部网关协议$EGP$，自治系统之间所使用的路由选择协议，用于不同自治系统的路由器之间交换路由信息，并负责为分组在不同自治系统之间选择最优的路径
     10.   网际组管理协议$IGMP$：让==连接在本地局域网上的多播路由器==知道本局域网上是否还有主机的某个进程参加或退出了某个多播组，以便把组播数据报用最小代价传送给所有组成员。
     11.   组播路由协议：目的是找出以源主机为根节点的组播转发树。
4.   传输层：为端到端连接提供可靠的传输服务，为端到端连接提供==流量控制、差错控制、服务质量、数据传输管理==等服务。 
     1.   用户数据报协议$UDP(User \;Datagram \;Protocol)$：只在$IP$数据报服务之上添加了两个功能，即只有复用分用（接收方的传输层剥去报文首部后，能把这些数据正确交付到目的进程。通过目的端口实现）与差错检测功能。
          1.   无须建立连接，没有建立连接的时延

               +   $HTTP$使用$TCP$而非$UDP$，是因为对于基于文本数据的$Web$网页来说，可靠性是至关重要的
          2.   无连接状态

          3.   使用最大努力交付，而非保证可靠交付。所以不会对报文编号。

               +   ==因为$UDP$不保证可靠性，所以其可靠性由应用层完成。同时，应用开发者可根据应用的需求来灵活设计自己的可靠性机制。==

          4.   面向报文（不对报文拆分，应用层给多长报文，$UDP$就会照样一次发送一个完整报文），适合一次性传输少量数据的网络应用。

               +   应用程序必须选择合适大小的报文

          5.   ==无拥塞控制==，适合很多实时应用，实时应用延迟要求高，需要立即响应。

          6.   $UDP$支持一对一、一对多、多对一、多对多的交互通信。

          7.   ==首部开销小$UDP$只需要$8B$，而$TCP$需要$20B$。==
     2.   传输控制协议$TCP(Transmission \;Control\; Protocol)$
          1.   $TCP$是面向连接（==虚连接==）的传输层协议，==$TCP$连接是一条逻辑连接==。
          2.   每一条$TCP$连接只能有两个端点，所以连接是一对一的。
               +   相较而言，$UDP$支持一对一、一对多、多对一、多对多的交互通信。
          3.   $TCP$提供可靠有序的服务，保证传送的数据无差错、不丢失、不重复且有序
               +   使用确认机制实现
          4.   $TCP$提供全双工通信，允许通信双方的应用进程在任何时候都能发送数据，包含发送缓存，连接的两端都设有发送缓存和接收缓存，用来临时存放双向通信的数据
               +   发送缓存用来暂时存放以下数据
                   +   发送应用程序传送给发送方$TCP$准备发送的数据
                   +   $TCP$已发送但尚未收到确认的数据
               +   接受缓存用来暂时存放以下数据
                   +   按序到达但尚未被接收应用程序读取的数据
                   +   不按序到达的数据
          5.   $TCP$是面向字节流的
               +   虽然应用程序和$TCP$的交互是一次一个大小不等的数据块，但$TCP$把应用程序交下来的数据仅视为一连串的无结构的字节流
5.   应用层
     1.   文件传送协议$FTP$
          +   默认数据传输端口$20$，控制端口$21$
          +   ==使用$TCP$可靠的传输服务==
     2.   简单文件传送协议$TFTP$
     3.   远程终端协议$TELNET$
     4.   超文本传输协议HTTP：定义了浏览器即**万维网客户进程**怎样向万维网服务器请求文档，以及服务器如何传输文档。
          +   服务器监听$TCP$运行于`80`端口，当监听到连接请求后便与浏览器建立$TCP$连接
     5.   简单邮件传送协议SMTP：==$SMTP$使用$TCP$连接，端口号为`25`==
     6.   多用途网际邮件扩充MIME：改善$SMTP$发送数据的缺点，是$SMTP$的功能性扩展。
          +   ==MIME邮件可在现有的电子邮件程序和协议下传送==
     7.   邮局协议$POP$，现在一般是$POP3$：建立于$TCP$连接上，端口号是$110$，使用$C/S$模型。
     8.   网际报文存取协议IMAP：提供创建文件夹，移动邮件、查询邮件等连接命令，从而维护了会话用户的状态信息。
          +   适用于低带宽的情况，避免取回不想取回的大数据。
     9.   动态主机配置协议DHCP

### 输入域名后的路由转发过程

![23September26-221718-1695737838-74b3a424-1e43-4216-b11c-4b853bd26c1a](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202309262217922.png)

### 三个表

1.   转发表
2.   ARP表
3.   路由表

### 设备

#### 物理层设备

##### 中继器RP/转发器

也称为转发器，仅作用于信号电气部分，中继器的主要功能是将信号整形并放大再转发出去，以消除信号经过一长段电缆后而产生的失真和衰减，使信号的波形和强度达到所需要的要求，进而扩大网络传输的距离

其原理是信号再生，而非简单地将衰减的信号放大

中继器的网络特性

+   中继器是用来扩大网络规模的最简单廉价的互连设备
+   中继器两端的网络部分是网段，而不是子网，==使用中继器连接的几个网段仍然是一个局域网==
+   中继器若出现故障，对相邻两个网段的工作都将产生影响
+   由于中继器工作在物理层，因此它==不能连接两个具有不同速率的局域网==。
    +   如果某个网络设备具有存储转发的功能，那么可以认为它能连接两个不同的协议
    +   如果该网络设备没有存储转发功能，那么认为它不能连接两个不同的协议
    +   中继器没有存储转发功能，因此它不能连接两个速率不同的网段，中继器两端的网段一定要使用同一个协议。

中继器的两端：

+ 网段而非子网。使用中继器连接的几个网段仍然是一个局域网。
+ 类型相同。
+ 网络速度相同。
+ 只负责发送而不关心是否有问题。
+ 可以连不同媒体也可以连相同媒体。
+ 不会存储转发所以一定要协议相同。
+ $5-4-3$规则
    + 采用粗同轴电缆的$10BASE5$以太网规范
    + $5$是指==不能超过五个网段==
    + $4$是指在这些网段中的==物理层网络设备（中继器，集线器）最多不超过四个==
    + $3$是指这些网段中==最多只有三个网段挂有计算机==。

<span style="color:orange">注意：</span>放大器和中继器都起放大作用，只不过放大器放大的是模拟信号，原理是将衰减的信号放大，而中继器放大的是数字信号，原理是将衰减的信号整形再生。

##### 集线器Hub

集线器实质上是一个多端口的中继器，功能类似。

当Hub工作时，一个端口接收到数据信号后，由于信号在从端口到Hub的传输过程中已有衰减，所以Hub便将该信号进行整形放大，使之再生到发送时的状态，紧接着转发到其他所有（除输入端口外）处于工作状态的端口

如果同时有两个或多个端口输入，那么输出时会发生冲突，致使这些数据都无效

+ 在网络中只起信号放大和转发作用，目的是扩大网络的传输范围
+ 而不具备信号的定向传送能力，即信息传输的方向是固定的，是一个标准的共享式设备
    + 广播
+ 因为是广播所以有大的冲突域，==不能分割冲突域==
    + 所以多少个计算机连入集线器，同时工作时就平分集线器拥有的带宽。
    + 一个带宽为10Mb/s的集线器上连接了 8台计算机，当这8台计算机同时工作时，每台计算机真正所拥有的带宽为10/8Mb/s=1.25Mb/s

+ 同时只能有两个设备进行通讯，只会传输信号。
+ 只能在半双工状态下工作。
    + 网络的吞吐率因而受到限制

+ 集线器连接的网络拓扑结构上属于星型。
+ 用集线器连接的工作站集合同属一个冲突域，也同属一个广播域
+ 当集线器的一个端口收到数据后立刻从除输入端口外的所有端口广播出去。

==以太网上的集线器默认是$100Base-T$==，即传输速率为$100Mb/s$。

#### 数据链路层设备

##### 网桥Bridge

+   多格以太网通过网桥连接后就形成了更大的以太网，原来的每个以太网就是一个网段。
    +   由于各网段相对独立，因此==一个网段的故障不会影响到另一个网段的运行==
+   可以互联不同的物理层、不同的$MAC$子层及不同速率的以太网。
+   网桥工作在数据链路层的$MAC$子层，可以使以太网各网段成为隔离开的碰撞域（又称冲突域）。
    +   如果把网桥换成工作在物理层的转发器，那么就没有这种过滤通信量的功能
+   当网桥收到一个帧时，不会广播此帧，而是先检测该帧的目的$MAC$地址，然后确定转发到哪一个接口或丢弃（过滤）。
+   网桥根据MC帧的目的地址对帧进行转发和过滤
    +   ==当网桥收到一个帧时，并不向所有接口转发此帧，而是先检查此帧的目的MAC地址，然后再确定将该帧转发到哪一个接口，或者是把它丢弃（即过滤）。==
+   网桥的优点
    1. 过滤通信量，增大吞吐量。
    2. 扩大了物理范围。
    3. 提高了可靠性。
    4. 可互联不同物理层、不同$MAC$子层和不同速率的以太网。
+   网桥的缺点
    1. 增加时延。
    2. $MAC$子层没有流量控制功能（流量控制需要编号机制，而编号机制在$LLC$子层）。
    3. 不同$MAC$子层的网段连接需要帧格式转换。
    4. 适用于少数据量的局域网，否则会出现广播风暴。
+   网桥的种类
    + 透明网桥：指以太网上的站点并不知道所发送的帧将经过哪几个网桥
        + 是一个即插即用的设备
        + 通过自学习的方式来寻找和更新路径。

    + 源路由网桥：在发送帧时，把详细的最佳路由信息（路由最少/时间最短）放在帧的首部中
        + 源站以广播的方式向欲通信的目的站发送多个发现帧，目的站再根据每一个发现帧的路径发送响应帧，从而以枚举的方式获得最优路径。


>   联系和区别
>
>   + 使用源路由网桥可以利用最佳路由。若在两个以太网之间使用并联的源路由网桥，则还可使通信量较平均地分配给每个网桥。采用透明网桥时，只能使用生成树，而使用生成树一般并不能保证所用的路由是最佳的，也不能在不同的链路中进行负载均衡。
>   + 透明网桥和源路由网桥中提到的最佳路由并不是经过路由器最少的路由，而可以是发送帧往返时间最短的路由，这样才能真正地进行负载平衡，因为往返时间长说明中间某个路由器可能超载了，所以不走这条路，换个往返时间短的路走。

##### 局域网交换机Switch/以太网交换机

+   交换机种类

    +   直通式交换机：查完目的地址（$6B$）直接转发
        +   延迟小，可靠性低，无法支持具有不同速率的端口的交换。
    +   存储转发式交换机：将帧放入高速缓存，并检查是否正确，正确则转发，错误则丢弃
        +   延迟大，可靠性高，可以支持具有不同速率的端口的交换。

+   交换机特点

    + 以太网交换机的每个端口都直接与单台主机相连（普通网桥的端口往往连接到以太网的一个网段)，并且一般都工作在全双工方式。
    + 交换机的总带宽是各端口带宽之和。
    + 以太网交换机能同时连通许多对端口，使每对相互通信的主机都能像独占通信媒体那样，无碰撞地传输数据。支持多对用户同时通信。
    + 以太网交换机也是一种即插即用设备（和透明网桥一样)，其内部的帧的转发表也是通过自学习算法自动地逐渐建立起来的。
    + 以太网交换机由于使用了专用的交换结构芯片，因此交换速率较高。以太网交换机独占传输媒体的带宽。

    >   使用交换机后带宽总容量会变为原来的$N$倍，$N$为端口数。

#### 路由器-网络层设备

##### 路由器的组成和功能

+   路由器是一种具有多个输入/输出端口的专用计算机，其任务是连接不同的网络（连接异构网络）并完成路由转发。在多个逻辑网络（即多个广播域）互连时必须使用路由器。 

+   ==功能主要为分组转发和路由计算，不进行差错检测。==

+   路由器作用于物理层、数据链路层、网络层。

+   可以通过用路由器将网络分段控制制网络上的广播风暴

+   当源主机要向目标主机发送数据报时，路由器先检查源主机与目标主机是否连接在同一个网络上。

    +   如果源主机和目标主机在同一个网络上，那么直接交付而无须通过路由器。
    +   如果源主机和目标主机不在同一个网络上，那么路由器按照转发表（路由表）指出的路由将数据报转发给下一个路由器，这称为**间接交付**。

+   可见，在同一个网络中传递数据无须路由器的参与，而跨网络通信必须通过路由器进行转发。

    +   例如，路由器可以连接不同的$LAN$,连接不同的$VLAN$,连接不同的$WAN$,或者把$LAN$和$WAN$互连起来。
    +   路由器隔离了广播域。 

+   从结构上看，路由器由路由选择和分组转发两部分构成

    ![image-20230801234812750](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202308012348886.png)

    而从模型的角度看,路由器是网络层设备，它实现了网络模型的下三层，即物理层、数据链路层和网络层

    +   **路由选择部分**也称**控制部分**，其核心构件是**路由选择处理机**

        +   路由选择处理机的任务是根据所选定的路由选择协议构造出路由表
        +   同时经常或定期地和相邻路由器交换路由信息而不断更新和维护路由表。 

    +   **分组转发部分**由三部分组成：

        1.   交换结构

        2.   一组输入端口

        3.   一组输出端口

             +   输入端口在从物理层接收到的比特流中提取出数据链路层帧，进而从帧中提取出网络层数据报，输出端口则执行恰好相反的操作

             +   交换结构是路由器的关键部件，它根据转发表对分组进行处理，将某个输入端口进入的分组从一个合适的输出端口转发出去

                 +   有三种常用的交换方法：
                     1.   通过存储器进行交换
                     2.   通过总线进行交换
                     3.   通过互联网络进行交换

                 +   交换结构本身就是一个网络。 

+   路由器主要完成两个功能：一是分组转发，二是路由计算

    +   前者处理通过路由器的数据流， 关键操作是转发表查询、转发及相关的队列管理和任务调度等
    +   后者通过和其他路由器进行基于路由协议的交互，完成路由表的计算。

+   路由器和网桥的重要区别是：

    +   网桥与高层协议无关
    +   而路由器是面向协议的
        +   它依据网络地址进行操作，并进行路径选择、分段、帧格式转换、对数据报的生存时间和流量进行控制等
        +   现今的路由器一般都提供多种协议的支持，包括$OSI、TCP/IP、IPX$等。

>   ==如果一个=存储转发设备=实现了某个层次的功能，那么它就可以互连两个在该层次上使不同协议的网段/网络 。==
>
>   如网桥实现了物理层和数据链路层，那么网桥可以互连两个物理层和数据链路层不同的网段，但中继器实现了物理层后，却不能互连两个物理层不同的网段，这是因为中继器不是存储转发设备它属于直通式设备。

##### 路由表与路由转发

路由表由路由选择算法得到，主要用于路由选择，总由软件实现。包括四个部分：目的网络$IP$地址、子网掩码、下一跳$IP$地址、接口。

其中路由表都有一个默认路由：$0.0.0.0$，其子网掩码为$0.0.0.0$，当不知道发送给谁时就发送这个默认路由，让别的路由器帮忙处理。

转发表是从路由表得出的，其表项和路由表项有直接的对应关系，可以使用软件实现也可以使用特殊的硬件实现。转发表必须包含完成转发功能所必须的信息，在每一行都包含要达到的目的网络到输出端口和某些$MAC$地址信息的映射。

一般默认网关地址就是路由器的$LAN$端口地址。

注意转发和路由选择的区别：

+   “转发”是路由器根据转发表把收到的IP数据报从合适豹端口转发出去，它仅涉及一个路由器
+   而“路由选择”则涉及很多路由器，路由表是许多路由器协同工作的结果
    +   这些路由器按照复杂的路由算法，根据从各相邻路由器得到的关于网络拓扑的变化情况，动态地改变所选择的路由，并由此构造出整个路由表。 

![image-20230801235638958](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202308012356108.png)

>   注意，在讨论路由选择的原理时，往往不去区分转发表和路由表的区别，但要注意路由表不等于转发表。分组的实际转发是靠直接查找转发表，而不是直接查找路由表

##### 输入端口处理

1. 从线路接受分组。
2. 物理层进行处理。
3. 数据链路层进行处理。
4. 网络层进行处理，首先对数据进行分组与排队，再进行查表与转发。
5. 输出到交换结构中。

输入端口的查找和转发功能在路由器的交换功能中最重要。

##### 输出端口处理

1. 交换结构中收到分组。
2. 网络层进行处理，首先对数据进行分组与排队，速度太快需要放在缓存中并进行缓存处理。
3. 数据链路层进行处理。
4. 物理层进行处理。
5. 向线路发送分组。

若路由器处理分组的速率赶不上分组进入队列的速率，则队列的存储空间必然最终降为$0$，使后面再进入队列的分组由于没有存储而被丢弃。

路由器中的输入或输出队列产生溢出是造成分组丢失的主要原因。

#### 冲突域与广播域

+ 冲突域：在以太网中，如果某个$CSMA/CD$网络上的两台计算机在同时通信时会发生冲突（==即同一时间只能一台主机发送信息==），那么这个$CSMA/CD$网络就是一个冲突域（$collision\,domain$）。如果以太网中各个网段以集线器连接，因为不能避免冲突，所以它们仍然是一个冲突域。（==一个数据链路层设备的接口一个冲突域==）
    + 内网通信量更大的情况下更需要隔离冲突域

+ 广播域：网络中能接收任一设备发出的广播帧的所有设备的集合。即==如果一个站点发出一个广播信号，那么所有能接收到该信号的设备范围就是一个广播域。==（==一个网络层设备一个广播域==）
+ 网段：指一个计算机网络中使用同一物理层设备（传输介质、中继器、集线器等）能直接通信的一部分。

一个网段就是一个冲突域，一个局域网就是一个广播域。

|   &nbsp;   | 能否隔离冲突域 | 能否隔离广播域 | 举例                |
| :--------: | :------------: | :------------: | ------------------- |
| 物理层设备 |       否       |       否       | 中继器，集线器$Hub$ |
| 链路层设备 |       是       |       否       | 网桥，交换机        |
| 网络层设备 |       是       |       是       | 路由器              |

