# 第五章 树与二叉树

## 导读

### 【考纲内容】

1. 树的基本概念
2. 二叉树
    + 二叉树的定义及其主要特征
    + 二叉树的顺序存储结构和链式存储结构：
    + 二叉树的遍历
    + 线索二叉树的基本概念和构造
3. 树、森林
    + 树的存储结构
    + 森林与二叉树的转换
    + 树和森林的遍历
4. 树与二叉树的应用
    + 哈夫曼($Huffman$)树和哈夫曼编码
    + 并查集及其应用

### 【知识导图】

![23July19-102159-1689733319-48395b29-0c50-496e-aee8-2b887e16035c](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307191022190.png)

### 【复习提示】

+ 本章内容多以选择题的形式考查，但也会出涉及树遍历相关的算法题
+ 树和二叉树的性质、遍历操作、转换、存储结构和操作特性等，满二叉树、完全二叉树、线索二叉树、哈夫曼树的定义和性质，都是选择题必然会涉及的内容。

## 基本概念

### 树的基本概念

![23July19-103508-1689734108-6a3809c6-97f3-41be-b3d5-20244f205540](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307191035870.png)

+ 树：$n$个结点的有限集（树是一种递归的数据结构，适合于表示具有层次的数据结构）
    + 递归定义
    + 在树的定义中又用到了其自身，树是一种递归的数据结构
    + 树作为一种逻辑结构，同时也是一种分层结构，具有以下两个特点：
        1.   树的==根结点没有前驱，除根结点外的所有结点有且只有一个前驱==。
        2.   树中所有结点都可以有==零个或多个后继==。 
+ 根结点：只有子结点没有父结点的结点
    + 除了根结点外，树任何结点都有且仅有一个前驱。

+ 分支结点：有子结点也有父结点的结点。
+ 叶子结点：没有子结点只有父结点的结点。
    + 叶子结点度为$0$
    + 例如上图中的$K\; L\; M$
+ 祖先：根结点到结点的路径上的任意结点都是该结点的祖先。
    + 如结点$B$是结点$K$的祖先

+ 子孙：子树上的所有结点
    + 而结点$K$是结点$B$的子孙

+ 双亲：靠近根结点且最靠近该结点的结点。
+ 兄弟：有共同双亲结点的结点。
+ 堂兄弟：双亲结点在同一层的结点。
+ 空树：结点数$n=0$的数。
+ 子树：当$n>1$时，其余结点可分为$m$个互不相交的有限集合，每个集合本身又是一棵树，其就是根结点的子树。
+ ==结点的度：一个结点的孩子（分支）个数。==
+ ==树的度：树中结点的最大度数。==
+ 结点的层次（深度）：从上往下数。
    + 默认从$1$开始
    + 根结点为第1层，它的子结点为第2层，以此类推
+ 结点的高度：从下往上数。
+ 树的高度（深度）：多少层。
    + 结点的深度是从根结点开始自顶向下逐层累加的
    + 结点的高度是从叶结点开始自底向上逐层累加的
    + 树的高度（或深度）是树中结点的最大层数
        + 图5.1中树的高度为4。

+ 两结点之间的路径：由两个结点之间所经过的结点序列构成。
+ 两结点之间的路径长度：路径上所经过的边的个数。
+ ==树的路径长度：指树根到每个结点的路径长的总和==，根到每个结点的路径长度的最大值是树的高。
+ 有序树：树各结点的子树从左至右有次序，不能互换。
    + 假设图`5.1`为有序树，若将子结点位置互换，则变成一棵不同的树。

+ 无序树：树各结点的子树从左至右无次序，可以互换。

+ 森林：$m(m\ge 0)$棵互不相交的树的集合。
    + 森林的概念与树的概念十分相近，因为只要把树的根结点删去就成了森林
    + 反之，只要给m棵独立的树加上一个结点，并把这$m$棵树作为该结点的子树，则森林就变成了树。


>    上述概念无须刻意记忆，根据实例理解即可。统考时不大可能直接考查概念，而都是结合具体的题目考查。做题时，遇到不熟悉的概念可以翻书，练习得多自然就记住了

### 树的性质

![1689338040t-20230714-2000-1210.718](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689338040t-20230714-2000-1210.718.png)

+ 结点数$=$总度数$+1$

    + 每个节点都会为树的总度数贡献一个度数，并且根节点没有父节点，所以需要额外加$ 1$
    + 总度数=总边数=总孩子数

+ 同上，包含$n$棵树的森林，结点数$=$总度数/总边数/总孩子数$+n$

+ ==度为$m$的树至少有$h+m-1$个结点。==

+ 度为$m$的树以及$m$叉树的第$i(i\ge 1)$层至多有$m^{i-1}$个结点

    + 如完全二叉树

+ 树的度$m$代表至少一个结点度是为$m$，且一定是非空树，至少有$m+1$个结点；而==$m$叉树指所有结点的度都小于等于$m$，可以是空树。==

+ ==高度为$h$的$m$叉树至少有$h$个结点==

+ 高度为$h$的$m$叉树至多有$\dfrac{m^h-1}{m-1}$个结点。

    ![1689337606t-20230714-2046-702.266](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689337606t-20230714-2046-702.266.png)

    +   等比数列求和公式:

    $$
    a+a \mathrm{q}+a q^{2}+\cdots+a q^{n-1}=\frac{a(1-q n)}{1-q}
    $$

+ 具有$n$个结点的$m$叉树最小高度为$\lceil\log_m(n(m-1)+1)\rceil$

    + 若使高度最小应使所有结点都有$m$个孩子
    + 所以$\dfrac{m^{h-1}-1}{m-1}<n\leqslant\dfrac{m^h-1}{m-1}$
        + $\dfrac{m^{h-1}-1}{m-1}$表示前$h-1$层最多有几个结点
        + $\dfrac{m^h-1}{m-1}$表示前$h$层最多有几个结点

    + 从而得到$h-1<\log_m(n(m-1)+1)\leqslant h$
    + 即高度最小取值$h_{min}=\lceil\log_m(n(m-1)+1)\rceil$
        + ==$\lceil\cdots\rceil$是向上取整符号==

## 二叉树

### 二叉树的基本概念

#### 二又树的定义

![23July19-111155-1689736315-6e92459f-b12f-4a55-a3eb-aef23eb5a3d4](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307191111371.png)

+ 二叉树是$n$个结点构成**每个结点至多只有两棵子树**的有限集合。
    + 即二叉树中不存在度大于$2$的结点

    + 二叉树与度为$2$的有序树的区别： 
        1.   度为$2$的树至少有$3$个结点，而二叉树可以为空。
        2.   度为$2$的有序树的孩子的左右次序是相对于另一孩子而言的，若某个结点只有一个孩子,则这个孩子就无须区分其左右次序，而二叉树无论其孩子数是否为$2$,均需确定其左右次序，即==二叉树的结点次序不是相对于另一结点而言的，而是确定的。==

+ 二叉树可以为空二叉树，也可以是由一个根结点和两个互不相交的被称为根的左子树和右子树构成
    + 左子树和右子树又分别是一棵二叉树，左右子树不能颠倒。


##### 特殊的二叉树

+ 满二叉树：一棵高度为$h$，含有$2^h-1$个结点的二叉树

    + 即树中的每层都含有最多的结点
    + 满二叉树的叶结点都集中在二叉树的最下一层，并且除叶结点之外的每个结点度数均为$2$。 
    + 只有最后一层有叶子结点，不存在度为$$1$$的结点
    + 按层序从$1$开始编号，存在相应结点的情况下，结点$i$的左孩子为$2i$，右孩子为$2i+1$，父结点为$\lfloor\dfrac{i}{2}\rfloor$。

    ![1689338185t-20230714-2025-464.261](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689338185t-20230714-2025-464.261.png)

+ 完全二叉树：一棵高度为$h$，含有$n$个结点，当且仅当其每个结点都与高度$h$满二叉树编号$1$到$n$的结点一一对应时该二叉树就是完全二叉树

    + 只有最后两层有叶子结点
        + 对于最大层次中的叶结点，都依次排列在该层最左边的位置上

    + 最多只有一个度为$1$的结点，即$n_1=0/1$，==且该结点只有左孩子而无右孩子==
    + $i\leqslant\lfloor\dfrac{n}{2}\rfloor$为分支结点，$i>\lfloor\dfrac{n}{2}\rfloor$为叶子结点。
        + 按层序编号后，一旦出现某结点（编号为$i$）为叶结点或只有左孩子，则编号大于$i$的结点均为叶结点

    + 按层序从$1$开始编号，结点$i$的左孩子为$2i$，右孩子为$2i+1$，父结点如果有为$\lfloor\dfrac{i}{2}\rfloor$。
    + 若$n$为奇数，则每个分支结点都有左孩子和右孩子；若$n$为偶数，则编号最大的分支结点（编号为$\dfrac{n}{2}$）只有左孩子，没有右孩子，其余分支结点左、右孩子都有。
    
    ![1689338260t-20230714-2040-434.250](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689338260t-20230714-2040-434.250.png)
    
+ 二叉排序树：

    + 左子树上所有结点的关键字均小于根结点的关键字
    + 右子树上所有结点的关键字均大于根结点的关键字
    + 左右子树又各是一棵二叉排序树。

    ![1689338466t-20230714-2006-533.301](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689338466t-20230714-2006-533.301.png)

+ 平衡二叉树：树上任一结点的左子树和右子树的深度之差不超过$1$。

    + 平衡二叉树能有更高的搜索效率

    ![1689338574t-20230714-2054-383.323](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689338574t-20230714-2054-383.323.png)

### 二叉树的性质

+ 设非空二叉树中度为$0$、$1$和$2$的结点个数分别为$n_0$，$n_1$、$n_2$，则$n_0=n_2+1$

    + 即==叶子结点比二分结点(度为$2$的结点)多一个==
    + 假设树中结点的总数为$n$，则$n=n_0+n_1+n_2$
    + 又根据树的结点$=$总度数$+1$得到$n=n_1+2n_2+0n_0+1$
    + 联立得到结论
    + ==拓展到任意一棵树，若结点数量为$n$，则边的数量为$n-1$==

+ 二叉树的第$i$层至多有$2^{i-1}$个结点。

    + $m$叉树的第$i$层至多有$m^{i-1}$个结点。

+ 高度为$h(h\ge 1)$的二叉树至多有$2^h-1$个结点。

    + 高度为$h$的$m$叉树至多有$\dfrac{m^h-1}{m-1}$个结点。

+ 具有$n$个结点的完全二叉树的高度$h=\lceil\log_2(n+1)\rceil$或$h=\lfloor\log_2n\rfloor+1$

    1.   高为$h$的满二叉树共有$2^h一1$个结点，高为$h-1$的满二叉树共有$2^{h-1}-1$个结点
         + $2^{h-1}-1<n\leqslant2^h-1$
         
         + $h-1<\log_{2}{(n+1)}\le h$
         
         + $h=\lceil\log_{2}(n+1)\rceil$
         
    2.   高为$h-1$的满二叉树共有$2^{h-1}-1$个结点，高为$h$的完全二叉树至少$2^{h-1}$个结点至多$2^{h-1}$个结点
         +   $2^{h-1}< n\le2^h$
         +   $h-1<\log_{2}{n}\le h$
         +   $h=\lfloor\log_2n\rfloor+1$
    
+ 完全二叉树==最多只有一个度为$1$的结点，度为$0$度为$2$的结点的个数和一定为奇数==

+ 若完全二叉树有$2k$个结点，则必然$n_1=1$，$n_0=k$，$n_2=k-1$
    + 即结点个数为$2k$的完全二叉树只有一个度为$1$的结点，叶子结点个数为$k$，二分结点个数为$k-1$

+ 若完全二叉树有$2k-1$个结点，则必然$n_1=0$，$n_0=k$，$n_2=k-1$。
    + 即结点个数为$2k-1$的完全二叉树没有度为$1$的结点，叶子结点个数为$k$，二分结点个数为$k-1$

### 二叉树存储结构

![23July19-133343-1689744823-72a2916a-971c-40b0-80ca-989d58 ce7133](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307191333372.png)

#### 顺序存储

依据二叉树的性质，完全二叉树和满二叉树采用顺序存储比较合适，树中结点的序号可以唯一地反映结点之间的逻辑关系，这样既能最大可能地节省存储空间，又能利用数组元素的下标值确定结点在二叉树中的位置，以及结点之间的关系

![1689340059t-20230714-2139-386.178](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689340059t-20230714-2139-386.178.png)

+   **如果不是完全二叉树,则需要将包含节点的每一层都全部存储**

    +   ==高度为$h$的二叉树需要$2^h-1$个存储单元==
    +   **所有**二叉树都需要$2^h-1$个存储单元

+   如果是完全二叉树，可以按照顺序进行存储,编号为$i$的结点的

    1.   左孩子为$2i$
    2.   右孩子为$2i+1$
    3.   父节点为$\lfloor\dfrac{i}{2}\rfloor$
    4.   所在层次高度$h=\lceil\log_2(i+1)\rceil$或$h=\lfloor\log_2i\rfloor+1$

    ![1689338260t-20230714-2040-434.250](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689338260t-20230714-2040-434.250.png)

如果不是完全二叉树，则**让一般二叉树的编号与完全二叉树一一对应再存入数组**，其他的结点为空，这种存储方法会浪费较多内存，最坏情况下==高度为$h$，且只有$h$个结点的单支树也需要$2^h-1$个存储单元==。

这种存储结构需要从下标$1$开始存储，若从$0$开始则不满足父子结点的性质。

#### 链式存储

由于顺序存储的空间利用率较低，因此一般二叉树通常都采用链式存储结构，用链表结点来存储一般二叉树中的每个结点

![1689340577t-20230714-2117-891.434](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689340577t-20230714-2117-891.434.png)

链式树具有两个分别指向左右孩子的指针。

==在含有$n$个结点的二叉链表中，含有$n+1$个空链域。==

含有$n$个结点的二叉链表中，链域一共有$2n$个（每个点有两个链域）。对于除了根结点以外的每个点都是有一个父亲结点，所以一共有$n-1$个指针指向某个结点，于是形成$n-1$个有内容的链域（减$1$即是根结点）所以一共有$2n-(n-1)=n+1$个链域没有指向任何东西。

如果要保存父结点的位置，可以添加一个父结点指针，从而变成三叉链表。

### 二叉树遍历

二叉树的遍历是指按某条搜索路径访问树中每个结点，使得每个结点均被访问一次，而且仅被访问一次。

#### 顺序遍历

顺序遍历就是深度优先的遍历，分为三种：

+ 先序遍历：**根**左右$NLR$。
    + 若二叉树为空，则什么也不做；否则，
        1.   访问根结点;
        2.   先序遍历左子树;
        3.   先序遍历右子树。 

+ 中序遍历：左**根**右$LNR$。
    + 若二叉树为空，则什么也不做；否则，
        1.   中序遍历左子树
        2.   访问根结点；
        3.   中序遍历右子树。

+ 后序遍历：左右**根**$LRN$。
    + 若二叉树为空，则什么也不做；否则，
        1.   后序遍历左子树；
        2.   后序遍历右子树；
        3.   访问根结点。 


根据算数表达式的分析树的不同先序、中序、后序遍历方式可以得到前缀、中缀、后缀表达式。

三种遍历算法中，递归遍历左、右子树的顺序都是固定的，只是访问根结点的顺序不同。不管采用哪种遍历算法，每个结点都访问一次且仅访问一次，故时间复杂度都是$O(n)$

在递归遍历中，递归工作栈的栈深恰好为树的深度，所以在最坏情况下，二叉树是有$n$个结点且深度为$n$的单支树，遍历算法的空间复杂度为$O(n)$

#### 递归与非递归

借助栈可以将本来是递归算法的顺序遍历变为非递归方式。

对于中序遍历

**基本思路：**

1. 沿着根将其上所有左孩子结点依次入栈，直到左孩子为空。                    
2. 栈顶元素出栈并访问。
3. 若栈顶元素的右孩子为空，则继续执行步骤二。
4. 若栈顶元素的右孩子不为空，则对其右子树执行步骤一。

**举个例子：**

![23July19-123140-1689741100-9e67b060-7655-4d41-80f8-62ef5c772cf7](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307191231496.png)

+   步骤`1.`时栈内元素依次为(栈底)$A\;B\;D$(栈顶)

+   栈顶$D$出栈并进行访问，它是中序序列的第一个结点
+   $D$右孩子为空，栈顶$B$出栈并访问
+   $B$右孩子不空，将其右孩子$E$入栈
+   $E$左孩子为空， 栈顶$E$出栈并进行访问
+   $E$右孩子为空， 栈顶$A$出栈并进行访问
+   $A$右孩子不空，将其右孩子$C$入栈
+   $C$左孩子为空，栈顶$C$出栈并访问
+   由此得到中序序列$DBEAC$.

**代码实现：**

代码实现的测试二叉树

```c++
vector<int> NInOrder(TreeNode* root){
    vector<int> result;
    stack<TreeNode*> s;

    if (root == NULL){
        return result;
    }

    while (root || !s.empty()){
        while (root){
            s.push(root);
            root = root->left;
        }
        root = s.top();
        result.push_back(root->val);
        s.pop();
        root = root->right;
    }
    
    return result;
}
```

先序遍历与中序遍历类似，只是第一步就需要访问中间结点。

后序非递归遍历算法的思路：从根结点开始，将其入栈，然后沿其左子树一直往下搜索，直到搜索到没有左孩子的结点，但是此时不能出栈并访问，因为如果其有右子树，还需按相同的规则对其右子树进行处理。直至上述操作进行不下去，若栈顶元素想要出栈被访问，要么右子树为空，要么右子树刚被访问完（此时左子树早已访问完），这样就保证了正确的访问顺序。

#### 层序遍历

层序遍历就是广度优先的遍历。

要进行层次遍历，需要借助**队列**

![1689506321t-20230716-1941-861.359](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689506321t-20230716-1941-861.359.png)

1. 初始化一个辅助队列。
2. 根结点入队。
3. 若队列非空，则队头结点出队，访问该结点，如果有并将其左右孩子入队。
    +   先左孩子后右孩子
4. 重复步骤三直至队列空。

>   遍历是二叉树各种操作的基础，可以在遍历的过程中对结点进行各种操作。例如，对于一棵已知树求结点的双亲、求结点的孩子结点、求二叉树的深度、求二叉树的叶结点个数、判断两棵二叉树是否相同等。所有这些操作都建立在二叉树遍历的基础上，因此必须掌握二叉树的各种遍历过程，并能灵活运用以解决各种问题。

### 遍历序列构造二叉树

==一个前序遍历序列可能对应多种二叉树形态==

若只给出一棵二叉树的前/中/后/层序遍历序列中的一种不能唯一确定一棵二叉树。只有给出中序遍历序列才可能推出唯一二叉树，因为无法确定根结点相对于左右结点的位置：

![1689506881t-20230716-1901-640.268](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689506881t-20230716-1901-640.268.png)

+ 前序+中序：
+ 后序+中序。
+ 层序+中序。

#### 前序+中序

![1689506822t-20230716-1902-519.233](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689506822t-20230716-1902-519.233.png)

前序：`根+左+右`；中序：`左+根+右`。所以根据三个部分对应相同可以推出。

先序遍历的第一个结点一定是二叉树根结点。中序遍历中根结点必然将序列分为两个部分，前一个序列是左子树的中序序列，后一个序列是右子树的中序序列。同理先序序列中子序列的第一个结点就是左右子树的根结点。

根据二叉树前序遍历和中序遍历的递归算法中递归工作栈的状态变化得出：前序序列和中序序列的关系相当于以前序序列为入栈次序，以中序序列为出栈次序。因为前序序列和中序序列可以唯一地确定一棵二叉树，所以对于先序遍历的$n$个元素，可以确定卡特兰数$\dfrac{1}{n+1}C_{2n}^n$个二叉树。

#### 后序+中序

![1689506831t-20230716-1911-509.215](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689506831t-20230716-1911-509.215.png)

后序：左+根+右；中序：左+根+右。所以根据三个部分对应相同可以推出。

#### 层序+中序

![1689506790t-20230716-1930-701.236](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689506790t-20230716-1930-701.236.png)

层序：根+左根+右根；中序：左+根+右。所以根据根结点和左右子树的根结点来确定。

### 线索二叉树

![23July19-132239-1689744159-31e47bac-1588-43b3-bfbd-930ffc44c3c7](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307191322198.png)

+   对于二叉树的遍历，只能从根结点开始遍历，如果给任意一个结点是无法完成遍历的

+   一般的二叉树的左右结点是用来表示父子关系，而不能直接得到结点在遍历中的前驱或后继。

+   所以我们就想能否保存结点的前驱和后继，从而能减少重复遍历树

    +   因为一棵树很多结点的左右结点可能是空的

    +   $n$个结点的二叉树，有$n+1$个空链域，可用来记录前驱、后继的信息

    +   $n$由个结点共有链域指针 $2n$ 个，其中，除根结点外，每个结点都被一个指针指向。剩余的链域
        建立线索，共$2n-(n-1)=n+1$个线索。
        
        ![1689507309t-20230716-1909-390.290](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689507309t-20230716-1909-390.290.png)
        
    +   这些空闲的指针可以不代表左右子树的根结点，而是用来表示==当前遍历方法的前驱或后继==
    
    +   当这个指针表示的是前驱或后继就称为线索
    
        ![1689507447t-20230716-1927-532.290](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689507447t-20230716-1927-532.290.png)
    
    +   指向前驱的就是前驱线索，由左孩子指针担当
    
    +   指向后继的就是后继线索，由右孩子指针担当。

#### 线索化构造

线索化就是要遍历一遍二叉树，然后对当前结点进行处理。

==所以只需要在原来遍历算法的`visit()`函数中进行线索化即可==

为了区分其左右孩子指针是指向什么，要在结点中新建两个$tag$位，如当$ltag=0$表示$lchild$指向的是左孩子结点，而为$1$表示其指向前驱。

![1689507538t-20230716-1958-684.139](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689507538t-20230716-1958-684.139.png)

+ 确定线索二叉树类型——中序 、先序或后序。
    + 在先序线索化的时候要注意
        + 由于是根左右的顺序，在访问根结点时候进行线索化可能就会将左孩子结点由指向左孩子变成指向前驱的线索（该结点本来就没有左孩子）
        + 然后处理左子树时会跳到这里指向前驱的线索即前一个结点，就会不断在这里循环（程序把前驱当作左孩子不断回撤）
        + 所以在先序遍历二叉树时要根据$ltag$值判断是否是前驱线索再进行遍历左子树
        + 而右孩子结点则不会有这个问题，因为访问顺序必然是左右，所以不管二叉树右孩子结点指向的是右孩子还是后继都是在当前访问结点后应该访问的结点。

    + 而中序线索化和后序线索化都没有这种问题，因为当前结点的前驱在此时按处理顺序都已经处理完了
    + 同时三种线索化都需要处理最后一个结点，当最后一个结点的右孩子指针为`NULL`，要$pre->rtag=1$。

+ 按照对应遍历规则，确定每个结点访问顺序并写上编号。
+ 将$n+1$个空链域连上前驱后继。
+ 没有前驱或后继就指向`NULL`。
+ 这种结构称为线索链表。

##### 中序遍历的线索化

以中序线索二叉树的建立为例。

附设指针`pre`指向刚刚访问过的结点，指针`p`指向正在访问的结点，即`pre`指向`p`的前驱。在中序遍历的过程中，检查`p`的左指针是否为空，若为空就将它指向`pre`；检查`pre`的右指针是否为空，若为空就将它指向`p`

![23July19-125407-1689742447-34c24596-684e-4b72-8076-c270351e044a](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307191254059.png)

**代码实现：**

<img src="https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689508031t-20230716-1911-430.453.png" alt="1689508031t-20230716-1911-430.453" style="zoom:150%;" />

#### 查找前驱后继

![1689513149t-20230716-2129-755.215](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689513149t-20230716-2129-755.215.png)

如果某结点的左右孩子指针有孩子而不是指向前驱后继，那么怎么找其前驱后继？

##### 中序线索二叉树的前驱后继

+ 中序线索二叉树中找到结点$*P$的中序后继$next$：

    + 若$p$右孩子指针指向后继：$p->rtag==1$，则$next=p->rchild$。

    + 若$p$右孩子指针指向右子树根结点：$p->rtag==0$，则==后继$next=p$右子树中最左下结点。==

    + 所以可以利用线索对二叉树实现非递归的中序遍历

        ![1689509269t-20230716-2049-365.114](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689509269t-20230716-2049-365.114.png)

        ![1689509348t-20230716-2008-561.444](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689509348t-20230716-2008-561.444.png)

+ 中序线索二叉树中找到结点$*P$的中序前驱$pre$：

    + 若$p$左孩子指针指向前驱：$p->ltag==1$，则$pre=p->lchild$。

    + 若$p$左孩子指针指向左子树根结点：$p->ltag==0$，==则前驱$pre=p$左子树中的最右下结点==。

    + 所以可以利用线索对二叉树实现非递归的逆向中序遍历

        ![1689509498t-20230716-2038-558.136](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689509498t-20230716-2038-558.136.png)

##### 先序线索二叉树的前驱后继

+ 先序线索二叉树中找到结点$*P$的先序后继$next$：

    + 若$p$右孩子指针指向后继：$p->rtag==1$，则$next=p->rchild$。

    + 若$p$右孩子指针指向右子树根结点：$p->rtag==0$

        + 如果$p$有左孩子，则$p->next=p->lchild$

        + 如果$p$没有左孩子，则肯定有右孩子，$p->next=p->rchild$

            ![1689510670t-20230716-2010-515.187](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689510670t-20230716-2010-515.187.png)

    + 所以可以利用线索对二叉树实现非递归的先序遍历。

+ 先序线索二叉树中找到结点$*P$的先序前驱$pre$：

    + 若$p$左孩子指针指向前驱：$p->ltag==1$，则$pre=p->lchild$。

    + 若$p$左孩子指针指向左子树根结点：$p->ltag==0$，先序遍历中左右子树的根结点只可能是后继，必须向前找。

    + 如果没有父结点所以这时候就找不到$p$的前驱，只能从头开始先序遍历。

    + 如果有父结点，则又有四种情况：

        + $p$为左孩子，则根据根左右，$p$的父结点为根所以在$p$的前面，$p->pre=p->parent$。

        + $p$为右孩子，其左兄弟为空，则根据根左右，顺序为根右，所以$p->pre=p->parent$。

        + $p$为右孩子且有左兄弟，根据根左右，$p$的前驱就是左兄弟子树中最后一个被先序遍历的结点，即在$p$的左兄弟子树中优先右子树遍历的底部。

        + 若$p$是根结点，则没有先序前驱

            ![1689510874t-20230716-2034-917.377](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689510874t-20230716-2034-917.377.png)

###### 后序线索二叉树的前驱后继

+ 后序线索二叉树中找到结点$*P$后序后继$next$：
    + 若$p$右孩子指针指向后继：$p->rtag==1$，则$next=p->rchild$。
    + 若$p$右孩子指针指向右子树根结点：$p->rtag==0$，则根据左右根顺序，左右孩子结点必然是$p$的前驱而不可能是后继，所以找不到后序后继。
    + 如果没有父结点只能使用从头开始遍历的方式。
    + 如果有父结点则又有四种情况：
        + $p$为右孩子，根据左右根，所以$p->next=p->parent$。
        + $p$为左孩子，右孩子为空，根据左右根，所以$p->next=p->parent$。
        + $p$为左孩子，右孩子非空，根据左右根，所以$p->next=$右兄弟子树中第一个被后序遍历的结点，即右子树优先左兄弟子树遍历的底部。
        + 若$p$是根结点，则没有后序后继。
+ 后序线索二叉树中找到结点$*P$后序前驱$pre$：
    + 若$p$左孩子指针指向前驱：$p->ltag==1$，则$pre=p->lchild$。
    + 若$p$左孩子指针指向左子树根结点：$p->ltag==0$，则又有两种情况：
        + 若$p$有右孩子，则按照左右根的情况遍历，右在根的前面，所以$p->pre=p->rchild$。
        + 若$p$没有右孩子，按照左根的顺序，则$p->pre=p->lchild$。

## 树与森林

### (一般)树的存储结构

+ 双亲表示法：是一种顺序存储方式，一般采用一维数组，每个结点中保存指向双亲的伪指针

    ![1689514125t-20230716-2145-802.497](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689514125t-20230716-2145-802.497.png)

    + 查找双亲(父节点)方便
    + 查找孩子不方便,只能从头遍历
    + 适用于“找父亲”多，“找孩子”少的应用场景。如：==并查集==

>   区别树的顺序存储结构与二叉树的顺序存储结构。
>
>   在树的顺序存储结构中，数组下标代表结点的编号，下标中所存的内容指示了结点之间的关系。
>
>   而在二叉树的顺序存储结构中，数组下标既代表了结点的编号，又指示了二叉树中各结点之间的关系。
>
>   当然，二叉树属于树，因此二叉树都可以用树的存储结构来存储，但树却不都能用二叉树的存储结构来存储。

+ 孩子表示法：是**顺序存储加链式存储**方法，顺序存储所有元素，添加一个$firstChild$域，指向第一个孩子结构体的指针

    + 孩子结构体包括元素位 置索引与指向下一个孩子结构体的$next$指针

    + 寻找孩子比较方便，

    + 寻找双亲需要遍历$n$个结点中孩子链表指针域所指向的$n$个孩子链表

    + 适用于“找孩子”多，“找父亲”少的应用场景。如：服务流程树

        ![1689514276t-20230716-2116-901.489](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689514276t-20230716-2116-901.489.png)

+ 孩子兄弟表示法：是一种链式存储方式，定义了两个指针，分别指向第一个孩子与右兄弟，类似于二叉树，可以利用二叉树来实现对树的处理，也称为二叉树表示法

    + 最大的优点是可以将树操作==转换为二叉树==的操作

        + 易于查找结点的孩子

    + 缺点是查找双亲麻烦

        + 不过可以为每个结点设置一个指向双亲的结点`parent`

        ![23July20-110404-1689822244-bf4e20e7-045d-4b70-87dc-e4c05358d5d0](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307201104340.png)

### 森林与树的转换

树与森林的转换，树与二叉树的转换都可以使用==孩子兄弟表示法==来实现，左孩子右兄弟，如果是森林则认为其根结点为兄弟。

![23July20-110623-1689822383-43916536-2881-44e6-a837-5e03301d9631](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307201106473.png)

#### 树与二叉树互换

![23July20-110917-1689822557-0b249743-6561-4338-ae9c-20111d556ff2](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307201109146.png)

树转换为二叉树的规则：每个结点左指针指向它的第一个孩子，右指针指向它在树中的相邻右兄弟，这个规则又称“左孩子右兄弟”。由于根结点没有兄弟，所以对应的二叉树没有右子树。

==二叉树使用孩子兄弟表示法，一般树使用双亲表示法==

树转换成二叉树的画法：

1. 在兄弟结点之间加一连线。
2. 对每个结点，只保留它与第一个孩子的连线，而与其他孩子的连线全部抹掉。
3. 以树根为轴心，顺时针旋转$45°$。

#### 森林转换为二叉树

![1689515190t-20230716-2130-909.385](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689515190t-20230716-2130-909.385.png)

将森林转换为二叉树的规则与树类似。先将森林中的每棵树转换为二叉树，由于任何一棵和树对应的二叉树的右子树必空，若把森林中第二棵树根视为第一棵树根的右兄弟，即将第二棵树对应的二叉树当作第一棵二叉树根的右子树，将第三棵树对应的二叉树当作第二棵二叉树根的右子树……以此类推，就可以将森林转换为二叉树。

森林转换成二叉树的画法：

1. 将森林中的每棵树转换成相应的二叉树。
2. 每棵树的根也可视为兄弟关系，在每棵树的根之间加一根连线。
3. 以第一棵树的根为轴心顺时针旋转$45°$。

#### 二叉树转换为森林

![1689515362t-20230716-2122-914.393](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689515362t-20230716-2122-914.393.png)

二叉树转换为森林的规则：若二叉树非空，则二叉树的根及其左子树为第一棵树的二叉树形式，故将根的右链断开。二叉树根的右子树又可视为一个由除第一棵树外的森林转换后的二叉树，应用同样的方法，直到最后只剩一棵没有右子树的二叉树为止，最后再将每棵二叉树依次转换成树（左边是孩子右边是兄弟还原），就得到了原森林。

### 一般树的遍历

>   这里的树指的是普通的树,上面的先中后序遍历都是针对二叉树而言,对于普通的树不能使用先中后序遍历(因为可能没有两个孩子)

+ 先根遍历：若树非空，先访问根结点，再依次对每棵子树进行先根遍历。

    ![1689516564t-20230716-2224-437.178](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689516564t-20230716-2224-437.178.png)

    + ==树的先根遍历序列与这棵树相应二叉树的先序序列相同==

        ![1689516660t-20230716-2200-925.384](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689516660t-20230716-2200-925.384.png)

+ 后根遍历：若树非空，先依次对每棵子树进行后根遍历，最后访问根结点。

    ![1689516725t-20230716-2205-413.178](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689516725t-20230716-2205-413.178.png)

    + ==树的后根遍历序列与这棵树相应二叉树的中序序列相同==

+ 层次遍历：用辅助队列实现：

    1. 若树非空，根结点入队。
    2. 若队列非空，队头元素出队并访问，同时将该元素的孩子依次入队。
    3. 重复步骤二直到队列为空。

### 森林的遍历

先序遍历森林：

1. 访问森林中第一棵树的根结点。
2. 先序遍历第一棵树中根结点的子树森林。
3. 先序遍历除去第一棵树之后剩余的树构成的森林。

>   因为存在两层的递归,人脑没有办法这样堆栈,建议先依次把每棵树先根遍历然后按顺序排列就行

![1689517106t-20230716-2226-904.344](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689517106t-20230716-2226-904.344.png)

中序遍历森林：

1. 先序遍历第一棵树中根结点的子树森林。
2. 访问森林中第一棵树的根结点。
3. 中序遍历除去第一棵树之后剩余的树构成的森林。

>    因为存在两层的递归,人脑没有办法这样堆栈,建议先依次把每棵树后根遍历然后按顺序排列就行

![1689517160t-20230716-2220-911.344](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689517160t-20230716-2220-911.344.png)

如果通过转换，那么遍历的结果是等价的

![1689517216t-20230716-2216-676.149](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689517216t-20230716-2216-676.149.png)

>   注:上图中
>
>   ==树的后根遍历对应二叉树中序遍历指的是一般树的后根遍历序列与转换成二叉树后的中序遍历序列相同==
>
>   ==指的不是一般树的后根遍历就是使用二叉树的类似左**根**右$LNR$的遍历算法==
>
>   一般树没有左根右(不一定有且只有两个孩子)

### 转换关系

假设森林为$F$，树为$T$，转换而来的二叉树为$B$。

#### 结点关系

+ $T$有$n$个结点，叶子结点个数为$m$，则$B$中无右孩子的结点个数为$n-m+1$个。

树转换为二叉树时，树的每个分支结点的所有子结点的最右子结点无右孩子，根结点转换后也无右孩子。

>    n个节点的树，有n-1个边（度）
>
>   由于叶子节点个数为x，此树有n-x个非叶结点
>
>   每个非叶结点有且仅有一个长子，对应二叉树有n-x左向边
>
>   右向边 = 总边数 - 左向边 = (n-1) - (n-x) = x-1
>
>   总共有n个点，其中只有x-1个点有右孩子，剩下的n-x+1个点没有右孩子(即证)

+ $F$有$n$个非终端结点，则$B$中无右孩子的结点有$n+1$个。

根据森林与二叉树转换规则“左孩子右兄弟”，$B$中右指针域为空代结点没有兄弟结点。森林中每棵树的根结点从第二个开始依次连接到前一棵树的根的右孩子，因此最后一棵树的根结点的右指针为空，这里有一个。另外，每个非终端结点即代表有孩子，其所有孩子结点不论有多少个兄弟，在转换之后，最后一个孩子的右指针一定为空，故树$B$中右指针域为空的结点有$n+1$个。

> ==$B$中无右孩子的结点数等于$T/ F$中非终端结点数加一==

#### 边关系

+ $F$有$n$条边、$m$个结点，则$F$包含$T$的个数为$m-n$。

若有$n$条边，则如果全部组成最小的树每个需要两个结点，总共需要$2n$个结点，组成$n$根树。假定$2n>m$，则还差$2n-m$个结点才能两两成树，所以少的这些结点不能单独成树，导致有$2n-m$个结点只能跟其他现成的树组成结点大于二的树。所以此时只能组成$n-(2n-m)=m-n$棵树。

## 树的应用

### 哈夫曼树

#### 哈夫曼树的定义

+ 路径和路径长度：从树中的一个结点到另一个结点之间的分支构成这两个结点之间的路径，路径上的分支数目称作路径长度。

+ 结点的权($weight,w$)：有某种现实含义的数值。

+ 结点的带权路径长度($length,l$)：从根到该结点的路径长度（经过边数）与该结点权的乘积称为结点的带权路径长度。

+ 树的带权路径长度$WPL$：树中所有**叶子**的带权路径长度之和称为树的带权路径长度
    $$
    WPL=\sum_{i=1}^nw_il_i
    $$

+ 哈夫曼树（最优二叉树）：带权路径长度最短的二叉树。不一定是完全二叉树。

+ ==哈夫曼树不存在度为$1$的结点。==

#### 构造哈夫曼树

给定$n$个权值分别为$w_1, w_2\cdots w_n$的结点，构造哈夫曼树的算法描述如下：

1. 将这$n$个结点分别作为$n$棵仅含一个结点的二叉树，构成森林$F$。
    +   这一步实际上就是把各个节点写在一堆
2. 构造一个新结点，从$F$中选取两棵根结点权值最小的树作为新结点的左、右子树，并且将新结点的权值置为左、右子树上根结点的权值之和
    +   ==默认树较深的在右侧==
3. 从$F$中删除刚才选出的两棵树，同时将新得到的树加入$F$中。
4. 重复步骤二和三，直至$F$中只剩下一棵树为止。

#### 哈夫曼树的性质※

+ 每个初始结点最终都会变成叶子结点，且权值越小到根结点的路径长度越长。
+ 哈夫曼树的结点总数为$2n-1$。
+ 构建哈夫曼树时，都是两个两个合在一起的，所以没有度为一的结点，即$n_1=0$。
+ 哈夫曼树一般不唯一，但是$WPL$必然最优。
+ 哈夫曼树适合采用顺序结构：已知叶子结点数$n_0$，且$n_1=0$，则总结点数为$2n_2+1$（或$2n_0-1$），且哈夫曼树构造过程需要不停地修改指针，用链式存储的话很容易造成指针偏移。

#### 哈夫曼编码

哈夫曼编码基于哈夫曼树，利用哈夫曼树对$01$的数据进行编码，来表示不同的数据含义，因为哈夫曼树必然权值最小，所以对于越常使用的编码越短，越少使用的编码越长，所以发送信息的总长度是最小的。

将编码使用次数作为权值构建哈夫曼树，然后根据左$0$右$1$的原则，按根到叶子结点的路径就变成了哈夫曼编码。

若没有一个编码是另一个编码的前缀，则称这样的编码为前缀编码

哈夫曼编码是可变长度编码，即允许对不同字符用不等长的二进制表示，也是一个前缀编码，没有一个编码是另一个编码的前缀。

![1689514969t-20230716-2149-848.329](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689514969t-20230716-2149-848.329.png)

利用哈夫曼树可以设计出总长度最短的二进制前缀编码

所以哈夫曼编码也可以用于压缩。

### 并查集

![1689517634t-20230716-2214-910.368](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689517634t-20230716-2214-910.368.png)

将一个集合划分为互不相交的子集。类似森林。

一般用树或森林的双亲表示作为并查集的存储结构，每个子集用一个树表示。

用数组元素的下标表示元素名，用根结点的下标表示子合集名，根节点的双亲结点为负数。

![1689516163t-20230716-2243-296.190](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689516163t-20230716-2243-296.190.png)

#### 存储结构

![1689516055t-20230716-2255-763.417](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689516055t-20230716-2255-763.417.png)

其中为$-1$表示这个点有子结点，其绝对值为孩子数量，为正数表示其父结点的索引值。

#### 查找

![1689516253t-20230716-2213-319.122](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689516253t-20230716-2213-319.122.png)

查找两个元素是否属于同一个集合。

判断两个元素是否属于同一集合只需要找到其根节点进行比较。需要通过向上递归的方式不断查找父结点直到找到根节点判断是否为同一个即可。

查的时间复杂度为$O(n)$

#### 合并

![1689516259t-20230716-2219-329.150](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689516259t-20230716-2219-329.150.png)

如果两个元素不属于同一个集合，且所在的两个集合互不相交，则合并这两个集合。

只需将一个集合的根节点的父节点指向另一个集合的根节点即可

并的时间复杂度为$O(1)$。

#### 路径压缩

![1689517291t-20230716-2231-430.255](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689517291t-20230716-2231-430.255.png)

用于提高并查集效率。

如果一个结点只有一个子结点，且子结点也只有一个子结点，那么这条链路会非常长，即对应的树的树高会很大，影响查询效率。

路径压缩优化可以使用递归或迭代的方式实现。在递归实现中，==当递归地查找到根节点时，依次将路径上的每个节点的父节点更新为根节点==。

+   通过路径压缩优化，可以将==查找操作的时间复杂度接近于常数级别==
    +   经过路径压缩优化后，查找操作的时间复杂度可以接近于常数级别，具体为近似于 O(α(n))，其中 α(n) 是阿克曼函数([阿克曼函数 - 维基百科](https://zh.wikipedia.org/wiki/阿克曼函數))的反函数。
    +   阿克曼函数是一个高度递归的函数，它的增长非常快，难以用常规的数学记法表示。然而，它的反函数 α(n) 是一个非常慢增长的函数，在实际应用中可以近似为常数。因此，可以说路径压缩优化后的查找操作时间复杂度近似为常数级别。
    +   路径压缩优化通过将查找路径上的每个节点的父节点直接指向根节点，使得树的高度变小，将每个节点与根节点之间的路径压缩为常数长度。在后续的查找操作中，由于路径已经被压缩，根节点的查找可以在几个常数时间内完成。
    +   需要注意的是，虽然路径压缩优化使得查找操作的时间复杂度接近于常数级别，但实际上并查集的合并操作的时间复杂度仍为 O(1)。路径压缩只影响查找操作的效率，不会对合并操作的时间复杂度产生影响。
+   这是因为路径压缩使得每个节点都更接近根节点，从而缩短了查找路径的长度。路径压缩优化在实践中被广泛应用，特别适用于大规模的并查集操作。

#### 按秩合并

![1689516378t-20230716-2218-364.232](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689516378t-20230716-2218-364.232.png)

并查集经过路径压缩优化之后，并查集并不是是只有两层的一颗树。因为路径压缩只在查找的时候进行，也==只压缩一条路径==，所有并查集的最终结构仍然可能是比较复杂的。

在将一个新元素并入并查集前，就应该使用按秩合并的方式对并查集进行优化。

为了避免加深树高，所以新的结点应该合并到矮的子树上。

为了降低树的结点数和合并操作的难度，应该将简单的树合并到复杂的树上。

==根节点的存储方式采用了负数，并不是始终为-1==

==根节点的绝对值表示这个集合的大小（包括根节点本身）。因此，当进行`Union`操作时，要考虑两个集合的大小，将节点数少的根节点合并到节点数多的根节点上，并更新节点数。==

用秩数组来记录每个根结点对应的树的深度（如果不是根结点，则秩数组中的元素大小表示的是以当前结点作为根结点的子树的深度）；一开始，把所有元素的秩值设为1，即自己就为一颗树，且深度为1；合并的时候，比较两个根结点，把秩值较小者合并到较大者中去。

#### 应用

1.   判断图的连通分量数--遍历各边，有边相连的两个顶点确认连通，“并”为同一个集合。只要是相互连通的顶点都会被合并到同一个子集合中，相互不连通的顶点一定在不同的子集合中。
2.   $Kruskal$算法的最小生成树--各边按权值递增排序，依次处理：判断是否加入一条边之前，先查找这条边关联的两个顶点是否属于同一个集合（即判断加入这条边之后是否形成回郡），若形成回路，则继续判断下一条边；若不形成回路，则将该边和边对应的顶点加入最小生成树$T$，并继续判断下一条边，直到所有顶点都已加入最小生成树$T$。
