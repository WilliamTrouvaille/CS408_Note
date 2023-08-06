# 第六章 图

## 导读

### 【考纲内容】

1. 图的基本概念
2. 图的存储及基本操作
    + 邻接矩阵
    + 邻接表
    + 邻接多重表
    + 十字链表
3. 图的遍历
    + 深度优先搜索
    + 广度优先搜索
4. 图的基本应用
    + 最小（代价）生成树
    + 最短路径
    + 拓扑排序
    + 关键路径

### 【知识导图】

![23July20-225936-1689865176-0f81b088-843d-4d83-a198-b9b012cb6d5e](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202259594.png)

### 【复习提示】

+ 图算法的难度较大，主要掌握深度优先搜索与广度优先搜索
+ 掌握图的基本概念及基本性质、图的存储结构（邻接矩阵、邻接表、邻接多重表和十字链表）及其特性、存储结构之间的转化、基于存储结构上的遍历操作和各种应用（拓扑排序、最小生成树、最短路径和关键路径）等
+ 图的相关算法较多、易混，通常==只要求掌握其基本思想和实现步骤，而算法的具体实现不是重点==。

## 基本概念

![image-20230720230423846](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202304066.png)

---

![1689597937t-20230717-2037-937.474](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689597937t-20230717-2037-937.474.png)

### 常见考点

#### 完全图

+   无向完全图，无向图中任意两个顶点之间都存在边
    +   即$\vert E\vert=\dfrac{|V|(|V|-1)}{2}$
    +   就是上图那个$C_n^2$
+   有向完全图：有向图中任意两个顶点之间都存在方向相反的两条弧
    +   即$\vert E\vert=\vert V\vert(\vert V\vert-1)$
    +   就是上图那个$2C_n^2$

#### 连通图

+   成环

    +   若一个无向图有$n$个顶点和$n-1$条边，可以使它连通但没有环（即生成树）
    +   但若再加一条边，在不考虑重边的情形下，则必然会构成环
    +   即$\vert E\vert>n-1$时，一定存在回路

+   一个有$|E|$条边的非连通无向图至少有$m$个顶点(求$m$)

    +   考虑该非连通图最极端的情况，即它由一个无向完全图加一个独立的顶点构成，此时若再加一条边，则必然使图变成连通图
    +   由$\vert E\vert=\dfrac{|V|(|V|-1)}{2}$算出$\vert V\vert$
    +   加一个独立的顶点，即$m=\vert V\vert+1$

+   对于一个有$n$个顶点的图

    +   若是连通无向图，其边的个数至少为$n-1$
        +   对于连通无向图，边最少即构成一棵树
    +   若是强连通有向图，其边的个数至少为$n$
        +   对于强连通有向图，边最少即构成一个有向环

+   具有$\vert V\vert$个顶点的无向图，当有$m$条边时能确保是一个连通图(求$m$)

    +   考虑该非连通图最极端的情况，即它由一个无向完全图加一个独立的顶点构成，此时若再加一条边，则必然使图变成连通图
    +   由$\vert E\vert=\dfrac{|V|(|V|-1)}{2}$算出$\vert E\vert$
    +   加一条边，即$m=\vert E\vert+1$

+   图的顶点数为$\vert V\vert$，图的边数为$\vert E\vert$​，当且仅当
    $$
    \vert E\vert\ge\dfrac{(|V|-1)(|V|-2)}{2}+1
    $$
    时，图才一定是连通的

    +   此时$\vert V\vert-1$个顶点构成一个完全图，若再加入一条边，则一定变成连通图

+   图的顶点数为$\vert V\vert$，图的边数为$\vert E\vert$，当$\vert E\vert\ge\vert V\vert+1$时，图可能是连通的

####  度

+ 对于具有$n$个顶点，$e$条边的无向图
    $$
    \sum\limits_{i=1}^nTD(v_i)=2\vert E\vert=2e
    $$

    + 即无向图的全部顶点的度的和等于边数的两倍
    + 因为每条边都与两个顶点关联

+ 对于具有$n$个顶点，$e$条边的有向图
    $$
    \sum\limits_{i=1}^nID(v_i)=\sum\limits_{i=1}^nOD(v_i)=\vert E\vert=e
    $$

    + 因为每条有向边都有一个起点和终点

+ 对于邻接矩阵存储的图，顶点$i$的度（入度+出度）等于第$i$行第$i$列所有数字加起来的和

    ![23July21-120535-1689912335-f2df6070-6a03-45b7-9343-cbd240e94685](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307211206116.png)

#### 易错概念

+   子图
    +   必须满足原图的==关系(即为边和对应的端点)==的子集才算子图
+   图的遍历
    +   图的遍历要求每个结点访问且只能被访问一次
    +   且若图非连通，则从某一顶点出发无法访问到其他全部顶点
+   稀疏图和稠密图
    +   稀疏图：边数很少的图，一般$\vert E\vert<\vert V\vert\log\vert V\vert$
    +   稠密图：边数很多的图，一般$\vert E\vert>\vert V\vert\log\vert V\vert$
+   回路（环）：第一个顶点与最后一个顶点相同的路径。
    + 一个只含有一个顶点的图不是环
    + 对于只含有一个顶点的图，由于没有边连接任何其他顶点，因此无法构成环
+   简单路径：路径中的顶点不重复出现。
    +   回路（环）一定不是简单路径

### 图的定义

图是顶点集和边集构成的二元组，即图$G$由顶点集$V$和边集$E$组成，记为$G=(V,E)$，其中$V(G)$表示图$G$中顶点的有限非空集，$E(G)$表示图$G$中顶点之间的关系（边）集合。

若$V=\{v_1,v_2\cdots,v_n\}$，则==用$\vert V\vert$表示图$G$中顶点的个数==，也称图$G$的阶，$E=\{(u,v)\vert u\in V,v\in V\}$==，用$\vert E\vert$表示图$G$中边的条数==。

图一定是非空的，即$V$一定是非空集。

### 图的类别

![image-20230720230323647](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202303315.png)

+ 无向图：若$E$是无向边（简称边）的有限集合时，则图$G$为无向图。
    + 边是顶点的无序对，记为$(v,w)$或$(w,v)$，因为$(v,w)=(w,v)$
        + 其中$v$、$w$是顶点。可以说顶点$w$和顶点$v$互为邻接点

    + 边$(v,w)$依附于顶点$w$和$v$，或者说边$(v,w)$和顶点$v$、$w$相关联。

+ 有向图：若$E$是有向边（也称弧）的有限集合时，则图$G$为有向图。
    + 弧是顶点的有序对，记为$<v,w>$
        + 其中$v$、$w$是顶点，$v$称为弧尾，$w$称为弧头，$<v,w>$称为从顶点$v$到顶点$w$的弧，也称$v$邻接到$w$，或$w$邻接自$v$

    + $<v,w>\neq<w,v>$

+ 简单图：不存在重复边，且不存在顶点到自身的边
    + 一般的图默认是简单图。

+ 多重图：图$G$中某两个顶点之间的边数多于一条，又允许顶点通过同一条边与自己关联。
+ 无向完全图：对于无向图$\vert E\vert\in[0,\dfrac{n(n-1)}{2}]$，无向图中任意两个顶点之间都存在边，即$\vert E\vert=\dfrac{n(n-1)}{2}$。
+ 有向完全图：对于有向图$\vert E\vert\in[0,n(n-1)]$，有向图中任意两个顶点之间都存在方向相反的两条弧，即$\vert E\vert=n(n-1)$。
+ 稀疏图：边数很少的图，一般$\vert E\vert<\vert V\vert\log\vert V\vert$
+ 稠密图：边数很多的图，一般$\vert E\vert>\vert V\vert\log\vert V\vert$
+ 树：不存在回路，且连通的无向图
    + **与图是逻辑的区别**

+ 有向树：一个顶点的入度为$0$，其余顶点入度均为$1$的有向图。

### 顶点的度

#### 度的定义

对于无向图，顶点$v$的度是指依附于该顶点的边的条数，记为$TD(v)$。

对于有向图，入度是指以顶点$v$为终点的有向边的条数，记为$ID(v)$；出度指以顶点$v$为起点的有向边的条数，记为$OD(v)$；顶点$v$的度就是其入度和出度之和，即
$$
TD(v)=ID(v)+OD(v)
$$

#### 度的关系

+ 对于具有$n$个顶点，$e$条边的无向图
    $$
    \sum\limits_{i=1}^nTD(v_i)=2\vert E\vert=2e
    $$

    + 即无向图的全部顶点的度的和等于边数的两倍。因为每条边都与两个顶点关联。
+ 对于具有$n$个顶点，$e$条边的有向图
    $$
    \sum\limits_{i=1}^nID(v_i)=\sum\limits_{i=1}^nOD(v_i)=\vert E\vert=e
    $$

    + 因为每条有向边都有一个起点和终点。
+ 对于$n$个顶点的无向图，每个顶点的度最大为$n-1$。因为任意一个顶点可以与其他$n-1$个顶点相联。默认是简单图，即不能自己连向自己。
+ 对于$n$个顶点的有向图，每个顶点的度最大为$2n-2$。因为任意一个顶点可以与其他$n-1$个顶点有指向相反的两条边。
+ 无向连通图的每个顶点的度都是$2$。

### 顶点的关系

+ 路径：从一个点到另一个点所经过的顶点序列。由顶点和相邻顶点序偶构成的边所形成的**序列**。
    + 路径是**序列**，距离是路径的长度是个**数**

+ 回路（环）：第一个顶点与最后一个顶点相同的路径。
    + 一个只含有一个顶点的图不是环
    + 对于只含有一个顶点的图，由于没有边连接任何其他顶点，因此无法构成环

+ 长度（无权图）：沿路径所经过的边数成为该路径的长度。
+ 简单路径：路径中的顶点不重复出现。
+ 简单回路：由简单路径组成的回路。
    + 除第一个和最后一个顶点外其余顶点不重复出现的回路

+ 点到点的距离：从顶点$u$到顶点$v$的最短路径若存在，则此路径的长度就是从$u$到$v$的路径
    + 若不存在路径，则记该路径为无穷$\infty$
    + 路径是**序列**，距离是路径的长度是个**数**


若一个图有$n$个顶点，有大于$n-1$条边，则此图一定有环。

### 图的连通

#### 连通概念

+ 连通：在==无向图==中，若从顶点$v$到顶点$u$有**路径**存在，则称$uv$是连通的。

+ 强连通：在==有向图==中，若从顶点$v$到顶点$u$和从顶点$u$到顶点$v$之间都有**路径**（而不是弧），则称$uv$是强连通。

+ 连通图：无向图中任意两个顶点之间都是连通的。

    + 只含有一个顶点的图是连通的
    + ==当一个连通图包含至少三个节点时，它必定含有环。==

+ 强连通图：有向图中任意两个顶点之间都是强连通的。

    + ==当一个强连通图包含至少三个节点时，它必定含有环==

+ 子图：设有两个图$G=(V,E)$和$G'=(V',E')$，若$V'$是$V$的子集，$E'$是$E$的子集，则$G'$是$G$的子图。

    ![1689597377t-20230717-2017-596.338](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689597377t-20230717-2017-596.338.png)

    + 但是不是所有的子集都能构成子图，必须满足原图的==关系(即为边和对应的端点)==的子集才算

        + 即存在$\varphi'\in\varphi$

    + 如原图中一条边的两个端点而子图的点集不包含两个端点 ，则这个图不是子图

        ![1689597427t-20230717-2007-393.168](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689597427t-20230717-2007-393.168.png)

+ 生成子图：若有满足$V(G')=V(G)$的子图$G'$，则$G'$是$G$的生成子图。

    + 即子图包含所有顶点，但不一定包含所有的边

        ![1689597455t-20230717-2035-886.361](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689597455t-20230717-2035-886.361.png)

+ 连通分量：无向图$G$中的极大连通子图称为$G$的连通分量

    + 对任何连通图而言，连通分量就是其自身。

    + 对非连通图而言，连通分量可能有多个

        ![1689597577t-20230717-2037-890.372](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689597577t-20230717-2037-890.372.png)

+ 强连通分量：有向图$G$中的极大连通子图称为$G$的强连通分量

    + 对任何强连通图而言，强连通分量就是其自身。

    + 对非强连通图而言，强连通分量可能有多个。

        ![1689597549t-20230717-2009-883.357](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689597549t-20230717-2009-883.357.png)

+ 生成树：包含==连通图==中全部顶点的一个极小连通子图

    + 顶点全部要有，边尽可能的少，但要保持连通

    + 若图的顶点为$n$，则其生成树包含$n-1$条边

    + 若去掉生成树的一条边则会变成非连通图，若加上一条边则会形成一个回路。

        ![1689597632t-20230717-2032-603.349](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689597632t-20230717-2032-603.349.png)

+ 生成森林：在非连通图中，连通分量的生成树构成了非连通图的生成森林。

对于无向图：

+ 极大连通子图：用来讨论图的连通分量，要求连通子图包含其所有的边。
    + 连通图的极大连通子图就是它本身。
    + 非连通图中有多个连通分量（不同的点相连从而连通），也就是可以有多个极大连通子图。
+ 极小连通子图：用来讨论图的生成树，要保持图连通也要让边数最小的子图。
    + 极小连通子图只在无向图中才有。
    + 极小连通子图中包含图中全部的顶点（和极大不同，极大不要求包含所有的顶点）。
    + 用边将极小连通图中的所有边都连接起
    + 极小连通子图和生成树的概念不是等价的，生成树是包含图中全部顶点的一个极小连通子图。

对于有向图：

+ 极大强连通子图：
    + 强连通图的极大强连通子图为其本身。（是唯一的）
    + 非强连通图有多个极大强连通子图。（非强连通图的极大强连通子图叫做强连通分量）
+ 不存在极小强连通子图的概念，因为树没有方向性。

无向图研究连通性，有向图研究强连通性。

#### 连通与边点关系

+ 对于$n$个顶点的无向图
    + 若其是连通图，则最少需要$n-1$条边
    + 若其是非连通图，则最多有$C_{n-1}^2$条边
        + $n-1$个顶点构成一个完全图

+ 对于$n$个顶点的有向图
    + 若其是强连通图，则最少需要$n$条边
        + 形成环路的情况

+ 对于$n$个顶点的环，有$n$棵生成树。因为$n$个顶点的环的生成树的顶点为$n-1$，去掉任意一条边就能得到一棵生成树，环一共有$n$条边，所以可以去掉$n$条，得到$n$棵生成树。
+ 对于$n$个顶点、$e$条边的无向图是一个森林，则一共有$n-e$棵树。设一共有$x$棵树，则只需要$x-1$条边就能将森林连接为一整棵树，所以边数$+1=$顶点数（树的性质），即$e+(x-1)+1=n$，解得$x=n-e$。

#### 连通概念关系

+ 有向完全图是强连通有向图。（完全就代表所有边都有双向弧，强连通代表所有边都有双向路径，条件更强）
+ 生成树是原图的无环子图、极小连通子图且点集相同。（不是连通分量）

如何求/连通分量强连通分量数？

+ 当某个顶点只有出弧而没有入孤时，其他顶点无法到达这个项点，不可能与其他顶点和边构成强连通分量（这个单独的顶点构成一个强连通分量）。
+ 依次选择无入弧顶点构成连通分量，删除该顶点以及所有以之为结尾的弧。
+ 最后得到的每个顶点就是一个强连通分量，其数量就是强连通分量数。

### 图的权

+ 边的权：在一个图中，每条边都可以表上具有某种含义的数值，这就是该边的权值。
+ 网络（网）：若图中的每条边都有权，这个带权图被称为网。
+ 带权路径长度：取沿路径各边的权之和作为此路径的长度。

#### 无权图

若$v[i][j]=0$，表示$v_{i+1}$到$v_{j+1}$是不连通的，若$v[i][j]=1$，表示$v_{i+1}$到$v_{j+1}$是连通的。

#### 网

若$v[i][j]=\infty$，表示$v_{i+1}$到$v_{j+1}$是不连通的，若$v[i][j]=$某权值，表示$v_{i+1}$到$v_{j+1}$是连通的。

## 图的存储结构

![image-20230720230734473](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202307362.png)

---

![1689600965t-20230717-2105-1524.761](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689600965t-20230717-2105-1524.761.png)

### 邻接矩阵

#### 邻接矩阵定义

+   用一个一维数组保存顶点，用一个二维数组保存边，这个二维数组就是邻接矩阵。
+   使用一个长宽皆为$\vert v\vert$的二维矩阵$v$，从左上角到右上角，从左上角到左下角，分别标识表示$v_1,v_2\cdots,v_n$。

<!-- 假设矩阵的索引从$0$开始，而顶点编号从$1$开始。 -->

+   对于无向图$v_{ij}=v_{ji}=1$，表示存在边$(v_i,v_j)$；对于有向图$v_{ij}=1$表示存在边$<v_i,v_j>$。
+   若$(v_i,v_j)$是$E(G)$中的边
    +   对于无权图，矩阵$A[i][j]$存在就设为$1$，否则是$0$
    +   对于有权图，矩阵$A[i][j]$存在就设为其权重$w_{ij}$，否则是$0$或$\infty$

![1689598210t-20230717-2010-640.204](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689598210t-20230717-2010-640.204.png)

>   注 这里的无穷大宏定义的还挺...照顾新手的`(¬д¬。)`
>
>   C++ 中一般使用各种数据类型的最大值表示无穷大,根据题意一般使用`int`类型或`long`类型的最大值.
>
>   ~~当前也可以写`1e10`或者很多个`9`~~
>
>   在 C++ 中，可以使用 `<limits>` 头文件中的模板类 `std::numeric_limits` 来表示各种数据类型的最大值。该模板类提供了静态成员函数和静态成员变量，用于获取数据类型的特性，包括最大值。
>
>   以下是几种常见数据类型的最大值表示方法：
>
>   - 获取整数类型的最大值：
>
>     ```cpp
>     #include <limits>
>     #include <iostream>
>           
>     int main() {
>         //获取整数类型的最大值
>         std::cout << "Max value of int: " 		<< std::numeric_limits<int>::max() << std::endl;
>         std::cout << "Max value of long long: " << std::numeric_limits<long long>::max() << std::endl;
>         // 其他整数类型的获取方式类似
>               
>         //获取整数类型的最大值
>         std::cout << "Max value of float: "  << std::numeric_limits<float>::max() << std::endl;
>         std::cout << "Max value of double: " << std::numeric_limits<double>::max() << std::endl;
>         // 其他浮点数类型的获取方式类似
>         return 0;
>     }
>     ```
>

#### 邻接矩阵性质

度的性质：

+ 对于无向图
    + 第$i$个顶点的度=第$i$行或第$i$列的非零元素个数。

+ 对于有向图
    + 第$i$个顶点的==出度$=$第$i$行==的非零元素个数
    + 第$i$个顶点的==入度$=$第$i$列==的非零元素个数
    + 第$i$个顶点的度$=$第$i$行的非零元素个数$+$第$i$列的非零元素个数。


存储性质：

+ 适用于存储稠密图。
+ 对于无向图，因为没有方向，所以只有两点连接就是连通的，从而==无向图的邻接矩阵都是主对角线对称的==
    + 因为对称，所以可以使用上下三角矩阵压缩存储。

+ 对于无向图，图的边数等于上三角或下三角不包括主对角线的区域内的非零点的数量。
+ 对于有向图，图的边数等于矩阵内所有非零点的数量。
+ 空间复杂度是$O(\vert V\vert^2)$。
    + 只和顶点数相关，和实际的边数无关
    + 适合存储稠密图
        + 邻接矩阵更适合存储稠密图，即具有大量边的图
        + 稠密图的边数接近于顶点数的平方级别，因此使用邻接矩阵可以==更有效地表示连接关系==
            + 稠密图：边数很多的图，一般$\vert E\vert>\vert V\vert\log\vert V\vert$
        + 在稠密图中，邻接矩阵中的大部分元素都是非零的，因此没有大量的空间浪费。

+ 邻接矩阵存储图，很容易确定图中任意两个顶点之间是否有边相连，时间复杂度为$O(1)$
    + 但是，要确定图中有多少条边，则必须按行、按列对每个元紊进行检测，所花费的时间代价很大。

+ 给定顶点找到其邻边要扫描一行，时间复杂度为$O(\vert V\vert)$。
+ **邻接矩阵的表示方式是唯一的**。
+ 若一个有向图的邻接矩阵为三角矩阵（对角线以上或以下的元素为$0$）,则图中必不存在环
+ 对有向图中的顶点适当地编号，使其邻接矩阵为三角矩阵且主对角元全为零的充分必要条件是，该有向图可以进行拓扑排序

数学性质：

+ ==设图$G$的邻接矩阵为$A$，$A^n$的元素$A^n[i][j]$表示由顶点$i$到顶点$j$长度为$n$的路径数量。==

    ![1689599354t-20230717-2114-884.159](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689599354t-20230717-2114-884.159.png)

设图$G$的邻接矩阵为$A$，矩阵元素为$0$或$1$，则$A^n$的元素$A^n[i][j]$表示由顶点$v_{i+1}$到顶点$v_{j+1}$的长度为$n$的路径的数目。

### 邻接表

![1689600006t-20230717-2106-798.227](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689600006t-20230717-2106-798.227.png)

#### 表定义

+   邻接表存储方式是顺序存储与链式存储的结合，存储方式和树的孩子表示法类似。
+   使用一个数组顺序保存图的每一个顶点，称为顶点表
+   使用链式存储让每一个顶点元素包含一个指向后一条边的指针，称为边表。

![1689599463t-20230717-2103-725.270](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689599463t-20230717-2103-725.270.png)

#### 表性质

![image-20230721124251467](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307211242612.png)

度的性质：

+ 对于无向图，每个顶点的边链表的顶点数就是该顶点的度。
+ 对于有向图，每个顶点的边链表的顶点数就是该顶点的出度，而对于入度就只能遍历所有顶点的顶点链表。

存储性质：

+ 对于无向图，==因为同一条边两端的点会重复存储==，所以空间复杂度为$O(\vert V\vert+2\vert E\vert)$
+ 而对于有向图空间复杂度为$O(\vert V\vert+\vert E\vert)$。
+ 给定顶点找到其邻边只需要读取其邻接表，时间复杂度为$O(1)$。
+ 找到两个顶点是否存在邻边，需要在相应边表中查找另一个顶点。
+ 对于稀疏图，使用邻接表回更方便。
    + 稀疏图：边数很少的图，一般$\vert E\vert<\vert V\vert\log\vert V\vert$

+ ==邻接表的表示方式是不唯一的==。

### 十字链表

==十字链表置只能用于存储有向图==。可以解决邻接矩阵空间复杂度高和邻接表计算入度入边不方便的问题。

十字链表定义了两种顶点：

![1689600430t-20230717-2110-839.218](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689600430t-20230717-2110-839.218.png)

+ 顶点顶点：用于表示顶点，被一个数组包裹。
    + 数据域。
    + 该顶点作为弧头的第一条弧。
    + 该顶点作为弧尾的第一条弧。
+ 弧顶点：被顶点顶点指向的顶点。
    + 弧尾顶点编号。
    + 弧头顶点编号。
    + 权值。
    + 弧头相同的下一条弧。
    + 弧尾相同的下一条弧。

空间复杂度为$O(\vert V\vert+\vert E\vert)$。

十字链表法存储入边和出边都很方便

==十字链表图表示是不唯一的==

![1689600529t-20230717-2149-894.269](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689600529t-20230717-2149-894.269.png)

### 邻接多重表

==邻接多重表只能用于存储无向图==，可以解决邻接矩阵空间复杂度高和邻接表删除插入顶点不方便的问题。

![1689600753t-20230717-2133-850.465](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689600753t-20230717-2133-850.465.png)

邻接多重表定义了两种顶点：

+ 顶点顶点：用于表示顶点，被一个数组包裹。
    + 数据域。
    + 该顶点相连的第一条边。
+ 边顶点：被顶点顶点指向的顶点。
    + 边一端编号$i$。
    + 边另一端编号$j$。
    + 权值。
    + 依附于$i$的下一条边。
    + 依附于$j$的下一条边。

空间复杂度为$O(\vert V\vert+\vert E\vert)$。

邻接多重表每个结点只需要维护一个结点,没有冗余

邻接多重表表示是不唯一的。

## 图的基本操作

+   `Adjacent(G, x, y)`
    +   判断图G是否存在有向边`<x,y>`或无向边`(x,y)`

+   `Neighbors(G, x)`
    +   列出图G中与结点x邻接的边。
+   `Insertvertex(G, x)`
    +   在图 G 中插入顶点 x。
+   `DeleteVertex (G, x)`
    +   从图 G 中删除顶点 x。
+   `AddEdge(G, x, y)`
    +   若无向边`(x,y)`)或有向边`<x,y>`不存在，则向图G中添加该边。
+   `RemoveEdge(G, x, y)`
    +   若无向边`(x,y)`)或有向边`<x,y>`存在，则从图G中删除该边。
+   `FirstNeighbor (G, x)`
    +   求图G中顶点x的第一个邻接点，若有则返回顶点号。若x没有邻接点或图中不存在x,则返回-1。
+   `NextNeighbor (G, x, y)`
    +   假设图G中顶点y是顶点x的一个邻接点，返回除y外顶点x的下一个邻接点的顶点号，若y是x的最后一个邻接点，则返回-1。
+   `Get_edge_value (G, x, y)`
    +   获取图 G 中有向边`<x,y>`或无向边`(x,y)`对应的权值。
+   `Set_edge_value (G, x, y, v)`
    +   设置图G中无向边`(x,y)`或有向边`<x,y>`对应的权值为`v`。 

---

>   以下是基于==邻接表==表示的图的基本操作的 C++ 实现代码：
>
>   ```cpp
>   #include <iostream>
>   #include <vector>
>   
>   // 图的顶点结构体
>   struct Vertex {
>       int value;                        // 顶点的值
>       std::vector<int> neighbors;       // 与顶点邻接的顶点列表
>   };
>   
>   // 图的类
>   class Graph {
>   public:
>       Graph(int numVertices) {
>           vertices.resize(numVertices);
>       }
>   
>       // 判断图中是否存在边 <x, y> 或 (x, y)
>       bool Adjacent(int x, int y) {
>           if (x < 0 || x >= vertices.size() || y < 0 || y >= vertices.size())
>               return false;
>   
>           // 检查顶点 x 的邻接顶点中是否包含顶点 y
>           for (int neighbor : vertices[x].neighbors) {
>               if (neighbor == y)
>                   return true;
>           }
>   
>           return false;
>       }
>   
>       // 列出与顶点 x 邻接的边
>       std::vector<int> Neighbors(int x) {
>           if (x < 0 || x >= vertices.size())
>               return {};
>   
>           return vertices[x].neighbors;
>       }
>   
>       // 在图中插入顶点 x
>       void InsertVertex(int x) {
>           Vertex v;
>           v.value = x;
>           vertices.push_back(v);
>       }
>   
>       // 从图中删除顶点 x
>       void DeleteVertex(int x) {
>           if (x < 0 || x >= vertices.size())
>               return;
>   
>           vertices.erase(vertices.begin() + x);
>   
>           // 删除与顶点 x 相关的边
>           for (auto& vertex : vertices) {
>               std::vector<int>& neighbors = vertex.neighbors;
>               neighbors.erase(std::remove(neighbors.begin(), neighbors.end(), x), neighbors.end());
>           }
>       }
>   
>       // 向图中添加边 (x, y) 或 <x, y>
>       void AddEdge(int x, int y) {
>           if (x < 0 || x >= vertices.size() || y < 0 || y >= vertices.size())
>               return;
>   
>           vertices[x].neighbors.push_back(y);
>           // 若为无向图，需加上下面一行
>           // vertices[y].neighbors.push_back(x);
>       }
>   
>       // 从图中删除边 (x, y) 或 <x, y>
>       void RemoveEdge(int x, int y) {
>           if (x < 0 || x >= vertices.size() || y < 0 || y >= vertices.size())
>               return;
>   
>           std::vector<int>& neighbors = vertices[x].neighbors;
>           neighbors.erase(std::remove(neighbors.begin(), neighbors.end(), y), neighbors.end());
>           // 若为无向图，需加上下面一行
>           // vertices[y].neighbors.erase(std::remove(vertices[y].neighbors.begin(), vertices[y].neighbors.end(), x), vertices[y].neighbors.end());
>       }
>   
>       // 求顶点 x 的第一个邻接点，若有则返回顶点号。若 x 没有邻接点或图中不存在 x，则返回 -1。
>       int FirstNeighbor(int x) {
>           if (x < 0 || x >= vertices.size())
>               return -1;
>   
>           if (!vertices[x].neighbors.empty())
>               return vertices[x].neighbors[0];
>   
>           return -1;
>       }
>   
>       // 返回除顶点 y 外顶点 x 的下一个邻接点的顶点号，若 y 是 x 的最后一个邻接点，则返回 -1。
>       int NextNeighbor(int x, int y) {
>           if (x < 0 || x >= vertices.size() || y < 0 || y >= vertices.size())
>               return -1;
>   
>           std::vector<int>& neighbors = vertices[x].neighbors;
>           auto it = std::find(neighbors.begin(), neighbors.end(), y);
>           if (it != neighbors.end() && std::next(it) != neighbors.end())
>               return *(std::next(it));
>   
>           return -1;
>       }
>   
>   private:
>       std::vector<Vertex> vertices;     // 存储图的顶点
>   };
>   
>   int main() {
>       // 创建图并进行基本操作示例
>       Graph graph(5);
>   
>       graph.InsertVertex(0);
>       graph.InsertVertex(1);
>       graph.InsertVertex(2);
>       graph.InsertVertex(3);
>       graph.InsertVertex(4);
>   
>       graph.AddEdge(0, 1);
>       graph.AddEdge(0, 2);
>       graph.AddEdge(1, 3);
>       graph.AddEdge(2, 3);
>       graph.AddEdge(2, 4);
>   
>       std::cout << "Adjacent(0, 1): " << graph.Adjacent(0, 1) << std::endl;
>       std::cout << "Neighbors of 2: ";
>       std::vector<int> neighbors = graph.Neighbors(2);
>       for (int neighbor : neighbors) {
>           std::cout << neighbor << " ";
>       }
>       std::cout << std::endl;
>   
>       graph.DeleteVertex(2);
>   
>       std::cout << "First neighbor of 0: " << graph.FirstNeighbor(0) << std::endl;
>       std::cout << "Next neighbor of 0 after 1: " << graph.NextNeighbor(0, 1) << std::endl;
>   
>       return 0;
>   }
>   ```
>

---

>   以下是使用==邻接矩阵==表示图的基本操作的 C++ 实现代码：
>
>   ```cpp
>   #include <iostream>
>   #include <vector>
>   
>   class Graph {
>   private:
>       std::vector<std::vector<int>> adjacencyMatrix;
>       int numVertices;
>   
>   public:
>       // 构造函数
>       Graph(int n) {
>           numVertices = n;
>           adjacencyMatrix.resize(n, std::vector<int>(n, 0));
>       }
>   
>       // 判断图中是否存在边 (x, y)
>       bool Adjacent(int x, int y) {
>           return adjacencyMatrix[x][y] != 0;
>       }
>   
>       // 列出与顶点 x 相邻接的边
>       std::vector<int> Neighbors(int x) {
>           std::vector<int> neighbors;
>           for (int i = 0; i < numVertices; i++) {
>               if (adjacencyMatrix[x][i] != 0) {
>                   neighbors.push_back(i);
>               }
>           }
>           return neighbors;
>       }
>   
>       // 在图中插入顶点 x
>       void InsertVertex(int x) {
>           numVertices++;
>           adjacencyMatrix.resize(numVertices, std::vector<int>(numVertices, 0));
>           for (int i = 0; i < numVertices; i++) {
>               adjacencyMatrix[i].resize(numVertices, 0);
>           }
>       }
>   
>       // 从图中删除顶点 x
>       void DeleteVertex(int x) {
>           if (x < numVertices) {
>               numVertices--;
>               adjacencyMatrix.erase(adjacencyMatrix.begin() + x);
>               for (int i = 0; i < numVertices; i++) {
>                   adjacencyMatrix[i].erase(adjacencyMatrix[i].begin() + x);
>               }
>           }
>       }
>   
>       // 向图中添加边 (x, y)
>       void AddEdge(int x, int y) {
>           if (x < numVertices && y < numVertices) {
>               adjacencyMatrix[x][y] = 1;
>           }
>       }
>   
>       // 从图中删除边 (x, y)
>       void RemoveEdge(int x, int y) {
>           if (x < numVertices && y < numVertices) {
>               adjacencyMatrix[x][y] = 0;
>           }
>       }
>   
>       // 求顶点 x 的第一个邻接点
>       int FirstNeighbor(int x) {
>           for (int i = 0; i < numVertices; i++) {
>               if (adjacencyMatrix[x][i] != 0) {
>                   return i;
>               }
>           }
>           return -1;
>       }
>   
>       // 求顶点 x 的下一个邻接点
>       int NextNeighbor(int x, int y) {
>           for (int i = y + 1; i < numVertices; i++) {
>               if (adjacencyMatrix[x][i] != 0) {
>                   return i;
>               }
>           }
>           return -1;
>       }
>   
>       // 获取边 (x, y) 的权值
>       int GetEdgeValue(int x, int y) {
>           return adjacencyMatrix[x][y];
>       }
>   
>       // 设置边 (x, y) 的权值为 v
>       void SetEdgeValue(int x, int y, int v) {
>           adjacencyMatrix[x][y] = v;
>       }
>   };
>   
>   int main() {
>       // 创建一个图对象
>       Graph g(5);
>   
>       // 添加边和权值
>       g.AddEdge(0, 1);
>       g.AddEdge(0, 2);
>       g.AddEdge(1, 3);
>       g.AddEdge(2, 4);
>       g.SetEdgeValue(0, 1, 5);
>       g.SetEdgeValue(0, 2, 8);
>       g.SetEdgeValue(1, 3, 3);
>       g.SetEdgeValue(2, 4, 2);
>   
>       // 测试基本操作
>       std::cout << "Adjacent(0, 1): " << g.Adjacent(0, 1) << std::endl;
>       std::cout << "Neighbors(0): ";
>       std::vector<int> neighbors = g.Neighbors(0);
>       for (int n : neighbors) {
>           std::cout << n << " ";
>       }
>       std::cout << std::endl;
>   
>       g.InsertVertex(5);
>       g.DeleteVertex(4);
>   
>       std::cout << "FirstNeighbor(0): " << g.FirstNeighbor(0) << std::endl;
>       std::cout << "NextNeighbor(0, 1): " << g.NextNeighbor(0, 1) << std::endl;
>       std::cout << "GetEdgeValue(0, 1): " << g.GetEdgeValue(0, 1) << std::endl;
>   
>       return 0;
>   }
>   ```
>
>   以上代码定义了一个 `Graph` 类，使用邻接矩阵实现了图的基本操作。您可以根据需要自定义图的大小和具体操作，然后使用该类进行图的操作。在示例中，我们创建了一个图对象，添加了边和权值，并测试了各个基本操作的功能。
>

### 图查找

#### 查找边

使用邻接矩阵只用根据对应行列的元素是否为$1$或某值就可以了，如果是$0$或无穷，就代表没有该邻边。时间复杂度为$O(1)$。

而使用邻接矩阵需要从一端点出发遍历对应的顶点链表，如果能在链表中找到另一端点的索引，就代表有边。时间复杂度为$O(1)$到$O(\vert V\vert)$。

#### 查找点邻边

对于无向图，邻接矩阵需要遍历对应顶点的那一行，所有数值为$1$或某数值的列就是对应的有边的另一个端点。时间复杂度为$O(\vert V\vert)$。

对于有向图，邻接矩阵需要遍历对应顶点的那一行得到出边以及那一列代表入边，所有数值为$1$或某数值的列就是对应的有边的另一个端点。时间复杂度为$O(\vert V\vert)$。

对于无向图，邻接表只用遍历对应顶点的顶点链表就可以。时间复杂度为$O(1)$到$O(\vert V\vert)$。

对于有向图，邻接表用遍历对应顶点的顶点链表得到出边，而对于入边需要遍历所有邻接表的边顶点。出边时间复杂度为$O(1)$到$O(\vert V\vert)$，入边时间复杂度为$O(\vert E\vert)$。

#### 查找头邻接点

邻接矩阵只用扫描对应的行，找到顶点就可以了。时间复杂度为$O(1)$到$O(\vert V\vert)$。

对于无向图，邻接表只用找到顶点的边顶点的第一个顶点。时间复杂度为$O(1)$。

对于有向图，出边邻接表只用找到顶点的边顶点的第一个顶点。时间复杂度为$O(1)$。而对于入边需要遍历所有的顶点的第一个链表顶点。时间复杂度为$O(1)$到$O(\vert E\vert)$。

#### 查找下一个邻接点

邻接矩阵只用扫描对应的行，找到顶点就可以了。时间复杂度为$O(1)$到$O(\vert V\vert)$。

邻接表只用找到当前顶点的下一个顶点。时间复杂度为$O(1)$。

### 图插入

邻接矩阵只用在最后增加一行一列。时间复杂度是$O(1)$。

邻接表只用在存储顶点的数组的末尾添加一个顶点，指针设置为$NULL$。时间复杂度是$O(1)$。

### 图删除

邻接矩阵的删除元素分为两种方式，如果是直接删除对应元素行与列上的所有元素并移动其他元素，那么时间复杂度就是$O(\vert V\vert^2)$，如果删除对应元素行与列上的所有元素但是不移动其他元素，而是将保存顶点数据的数组中对应顶点的数据变为初始值，则时间复杂度就是$O(\vert V\vert)$。

对于无向图，邻接表的删除需要删除该顶点并删除顶点后连接的所有顶点链表元素，时间复杂度为$O(1)$到$O(\vert V\vert)$。

对于有向图，邻接表的删除需要删除该顶点并删除顶点后连接的所有顶点链表元素且还要遍历所有的边并删除，删除出边时间复杂度为$O(1)$到$O(\vert V\vert)$，删除入边时间复杂度为$O(\vert E\vert)$。

### 图遍历

![image-20230720230906345](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202309742.png)

指从图某一顶点出发按照某种搜索方法沿着图中的边==对图中所有顶点访问一次且仅访问一次==。

树的遍历也可以看作一种特殊图的遍历。

分为广度优先$BFS$与深度优先$DFS$。

==广度优先类似二叉树的层序遍历，深度优先类似二叉树的先序遍历。==

广度优先是每一次遍历都要把所有的相邻顶点全部遍历到，而深度优先是每一次遍历只遍历最近的一个一直深入。

==由于邻接矩阵是唯一的，所以其图遍历序列是唯一的==

==而对于邻接表是不唯一的，对于每一个邻接表的各个顶点的顺序是不唯一的，所以序列就是不唯一的==。

#### 广度优先遍历$BFS$

实现条件：

+ 找到一个与顶点相邻的所有顶点。
+ 标记哪些顶点被访问过。
+ 需要一个==辅助队列==保存顶点是否被访问的数据。

广度优先遍历过程：

1. 选择起始点并访问顶点$v$。
2. 访问$v$的所有未被访问的邻接点。
3. 依次从这些邻接点（在`2.`中访问的顶点）出发，访问它们的所有未被访问的邻接点
    +   依此类推，直到图中所有访问过的顶点的邻接点都被访问。
4. 若图中尚未有顶点被访问，则另选一个未曾被访问的顶点作为起始点重复过程。

<video src="C:/Users/willi/OneDrive/%E5%9B%BE%E7%89%87/ShareX/2023-07/23July21-132554-1689917154-13159992-5119-4232-9530-739fda28e0cd.mp4"></video>

性质：

+ ==广度优先类似二叉树的层序遍历==
+ 因为广度优先算法不需要回退，所以不是一个递归算法。
+ 对于无向图，调用$BFS$函数的次数等于连通分量数
+ 对于非带权图，使用$BFS$可以解决非带权图的单源最短路径问题，因为$BFS$按照距离有近到远。
+ 空间复杂度为$O(\vert V\vert)$
+ 邻接矩阵实现时的时间复杂度为$O(\vert V\vert^2)$
    + 访问$|V|$个结点需要$O(|V|)$的时间
    + 查找每个顶点的邻接点都需要$O(|V|)$的时间，而总共有$|V|$个顶点
    + 则总的时间为$O(\vert V\vert^2)+O(\vert V\vert)=O(\vert V\vert^2)$

+ 邻接表实现时的时间复杂度为$O(\vert V\vert+\vert E\vert)$
    + 访问$|V|$个结点需要$O(|V|)$的时间
    + 查找每个顶点的邻接点都需要$O(|E|)$的时间
    + 则总的时间为$O(\vert V\vert+|E|)$


---

>   $BFS$的时间复杂度分析如果利用最深层循环计算会有点问题还很复杂,直接简化问题为访问和查找两步即可

##### 广度优先生成树

+ 广度优先生成树
    + 根据广度优先遍历可以将所有第一次访问顶点时的路径组合生成一个广度优先生成树
    + 若图顶点为$n$个，则生成树边一共有$n-1$
    + 若邻接矩阵存储则唯一，若邻接表存储则不唯一。


![1689603317t-20230717-2217-1524.876-1](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689603317t-20230717-2217-1524.876-1.png)

**代码实现：**

```cpp
#include <iostream>
#include <vector>
#include <queue>

class Graph {
private:
    std::vector<std::vector<int>> adjacencyMatrix;
    int numVertices;

public:
    Graph(int n) {
        numVertices = n;
        adjacencyMatrix.resize(n, std::vector<int>(n, 0));
    }

    void AddEdge(int x, int y) {
        if (x < numVertices && y < numVertices) {
            adjacencyMatrix[x][y] = 1;
            adjacencyMatrix[y][x] = 1; // 无向图需要同时设置两个方向的边
        }
    }

    void BFS(int startVertex) {
        std::vector<bool> visited(numVertices, false); // 记录顶点是否已访问
        std::queue<int> queue; // 使用辅助队列进行广度优先搜索

        visited[startVertex] = true;
        queue.push(startVertex);

        while (!queue.empty()) {
            int currentVertex = queue.front();
            queue.pop();
            std::cout << currentVertex << " ";

            for (int i = 0; i < numVertices; i++) {
                if (adjacencyMatrix[currentVertex][i] == 1 && !visited[i]) {
                    visited[i] = true;
                    queue.push(i);
                }
            }
        }
    }

    void BFSTree(int startVertex) {
        std::vector<bool> visited(numVertices, false);
        std::queue<int> queue;

        visited[startVertex] = true;
        queue.push(startVertex);

        while (!queue.empty()) {
            int currentVertex = queue.front();
            queue.pop();

            for (int i = 0; i < numVertices; i++) {
                if (adjacencyMatrix[currentVertex][i] == 1 && !visited[i]) {
                    visited[i] = true;
                    queue.push(i);
                    std::cout << currentVertex << " - " << i << std::endl;
                }
            }
        }
    }
};

int main() {
    Graph g(6);

    g.AddEdge(0, 1);
    g.AddEdge(0, 2);
    g.AddEdge(1, 3);
    g.AddEdge(1, 4);
    g.AddEdge(2, 4);
    g.AddEdge(3, 5);

    std::cout << "BFS Traversal: ";
    g.BFS(0);
    std::cout << std::endl;

    std::cout << "BFS Tree Edges:" << std::endl;
    g.BFSTree(0);

    return 0;
}
```

+ 广度优先生成森林：若图是不连通的，那会生成连通分量个广度优先生成树，就构成了广度优先生成森林。

#### 深度优先遍历

实现条件：

+ 是一个递归算法，所以需要一个工作==栈==。

深度优先遍历过程：

1. 访问顶点$v$。
2. 依次从$v$的未被访问的邻接点出发，对图进行深度优先遍历。
3. 直至图中和$v$有路径相通的顶点都被访问。
4. 若此时图中尚有顶点未被访问，则从一个未被访问的顶点出发，重新进行深度优先遍历，直到图中所有顶点均被访问过为止。

<video src="C:/Users/willi/OneDrive/%E5%9B%BE%E7%89%87/ShareX/2023-07/23July21-132826-1689917306-0e1f1d3d-fcc8-4ac0-bf40-648799f3b870.mp4"></video>

![1689603921t-20230717-2221-850.420-5](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689603921t-20230717-2221-850.420-5.png)

性质：

+ ==深度优先类似二叉树的先序遍历==
+ 邻接表的深度优先序列会优先选择每个顶点的第一个相邻顶点，即顶点链中的第一个元素。
+ 邻接矩阵方式唯一所以深度优先序列唯一，而邻接表方式不唯一，所以深度优先序列不唯一。
+ 使用$DFS$算法递归地遍历一个无环有向图，并在退出递归时输出相应顶点，这样得到的顶点序列是逆拓扑有序
    + 因为栈的先进后出特性

+ 邻接矩阵实现时的时间复杂度为$O(\vert V\vert^2)$，邻接表实现时的时间复杂度为$O(\vert V\vert+\vert E\vert)$；空间复杂度为$O(\vert V\vert)$。

>   对于同样一个图，基于邻接矩阵存储的遍历所得到的DFS序列和BFS序列是唯一的
>
>   基于邻接表存储的遍历所得到的DFS序列和BFS序列是不唯一的。

##### 深度优先生成树

+ 深度优先生成树
    + 根据深度优先遍历可以将所有第一次访问顶点时的路径组合生成一个深度优先生成树
    + 若图顶点为$n$个，则生成树边一共有$n-1$条
    + 因为保存图的数据结构若是不唯一，则其深度优先生成树也是不唯一的
    + 如果无向图非连通，则一个顶点出发只能一次性遍历到该顶点所在连通分量的所有顶点。


下面是使用深度优先搜索算法生成树的实现代码：

```cpp
#include <iostream>
#include <vector>

class Graph {
private:
    std::vector<std::vector<int>> adjacencyMatrix;
    int numVertices;

public:
    Graph(int n) {
        numVertices = n;
        adjacencyMatrix.resize(n, std::vector<int>(n, 0));
    }

    void AddEdge(int x, int y) {
        if (x < numVertices && y < numVertices) {
            adjacencyMatrix[x][y] = 1;
            adjacencyMatrix[y][x] = 1; // 无向图需要同时设置两个方向的边
        }
    }

    void DFS(int vertex, std::vector<bool>& visited) {
        visited[vertex] = true;
        std::cout << vertex << " ";

        for (int i = 0; i < numVertices; i++) {
            if (adjacencyMatrix[vertex][i] == 1 && !visited[i]) {
                std::cout << vertex << " - " << i << std::endl;
                DFS(i, visited);
            }
        }
    }

    void DFSTree(int startVertex) {
        std::vector<bool> visited(numVertices, false);
        DFS(startVertex, visited);
    }
};

int main() {
    Graph g(6);

    g.AddEdge(0, 1);
    g.AddEdge(0, 2);
    g.AddEdge(1, 3);
    g.AddEdge(1, 4);
    g.AddEdge(2, 4);
    g.AddEdge(3, 5);

    std::cout << "DFS Tree Edges:" << std::endl;
    g.DFSTree(0);

    return 0;
}
```

+ 深度优先生成森林：若图是不连通的，那会生成连通分量个深度优先生成树，就构成了深度优先生成森林。

==图的广度优先生成树的高度小于等于深度优先生成树的高度==。

#### 图遍历与图连通性

+ 若起始顶点到其他各顶点都有路径，那么只需调用一次深度优先或广度优先遍历函数。
+ 对强连通图，从任意一顶点出发都只用调用一次深度优先或广度优先遍历函数。
+ 遍历时函数调用层数等于该图的连通分量数。（因为存在不同的连通分量需要多次调用才能全部访问到）

## 图的应用

![图的应用](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307202311915.png)

### 最小生成树

一个连通图的生成树包含图的所有顶点，并且只含尽可能少的边。对于生成树来说，若砍去它的一条边，则会使生成树变成非连通图；若给它增加一条边，则会形成图中的一条回路。

>   一般而言，这部分内容直接以算法设计题形式考查的可能性很小，而更多的是结合图的实例来考查算法的具体操作过程，应该学会手动模拟给定图的各个算法的执行过程
>
>   此外,还需掌握对给定模型建立相应的图去解决问题的方法。

#### 最小生成树定义

最小生成树$MST(Minimum-Spanning-Tree)$也是最小代价树。已知生成树就是最小边的能到任意顶点的树，这种树只关心边数，所以有多个不同的生成树。

而最小生成树就是带权生成树的最小权值和的情况。

设$\R$为图$G$的所有生成树的集合，若$T$为$\R$中边的权值之和最小的生成树，则$T$称$G$的最小生成树。

不难看出，最小生成树具有如下性质：

+ 最小生成树边的权值总是==唯一且最小==的。
+ ==如果没有权值相同的边，则最小生成树是唯一的。==
    + $MST$唯一性定理：$MST$没有使用无向网中相同权值的边，那么$MST$一定唯一
        + 连通图的任意一个环中所包含的边的权值均不相同
    + 无向图中存在相同权值的边是最小生成树不唯一的必要条件（但不是充分条件）
    + 充要条件
        + ==不存在一条非最小生成树上的边，满足该边的权值与其两端顶点在最小生成树上的路径最小边权相等。==
    
+ 最小生成树的==边数=顶点数$-1$==
    + 减去一条则不连通，增加一条则会出现回路。

+ 若一个连通图本身就是一棵树，则其最小生成树就是其本身。
+ 只用连通图才有生成树，非连通图只有生成森林。

构建最小生成树的算法都利用的性质：假设$G=(V,E)$是一个带权连通无向图，$U$是顶点集$V$的一个非空子集。若$(u,v)$是一条具有最小权值的边，其中$u\in U$，$v\in V-U$，则必存在一棵包含边$(u,v)$的最小生成树。

获取最小生成树的$Prim$算法和$Kruskal$算法都是基于贪心算法的策略。每次都加入一条边逐渐生成一棵生成树：

![image-20230722114541762](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307221145932.png)

其中$Prim$算法和$Kruskal$算法的主要区别就是上述代码中获取最小代价边的策略：

![1689604495t-20230717-2255-840.315-5](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689604495t-20230717-2255-840.315-5.png)

#### Prim算法

非常类似找到图最短路径的迪杰斯特拉算法。~~虽然$Dijkstra$在后面~~

普里姆算法：从某个顶点开始构建生成树，每次将代价最小的新顶点纳入生成树，直到所有顶点都纳入为止。

+ 假设$G=\{V,E\}$是连通图，其最小生成树$T=(U,E_T)$，$E_T$是最小生成树中边的集合。
+ 初始化：向空树$T=(U,E_T)$中添加图$G=(V,E)$的任一顶点$u_0$，使$U=\{u_0\}$，$E_T=\varnothing$。
+ 循环（重复下列操作直至$U=V$）：从图$G$中选择满足$\{(u,v)|u\in U,v\in V-U\}$且具有最小权值的边$(u,v)$，加入树$T$，置$U=U\cup\{v\}$，$E_T=E_T\cup\{(u,v)\}$。

![prim算法](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/prim%E7%AE%97%E6%B3%95.png)

+ 需要遍历$\vert V\vert$个顶点，每次要遍历其他所有顶点。
+ 时间复杂度为$O(\vert V\vert^2)$。
    + 从$V_0$开始,总共需要$n-1$轮处理
    + 每一轮时间复杂度为$O(2n)$
    + 总时间复杂度为$O(n^2)$,即$O(|V|)^2$

+ ==适用于边稠密图。==

**代码实现**

Prim算法是一种用于求解最小生成树的经典算法，它基于**贪心策略**，从一个起始顶点开始逐步选择边，构建最小生成树。下面是使用Prim算法构建最小生成树的实现代码：

```cpp
#include <iostream>
#include <vector>
#include <queue>

#define INF 9999999

class Graph {
private:
    int numVertices;
    std::vector<std::vector<int>> adjacencyMatrix;

public:
    Graph(int n) {
        numVertices = n;
        adjacencyMatrix.resize(n, std::vector<int>(n, INF));
    }

    void AddEdge(int x, int y, int weight) {
        adjacencyMatrix[x][y] = weight;
        adjacencyMatrix[y][x] = weight; // 无向图需要同时设置两个方向的边
    }

    void PrimMST() {
        std::vector<bool> visited(numVertices, false); // 记录顶点是否已访问
        std::vector<int> key(numVertices, INF); // 记录每个顶点到生成树的最小权值
        std::vector<int> parent(numVertices, -1); // 记录生成树中每个顶点的父结点

        // 使用优先队列来选择权值最小的边
        std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>, std::greater<std::pair<int, int>>> pq;

        int startVertex = 0; // 选择起始顶点

        pq.push(std::make_pair(0, startVertex));
        key[startVertex] = 0;

        while (!pq.empty()) {
            int u = pq.top().second;
            pq.pop();
            visited[u] = true;

            for (int v = 0; v < numVertices; v++) {
                if (adjacencyMatrix[u][v] != INF && !visited[v] && adjacencyMatrix[u][v] < key[v]) {
                    parent[v] = u;
                    key[v] = adjacencyMatrix[u][v];
                    pq.push(std::make_pair(key[v], v));
                }
            }
        }

        // 输出最小生成树的边
        std::cout << "Minimum Spanning Tree Edges:" << std::endl;
        for (int i = 1; i < numVertices; i++) {
            std::cout << parent[i] << " - " << i << std::endl;
        }
    }
};
```

#### Kruskal算法

克鲁斯卡尔算法：==每次选择一条权值最小的边，使这条边的两头连通，若本就连通的就不选，直到所有的顶点都连通。==

+ 假设$G=(V,E)$是连通图，其最小生成树$T=(U, E_T)$。
+ 初始化：$U=V,E_T=\varnothing$。即每个顶点构成一棵独立的树，$T$此时是一个仅含$\vert V\vert$个顶点的森林。
+ 循环（重复下列操作直至$T$是一棵树）：按$G$的边的权值递增顺序依次从$E-E_T$中选择一条边，若这条边加入$T$后不构成回路，则将其加入$E_T$，否则舍弃，直到$E_T$中含有$n-1$条边。

![kruskal算法](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/kruskal%E7%AE%97%E6%B3%95.png)

+ 使用堆来存放边（所以可以二分查找），所以每次旋转最小权值的边只需要$O(\log\vert E\vert)$的时间。
+ 时间复杂度为$O(\vert E\vert\log_2\vert E\vert)$。
+ ==适用于边稀疏顶点多的图。==

**代码实现;**

Kruskal算法是一种用于求解最小生成树的经典算法，它基于**贪心策略**，按照边的权值从小到大的顺序逐步选择边，构建最小生成树。下面是使用Kruskal算法构建最小生成树的实现代码：

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

class Edge {
public:
    int src, dest, weight;

    Edge(int s, int d, int w) {
        src = s;
        dest = d;
        weight = w;
    }
};

class Graph {
private:
    int numVertices;
    std::vector<Edge> edges;

public:
    Graph(int n) {
        numVertices = n;
    }

    void AddEdge(int src, int dest, int weight) {
        Edge edge(src, dest, weight);
        edges.push_back(edge);
    }

    // 查找顶点的根结点
    int FindRoot(std::vector<int>& parent, int vertex) {
        if (parent[vertex] == -1)
            return vertex;
        return FindRoot(parent, parent[vertex]);
    }

    // 合并两个不同的集合
    void Union(std::vector<int>& parent, int x, int y) {
        int xroot = FindRoot(parent, x);
        int yroot = FindRoot(parent, y);
        parent[xroot] = yroot;
    }

    void KruskalMST() {
        std::vector<int> parent(numVertices, -1); // 记录顶点的父结点
        std::vector<Edge> result; // 存储最小生成树的边

        // 按照边的权值从小到大进行排序
        std::sort(edges.begin(), edges.end(), [](const Edge& a, const Edge& b) {
            return a.weight < b.weight;
        });

        int edgeCount = 0;
        int i = 0;

        while (edgeCount < numVertices - 1 && i < edges.size()) {
            Edge currentEdge = edges[i++];

            int xroot = FindRoot(parent, currentEdge.src);
            int yroot = FindRoot(parent, currentEdge.dest);

            // 判断边的两个顶点是否在不同的集合中，避免形成环路
            if (xroot != yroot) {
                result.push_back(currentEdge);
                Union(parent, xroot, yroot);
                edgeCount++;
            }
        }

        // 输出最小生成树的边
        std::cout << "Minimum Spanning Tree Edges:" << std::endl;
        for (const Edge& edge : result) {
            std::cout << edge.src << " - " << edge.dest << " (Weight: " << edge.weight << ")" << std::endl;
        }
    }
};
```

### 最短路径

+ 单源最短路径：单个顶点到图的其他顶点的最短路径。
    + $BFS$算法（无权图）。
    + $Dijkstra$迪杰斯特拉算法（==无负权==的带权图、无权图）。
+ 每对顶点间最短路径：每对顶点之间的最短路径。
    + $Floyd$弗洛伊德算法（带权图、无权图）。

![23July19-204218-1689770538-d2cf8145-1828-4aa2-9adc-1dc38450fd66](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307192042499.png)

最短路径一定是简单路径（不存在环）。但是无论有没有环的有向图与是否存在最短路径无关。

#### 单源最短路径-BFS算法

广度优先遍历算法

![1689602682t-20230717-2242-983.899-7](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689602682t-20230717-2242-983.899-7.png)

广度优先算法可以计算无权图的单源最短路径。

实际上无权图可以视为一种特殊的带权图，只是每条边的权值全部为$1$。

广度优先算法基本上就是对广度优先遍历的改进。定义两个数组，索引号就代表元素的序号，一个数组表示从起点开始到该点的最短路径长度，另一个数组表示从起点开始到该点的最短路径的上一个顶点的索引值。

![23July19-194619-1689767179-c5666879-00e8-4018-8957-b5abe10788cd](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307191946825.png)

```cpp
#include <iostream>
#include <vector>
#include <queue>

class Graph {
private:
    std::vector<std::vector<int>> adjacencyList;	//邻接表表示法
    int numVertices;

public:
    Graph(int n) {
        numVertices = n;
        adjacencyList.resize(n); // 初始化邻接表
    }

    void AddEdge(int x, int y) {
        adjacencyList[x].push_back(y); // 添加边到邻接表
        adjacencyList[y].push_back(x); // 无向图需要同时设置两个方向的边
    }

    std::vector<int> BFS(int startVertex, int endVertex) {
        std::vector<bool> visited(numVertices, false); // 记录顶点是否已访问
        std::vector<int> distance(numVertices, -1); // 记录从起始顶点到每个顶点的最短距离
        std::vector<int> parent(numVertices, -1); // 记录每个顶点的父结点

        std::queue<int> queue; // 使用队列进行BFS
        queue.push(startVertex);
        visited[startVertex] = true;
        distance[startVertex] = 0; // 起始顶点到自身的距离为0

        while (!queue.empty()) {
            int currentVertex = queue.front();
            queue.pop();

            for (int neighbor : adjacencyList[currentVertex]) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    queue.push(neighbor);
                    parent[neighbor] = currentVertex; // 设置当前顶点为邻居顶点的父结点
                    distance[neighbor] = distance[currentVertex] + 1; // 更新距离

                    if (neighbor == endVertex) {
                        // 已找到终点，可以提前结束BFS
                        return ShortestPath(parent, startVertex, endVertex);
                    }
                }
            }
        }

        return {}; // 如果没有找到路径，返回空的路径
    }

    std::vector<int> ShortestPath(const std::vector<int>& parent, int startVertex, int endVertex) {
        std::vector<int> path;
        int currentVertex = endVertex;

        while (currentVertex != -1) {
            path.insert(path.begin(), currentVertex); // 将当前顶点插入到路径的起始位置
            currentVertex = parent[currentVertex]; // 移动到父结点，继续回溯路径
        }

        return path; // 返回最短路径
    }
};

int main() {
    Graph g(6);

    g.AddEdge(0, 1);
    g.AddEdge(0, 2);
    g.AddEdge(1, 3);
    g.AddEdge(1, 4);
    g.AddEdge(2, 4);
    g.AddEdge(3, 5);

    int startVertex = 0;
    int endVertex = 5;
    std::vector<int> shortestPath = g.BFS(startVertex, endVertex);

    if (!shortestPath.empty()) {
        std::cout << "Shortest Path from " << startVertex << " to " << endVertex << ": ";
        for (int vertex : shortestPath) {
            std::cout << vertex << " "; // 输出最短路径
        }
        std::cout << std::endl;
    } else {
        std::cout << "No path found from " << startVertex << " to " << endVertex << "." << std::endl;
    }

    return 0;
}

```



#### 单源最短路径-Dijkstra算法

即迪杰斯特拉算法。用于计算单源最短路径（要计算从源到其他所有各顶点的最短路径长度）。

算法思路

1. 从$v_0$开始，初始化三个数组
    +   标记各顶点是否已找到最短路径$final$
    +   源$v_0$到其他所有各顶点的最短路径长度$dist$
        +   初态为
            +   若从$v_0$到$v_i$有弧，则`dist[i]`为弧上的权值
            +   否则置为$\infty$。
    +   源$v_0$到其他所有各顶点最短路径上的前驱$path$
        +   `path[i]`表示从源点到顶点，之间的最短路径的前驱结点
2. 然后将$v_0$直连的点的$dist$值初始化为直连路径长度，对应的$path=0$
    +   但是不要将对应的$final=true$，因为还没有确定对应的直连路径就是最短路径
    +   其他顶点的$dist=\infty$。
3. 遍历所有顶点确定下一个最短路径的顶点
    +   要求
        +   找到还没确定最短路径
            +   即$final=0$
        +   且最短路径长度值最小的的一个顶点
            +   即且$dist$最小
    +   令其各顶点是否已找到最短路径的值$final=true$
4. 检查所有邻接这个顶点的其他顶点，若其点还没有找到最短路径，则更新最短路径长度值与最短路径上前驱的值。
5. 重复步骤二再次循环遍历所有顶点并找到没确定最短路径则最短路径长度最小的顶点。重复次数为$n-1$次。

![dijkstra算法](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/dijkstra%E7%AE%97%E6%B3%95.png)

对应求解过程如下

![image-20230722121938568](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307221219712.png)

**算法分析**

+   $Dijkstra$算法与$Prim$算法类似，都是优先与最短的路径结合(贪心算法)。

+   时间复杂度为$O(\vert V\vert^2)$。
    +   使用邻接矩阵表示时，时间复杂度为$O(\vert V\vert^2)$
    +   使用带权的邻接表表示时，虽然修改`dist[]`的时间可以减少，但由于在`dist[]`中选择最小分量的时间不变，时间复杂度仍为$O(\vert V\vert^2)$

+   ==$Dijkstra$算法不适用于含有负权值的带权图==
    +   处理含负权边的带权图最短路径算法使用$Bellman-Ford$算法等,见最后扩展部分

**代码思路：**

![23July19-200959-1689768599-4d807ff1-5865-4faa-b5f3-9f79f0908d27](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307192010451.png)

**代码实现：**

```cpp
#include <iostream>
#include <vector>
#include <queue>

#define INF 1e9 // 定义无穷大的距离

class Graph {
private:
    int numVertices;
    std::vector<std::vector<std::pair<int, int>>> adjacencyList; // 使用邻接表存储图的边和权值

public:
    Graph(int n) {
        numVertices = n;
        adjacencyList.resize(n);
    }

    void AddEdge(int x, int y, int weight) {
        // 添加边以及对应的权值到邻接表
        adjacencyList[x].push_back(std::make_pair(y, weight));
        adjacencyList[y].push_back(std::make_pair(x, weight)); // 无向图需要同时设置两个方向的边
    }

    std::vector<int> Dijkstra(int startVertex) {
        std::vector<int> distance(numVertices, INF); // 记录从起始顶点到每个顶点的最短距离
        std::vector<bool> visited(numVertices, false); // 记录顶点是否已访问

        distance[startVertex] = 0; // 起始顶点到自身的距离为0

        // 创建优先队列，使用std::greater来实现最小堆
        std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>, std::greater<std::pair<int, int>>> pq;
        pq.push(std::make_pair(0, startVertex));

        while (!pq.empty()) {
            int currentVertex = pq.top().second;
            pq.pop();

            if (visited[currentVertex]) {
                // 如果当前顶点已经访问过，跳过
                continue;
            }

            visited[currentVertex] = true;

            // 遍历当前顶点的邻接顶点
            for (const auto& edge : adjacencyList[currentVertex]) {
                int neighbor = edge.first;
                int weight = edge.second;

                // 如果通过当前顶点到邻接顶点的路径距离更短，更新距离并加入优先队列
                if (distance[currentVertex] + weight < distance[neighbor]) {
                    distance[neighbor] = distance[currentVertex] + weight;
                    pq.push(std::make_pair(distance[neighbor], neighbor));
                }
            }
        }

        return distance; // 返回从起始顶点到每个顶点的最短距离
    }
};

int main() {
    Graph g(6);

    g.AddEdge(0, 1, 7);
    g.AddEdge(0, 2, 9);
    g.AddEdge(0, 5, 14);
    g.AddEdge(1, 2, 10);
    g.AddEdge(1, 3, 15);
    g.AddEdge(2, 3, 11);
    g.AddEdge(2, 5, 2);
    g.AddEdge(3, 4, 6);
    g.AddEdge(4, 5, 9);

    int startVertex = 0;
    std::vector<int> shortestDistances = g.Dijkstra(startVertex);

    // 输出从起始顶点到每个顶点的最短距离
    for (int i = 0; i < shortestDistances.size(); i++) {
        std::cout << "Shortest distance from " << startVertex << " to " << i << " is " << shortestDistances[i] << std::endl;
    }

    return 0;
}
```

#### 多顶点间最短路径-Floyd算法

即弗洛伊德算法

是一种动态规划算法，将问题的求解分为多个阶段。

对于$n$个顶点的图$G$，求任意一对顶点$v_i$到$v_j$之间的最短路径可分为如下阶段：

![23July19-201644-1689769004-2516e8c1-9670-41b7-aee2-06a6820f7bd5](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307192016074.png)

算法需要遍历$n$次，每次遍历都需要查看$n\times n$的矩阵中是否有更优的中转点。

即

+   判断若$A^{(k-1)}[i][j]>A^{(k-1)}[i][k]+A^{(k-1)}[k][j]$是否成立
    +   若成立则
        1.   $A^{(k)}[i][j]=A^{(k-1)}[i][k]+A^{(k-1)}[k][j]$
             +   其中二维数组$A^{(k)}$表示的是允许$v_0\cdots v_k$个点中转后各顶点的最短路径长度
             +   本式表示在添加中转结点$k$后结点$i$到结点$j$的距离($A[i][j]$)是否在路过结点$k$后变短，变短确认通过中间顶点$k$缩短了起始顶点$i$到目标顶点$j$的距离后更新最短路径$A^{(k)}[i][j]$
        2.   $path^{(k)}[i][j]=k$
             +   $path^{(k)}$表示允许$v_0\cdots v_k$个点中转后两个点之间的中转点
             +   本式表示确认通过中间顶点$k$缩短了起始顶点$i$到目标顶点$j$的距离在$path$处做标记，表示通过结点$k$可以缩短路径
    +   否则$A^{(k)}$和$path^{(k)}$保持原样

[具体算法演示]:

$-1$代表是初始状态。

![23July19-202439-1689769479-ad2dec7b-8546-4cc4-8f2c-2ce1ecbc45ff](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307192024297.png)

---

$0$代表用$0$进行中转，以$0$行和$0$列的数据都不用变，因为$0$行的数据表示$v_0$到$v_1$、$v_2$的距离，$0$列的数据表示从$v_1$、$v_2$到$v_0$的距离，这些不会以$0$进行中转。可以优化的只有$12$和$21$两个位置。同理对角线的元素都不用优化，固定为$0$因为是简单路径。$12$位置从$v_0$中转是这个点对应行列的$v_1v_0+v_0v_2=10+13=23>4$，所以不用优化。$21$位置原来为正无穷，所以可以优化为$v_2v_0+v_0v_1=11$。

![23July19-203314-1689769994-ec3f5df0-e92d-4af8-a6c4-5696750a605a](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307192033363.png)

---

$1$代表用$1$进行中转，以$1$行和$1$列的数据都不用变，$02$位置若以$v_1$中转则$v_0v_1+v_1v_2=6+4=10<13$，所以优化为$10$。$20$位置若以$v_1$中转则$v_2v_0+v_0v_1=5+6=11>5$，不用优化。

![23July19-203335-1689770015-94010b16-ce7f-4960-a873-47065a8b7614](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307192033831.png)

---

$2$可以优化的位置为$10$、$01$，$v_1v_2+v_2v_0=4+5=9>10$，所以优化为$10$，$v_0v_2+v_2v_1=10+11=21>6$，不用优化。

![23July19-203356-1689770036-d7783ec6-0295-4c87-ac86-66bf6982b472](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307192034192.png)

得到$A^{(n-1)})$和$path^{(n-1)})$即可,算法结束

[算法分析]:

+   从$A^{(-1)}$和$path^{(-1)}$开始，经过$n$轮递推，得到$A^{(n-1)})$和$path^{(n-1)})$
    +   此时$A^{(n-1)})$就是各点之间的最短路径长度。
    +   $path^{(n-1)})$保存完整路径信息

+   时间复杂度为$O(\vert V\vert^3)$
    +   不过由于其代码很紧凑，且并不包含其他复杂的数据结构，因此隐含的常数系数是很小的
    +   即使对于中等规模的输入来说，它仍然是相当有效的。
+   空间复杂度为$O(\vert V\vert^2)$。
+   也可以用单源最短路径算法来解决每对顶点之间的最短路径问题
    +   轮流将每个顶点作为源点，并且在所有边权值均非负时，运行一次$Dijkstra$算法
    +   其时间复杂度为$O(\vert V\vert^2)\cdot\vert V\vert= O(\vert V\vert^3)$

+   $Floyd$算法复杂度高，所以基本上都是四个顶点以下的图，==能解决带负权值的问题，但是不能解决带有负权回路的图，即有负权值的边组成回路，这种图可能没有最短路径。==

```cpp
#include <iostream>
#include <vector>

#define INF 9999999 // 定义无穷大的距离

class Graph {
private:
    int numVertices;
    std::vector<std::vector<int>> distance;

public:
    Graph(int n) {
        numVertices = n;
        distance.resize(n, std::vector<int>(n, INF)); // 初始化距离矩阵为无穷大
    }

    void AddEdge(int x, int y, int weight) {
        // 添加边以及对应的权值到距离矩阵
        distance[x][y] = weight;
    }

    void FloydWarshall() {
        // 三重循环，逐步更新距离矩阵
        for (int k = 0; k < numVertices; k++) {				//考虑以Vk作为中转点
            for (int i = 0; i < numVertices; i++) {			//更新最短路径长度
                for (int j = 0; j < numVertices; j++) {		//中转点
                    // 如果通过中间顶点k缩短了起始顶点i到目标顶点j的距离，更新距离矩阵
                    if (distance[i][k] + distance[k][j] < distance[i][j]) {
                        distance[i][j] = distance[i][k] + distance[k][j];
                    }
                }
            }
        }
    }

    void PrintShortestPaths() {
        // 输出所有顶点对之间的最短路径
        for (int i = 0; i < numVertices; i++) {
            for (int j = 0; j < numVertices; j++) {
                if (distance[i][j] == INF) {
                    std::cout << "Shortest path from " << i << " to " << j << " is not reachable." << std::endl;
                } else {
                    std::cout << "Shortest distance from " << i << " to " << j << " is " << distance[i][j] << std::endl;
                }
            }
        }
    }
};

```

### 有向无环图

若一个有向图中不存在环，则是有向无环图，简称$DAG$图。

![23July19-204341-1689770621-c554e6d1-2254-4c85-8716-c71f0748ad9e](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307192043121.png)

#### 表达式应用

有向无环图是描述含有公共子式的表达式的有效工具，用树表示表达式，将操作数共同的顶点部分删除并将边合并到一起，这就形成了图，从而能精简表达式。

顶点中不可能出现重复的操作数。表达式树不唯一。

+   精简表达式

    +   表达式：$((a+b)*(b*(c+d))+(c+d)*e)*((c+d)*e)$
    +   首先从第六层层有$c+d$与第五层有$c+d$重合，将第六层的$c+d$删掉，将第五层的乘号指向右边的加号上。第五层都有一个$b$，所以将加号和乘号都指向同一个$b$。然后往上面看还有相同的$(c+d)*e$，将其中一个删掉，连接到一起。
    +   ![表达式](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/%E8%A1%A8%E8%BE%BE%E5%BC%8F.png)

+   做题思路

    1. 把各个单个的操作数不重复的排成一排。

        ![23July19-205229-1689771149-954ed4b9-e277-4f00-b2ca-5fa3894fa04e](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307192052456.png)

    2. 标出各个运算符的生效顺序。

        ![23July19-205239-1689771159-c63d8a7e-8938-4929-ac8c-6492e5b900c6](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307192052038.png)

    3. 按顺序加入运算符，并注意对运算符的优先级进行分层。

        ![23July19-205255-1689771175-22436cc4-9875-4b1b-81cc-de1c9245eb79](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307192053216.png)

    4. 当构建完成后从底向上逐层检查同层的运算符是否可以合并

        ![23July19-205315-1689771195-532ec75e-e9b2-4042-bce1-4564eed0c5a6](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307192053791.png)


>   最终结果中==不可能出现相同的操作数==

#### (逆)拓扑排序

##### 拓扑排序

$AOV$网：用$DAG$图表示一个工程，顶点表示活动，有向边$<v_i,v_j>$表示活动$v_i$必须先于活动$v_j$进行。

拓扑排序：在图论中，由一个有向无环图的顶点组成的序列，当且仅当满足下列条件时，称为该图的一个拓扑排序：

1. 每个顶点出现且只出现一次。
2. 若顶点$A$在序列中排在顶点$B$的前面，则在图中不存在从顶点$B$到顶点$A$的路径。

或定义为：拓扑排序是对有向无环图的顶点的一种排序，它使得若存在一条从顶点$A$到顶点$B$的路径，则在排序中顶点$B$出现在顶点$A$的后面。每个$AOV$网都有一个或多个拓扑排序序列。

==拓扑排序简单来说就是找到工程执行的先后顺序。==

[拓扑排序的实现]:

1. 从$AOV$网中选择一个没有前驱的==入度为$0$的顶点==并输出。
    +   即完成一件不需要前置的活动
2. 从网中删除该顶点和所有以它为起点的有向边。
    +   即完成该活动后，所有以该活动为前置的后置活动都可以正常进行了
3. 重复步骤一和二直到当前的$AOV$网为空或当前网中不存在无前驱的顶点（存在环路所以不能拓扑排序）为止。
    +   即一件事情一件事情的做完

![23July19-205952-1689771592-88250cb2-a438-4649-ba28-eb63d6951f71](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307192100102.png)

+   其中，
    +   数组`indegree`用于存储当前所有结点的入度
    +   数组`print`用于存储当前的拓扑排序序列
        +   初始化全部为$0$
    +   栈`S`用于保存度为$0$的结点
        +   也可以用队列实现
        +   若两个结点之间不存在祖先或子孙关系，则它们在拓扑序列中的关系是任意的（即前后关系任意)，因此使用栈和队列都可以保存结点，因为进栈或队列的都是入度为$0$的结点，此时入度为$0$的所有结点是没有关系的。
+   时间复杂度若使用邻接表为$O(\vert V\vert+\vert E\vert)$，若使用邻接矩阵则是$O(\vert V\vert^2))$。

##### 逆拓扑排序

[逆拓扑排序的实现]:

1. 从$AOV$网中选择一个没有后继的==出度为$0$的顶点==并输出。
2. 从网中删除该顶点和所有以它为起点的有向边。
3. 重复步骤一和二直到当前的$AOV$网为空或当前网中不存在无前驱的顶点为止。

时间复杂度若使用邻接表为$O(\vert V\vert+\vert E\vert)$，若使用邻接矩阵则是$O(\vert V\vert^2))$。

[代码实现]:

+   除了使用与拓扑排序类似的算法外也可以使用$DFS$算法实现
    +   拓扑排序也可以使用$DFS$
+   使用$DFS$算法实现的逆拓扑排序如下

![23July19-211126-1689772286-59d549fa-e73e-4256-b36b-517712551506](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307192111987.png) 

+   其中可使用数组判断是否存在环路

    ```cpp
        bool HasCycle() {
            std::vector<bool> visited(numVertices, false);
            std::vector<bool> inRecursionStack(numVertices, false); // 用于记录顶点是否在递归栈中
            std::stack<int> dfsStack;
    
            // 从每个未访问的顶点开始进行DFS
            for (int i = 0; i < numVertices; i++) {
                if (!visited[i]) {
                    if (DFSHasCycle(i, visited, inRecursionStack, dfsStack)) {
                        // 如果找到环路，返回true
                        return true;
                    }
                }
            }
    
            // 如果没有找到环路，返回false
            return false;
        }
    ```
    
+   使用$DFS$的递归栈（也称为回溯栈）来跟踪当前访问路径中的顶点。在$DFS$的过程中，如果我们遇到了一个已经在递归栈中的顶点，那么说明我们在当前路径上找到了一个环路。

##### 图和拓扑序列的关系

+ 如果有向图顶点不能排成一个拓扑序列，则有向图含有顶点大于$1$的强连通分量（即存在非自身环路）
+ 有向无环图的唯一拓扑序列不能唯一确定该图。
+ 对有向图中的顶点适当地编号，使其邻接矩阵为三角矩阵且主对角元全为零的充分必要条件是，该有向图可以进行拓扑排序

#### 关键路径

在带权有向图中，以顶点表示时间，以有向边表示活动，以边上的权值表示完成该活动的开销，称之为用边表示活动的网络，简称$AOE$网。

![23July19-212128-1689772888-e6b49ad3-d35a-45db-a76b-438d2628d648](C:/Users/willi/OneDrive/%E5%9B%BE%E7%89%87/ShareX/2023-07/23July19-212128-1689772888-e6b49ad3-d35a-45db-a76b-438d2628d648.png)

+ 只有在==某顶点所代表的事件==发生后，从该顶点出发的各有向边所代表的活动才能开始。
+ 只有在进入某顶点的==各有向边所代表的活动==都已经结束时，该顶点所代表的事件才能发生
    + ==活动可以并行进行==
+ ==源点：只有一个入度为$0$的顶点，即开始顶点，表示整个工程的开始。==
    + $v_1$不一定是源点

+ ==汇点：只有一个出度为$0$的顶点，即结束顶点，表示整个工程的结束。==
    + $v_n$不一定是汇点

+ 从源点到汇点的有向路径可能有多条，所有路径中，==具有最大路径长度的路径称为关键路径（即决定完成整个工程所需的最小时间）==，而关键路径上的活动称为关键活动。

[对于事件]:

+ 事件$v_k$的==最早发生时间$ve(k)$==：指从源点$v_1$到顶点$v_k$处的最长路径长度

    + $v_k$的最早发生时间决定了所有从$v_k$开始的活动能够开工的最早时间

    + 与以该事件为始的弧的活动的最早开始时间相同

    + 计算公式

        + 当$v_k$是源点时

        $$
        ve(v_k)=0
        $$

        + 当$v_k$是结点$v_j$任意后继时
            $$
            ve(k) = Max\{ve(j) + Weight(v_j,v_k)\}
            $$
            ​	其中$Weight(v_j,v_k)$是弧$<v_j,v_k>$上的权值

+ 事件$v_k$的==最迟发生时间$vl(k)$==：在不推迟整个工程完成的前提下，该事件最迟必须发生的时间

    + 一个事件的最迟发生时间等于$\min${以该事件为尾的弧的活动的最迟开始时间，最迟结束时间与该活动的持续时间的差}。

    + 计算公式

        + 当$v_k$是汇点时

        $$
        vl(v_k)=0
        $$

        + 当$v_k$是结点$v_j$任意前驱时
            $$
            vl(k) = Min \{vl(j) - Weight(v_k,v_j)\}
            $$
            ​	其中$Weight(v_k,v_j)$是弧$<v_k,v_j>$上的权值

[对于活动]:

+ 活动$a_i$的==最早开始时间$e(i)$==：指该活动弧的起点所表示的事件最早发生时间。
    + 若弧$<v_k,v_j>$表示活动$a_i$,则有$e(i) = ve(k)$
+ 活动$a_i$的==最迟开始时间$l(i)$==：指该活动弧的终点所表示的事件的最迟发生时间与该活动所需时间之差。
    + 若弧$<v_k,v_j>$表示活动$a_i$,则有$l(i) = vl(k)-Weight(v_k,v_j)$
+ 活动$a_i$的==时间余量$d(i)=l(i)-e(i)$==：在不增加完成整个工程所需总时间的情况下，活动可以拖延的时间。
    + 若一个活动的时间余量为零，则说明该活动必须要如期完成
    + ==$d(i)=0$即$l(i)=e(i)$的活动$a_i$是关键活动==
    + ==由关键活动组成的路径就是关键路径==


求关键路径的步骤：

1. 求所有**事件**的最早发生时间$ve$

    +   根据拓扑排序序列，从源点出发，令$ve($源点$)= 0$，依次按照所有路径的==最大值==求出各个顶点的最早发生时间。

        ![23July19-213640-1689773800-0f890294-e02b-467c-a47f-11f1527df79e](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307192136548.png)

2. 求所有**事件**的最迟发生时间$vl$

    +   根据逆拓扑排序序列，从汇点出发，令$ve($汇点$)= 0$，回退依次将每个顶点按第一步计算的整个工程的时间减去本顶点需要处理的时间，得到每个活动的最迟发生时间$vl$，==交叉的顶点取最小值==。

    ![23July19-213835-1689773915-2a7153e5-10e7-4855-b6e5-479f0b5c5739](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307192139101.png)

3. 求所有**活动**的最早发生时间$e$

    +   根根据各顶点的$ve()$值求所有弧的最早开始时间$e()$
    +   ==弧的最早开始时间$e()$等于起点顶点的$ve()$值==

    ![23July19-213945-1689773985-8000a579-a951-4d23-8920-c7d8745a4dd8](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307192139311.png)

4. 求所有**活动**的最迟发生时间$l$

    +   根根据各顶点的$vl()$值求所有弧的最迟发生时间$l$
    +   ==最迟发生时间$l$等于终点顶点的$vl()$值减去弧的权值==

    ![23July19-214405-1689774245-cdc90eab-a561-49e5-814c-e21f1aa1d2a9](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307192144932.png)

5. 求所有**活动**的时间余量$d$。将$l-e$得到余量$d$

    +   ==此时一定有$l>e$==

    ![image-20230719214512037](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307192145204.png)

6.   求关键活动和关键路径。余量为$0$的活动就是关键活动，表示如果该活动拖延就会影响整个工程的进度

![23July19-214535-1689774335-994913f5-81be-404f-a777-7e34614418c6](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307192145481.png)

求关键路径的步骤

+ 关键活动时间增加，整个工程工期延长。
+ 关键活动时间减少，整个工程工期缩短。
    + 关键活动时间减少，可能变为非关键活动。
    + 所以不是关键活动时间越少，整个工程工期越短

+ ==若有多条关键路径，则应加快包含所有关键路径上的关键活动才能缩短工期。==
+ 关键路径是从源点到汇点路径长度量长的路径。

## 附录/拓展

### 环判定

#### 无向图回路的判断

1. 在图的邻接表表示中，首先统计每个顶点的度，然后重复寻找一个度为$1$的顶点，将度为$1$和$0$的顶点从图中删除，并将与该顶点相关联的顶点的度减$1$，然后继续反复寻找度为$1$的。如果最后存在点没有被删除，即在寻找过程中若出现若干顶点的度都为$2$，则这些顶点组成了一个回路；否则，图中不存在回路。
2. 利用深度优先搜索$DFS$，在搜索过程中判断是否会出现后向边（$DFS$中，连接顶点$u$到它的某一祖先顶点$v$的边），即在$DFS$对顶点进行着色过程中，若出现所指向的顶点已经着色，则此顶点是一个已经遍历过的顶点（祖先），出现了后向边，则图中有回路。
3. 利用$BFS$，在遍历过程中，为每个结点标记一个深度$deep$，如果存在某个结点为$v$，除了其父结点$u$外，还存在与$v$相邻的结点$w$使得$deep[v]<=deep[w]$（可以通过相邻点上升返祖）的，那么该图一定存在回路。
4. 用$BFS$或$DFS$遍历，最后判断对于每一个连通分量当中，如果边数$m>=$结点个数$n$成立，那么改图一定存在回路。因此在$DFS$或$BFS$中，我们可以统计每一个连通分量的顶点数目$n$和边数$m$两个直值，如果$m>=n$则返回假，直到访问完所有的结点才返回真。

判断无向图是否存在环路可以使用深度优先搜索（$DFS$）或广度优先搜索（$BFS$）来实现。其中，$DFS$是较为常用的方法，它通过遍历图的所有顶点，并检查是否存在后向边（$Back\; Edge$）。如果存在后向边，那么图中就包含环路。

**代码实现：**

使用$DFS$判断无向图是否存在环路

```cpp
#include <iostream>
#include <vector>

class Graph {
private:
    int numVertices;
    std::vector<std::vector<int>> adjacencyList;

public:
    Graph(int n) {
        numVertices = n;
        adjacencyList.resize(n);
    }

    void AddEdge(int x, int y) {
        adjacencyList[x].push_back(y); // 添加边到邻接表
        adjacencyList[y].push_back(x); // 无向图需要同时设置两个方向的边
    }

    bool HasCycle() {
        std::vector<bool> visited(numVertices, false); // 记录顶点是否已访问
        for (int i = 0; i < numVertices; i++) {
            if (!visited[i]) {
                // 对每个未访问的顶点进行DFS
                if (DFSHasCycle(i, -1, visited)) {
                    return true; // 如果找到环路，返回true
                }
            }
        }
        return false; // 如果没有找到环路，返回false
    }

    bool DFSHasCycle(int vertex, int parent, std::vector<bool>& visited) {
        visited[vertex] = true; // 将当前顶点标记为已访问

        // 对当前顶点的邻接顶点进行DFS
        for (int neighbor : adjacencyList[vertex]) {
            if (!visited[neighbor]) {
                // 邻接顶点未访问，递归进行DFS
                if (DFSHasCycle(neighbor, vertex, visited)) {
                    return true; // 如果找到环路，返回true
                }
            } else if (neighbor != parent) {
                // 邻接顶点已访问，且不是当前顶点的父节点，说明存在后向边，即存在环路
                return true;
            }
        }

        return false; // 如果当前顶点及其邻接顶点没有形成环路，返回false
    }
};
```

#### 有向图回路的判断

1. 与无向图类似，若在$DFS$中出现后向边，即存在某一顶点被第二次访问到，则有回路出现。
2. 同样，利用拓扑排序的思想，通过这一步骤来执行拓扑排序，即重复寻找一个入度为$0$的顶点，将该顶点从图中删除，并将该顶点及其所有的出边从图中删除（即与该点相应的顶点的入度减$1$），最终若途中全为入度为$1$的点，则这些点至少组成一个回路。

**代码实现：**

使用拓扑排序判断有向图是否存在环路

```cpp
#include <iostream>
#include <vector>
#include <queue>

class Graph {
private:
    int numVertices;
    std::vector<std::vector<int>> adjacencyList;
    std::vector<int> indegree; // 记录每个顶点的入度

public:
    Graph(int n) {
        numVertices = n;
        adjacencyList.resize(n);
        indegree.resize(n, 0);
    }

    void AddEdge(int x, int y) {
        adjacencyList[x].push_back(y);
        indegree[y]++; // 更新目标顶点的入度
    }

    bool HasCycle() {
        std::queue<int> q;
        int visitedVertices = 0; // 记录访问过的顶点数

        // 将入度为0的顶点加入队列作为初始顶点
        for (int i = 0; i < numVertices; i++) {
            if (indegree[i] == 0) {
                q.push(i);
            }
        }

        while (!q.empty()) {
            int currentVertex = q.front();
            q.pop();
            visitedVertices++;

            // 遍历当前顶点的邻接顶点
            for (int neighbor : adjacencyList[currentVertex]) {
                indegree[neighbor]--; // 邻接顶点的入度减1

                // 如果邻接顶点的入度变为0，加入队列作为下一个访问顶点
                if (indegree[neighbor] == 0) {
                    q.push(neighbor);
                }
            }
        }

        // 如果访问过的顶点数小于总顶点数，说明存在环路
        return visitedVertices != numVertices;
    }
};

int main() {
    Graph g(6);

    g.AddEdge(0, 1);
    g.AddEdge(0, 2);
    g.AddEdge(1, 2);
    g.AddEdge(2, 3);
    g.AddEdge(3, 4);
    g.AddEdge(4, 2); // 创建一个含有环路的有向图

    if (g.HasCycle()) {
        std::cout << "Graph contains a cycle." << std::endl;
    } else {
        std::cout << "Graph does not contain a cycle." << std::endl;
    }

    return 0;
}
```

### $Bellman-Ford$算法

适用于负权边的带权图最短路径算法

$Bellman-Ford$算法基于动态规划的思想，通过多次迭代来逐步逼近最短路径的值。算法的主要思想是对图中的所有边进行V-1次松弛操作（V为图中顶点数），这样可以保证所有的最短路径都会被正确计算出来，即使存在负权边。如果在V-1次松弛操作后，仍然存在可以松弛的边，那么这意味着图中存在负权环，无法计算出最短路径。

下面是使用Bellman-Ford算法计算最短路径的C++实现，逐行注释解释每个步骤的作用：

```cpp
#include <iostream>
#include <vector>

#define INF 9999999 // 定义无穷大的距离

class Edge {
public:
    int src, dest, weight;

    Edge(int s, int d, int w) {
        src = s;
        dest = d;
        weight = w;
    }
};

class Graph {
private:
    int numVertices;
    std::vector<Edge> edges;

public:
    Graph(int n) {
        numVertices = n;
    }

    void AddEdge(int src, int dest, int weight) {
        Edge edge(src, dest, weight);
        edges.push_back(edge);
    }

    std::vector<int> BellmanFord(int startVertex) {
        std::vector<int> distance(numVertices, INF); // 记录从起始顶点到每个顶点的最短距离
        distance[startVertex] = 0; // 起始顶点到自身的距离为0

        // 进行V-1次松弛操作
        for (int i = 0; i < numVertices - 1; i++) {
            for (const Edge& edge : edges) {
                int u = edge.src;
                int v = edge.dest;
                int w = edge.weight;

                if (distance[u] != INF && distance[u] + w < distance[v]) {
                    distance[v] = distance[u] + w; // 更新最短距离
                }
            }
        }

        // 检查是否存在负权环
        for (const Edge& edge : edges) {
            int u = edge.src;
            int v = edge.dest;
            int w = edge.weight;

            if (distance[u] != INF && distance[u] + w < distance[v]) {
                // 存在负权环，无法计算最短路径
                std::cout << "Graph contains a negative-weight cycle!" << std::endl;
                return {};
            }
        }

        return distance; // 返回从起始顶点到每个顶点的最短距离
    }
};

int main() {
    Graph g(5);

    g.AddEdge(0, 1, -1);
    g.AddEdge(0, 2, 4);
    g.AddEdge(1, 2, 3);
    g.AddEdge(1, 3, 2);
    g.AddEdge(1, 4, 2);
    g.AddEdge(3, 2, 5);
    g.AddEdge(3, 1, 1);
    g.AddEdge(4, 3, -3);

    int startVertex = 0;
    std::vector<int> shortestDistances = g.BellmanFord(startVertex);

    // 输出从起始顶点到每个顶点的最短距离
    for (int i = 0; i < shortestDistances.size(); i++) {
        std::cout << "Shortest distance from " << startVertex << " to " << i << " is " << shortestDistances[i] << std::endl;
    }

    return 0;
}
```

