# 第五章 中央处理器$(CPU)$

## 导读

### 【考纲内容】

1. CPU 的功能和基本结构                                                       
2. 指令执行过程
3. 数据通路的功能和基本结构
4. 控制器的功能和工作原理
5. 异常和中断机制
    1. 异常和中断的基本概念，异常和中断的分类; 异常和中断的检测与响应

6. 指令流水线
    1. 指令流水线的基本概念
    2. 指令流水线的基本实现
    3. 结构冒险、数据冒险和控制冒险的处理
    4. 超标量和动态流水线的基本概念

7. 多处理器基本概念
    1. $SISD$、$SIMD$、$MIMD$、向量处理器的基本概念
    2. 硬件多线程的基本概念
    3. 多核$(multi-core)$处理器的基本概念
    4. 共享内存多处理器 $(SMP) $的基本概念

### 【知识导图】

![img](http://res.ptpress.cn/36AC35AB87F046638825B525E7461D78.png)

![img](http://res.ptpress.cn/AE377BFDA3CC41C58E31B10C083A6ACF.png)

![img](http://res.ptpress.cn/0ECBAB73C5B0462282B64370F38841D7.png)

![img](http://res.ptpress.cn/31B7D7A28541465F8ED83ED0BFD6D922.png)

![img](http://res.ptpress.cn/D27FEA23ED4E4E349133D0D1136B1B49.png)

![img](http://res.ptpress.cn/E7F58AB909C74113B87FDA486B3FE01D.png)

![img](http://res.ptpress.cn/B1E5799F550A46C7B10A6A8888E5FF31.png)

![image-20230603095230601](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/image-20230603095230601.png)

![img](http://res.ptpress.cn/23C5A832CE9D497797DAE3E3A5EEAC23.png)

![img](http://res.ptpress.cn/27C7442F819549B5BE55A3427B590CAC.png)

![img](http://res.ptpress.cn/CC9FD1FC8E21462AB2DA312E22E7C149.png)

![img](http://res.ptpress.cn/DABCD40FB97049B9A69A3A3E2989570C.png)

![img](http://res.ptpress.cn/A49C1C1C425142009CE514B3DBD18DD9.png)

![img](http://res.ptpress.cn/897A9D9405864D7280040DECC2523212.png)

![img](http://res.ptpress.cn/F3D19288780642AF985FE0D671613D4A.png)

![img](http://res.ptpress.cn/B2FBB242C124458D87EA49977291E49D.png)

![img](http://res.ptpress.cn/967CF1FA3A71418482699C75341D3EDD.png)

![image-20230603095542154](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/image-20230603095542154.png)



![img](http://res.ptpress.cn/292CC4CB6D0649C4998F669D2AAB1654.png)

![image-20230603095807976](C:/Users/willi/AppData/Roaming/Typora/typora-user-images/image-20230603095807976.png)



### 【复习提示】

中央处理器是计算机的中心，也是本书的难点。其中，数据通路的分析,指令执行阶段的节拍与控制信号的安排、流水线技术与性能分析易出综合题。而关于各种寄存器的特点、指令执行的各种周期与特点、控制器的相关概念、流水线的相关概念也极易出选择题。

在学习本章时，请读者思考以下问题:

1) 指令和数据均存放在内存中，计算机如何从时间和空间上区分它们是指令还是数据?

2) 什么是指令周期、机器周期和时钟周期? 它们之间有何关系?

3) 什么是微指令? 它和第 4 章谈到的指令有什么关系?

4) 什么是指令流水线? 指令流水线相对于传统体系结构的优势是什么?

