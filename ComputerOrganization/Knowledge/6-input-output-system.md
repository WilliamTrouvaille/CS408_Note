# 第七章 输入输出$(I/O)$系统

## 导读

### 【考纲内容】

1. $I/O$接口($I/O$控制器)
    + $I/O$接口的功能和基本结构:$I/O$端口及其编址

2. $I/O$方式
    + 程序查询方式
    + 程序中断方式
        + 中断的基本概念、中断响应过程、中断处理过程
        + 多重中断和中断屏项的概念
    + $DMA $方式，$DMA $控制器的组成，$DMA $传送过程





### 【知识导图】

![img](http://res.ptpress.cn/E32C16AC9AA2428993797C71CA1125CA.png)

![img](http://res.ptpress.cn/050A2E52793840DAA3C46BAD14DC8833.png)

![img](http://res.ptpress.cn/8F1BA494D6C94FC7AD7C93C880680202.png)

![image-20230603100141352](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/image-20230603100141352.png)





### 【复习提示】

$I/O$方式是本章的重点和难点，每年不仅会以选择题的形式考查基本概念和原理，而且可能会以综合题的形式考查，特别是各种$I/O$方式效率的相关计算，中断方式的各种原理、特点、处理过程、中其屏蔽，$DMA $方式的特点、传输过程、与中断方式的区别等。

在学习本章时，请读者思考以下问题;

1) $I/O$设备有哪些编址方式? 各有何特点?

2) $CPU$ 响应中断应具备哪些条件?