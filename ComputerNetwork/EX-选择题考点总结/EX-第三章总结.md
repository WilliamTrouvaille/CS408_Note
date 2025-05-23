# EX-选择题考点总结

## 第三章 数据链路层

### 【考纲内容】

1.   数据链路层的功能

2.   组帧

3.   差错控制：检错编码，纠错编码

4.   流量控制与可靠传输机制

     +   流量控制、可靠传输与滑动窗口机制
     +   停止-等待协议
     +   后退$N$帧协议$GBN$
     +   选择重传协议$SR$

5.   介质访问控制

     1.   信道划分：频分多路复用、时分多路复用、波分多路复用、码分多路复用的概念和基本原理

     2.   随机访问
          1.   $ALOHA$协议

          2.   $CSMA$协议
          3.   $CSMA/CD$协议
          4.   $CSMA/CA$协议

     3.   令牌传递协议

6.   局域网
     +   局域网的基本概念与体系结构
     +   以太网与$IEEE 802.3$
     +   $IEEE 802.11$无线局域网
     +   $VLAN$基本概念与基本原理

7.   广域网
     +   广域网的基本概念
     +   $PPP$协议

8.   数据链路层设备
     +   局域网交换机及其工作原理

### 【复习提示】

+   本章是历年考试中考查的重点
+   要求在了解数据链路层基本概念和功能的基础上，重点掌握**滑动窗口机制、三种可靠传输协议、各种$MAC$协议、$HDLC$协议和$PPP$协议，特别是$CSMA/CD $协议和以太网帧格式，以及局域网的争用期和最小帧长的概念、二进制指数退避算法**。
+   此外，中继器、网卡、集线器、网桥和局域网交换机的原理及区别也要重点掌握。

## 选择题考点总结

### 数据链路层功能

主要作用是加强物理层传输原始比特流的功能，**将物理层提供的可能出错的物理连接改造为逻辑上无差错的数据链路**，使之对网络层表现为一条无差错的链路。

1.   为网络层提供服务：对网络层而言，数据链路层的基本任务是将源机器中来自网络层的数据传输到目标机器的网络层。数据链路层通常可为网络层提供如下服务：

     1. 无确认无连接服务：源机器发送数据帧时不需先建立链路连接，目的机器收到数据帧时不需发回确认。对丢失的帧，数据链路层不负责重发而交给上层处理。适用于实时通信或误码率较低的通信信道，如以太网。
     2. 有确认无连接服：源机器发送数据帧时不需先建立链路连接，但目的机器收到数据帧时必须发回确认。源机器在所规定的时间内未收到确定信号时，就重传丢失的帧以提高传输的可靠性。适用于误码率较高的通信信道，如无线通信。
     3. 有确认面向连接服务目的机器对收到的每一帧都要给出确认，源机器收到确认后才能发送下一帧，因而该服务的可靠性最高。适用于通信要求（可靠性、实时性）较高的场合。帧传输过程分为三个阶段：建立数据链路；传输帧；释放数据链路
     4. 有连接就一定有确认，即**不存在无确认的面向连接的服务**

2.   链路管理：数据链路层连接的建立、维持和释放过程称为链路管理，主要用于面向连接的服务，控制对物理传输介质访问。

     +   链路两端的结点要进行通信，必须首先确认对方己处于就绪状态，并交换一些必要的信息以对帧序号初始化，然后才能建立连接，在传输过程中则要能维持连接，而在传输完毕后要释放该连接
     +   在多个站点共享同一物理信道的情况下（如在局域网中）如何在要求通信的站点间分配和管理信道也属于数据链路层管理的范畴
3.   组帧（定义数据格式）：帧定界、帧同步、透明传输，规定帧的数据部分的长度上限即最大传送单元$MTU$。

     +   两台主机之间传输信息时，必须将网络层的分组**封装成帧**，以帧的格式进行传送
     +   将一段数据的前后分别添加首部和尾部，就构成了帧，帧长等于数据部分的长度加上首部和尾部的长度，首部和尾部中含有很多控制信息，它们的一个重要作用是确定帧的界限，即**帧定界**
     +   而**帧同步**指的是接收方应能从接收到的二进制比特流中区分出帧的起始与终止。
     +   如果在数据中恰好出现与帧定界符相同的比特组合（会误认为“传输结束”而丢弃后面的数据），那么就要采取有效的措施解决这个问题，即**透明传输**
         +   透明传输是指不管所传数据是什么样的比特组合，都应当能够在链路上传送。因此，链路层就“看不见”有什么妨碍数据传输的东西。
4.   流量控制：限制**发送方**的流量，使其发送速率不超过接收方的接收能力。

     +   流量控制并不是数据链路层特有的功能，许多高层协议中也提供此功能，只不过控制的对象不同而已
5.   差错控制：使发送方确定接收方是否正确收到由其发送的数据的方法称为差错控制。


### 组帧（定义数据格式）

主要解决帧定界、帧同步、透明传输等问题，规定帧的数据部分的长度上限即**最大传送单元**$MTU$。由于字节计数法的脆弱性与字符填充实现的复杂性与不兼容性，所以基本上使用零比特填充与违规编码法。
1.   字符计数法：帧首部使用一个计数字段（第一个字节，`8bit`）表明帧内字符数，题目计算时这个计数字段也要算成一个字符。
     +   缺点：如果在某一个帧内，标记位后面的某个字节的数据丢失，那么会影响后面所有的帧，收发双方扇失去同步，从而造成灾难性后果。比如`3 1 1`和`4 2 2 2`，如果前面的帧丢失变成$3\;1$，那么后面的$4$就会被补到前面变成$3\;1\;4$导致错误。
2.   字符填充的首尾定界符法：帧就是加头加尾分别标记开始结束，而为了使信息位中出现的特殊字符不被误判为帧的首尾定界符，可在特殊字符前面填充一个转义字符$ESC$，其后面紧跟的是**数据信息**，而不是控制信息。
     +   如果转义字符$ESC$也出现在数据中，那么解决方法仍是在转义字符前插入一个转义字符
     +   控制字符$SOH$表示帧开始，控制字符$EOT$表示帧结束。
3.   ※零比特填充法：零比特填充法使用一个特定的比特模式，即`0111 1110`来标志一帧的开始和结束。在发送端扫描整个信息字段（不包括首尾定界符），只要有连续的**五个$1$**，就立即在第五个$1$后加上一个$0$，无论后面是$1$还是$0$；在接收端收到一个帧时，先找到标志字段确定边界，再用硬件对比特流进行扫描，发现连续五个$1$时就把后面的$0$删除。
     +   比特填充法允许数据帧包含任意个数的比特，也允许每个字符的编码包含任意个数的比特。
     +   零比特填充法很容易由硬件来实现，性能优于字符填充法。

4.   违规编码法：使用帧中不会用到的编码来定帧的起始和终止。违规编码法不需要采用任何填充技术，便能实现数据传输的透明性，但它只适用于采用冗余编码的特殊编码环境。
     +   在物理层进行比特编码时，通常采用违规编码法
     +   局域网$IEEE 802$标准就采用了这种方法


### 差错控制

使发送方确定接收方是否正确收到由其发送的数据的方法称为差错控制。

1.  通常，这些错误可分为**位错**和**帧错**

    1.   位错指帧中某些位出现了差错，比特位出错/反转，$1$变为$0$，$0$变为$1$。也称为比特差错。通常采用循环冗余校验$CRC$方式发现位错
         +   通过自动重传请求$ARQ$方式来重传出错的帧，$ARQ$是位错的处理方式，是发现位错后要求重传的方法，与帧错无关
    2.   帧错指帧的丢失、重复或失序等错误。数据链路层引入**定时器**和**编号机制**，能保证每一帧最终都能有且仅有一次正确地交付给目的结点
2.  通常利用编码技术进行差错控制，主要有两类

    1.   自动重传请求$ARQ$：接收端检测到差错时，就设法通知发送端重发，直到接收到正确的码字为止
    2.   前向纠错$FEC$：接收端不但能发现差错，而且能确定比特串的错误位置，从而加以纠正
3.  噪声来源：

    + 全局噪声（热噪声，随机噪声）：产生随机差错。线路本身电气特征所产生的固有；可以通过改善传感器调高信噪比来减少。
    + 局部噪声（冲击噪声）：产生突发差错。外界特定短暂的原因所产生，是产生差错的主要原因；可以通过编码计数来解决。
4.  ※检错编码-奇偶校验码：奇校验要选校验码$1$或$0$使得$n$位的总数据里的$1$为奇数，而偶校验要选校验码$1$或$0$使得$n$位的总数据里的$1$为偶数。对于出错的数据仍有对应的奇偶个$1$则无法检验出是否出错。即只能检查出来奇数位出错的情况，偶数个位数不能检查出来，所以其检错能力为$50\%$。

#### 检错编码-循环冗余码$CRC$

检错编码-循环冗余码$CRC$分为三个部分：传输数据、生成多项式、$FCS$帧检验序列/冗余码。正式的计算方法如下，并给出例题：发送数据为`1101 0110 11`，采用$CRC$校验，生成多项式为`10011`，求最终发送数据。

1.   原数据加$0$：假设生成多项式$G(x)$的阶为$r$，则需要在原数据后面加$r$个$0$（多项式有$N$位，那么阶就是$N-1$位）。

     +   如题目中生成多项式`10011`阶为$4$，表示为多项式是$x^4+x^1+x^0$，最后数据就是`1101 0110 1100 00`。

2.   模二除法：在数据加$0$后使用模二除法除以多项式，余数就是冗余码$CRC$校验码的比特序列。

     +   模二除法得到了帧检验序列$FCS$为`1110`，所以要发送的数据就是要发送的数据加上帧检验序列：`1101 0110 1111 10`。

3.   检错过程：接收端

4.   把接收到的每一个帧都除以相同的生成多项式，然后检查得到余数$R$，如果$R=0$则接受，否则丢弃。

     +    $FCS$的生成与接收端$CRC$检验都是由硬件完成，所以处理很快，因此不会延误数据的传输。

          $CRC$具有纠错能力，但是数据链路层只使用了其检错能力，错误就直接丢弃。

#### 纠错编码-海明码

能发现双比特错，但是只能纠正单比特错，正式的计算方法如下，并给出例题：传输的数据`D=1011 01`，求最终发送数据。

1. ※海明不等式确定校验码位数r：
    $$
    2^r\geqslant k+r+1
    $$
    其中$r$为冗余信息位，$k$为待校验信息位，若要检测两位错，则需再增加$1$位校验位，即$k+1$位

    +   $D=1011 01$为$6$位，即信息位$k$为$6$，就可以代入得到最小的$r$为$4$，传输数据为$6+4=10$位。

2. 确定校验码和数据的位置：假设$r$位校验码分别为$P_1$、$P_2$、$P_3$、$\cdots$$、$$P_r$，验码$P_n$需要插入到原数据之中作为传输数据，且只能插入到$2^n$的位置，即$1，2，4\cdots$，然后在其他位置按需填入$D_n$

3. 求出校验码的值：根据二进制中校验码$P_n$位置的$1$来判断其可以校验哪些位，因为第一步校验位的插入规定，$P_n$的位置使用二进制表示一定只有一个$1$，那么就要根据这个$1$所在的位置找到所有同样这个位置是$1$的位置所在的数据位，就是可以被校验码校验的原数据位。同一位可以被不同的校验码同时校验。

    1. $P_1$是第一位，即$0001$位，所以$1$在二进制位置的第四个地方，找到$10$个位置中所有二进制位置的第四个位为$1$的数据位，即$0011$（$3$）：$D_1$；$0101$（$5$）：$D_2$；$0111$（$7$）：$D_4$；$1001$（$9$）：$D_5$。所以$P_1$可以校验这$4$个数据。

        要使其能校验，就要令所有要校验的位异或等于$0$，或直接等于所有校验数据位异或值。

        即令$P_1\oplus D_1\oplus D_2\oplus D_4\oplus D_5=0$。

    2.   同理$P_2\oplus D_1\oplus D_3\oplus D_4\oplus D_6=0,P_3\oplus D_2\oplus D_3\oplus D_4=0,P_4\oplus D_5\oplus D_6=0$
    3.   从而最后海明码就是`0010 0111 01`。
    4.   如果海明码需要一个最高的全校验位，则需要将其对全部数据位和其他全部校验位进行异或为$0$得到。

4.   检错并纠错：海明码“纠错”$d$位，需要码距为$2d+1$的编码方案；海明码“检错”$d$位，则只需码距为$d+1$。

     1.   已知海明码就是`0010 0111 01`。假如第五位出错，即变为`0010 1111 01`。

     2.   构成$r$个校验方程，令所有要校验的位进行异或运算：

          $P_1\oplus D_1\oplus D_2\oplus D_4\oplus D_5=0\oplus1\oplus1\oplus1\oplus0=1$。

          $P_2\oplus D_1\oplus D_3\oplus D_4\oplus D_6=0\oplus1\oplus1\oplus1\oplus1=0$。

          $P_3\oplus D_2\oplus D_3\oplus D_4=0\oplus1\oplus1\oplus1=1$。

          $P_4\oplus D_5\oplus D_6=1\oplus0\oplus1=0$。

          从$P_1$进行的运算结果开始，将所有计算结果组成为$1010$，这个数字即表示出错的位置，即第五位出错。

          若校验方程的结果全为$0$，说明无错，可以接收


### 流量控制

流量控制即限制**发送方**的流量，使其发送速率不超过接收方的接收能力。以防较高的发送速度和较低的接受能力不匹配时丢包

1.   ※数据链路层流量控制与网络层的拥塞控制、传输层的流量控制的区别：

     1.   作用范围：
          1.   数据链路层流量控制是点到点的，作用于两个相邻结点之间。
          2.   网络层的**拥塞控制**是整个网络的，作用于整个网络。
          3.   传输层的流量控制是端到端的，作用于两台主机上相应的端口之间。
     2.   实现方式：
          1.   数据链路层的流量控制是收不下就不回复确认（确认控制帧）
          2.   网络层的**拥塞控制**使用拥塞控制算法检测网络拥塞的迹象，并采取措施来减缓发送端和接收端之间的点对点通信。
          3.   传输层的流量控制是接收端发送给发送端一个窗口公告。

2.   数据链路层的流量控制由**自动重传请求$ARQ$**来实现。

     +   $ARQ$流量控制协议分为停止等待协议与滑动窗口协议，滑动窗口协议又分为两种后退$N$帧协议与选择重传协议。其实停止等待协议也是一种特殊的滑动窗口协议。
     +   后两种协议是滑动窗口技术和请求重复技术的结合，所以称为连续$ARQ$协议。
     +   滑动窗口可以解决流量控制和可靠传输两个部分的功能。
         +   可靠传输：发送端发啥，接收端收啥。
         +   流量控制：控制发送速率，使接收方有足够的缓冲空间来接收每一个帧。
     +   数据链路层协议的滑动窗口在同一次发送与接受中都是固定的
     +   现有的实际有线网络的数据链路层很少采用可靠传输

3.   ※※停止等待协议$SW$：停止-等待协议相当于发送窗口和接收窗口大小均为`1`的滑动窗口协议。每发送完一个帧就停止发送并等待对方确认，如果收到确认就再发送下一个帧。因为是停止等待，所以只用一位以$1$、$0$为值来对帧编号就可以了，相应的确认帧分别用`ACK0`和`ACK1`来表示。虽然同样标号的帧，却是不同的帧。特点：简单，信道利用率低。

     1.   发送帧出错丢失：帧出错时接收站利用前面讨论的差错检测技术检出后，简单地将该帧丢弃，两种情况都不会发送确认帧。若连续出现相同发送序号的数据帧，表明发送端进行了超时重传

     2.   确认帧出错丢失：此时接收方已收到正确的数据帧，但发送方收不到确认帧，因此发送方会重传已被接收的数据帧，接收方收到同样的数据帧时会丢弃该帧,并重传一个该帧对应的确认帧。连续出现相同序号的确认帧时，表明接收端收到了重复帧。 

     3.   超时重传机制

          1.   发送方和接收方都须设置一个帧缓冲区，发送完一个帧后，必须保存其副本以便重传，确认才能丢失该副本。
          2.   数据帧与确认帧必须编号。
          3.   超时计时器：每次发送一个帧就启动一个计时器。
          4.   超时计时器设置的重传事件应该比帧传输的平均$RTT$更长。
               +   $RTT$包括了数据在传输路径中的传播时延和处理时延，以及数据帧在传输过程中可能发生的排队时延等。当数据帧从发送方发送出去后，它需要经过一定的时间才能到达接收方。然后，接收方接收到数据帧后，会立即发送确认帧$ACK$回到发送方，表示数据帧已经成功接收。
               +   $RTT$对于网络性能非常重要，它影响着数据传输的速度和可靠性。较小的RTT意味着数据的往返时间短，数据传输的速度较快，而较大的$RTT$可能导致数据传输的延迟较高。
               +   $RTT$不仅受到网络传输的物理因素影响，还受到网络拓扑、网络拥塞情况以及传输协议的影响。网络中的链路质量、传输距离和传输速率等都会对$RTT$产生影响。

     4.   信道利用率$U$：发送方在一个发送周期内，有效地发送数据所需要的时间占整个发送周期的比率。

          1.   计算公式为
               $$
               U=\dfrac{T_D}{T_D+RTT+T_A}
               $$
               其中，$T_D$表示发送周期中有效地发送数据所占时间，$RTT$表示往返时延，$T_A$表示确认帧传输时间

          2.   当忽略确认帧传输时间$T_A$时，计算公式为
               $$
               U=\dfrac{L/C}{T},T=L/C+RTT
               $$
               其中，$L$表示发送周期中发送$L$比特的数据即帧长，$C$表示发送方数据传输率，$T$表示发送周期（发送时延加往返往返时间，其中发送时延可以使用$L/C$计算），$RTT$表示往返时延

          3. 要使信道利用率达到最高，应该先算出发送一帧到收到确认为止的总时间$T_D+RTT+T_A$，然后算出这些时间总共可以继续发送多少帧，然后取以二为底的对数即可

              + 数据帧长度是不确定的情况应以最短的帧长计算

          4. 信道吞吐率

              + 信道吞吐率 = 信道利用率 * 发送方的发送速率

4.   ※※后退$N$帧协议$GBN$：$GBN$会一次性将在发送窗口内的$n$个帧一个个全部发送完，然后再移动受确认的帧的个数个窗口，如果一直收不到确认信息则一直不移动发送窗口并不断依次重传。

     1.   使用**累计确认模式**，如果接发送方收到当前$n$号帧的确认消息，则前面的所有帧都已经确认接收到。

     2.   $GBN$发送方：

          1.   响应上层调用：上层发送数据，发送方必须检查发送窗口是否已满，未满则产生帧并发送，若已满则先缓存数据，等窗口不满时再发送。
          2.   收到一个$ACK$：对$n$号帧采取**累积确认**，偶尔捎带确认的方式，即标明接收方已经收到$n$号帧和其之前的全部帧。
               +   在计算机通信中，当一个数据帧到达的时候，接收方并不是立即发送一个单独的控制帧，而是抑制一下自己并且开始等待，直到网络层传递给它下一个分组，确认信息被附在往外发送的数据帧上即帧头中的$ACK$域
               +   实际上，相当于确认报文搭了下一个外发数据帧的便车，这种“将确认暂时延迟以便可以钩到下一个外发数据帧”的技术称为捎带确认
          3.   超时重传：如果出现超时，则发送方重传所有已发送但是未被确认的帧，即超时以及**后面**的帧
               +   接收方、发送方只按**顺序**接受帧，对于其非期待的（**乱序的**）确认帧则丢弃，只接受在接收窗口（大小为1）里面的帧，除此以外的帧会被丢弃

     3.   $GBN$接收方：

          1.   如果正确收到$n$号帧且顺序一致，则接收方为$n$帧发送一个$ACK$帧，并将该帧中的数据部分上交上层。
               +   成功接收到接收窗口中期待的$n$号帧后将接收窗口后移一位，开始期待接收$n+1$号帧
          2.   其余情况都丢弃帧，并为最近按序接收的帧重新发送$ACK$，向发送方要重传帧
               +   接收方无需缓存任何失序帧，只用维护下一个按序接收的帧序号。
               +   即成功接收$1 \;2 $号帧后又接收到$4\;5$号帧时会直接丢弃，并会向发送端重新发送$2$号帧的$ACK$

     4.   $GBN$窗口长度：若帧编号采用$n$个比特位，则$GBN$发送窗口的尺寸$W_T$应满足
          $$
          1\leqslant W_T\leqslant 2^n-1
          $$

          +   一句话总结，无论是$GBN$设置为 $2^n-1$ ，还是$SR$设置为 $2^{n-1}$ 均是为了超时重传接受方能够区分到底是新数据还是老数据。
          +   所以发送窗口和接收窗口总和不能超过$2^n$，而$GBN$接收窗口长度始终为`1`，故$GBN$发送窗口不超过$2^n-1$。

     5.   $GBN$协议特点

          1.   连续发送数据帧，信道利用率较高。但当信道的传输质量很差导致误码率较大时， 后退$N$帧协议不一定优于停止-等待协议
          2.   在重传时必须把原来已经正确传送的数据帧重传，从而令传送速率降低。

5.   ※※选择重传协议$SR$：累计确认会导致批量重传问题，所以$SR$为了实现只重传出错的帧，解决的办法就是设置单个确认而非累计确认，同时加大接收窗口，缓存乱序到达的帧。

     1.   发送窗口和接收窗口大小均不超过$2^{n-1}$

          1.   发送窗口中分为已经发送被确认的帧、已经发送但等待确认的帧、还能发送的帧三种，其中这三种帧不一定是连续的，而只有发送窗口的第一个帧是已经发送被确认的帧发送窗口才能移动一格或多格。
          2.   接收窗口分为希望收到但是没有收到的帧、希望收到且已收到的帧、等待接收的帧三种，其中这三种帧不一定是连续的，接收窗口中收到且确认的帧位于缓存之中，而只有接收窗口的第一个帧是希望收到且已收到的帧接收窗口才能移动一格或多格。

     2.   $SR$发送方：

          1.   响应上层调用：上层发送数据，发送方必须检查发送窗口是否已满，未满则产生帧并发送，若已满则先缓存数据，等窗口不满时再发送。
          2.   收到一个$ACK$：
               1.   如果收到$ACK$对应的帧序号在窗口内，则$SR$发送方将那个被确认的帧标记为已接收
               2.   如果该帧序号是窗口的下界（最左边），在窗口向前移动到具有最小序号的未确认帧处
               3.   如果窗口移动且有序号在窗口内未发送的帧，则发送这些帧。
          3.   超时重传：每一个帧都具有自己的计时器，一个超时事件发生后只重传一个帧。

     3.   $SR$接收方：

          1.   确认一个正确接收的帧而不管是否按序
               +   失序的帧将被缓存，并返回发送方一个该帧的确认帧，直到所有比其序号更小的帧都被接收为止，这时才能移动接收窗口并将这一批帧交付上层。
          2.   如果收到了接收窗口前的一个接收窗口长度以内的帧，就返回一个$ACK$表明发送方超时重传的帧已经得到了确认。
          3.   如果是其他情况就忽略该帧。

     4.   $SR$窗口长度：采用$n$个比特对帧编号，则发送/接收窗口的尺寸$W_T/W_R$应满足$W_R=W_T=2^{n-1}$​。（即编号数量的一半）
          $$
          W_R+W_T\le2^{n}
          $$

     5.   $SR$协议特点

          1.   对数据帧逐一确认，收到一个确认一个。
          2.   只会重传出错帧。
          3.   接收方有缓存，其他两个没有。
          4.   发送窗口与接收窗口相等。
          5.   全部收到后再一起上传。


### 介质访问控制

即采取措施使得两对结点之间的通信不会互相干扰

#### 静态划分信道

**静态划分信道不会发生碰撞**

前两种中$FDM$使用较少，而使用$TDM$较多，因为$TDM$抗干扰能力强，能逐级再生整形避免干扰累计，且数字信号易于自动转换，所以$FDM$常用于传输模拟信号，$TDM$常用于传输数字信号。

1.   频分多路复用$FDM$：用户在分配到一定的频带后，在通信过程中自始至终都占用这个频带。频分复用的所有用户在同样时间占用不同的频率带宽资源。
     + 充分利用传输介质带宽，系统效率较高；技术较成熟，实现容易。

2.   时分多路复用$TDM$：将时间划分为一段段等长的时分复用帧（$TDM$帧：在物理层传输的比特流所划分的帧，表明一个周期）。每一个时分复用的用户在每一个$TDM$帧中占用固定序号的时隙，所有用户轮流占用信道。
     + 当用户使用率较低时会导致信道的利用率很低；用户的等待时间长。
     + 共享带宽，但分时利用信道，参与带宽共享的每个时分复用的用户在每个$TDM$帧中占用固定序号的时隙，介质的位速率大于单个信号的位速率
     + 分为同步时分多路复用和异步时分多路复用（统计时分复用）其中统计时分复用$ST DM$：
         + 添加了一个集中器，将不同用户分散的数据集中在一起，单位时间的数据组成一个$STDM$帧，再一起发送出去。
         + 每一个$STDM$帧中的时隙数小于连接在集中器上的用户数。
         
         + $STD M$帧不是数据链路层的帧，是在物理层传输的比特流划分的帧
         + 每个用户有数据就随时发送给集中器的输入缓存，然后集中器按顺序依次扫描输入缓存，把缓存中的输入数据放入$STDM$帧中，一个$STDM$帧满了再发出。
         + $STDM$帧并不是固定分配时隙，而是按需动态分配时隙。

 3.   波分多路复用$WDM$：就是光的频分多路复用，根据同一根光纤中传输多种不断波长（频率）的光信号，根据不同的波长做用波长分解复用器分解出来。由于波长（频率）不同，各路光信号互不干扰

 4.   ※码分多路复用$CDM$：码分复用既共享了空间也共享了时间。码分多址$CDMA$是码分复用的一种方式。特点有频谱利用率高。抗干扰能力强。保密性强。语音质量好。减少投资和降低运行成本。主要用于无线通信系统，特别是移动通信系统。

      1.   一个比特分为多个码片/芯片，每一个站点被指定一个唯一的$m$位的码片序列。

           + 假设站点$A$的码片序列被指派为`0001 1011`，则$A$站发送`0001 1011`就表示发送比特$1$，发送`1110 0100`就表示发送比特$0$。

      2. 发送$1$时站点发送码片序列，发送$0$时发送码片序列反码（一般将码片中的$0$写为$-1$，将$1$写为$+1$）。

          + 因此$A$站的码片序列是`-1-1-1+1 +1-1+1+1`

      3. 如何复用：多个站点同时发送数据的时候，要求各个站点码片序列相互正交，即使各个码片序列规格化内积为$0$。

          + 令向量$\vec{S}$表示$A$站的码片向量，令$\vec{T}$表示$B$站的码片向量

          + 两个不同站的码片序列正交，即向量$\vec{S}$和$\vec{T}$的规格化内积为$0$：
              $$
              \vec{S}\cdot\vec{T}\equiv \frac{1}{m} \sum_{i=1}^{m} S_{i} \vec{T}_{i}=0
              $$
              则令向量$\vec{T}$为``-1-1+1-1 +1+1+1-1``，即``0010 1110``

          + 任何一个码片向量和该码片向量自身的规格化内积都是$1$，任何一个码片向量和该码片反码的向量的规格化内积是$-1$。即自己×自己$=1$ ，自己×别人$=0$ ，自己×反码$=-1$。

      4. 如何合并：各路数据在信道中按位线性相加。

          + 当$A$站向$C$站发送数据$1$时，就发送了向量`-1-1-1+1 +1-1+1+1`

          + 当$B$站向$C$站发送数据$0$时，就发送了向量`+1+1-1+1 -1-1-1+1`

              + 此处为发送数据$0$，即发送上一步中求出的向量$T$的反码`-1-1+1-1 +1+1+1-1`

          + 两个向量到了公共信道上就进行叠加，实际上就是线性相加，即计算
              $$
              \vec{S}-\vec{T}
              $$
              得到`0 0 -2 +2 0 -2 0 +2`

      5. 如何分离：合并的数据和源站码片序列规格化内积。

          + 到达$C$站后，进行数据分离：若想接受的发送站的数据序列长度为$m$，则应该先将接收到的序列按每$m$个数字一组分开

          + 如果要得到来自$A$站的数据，$C$站就必须知道$A$站的码片序列

          + 根据叠加原理，其他站点的信号都在内积的结果中被过滤掉了，内积的相关项都是$0$，而只剩下$A$站发送的信号

          + 让$\vec{S}$与$\vec{S}–\vec{T}$​进行规格化内积
              $$
              \frac{1}{m}\vec{S}\cdot(\vec{S}–\vec{T})=1
              $$
              所以$A$站发出的数据是$1$

          + 同理，如果要得到来自$B$站的数据，那么$\vec{T}\cdot(\vec{S}–\vec{T})=-1$，因此从$B$站发送过来的信号向量是一个反码向量，代表$0$。

#### 动态划分信道

##### $ALOHA$协议：不监听信道

 1.   纯$ALOHA$​协议：不监听信道，不按时间槽发送，随机重发（想发就发）。发送信息时占用全部带宽
      1.   检测冲突：如果发送冲突，接收方就会检测到差错然后不予确认，发送方在一定时间内收不到确认就判断冲突。
      2.   解决冲突：超时后等待随机时间重传。
      3.   假设网络负载（$T_0$时间内所有站点发送成功的和未成功而重传的帧数）为$G$，则纯$ALOHA$网络的吞吐量利用率（$T_0$时间内成功发送的平均帧数）为$S=Ge^{-2G}$。当$G=0.5$时极大，$S=0.5e^{-1}\approx0.184$。
           +   可见，纯$ALOHA$网络的吞吐量很低

 2.   时隙$ALOHA$协议：把时间划分为若干个相同的时间片（时间槽），所有用户只能在时间片的开始时刻同步接入网络信道，若发生冲突则必须等到下一个时间片开始的时刻再发送。

      +   假设网络负载（$T_0$时间内所有站点发送成功的和未成功而重传的帧数）为$G$，则间隙$ALOHA$网络的吞吐量利用率（$T_0$时间内成功发送的平均帧数）为$S=Ge^{-G}$。当$G=1$时极大，$S=e^{-1}\approx0.368$。还是很低。

##### ※载波监听多路访问协议$CSMA$

 1.   载波监听多路访问协议$CSMA$：发送前监听信道。当信道空闲发送帧，当信道忙推迟发送。
      1.   $CS$载波监听：表示每一个站在发送数据之前都要检查一下总线上是否有其他计算机在发送数据。当几个站同时在总线上发送数据时，总线上的信号电压摆动值就增大，因为互相叠加。当一个站检测到的信号电压摆动值超过一定门限值时，就认为总线上至少两个站在同时发送数据，表明产生了碰撞。
      2.   $MA$多点接入：表示许多计算机以多点接入的方式连接在一根总线上。适用于总线型网络

 2.   坚持$CSMA$​：发送信息前监听信道。当信道**空闲时直接传输**，不必等待；当**信道忙时一直监听**，直到空闲立刻传输。如果冲突（一段时间内未收到确认），则等待一个随机长的时间再监听。

      1.   优点：只要媒体空闲站点就立马发送，避免媒体使用率的损失。
      2.   缺点：若有两个或以上的站点有数据要发送，冲突就无法避免。

 3.   非坚持$CSMA$​：发送信息前监听信道。当**信道空闲时直接传输**，不必等待；当**信道忙时会等待一个随机时间之后再进行监听**。

      1.   优点：采用随机的重发延迟时间可以减少冲突发生的可能性。
      2.   缺点：可能存在所有站点都在延迟等待中，使得媒体空闲，媒体使用率降低。

 4.   $p-$坚持$CSMA$：发送信息前监听信道。当**信道空闲时以$p$概率直接传输**，不必等待，**概率$1-p$等待到下一个时间槽再监听**，以同样的概率监听或传输。当**信道忙时会推迟到下一个时隙再监听**（**适用于时隙信道**）。

      1.   当$p=1$时与坚持$CSMA$协议类似，但**当$p=0$时不与非坚持$CSMA$协议类似**
      2.   优点：既能像非坚持算法减少冲突，又能像$1$-坚持算法那样减少媒体空闲时间。
      3.   缺点：无法在冲突时就发现，所以发生冲突时还是坚持发送完数据帧，从而造成浪费。

##### ※※载波监听多点接入/碰撞检测协议$CSMA/CD$

载波监听多点接入/碰撞检测协议$CSMA/CD$：$CSMA/CD$是$CSMA$协议的完善，不仅**先监听再发送**，还可以**边监听边发送**，当发现碰撞就立刻停止发送，就避免了$CSMA$协议的无法在冲突时就发现，发生冲突时还是坚持发送完数据帧造成浪费的缺点。

1.   $CD$碰撞检测（冲突检测）：适配器边发送数据边检测信道上信号电压的变化情况，以便判断自己在发送数据时其他站是否也在发送数据。

     + 边发送边监听
     + 适用于半双工网络

2.   设${\LARGE \tau} $为单程传播时延，$ {\LARGE \delta}$为从$B$发送数据到$B$检测到碰撞的时间，则碰撞检测时间为
     $$
     {\LARGE t=2\tau-\delta}
     $$
     其中，${\LARGE \tau} $为**单程传播时延或争用期/冲突窗口/碰撞窗口**

     则当$ {\LARGE \delta}\to0$时碰撞检测时间最大，故超过这个时间可以认为自己发送的数据没有和别人碰撞

     即**只要某站在${\LARGE 2\tau}$时间内没有检测到碰撞，那么就可以肯定本次发送没有碰撞**。

3.    确定重传时机：采用截断二进制指数规避算法。

     1.   规定基本退避（推迟）时间为争用期${\LARGE 2\tau}$。

     2. 定义参数$k$，其表示重传次数，但是$k$不超过$10$

         +   当重传次数不超过$10$时，$k$等于重传次数
         +   当重传次数大于$10$时，$k$就一直等于$10$。

     3. 从离散的整数集合${0，1，2，3，4\cdots，2^k-1}$之中随机取出一个数$r$，重传所需要的退避的时间就是$r$​倍的基本退避时间，即
         $$
         {\LARGE r\cdot2\tau}
         $$

     4. 当重传达$16$次仍不能成功时，就说明网络太拥挤，认为该帧永远无法正确发出，就抛弃该帧并向高层报错。

4.   ※最小帧长：如果帧太短，那么如果发生碰撞就很容易发送完才检测到发生碰撞而无法停止发送。所以就需要规定一个最小的帧长。

     1.   则**最短帧长等于争用期时间内发出的比特数**，且**帧的传输时延至少要两倍于信号在总线中的传播时延**，即

          $$
          \dfrac{\text{帧长(bit)}}{比特率}\ge2\tau
          $$
          即**最小帧长=数据传输速率×$2\tau$=总线长度÷传播速率×数据传输速率×$2$=争用期×数据传输速率**。

     2.   **※※以太网规定争用期为$51.2\mu s$，最短帧长为$64B=512bit$，凡是小于则被判定为无效帧。**

##### ※※载波监听多点接入/碰撞避免协议$CSMA/CA$

载波监听多点接入/碰撞避免协议$CSMA/CA$可以避免碰撞。且$CSMA/CD$协议只能用于总线类型的以太网，而$CSMA/CA$可以用于无线局域网。
1.   发送流程：
     1.   发送信息前监听信道。
          + 当信道空闲时发送端发出$RTS$（$requests\;to\;send$），$RTS$包括发射端的地址、接收端的地址、下一份数据将持续发送的时间等信息

          + 当信道忙时会等待一个随机时间之后再进行监听。
     2.   接收端接收到$RTS$后，会响应$CTS$（$clear\;to\;send$）。
     3.   发送端收到$CTS$后，开始发送数据帧，同时**预约信道**，发送方告知其他站点要传输多久的数据。
     4.   接收端收到数据帧后，将用$CRC$来检验数据是否正确，正确则响应$ACK$帧。
     5.   发送方收到$ACK$帧后才可以进行下一个数据帧的发送，若没有则一直重传一直到规定重发次数为止（使用二进制指数退避算法确定推迟时间）。
2.   为了尽量避免碰撞，$IEEE802.11$规定，所有的站完成发送后，必须再等待一段很短的时间（继续监听）才能发送下一帧。这段时间称为帧间间隔$IFS（InterFrame\;Space）$。帧间间隔的长短取决于该站要发送的帧的类型。$802.11$使用了$3$种$IFS$：

     1. $SIFS$（短$IFS$）：最短的$IFS$，用来分隔属于一次对话的各帧，使用$SIFS$的帧类型有$ACK$帧、$CTS$帧、分片后的数据帧，以及所有回答$AP$探询的帧等。
     2. $PIFS$（点协调$IFS$）：中等长度的$IFS$，在$PCF$操作中使用。
     3. $DIFS$（分布式协调$IFS$：最长的$IFS$，用于异步帧竞争访问的时延。
3.   $CSMA/CA$协议与$CSMA/CD$协议的相同点：
     1.   都是需要先侦听信道。
     2.   冲突后都会进行有限的重传。
4.   $CSMA/CA$协议与$CSMA/CD$协议的不同点：
     1.   传输介质不同
          +   $CSMA/CA$协议适用于无线局域网即无线传输
          +   $CSMA/CD$协议适用于总线型以太网即有线传输。
     2.   载波检测方式不同
         +   $CSMA/CA$协议采用能量检测（$ED$）、载波检测（$CS$）和能量载波混合检测三种方式
         +   $CSMA/CD$协议采用电缆电压检测。
     3.   $CSMA/CA$协议避免冲突；$CSMA/CD$协议检测冲突。

#### 轮询访问介质访问控制

轮询访问介质访问控制：信道划分介质访问控制协议在网络负载重时信道效率高且公平，而在负载轻时则效率低；随机访问控制协议在网络负载重时会产生冲突开销，而负载轻时效率会比较高，单个结点可以利用信道全部带宽。也称为轮流协议。即减少产生冲突，又尽量发送时占用全部带宽。有轮询协议`15.`和令牌传递协议`16.`

1.   轮询协议：主结点会轮流以发送数据帧的形式询问从属结点是否发送数据，没有被询问的从属结点无法发送数据。
     +   存在的问题：轮询开销。等待延迟。单点故障。

2.   令牌传递协议：一般使用**令牌环网**实现，**逻辑上是环型的，但是物理上是星型的**。轮询介质访问控制既不共享时间，也不共享空间。采用令牌传输方式的网络基本上是负载较重，通信量较大的网络。

      1.   令牌是一个特殊格式的$MAC$控制帧，不包含任何信息，在令牌环上循环，控制信道使用，只有有令牌才能发送数据，确保同一个时刻只有一个结点独占信道，使用$TCP$转发。
      2.   每一个结点都可以在**令牌持有时间**内获得发送数据的权利，而不能无限制的持有令牌，超过时间无论是否发送完成都要归还令牌。
      3.   当计算机都不需要发送数据时，令牌就在环形网上游荡，而需要发送数据的计算机只有在拿到该令牌后才能发送数据帧，因此不会发送冲突
      4.   令牌环网中令牌和数据的传递过程如下

           1.   网络空闲时，环路中只有令牌帧在循环传递
           2.   令牌传递到有数据要发送的站点时，该站点就修改令牌中的一个标志位，并在令牌中附加自己需要传输的数据，将令牌变成一个数据帧，然后将这个数据帧发送出去。
           3.   数据帧沿着环路传输，接收到的站点一边转发数据，一边查看帧的目的地址。如果目的地址和自己的地址相同，那么接收站就复制该数据帧以便进一步处理
           4.   数据帧沿着环路传输，直到到送该帧的源站点，源站点收到自己发出去的帧后便不再转发
                +   同时，通过检验返回的帧来查看数据传输过程中是否出错，若有错则重传。
           5.   源站点传送完数据后，重新产生一个令牌，并传递给下一站点，以交出信道控制权。 
      5.   存在的问题：令牌开销。等待延迟。单点故障。

### 局域网**$LAN$**

局域网$LAN$指在一个较小的地理范围内，将各种计算机、外部设备和数据库系统等通过双绞线、同轴电缆等连接介质互相连接起来，组成资源和信息共享的计算机互连网络。使用广播信道。

1.    主要特点：

    1. 覆盖范围较小：为一个单位所拥有，且地理范围和站点数目均有限。
    2. 采用专门的传输介质（双绞线、同轴电缆）进行联网，数据传输速率较高。
    3. 通信延迟时间端，误码率低，可靠性高。
    4. 各站平等共享信道：各站为平等关系而非主从关系。
    5. 多采用分布式控制与广播式通信，可以广播与组播。

2.    局域网的特性主要由三个要素决定：拓扑结构。传输介质。介质访问控制方式。其中最重要的是介质访问控制方式，它决定着局域网的技术特性

    1.    拓扑结构：星形结构，总形结构，环形结构，星形结构和总线形结构结合的复合形结构
    2.    局域网的传输介质：双绞线为主流传输介质
    3.    局域网的介质访问控制
        1.    $CSMA/CD$：常用于总线型局域网，也用于树型网络。
        2.    令牌总线：常用于总线型局域网，也用于树型网络。把总线或树型网络中的各个工作站按一定的顺序如按接口地址大小排列形成一个逻辑环，只有令牌持有者才能控制总线发送信息。
        3.    令牌环：用于环型局域网，如令牌环网。

3.    局域网的拓扑实现

    1.    $802.3$局域网/以太网：是应用最为广泛的局域网。包括标准以太网（$10Mbps$）、快速以太网（$100Mbps$）、千兆以太网（$1000Mbps$）和$10G$以太网，都符合$IEEE802.3$系列标准规范。
        +   逻辑拓扑总线型，物理拓扑是星型或拓展星型，使用$CSMA/CD$。
    2.    令牌环网：逻辑拓扑是环形结构，物理拓扑是星形结构
        +   基本上已经过时。
    3.    $FDDI$网（光纤分布数字接口，$IEEE 802.8$）：逻辑拓扑是环形结构，物理拓扑是双环结构。使用光纤，造价高，基本上没有使用到。

4.    $MAC$子层与$LLC$子层：$IEEE\; 802$标准定义的局域网参考模型只对应于$OSI$参考模型的数据链路层和物理层，并将局域网的数据链路层分为逻辑链路层$LLC$子层与介质访问控制$MAC$子层。

    1.    $LLC$逻辑链路控制：

        + 建立和释放数据链路层连接、提供与高层接口、差错控制、帧加序号。
        + 为网络层提供服务：无确认无连接、面向连接、带确认无连接、高速传送。

    2.    $MAC$介质访问控制：

        + 组帧拆帧（根据$LLC$的序号），帧寻址和识别，竞争处理，比特差错控制等。
        + 屏蔽了不同物理链路种类的差异性。

        >   由于以太网在局域网市场中取得垄断地位，几乎成为局域网的代名词，而$802$委员会制定的$LLC$子层作用已经不大，因此现在许多网卡仅装有$MAC$协议而没有$LLC$协议。

### 以太网/$802.3$局域网

1.    以太网标准：通常把$802.3$局域网简称以太网

    1.    $DIX\;Ethernet\;V2$：第一个局域网产品（以太网）规约。
    2.    $IEEE802.3$：$IEEE$指定的第一个$IEEE$的以太网标准，帧格式有所不同。
        + $IEEE 802.3$标准是一种基带总线形的局域网标准，它描述物理层和数据链路层的$MAC$子层的实现方法

2.    以太网传输：当以太网发送一个数据，那么以太网将以**广播**形式发送，以太网上的**所有主机**包括发送端本身都能收到数据。通过曼彻斯特编码以中间电平跳动作为同步信号，无序其他同步操作。

    +   网络中的每台计算机都必须花费时间来处理此信息，若网络中存在大量的广播信息，则所有计算机的运行效率会受到影响，导致网络的性能降低
        +   广播信息只会在一个网络内部传输，路由器隔离了广播域

3.    ※以太网类型：**以太网都使用曼彻斯特编码**

    1.    $10Base5$：粗缆以太网，数据率为$10Mb/s$，每段电缆最大长度为$500m$；使用特殊的收发器连接到电缆上，收发器完成载波监听和冲突检测的功能。
    2.    $10Base2$：细缆以太网，数据率为$10Mb/s$，每段电缆最大长度为$185m$；使用$BNC$连接器形成$T$形连接，无源部件。
    3.    $10BASE-T$以太网：$10$表示传输速率为$10Mb/s$，$BASE$表示传输基带信号，$T$代表使用双绞线($Twisted Pair$)。现在一般采用无屏蔽双绞线$UTP$。
        1.    网络拓扑物理上是星型，逻辑上是总线型，每段双绞线最长为$100m$。
        2.    使用$CSMA/CD$介质访问机制。
        3.    超过覆盖范围拓展使用中继器。

    4.    $100BAST-T$以太网：在双绞线上传输$100Mb/s$基带信号的星型拓扑以太网，仍使用$IEEE802.3$的$CSMA/CD$协议
        +   支持全双工与半双工，但是可以做到全双工方式工作下无冲突，所以全双工方式下不使用$CSMA/CD$协议。
        +   $MAC$帧格式仍然是$802.3$标准规定的，保持最短帧长不变，但将一个网段的最大电缆长度减小到$100m$，帧时间间隔从原来的$9.6\mu s$改为现在的$0.96\mu s$
    5.    吉比特以太网：在光纤（$IEEE 802.3z$）或$4$对$UTP5$双绞线（$IEEE 802.3ab$）上传送$1Gb/s$信号。支持全双工与半双工，同上，全双工方式下不使用$CSMA/CD$协议。
    6.    $10$吉比特以太网：在光纤上传送$10Gb/s$信号。只支持全双工，同上，不使用$CSMA/CD$协议。

4.    适配器与$MAC$地址：计算机与外界局域网的连接通过通信适配器完成，过去通过单独网络接口卡即网卡$NIC$实现。

    1.    网卡主要工作在物理层和数据链路层。
    2.    适配器上有处理器和存储器，包括$RAM$和$ROM$，$ROM$上有计算机硬件地址，称为**介质访问控制$MAC$**地址。
    3.    **在使用静态$MAC$地址的系统中，如果有重复的硬件地址，那么这两个设备都不能正常通信**
    4.    $MAC$地址是全球唯一的$48$位二进制适配器地址，即$6$个字节，一般用连字符或冒号分隔的$12$个十六进制数表示
        + 前$24$位代表厂家（$IEEE$规定），后$24$位由厂家指定，称为厂商识别码；后24位是由设备制造商来分配的，用于唯一标识该设备。
    5.    需要注意的是，$MAC$地址是在数据链路层使用的地址，在不同的网络之间是不可见的
        + 当数据从局域网传输到广域网时，会使用其他层次的地址（如$IP$地址）进行路由和转发。

5.    ※※以太网$V2$标准的$MAC$帧格式：和以太网的两种标准一样，有两种MAC帧。最常用的是$V2$的$MAC$格式。**以太网帧附加信息$18B$，规定帧最短为$64B$，所以数据最短为$46B$，规定数据部分最长为$1500B$，所以以太网帧最长为$1518B$。**

    ![image-20230730121337884](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307301213026.png)

    1.    物理层会插入前导码，用于接收端与发送端时钟同步，包括**前同步码**（用于快速实现$MAC$帧的比特同步）与**帧开始定界符**（表示后面的信息就是$MAC$帧）两个部分。
    2.    目的地址和源地址均为$MAC$地址。
        + 通常使用$6B=48bit$

    3.    类型表示$IP$数据报的类型，表示数据应该交给哪个协议实体处理。
        + 固定$2B$

    4.    数据包括高层的协议信息
        + $46\sim 1500\; bit$
        + 最小值为$46$是因为$CSMA/CD$算法规定以太网最小帧长为$64$字节，所以数据最小就是$64-6-6-2-4=46$字节
        + 同样规定最长为$1500$字节。

    5.    填充用于帧长太短来填充到$64$字节
        + 长度为$0\sim46$字节。

    6.    校验码$FCS$：校验范围从目的地址到数据端末尾，采用$32$位循环冗余码$CRC$，但是不用校验$MAC$帧的前导码。
        + 固定$4B$

    7.    以太网帧没有结束定界符是因为以太网采用曼彻斯特编码，如果与数据必然是电压变化的，而如果电压没有变化就代表已经结束，所以不需要结束定界符，所以图`3.26`只有帧开始定界符

    8.    $MAC$帧也需要加尾部：**在数据链路层上，帧既要加首部，也要加尾部**

### 无线局域网

无线局域网可分为两大类，有固定基础设施的无线局域网和无固定基础设施的移动自组织网络。所谓“固定基础设施”，是指预先建立的、能覆盖一定地理范围的固定基站

1.    有固定基础设施的无线局域网

    1.    使用星型拓扑。其中心位接入点$AP(Access Point)$。

    2.  使用$802.11$系列协议的局域网又称$Wi-Fi$

    3.  无线局域网最小构建是基本服务集$BSS$($Basic Service Set$)。一个基本服务集包括一个基站和若干移动站。

    4.  各站在本$BSS$内之间的通信，或与本$BSS$外部站的通信，都必须通过本$BSS$的$AP$

    5.  $BSSID$（基本服务集$ID$）不超过$32$字节，代表基站的$MAC$地址。

    6.  一个基本服务集覆盖的地理范围称为一个基本服务区$BSA$。

    7.  一个$BSS$可以独立也可以通过$AP$连接到一个分配系统$DS$然后连接到另一个$BSS$，构建一个扩展服务集$ESS(Extended Service Set)$

        +   $ESS$通过门桥为无线用户提供以太网的接入。

    8.  $ESS$还可以通过一种称为**门户**$Portal $的设备为无线用户提供到有线连接的以太网的接入

        +   门户的作用相当于一个网桥

        ![image-20230730133622808](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307301336997.png)

        +   图`3.27`中，移动站A如果要和另一个基本服务集中的移动站$B$通信，就必须经过两个接入点$AP_1$和$AP_2$​，即
            $$
            A\to AP_1\to AP_2\to B
            $$
            

            注意$AP_1\to AP_2$的通信是使用有线传输的。

        +   移动站$A$从某个基本服务集漫游到另一个基本服务集时(图`3.27`中的`A'`)，仍然可保持与另一个移动站$B$的通信，但$A$在不同的基本服务集使用的$AP$改变了。

2.    无固定基础设施/自组网络：没有$AP$​，而是一些平等状态的移动站相互通信构成的临时网络。各结点之间地位平等，中间结点都为转发结点，因此都具有路由器的功能。自组网络通常是这样构成的：

    +   一些可移动设备发现在它们附近还有其他的可移动设备，并且要求和其他移动设备进行通信
    +   自组网络中的每个移动站都要参与网络中其他移动站的路由的发现和维护
    +   同时由移动站构成的网络拓扑可能随时间变化得很快，因此在固定网络中行之有效的一些路由选择协议对移动自组网络已不适用，需引起特别的关注：
        1. 静态路由协议：移动自组网络的节点会频繁地移动，导致网络拓扑不断变化，使用静态路由表来处理这种频繁变化是不切实际的。
        2. RIP：RIP不适用于移动自组网络，因为它的路由更新速度较慢，无法适应节点频繁移动的情况。
        3. OSPF：OSPF在移动自组网络中也不适用，因为它的洪泛式链路状态更新会导致较大的网络开销和延迟。
        4. BGP：BGP不适用于移动自组网络，因为它设计用于大规模的自治系统，而移动自组网络通常是小规模、自主组织的网络。

3.    ※※$IEEE802.11$的$MAC$帧格式：$802.11$帧的$MAC$首部中最重要的是$4$个地址字段，其内容取决于帧控制字段中的`去往AP`和`来自AP`这两个字段的数值，其中地址4用于自组网络。其他字段不是考试重点。![c95f6734-9480-49c1-93b7-1e1202658a30](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202310102300178.jpg)

4.    碰撞检测：无线局域网不使用$CSMA/CD$协议，而使用$CSMA/CA$协议，因为发送过程中不需要进行冲突检测

    1. 在无线局域网的适配器上，接收信号的强度往往远小于发送信号的强度，因此若要实现碰撞检测，那么硬件上的花费就会过大。
    2. 在无线局域网中，并非所有站点都能听见对方，由此引发了隐蔽站和暴露站问题，而“所有站点都能够听见对方”正是实现$CSMA/CD$协议必备的基础。

### 虚拟局域网$VLAN$

虚拟局域网$VLAN$基于交换技术按逻辑进行划分局域网一个广播域。**可以隔离冲突域，也可以隔离广播域**，每个VLAN是一个较小的广播域。

1.    虚拟局域网实现：通过$802.ac$标准定义。它在以太网帧中插入一个四字节的标识符（插入在源地址字段和类型字段之间)，称为$VLAN$标签，用来指明发送该帧的计算机属于哪个虚拟局域网。插入$VLAN$标签的帧称为$802.1Q$帧。由于首部增加了四字节，因此以太网的最大帧长从原来的$1518$字节变为$1522$字节。
    1.    $VLAN$技术可以将一个物理局域网在逻辑上划分成多个广播域，即划分为多个$VLAN$
    2.    虚拟局域网$VLAN$通过标识符实现逻辑分组和管理，不需要额外的硬件支持。
    3.    $VLAN$技术部署在数据链路层，用于隔离二层流量
    4.    同一个$VLAN$内的主机共享同一个广播域，它们之间可以进行二层通信
    5.    而$VLAN$间的主机属于不同的广播域，不能直接实现二层互通
    6.    这样，广播报文就被限制在各个相应的$VLAN$内，并提高了网络的安全性
    7.    本质上虚拟局域网使用的是三层架构的交换技术（否则不能隔离广播域和冲突域）。
    8.    同一个$VLAN$的计算机不一定连接在相同的物理网段
        +   可以连接在相同的交换机上，也可以连接在不同的局域网交换机上，只要这些交换机互连即可。

2.    虚拟局域网划分：虚拟局域网的划分与物理地址无关，可以处于不同的实际局域网中，本身只是一种技术。划分方式只能基于物理层、数据链路层和网络层（不能基于更上层的数据）：
    + 基于交换机端口。
    + 基于网卡$MAC$地址。
    + 基于网络层$IP$子网地址。
    + 基于协议。
    + 基于策略。

### 广域网

广域网由一些结点交换机及连接这些交换机的链路组成。广域网是单一的网络。$WAN$的通信子网主要使用分组交换技术，达到资源共享的目的。

1.  结点交换机：功能是将分组存储并转发。注意不是路由器，结点交换机和路由器都用来转发分组，它们的工作原理也类似。结点交换机在单个网络中转发分组，而路由器在多个网络构成的互联网中转发分组

    ![image-20230730221536300](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307302215488.png)

2.  广域网中的一个重要问题是路由选择和分组转发

    +   路由选择协议负责搜索分组从某个结点到目的结点的最佳传输路由，以便构造路由表，然后从路由表再构造出转发分组的转发表
    +   分组是通过转发表进行转发的。 

### 点对点协议$PPP$和$HDLC$协议

#### 点对点协议$PPP$

点对点协议$PPP$是使用最广泛的面向字节的数据链路层协议，用户使用拨号电话接入因特网时一般使用$PPP$协议。

1.    $PPP$协议的特点

    1.    $PPP$提供**差错检测**但**不提供纠错功能**，只**保证无差错接收**（通过硬件进行$CRC$校验）。 它是**不可靠**的传输协议，因此也**不使用序号和确认机制**。
        +   实现差错检测：错就丢弃，只检错不纠错。
        +   实现透明传输：面向字符。对于与帧定界符一样的比特组合的数据应如何处理
            +   异步线路使用字节填充（因为按字节或字符传送），同步线路使用比特填充（因为按比特传送）。
    2.    它仅支持点对点的链路通信，不支持多点线路。
    3.    $PPP$只支持全双工链路。
    4.    $PPP$的两端可以运行不同的网络层协议，但仍然可使用同一个$PPP$进行通信。
    5.    $PPP$是面向字节的，当信息字段出现和标志字段一致的比特组合时，$PPP$有两种不同的处理方法：若$PPP$用在异步线路（默认），则采用字符填充法；若$PPP$用在$SONET/SDH $等同步线路，则协议规定采用硬件来完成比特填充（和$HDLC$的做法一样）。

2.    $PPP$协议的组成

    1.    一个将$IP$数据报封装到串行链路（同步或异步）的方法。
        +   $IP$数据报在$PPP$帧中就是其信息部分，这个信息部分的长度受最大传送单元$MTU$的限制
    2.    链路控制协议$LCP$：建立并维护数据链路连接，并进行身份验证。
    3.    网络控制协议$NCP$：$PPP$支持多种网络层协议，每个不同的网络层协议都需要一个相应的$NCP$来配置，为网络层建立和配置逻辑连接。

3.    $PPP$协议的帧格式：$PPP$是面向字节的，因而**所有$PPP$帧的长度都是整数个字节**，不够的要舍入※※。由于$PPP$是点对点的，并不是总线形，所以无须采用$CSMA/CD$协议，自然就没有最短帧，所以信息段占0〜1500字节，而不是46〜1500字节

    ![PPP帧格式](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307251105437.png)

    + 标志字段$F$：表示帧定界符，用二进制表示就是$0111\;1110$
        + 当数据内出现帧定界符就需要插入转义字符$7D$：$0111\;101$。

    + 地址字段$A(Address)$：规定固定为`0xFF`。
    + 控制字段$C(Control)$：规定固定为`0x03`。
    + 协议：长度可变，表示信息部分的内容，是$IP$数据报、$LCP$数据或网络层控制数据等。
        + 在$HDLC$中没有该字段，它是说明信息段中运载的是什么种类的分组
        + 以比特$0$开始的是诸如$IP、IPX$和$AppleTalk$这样的网络层协议
        + 以比特$1$开始的被用来协商其他协议，包括$LCP$及每个支持的网络层协议的一个不同的$NCP$
        + 为了实现透明传输，当信息段中出现和标志字段一样的比特组合时，必须采用一些措施来改进
            + 插入转义字符$7D$：$0111\;101$

    + 帧检验序列$FCS$：用$CRC$实现的帧检验序列。
        + 检验区包括地址字段、控制字段、协议字段和信息字段

4.    $PPP$协议的工作流程

    1.    当线路处于静止状态时，不存在物理层连接
    2.    当线路检测到载波信号时，建立物理连接，线路变为建立状态
        +   此时，$LCP$开始选项商定，商定成功后就进入身份验证状态
    3.    身份验证通过后，进入网络层协议状态
        +   此时，采用$NCP$配置网络层，配置成功后，进入打开状态，然后就可进行数据传输
    4.    当数据传输完成后， 线路转为终止状态。载波停止后则回到静止状态。

#### $HDLC$协议*

高级数据链路控制协议是一个在同步网上传输数据、面向比特的数据链路层协议。

由$ISO$根据$SDLC$协议扩展开发，不属于$TCP/IP$族。

1.   特点：

     1.   可以透明传输。
     2.   面向比特。只使用$0$比特填充法。
     3.   易于硬件实现。
     4.   只支持全双工通信。
     5.   所有帧采用$CRC$检验。
     6.   对信息帧进行顺序编号，可以防止漏收和重收，传输可靠性高。

2.   数据操作方式：

     1.   正常响应方式：只有经过主站同意从站才能传输数据。
     2.   异步平衡方式：每一个复合站都能自主传输数据。
     3.   异步响应方式：无需经过主站同意从站就能传输数据。

3.   $HDLC$协议的帧格式

     ![HDLC协议的帧格式](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307251106911.png)

     1.   标志字段$F$：表示帧定界符，用二进制表示就是$0111\;1110$
          + 当一串比特流数据中有$5$个连续的$1$时， 就立即在其后填入一个$0$
     2.   地址字段$A$：表示$Address$
          + 当数据操作方式是正常响应方式或异步响应方式，都是表示从站的地址
          + 而如果是异步平衡方式，则填充应答站的地址。

     3.   控制字段$C$：表示$Control$：
          1. 信息帧（$I$）：第一位是$0$，用于传输数据信息，或使用捎带技术对数据进行确认。
          2. 监督帧（$S$）：$10$，用于流量控制与差错控制，执行对信息帧的确认、请求重发和请求暂停发送等功能。
          3. 无编号帧（$U$）：$11$，用于提供对链路的建立、拆除等多种控制功能。

4.   $HDLC$协议与$PPP$协议的联系与区别

     1.   联系
          1.   都只支持全双工链路。
          2.   都可以实现透明传输。
          3.   都只检测差错而不纠正差错。
     2.   区别：

          1.   $PPP$协议是面向字节的，$HDLC$协议是面向比特的。
          2.   $PPP$帧比$HDLC$帧多一个$2$字节的协议字段
               +   当协议字段值为`0x0021`时，表示信息字段是$IP$数据报。
          3.   $PPP$协议不使用序号和确认机制，只保证无差错接收（$CRC$检验），而端到端差错检测由高层协议负责，**不可靠**
               +   $HDLC$协议的信息帧使用了编号和确认机制，能够提供**可靠**传输。
          4.   $PPP$协议使用软件实现，而$HDLC$协议几乎使用硬件实现
          5.   $PPP$协议实现透明传输技术利用同步字符填充异步比特填充，而$HDLC$协议利用比特填充

### ※※数据链路层设备

主要包含网桥与局域网交换机，**均可以隔离碰撞域/冲突域**。

1.    网桥$Bridge$：工作在数据链路层的$MAC$子层，可以互联不同的物理层、不同的$MAC$子层及不同速率的以太网。

    1.    多个以太网通过网桥连接后就形成了更大的以太网，原来的每个以太网就是一个网段。
    2.    由于各网段相对独立，因此一个网段的故障不会影响到另一个网段的运行。
    3.    网桥根据$MAC$帧的目的地址对帧进行转发和过滤
        +   当网桥收到一个帧时，并不向所有接口转发此帧，而是先检查此帧的目的$MAC$地址，然后再确定将该帧转发到哪一个接口，或者是把它丢弃（即过滤）。
    4.    网桥的种类
        1.    透明网桥：指以太网上的站点并不知道所发送的帧将经过哪几个网桥
        2.    源路由网桥：在发送帧时，把详细的最佳路由信息（路由最少/时间最短）放在帧的首部中

    5.    网桥的优点：过滤通信量，增大吞吐量。扩大了物理范围。提高了可靠性。可互联不同物理层、不同$MAC$子层和不同速率的以太网。
    6.    网桥的缺点：增加时延。$MAC$子层没有流量控制功能（流量控制需要编号机制，而编号机制在$LLC$子层）。不同$MAC$子层的网段连接需要帧格式转换。适用于少数据量的局域网，否则会出现广播风暴。

2.    局域网交换机/以太网交换机$Switch$：实质上就是一个多端口的网桥，每一个接口都是一个冲突域。可以独占传输媒体带宽

    1.    交换机的种类
        1.    直通式交换机：查完目的地址（$6B$）直接转发。延迟小，可靠性低，无法支持具有不同速率的端口的交换。
        2.    存储转发式交换机：将帧放入高速缓存，并检查是否正确，正确则转发，错误则丢弃。延迟大，可靠性高，可以支持具有不同速率的端口的交换。

    2.    交换机的特点
        1.    以太网交换机的每个端口都直接与单台主机相连（普通网桥的端口往往连接到以太网的一个网段)，并且一般都工作在全双工方式。
        2.    交换机的总带宽是各端口带宽之和。
        3.    以太网交换机能同时连通许多对端口，使每对相互通信的主机都能像独占通信媒体那样，无碰撞地传输数据。支持多对用户同时通信。
        4.    以太网交换机也是一种即插即用设备（和透明网桥一样)，其内部的帧的转发表也是通过自学习算法自动地逐渐建立起来的。
        5.    以太网交换机由于使用了专用的交换结构芯片，因此交换速率较高。以太网交换机独占传输媒体的带宽。
        6.    使用交换机后带宽总容量会变为原来的$N$倍，$N$为端口数。
    3.    交换方式
        1.    直通式交换机：
            + 只检查帧的**目的地址**（$6B$），这使得帧在接收后几乎能马上被传出去。
            + 这种方式速度快，但缺乏智能性和安全性，也无法支持具有不同速率的端口的交换。
        2.    存储转发式交换机：
            + 先将接收到的帧缓存到高速缓存器中，并检查数据是否正确，确认无误后通过查找表转换成输出端口将该帧发送出去。
            + 如果发现帧有错，那么就将其丢弃。
            + 存储转发式的优点是可靠性高，并能支持不同速率端口间的转换。
            + 缺点是延迟较大。

3.    交换机与网桥

    1.    网桥的端口一般连接局域网，而交换机的端口一般直接与局域网的主机相连。
    2.    交换机允许多对计算机同时通信，而网桥仅允许每个网段上的计算机同时通信。
    3.    网桥采用存储转发进行转发，而以太网交换机还可以采用直通方式进行转发，且以太网交换机采用了专用的交换结构芯片，转发速度比网桥快。

4.    冲突域与广播域：一个网段就是一个冲突域，一个局域网就是一个广播域。

    1.    冲突域：在以太网中，如果某个$CSMA/CD$网络上的两台计算机在同时通信时会发生冲突（即同一时间只能一台主机发送信息），那么这个$CSMA/CD$网络就是一个冲突域。如果以太网中各个网段以集线器连接，因为不能避免冲突，所以它们仍然是一个冲突域。（一个数据链路层设备的接口一个冲突域）
        + 内网通信量更大的情况下更需要隔离冲突域
    2.    广播域=局域网：网络中能接收任一设备发出的广播帧的所有设备的集合。即如果一个站点发出一个广播信号，那么所有能接收到该信号的设备范围就是一个广播域。（一个网络层设备一个广播域）
    3.    网段=冲突域：指一个计算机网络中使用同一物理层设备（传输介质、中继器、集线器等）能直接通信的一部分。

