# 第二章 线性表

## 导读

### 【考纲内容】

1. 线性表的基本概念
2. 线性表的实现
    + 顺序存储
    + 链式存储
3. 线性表的应用

### 【知识导图】

![1689255705t-20230713-2145-333.175](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/1689255705t-20230713-2145-333.175.png)

### 【复习提示】

+ 线性表是算法题命题的重点。
+ 这类算法题实现起来比较容易且代码量较少，但是要求具有最优的性能（时间复杂度、空间复杂度），才能获得满分
+ 因此，应牢固掌握线性表的各种基本操作（基于两种存储结构），在平时的学习中多注重培养动手能力
+ 另外，需要提醒的是，算法最重要的是思想！考场上的时间紧迫，在试卷上不一定要求代码具有实际的可执行性，因此应尽力表达出算法的思想和步骤，而不必过于拘泥每个细节。
+ 注意算法题只能用`C++`语言实现。

## 线性表

### 逻辑结构

是具有**相同**（所占空间一样大）数据类型的$n$个数据元素的**有限序列**（有次序）。$n$表示表长。

$L=(a_1，a_2，\cdots，a_i，\cdots，a_n)$，其中$i$表示元素在线性表中的位序。

+ 存在唯一的第一个元素，存在唯一的最后一个元素。
+ 除第一个元素（表头元素）无直接前驱之外，每个元素均有且仅有一个直接前驱。
+ 除最后一个元素（表尾元素）无直接后继之外，每个元素均有且仅有一个直接后继。

### 线性表的特点

+   表中元素的个数有限。
+   表中元素具有逻辑上的顺序性，表中元素有其先后次序。
+   表中元素都是数据元素，每个元素都是单个元素
+   表中元素的数据类型都相同，这意味着每个元素占有相同大小的存储空间
+   表中元素具有抽象性，即仅讨论元素间的逻辑关系，而不考虑元素究竟表示什么内容。

---

>   ==线性表是一种逻辑结构==，表示元素之间一对一的相邻关系。==顺序表和链表是指存储结构==，两者属于不同层面的概念，因此不要将其混淆。

### 线性表的基本操作

+ 初始化表`InitList(&L)`：构造一个空的线性表`L`，分配内存空间。
+ 销毁操作`DestroyList(&L)`：销毁线性表，并释放线性表`L`所占用的内存空间。
+ 插入操作`ListInsert(&L，i，e)`：在表`L`中的第`i`个位置上插入指定元素`e`。
+ 删除操作`ListDelete(&L，i，&e)`：删除表`L`中第`i`个位置的元素，并用`e`返回删除元素的值
+ 按值查找操作`LocateElem(L，e)`：在表`L`中查找具有给定关键字值的元素。
+ 按位查我操作`GetElem(L，i)`：获取表`L`中第`i`个位置的元素的值。
+ 求表长`Length(L)`：返回线性表`L`的长度，即`L`中数据元素的个数。
+ 输出操作`PrintList(L)`：按前后顺序输出线性表`L`的所有元素值。
+ 判空操作`Empty(L)`：若`L`为空表，则返回`true`，否则返回`false`。
+ 销毁操作`DestroyList(&L)`：销毁线性表，并释放线性表L所占用的内存空间。

### 物理结构

+ 顺序存储结构：顺序表。
+ 链式存储结构：链表。

## 顺序表

+ **线性表的顺序存储又称顺序表**

+ 把逻辑上相邻的元素存储在物理位置上也相邻的存储单元中，元素之间的关系由存储单元的邻接关系来实现。$i$是元素$a_i$在线性表中的位序。


### 顺序表特点

1. ==随机访问，可以通过首元素和元素序号在$O(1)$时间内找到对应元素==；
2. ==插入删除操作不方便==；
3. 存储密度高，只用存储数据；
4. 拓展容量不方便；
5. 要求大片连续存储空间；
6. 表中元素的逻辑地址与物理地址顺序相同。

### 顺序表定义

使用$C$语言的结构体定义顺序表，使用`typedef`定义一个`ElemType`表示数据基本类型，并定义最大长度`MAXSIZE`：

```c
// 初始化最大长度
#define MAXSIZE 25
// 定义默认值
#define DEFAULTELEM 0
// 定义最大值
#define INFINITY 32767
// 定义默认数据类型
typedef char element_type;
```

可以使用**静态分配**空间：

![image-20230704224205453](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307042242503.png)

也可以使用动态分配空间，动态分配空间还是顺序的，只不过可以替换原来空间：

![image-20230704225442071](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307042254114.png)

其中长度是指有数据的长度，而最大容量是指已经分配给动态数组的长度，插入时要考虑这个长度，不能溢出。

+   $C$的初始动态分配语句为
    +   `L.data=(ElemType*)malloc(sizeof(ElemType)*InitSize);`
+   $C++$的初始动态分配语句为
    +   `L.data=new ElemType[InitSize];`
+   动态分配并不是链式存储，它同样属于==顺序存储结构==，物理结构没有变化，依然是**随机存取方式**，只是分配的空间大小可以在运行时动态决定。

---

>   +   **`new` 关键字**
>
>   `new` 是在C++中用于动态分配内存并创建对象的关键字。它用于在堆上分配内存空间来创建对象，与在栈上创建对象的方式不同。使用 `new` 关键字可以实例化单个对象、数组以及动态分配其他数据类型的内存。
>
>   1. 实例化单个对象：
>      - 语法：`new 类型名` 或 `new 类型名(参数列表)`
>      - 功能：在堆上分配内存空间并创建指定类型的对象。
>      - 示例：
>        ```cpp
>        int* ptr = new int;  // 在堆上分配内存并创建一个 int 对象
>        *ptr = 10;          // 对动态分配的对象进行操作
>        delete ptr;         // 释放内存空间
>        ```
>
>   2. 实例化对象数组：
>      - 语法：`new 类型名[元素数量]` 或 `new 类型名[元素数量](参数列表)`
>      - 功能：在堆上分配内存空间并创建指定类型的对象数组。
>      - 示例：
>        ```cpp
>        int* arr = new int[5];  // 在堆上分配内存并创建一个包含 5 个 int 对象的数组
>                                                
>        for (int i = 0; i < 5; ++i) {
>            arr[i] = i + 1;    // 对动态分配的对象数组进行操作
>        }
>                                                
>        delete[] arr;          // 释放内存空间（注意使用 delete[] 删除对象数组）
>        ```
>
>   3. 动态分配其他数据类型的内存：
>      - 语法：`new 类型名[元素数量]` 或 `new 类型名[元素数量](参数列表)`
>      - 功能：在堆上分配指定类型和数量的内存空间。
>      - 示例：
>        ```cpp
>        int* data = new int[10];   // 在堆上分配内存来存储 10 个 int 值
>                                                
>        for (int i = 0; i < 10; ++i) {
>            data[i] = i;   // 对动态分配的内存进行操作
>        }
>                                                
>        delete[] data;            // 释放内存空间（注意使用 delete[] 删除内存数组）
>        ```
>

---

>**malloc() 函数** 
>
>+ 函数作用： `malloc()` 是C语言中的函数，用于在堆上分配指定大小的内存空间。它接收一个参数，表示所需内存的大小（以字节为单位），并返回指向该内存块的指针。
>+ 函数实现： `malloc()` 函数的实现可以根据操作系统和编译器的不同而有所差异。通常情况下，它会在堆上查找足够大的连续内存块，然后将其分配给程序。如果找不到足够大的内存块，则返回一个空指针。
>+ 函数使用方法
>    + 首先，需要包含 `<stdlib.h>` 头文件，该头文件中包含了 `malloc()` 函数的声明。
>    + 使用 `malloc()` 函数时，需要指定所需内存的大小，并将返回的指针赋给一个指针变量。例如，可以使用以下语句分配一个大小为 `n` 字节的内存块：`int* ptr = (int*)malloc(n);`
>    +  注意，在C++中使用 `malloc()` 时，**需要将返回的指针强制类型转换为适当的指针类型**。
>
>        + 在分配内存后，可以使用指针变量 `ptr` 来访问和操作所分配的内存块。例如，可以对分配的内存进行初始化、读取和写入操作。
>        + 最后，在不再需要分配的内存块时，应该使用 `free()` 函数释放内存，以避免内存泄漏。例如，可以使用以下语句释放之前分配的内存块：`free(ptr);`
>        +  注意，与 `malloc()` 相对应，需要确保释放的是通过 `malloc()` 分配的内存块，否则可能会导致未定义的行为。
>
>
>- 返回值： `malloc()` 函数的返回值是一个指向分配内存块的指针。如果分配成功，则返回指向内存块的指针；如果分配失败（内存不足），则返回一个空指针（ `NULL` ）。
>- 注意事项：
>
>  - 使用 `malloc()` 分配的内存块不会自动初始化，所以在使用之前，需要手动进行初始化。
>
>  - 分配的内存块应该在使用完毕后及时释放，以免造成内存泄漏。
>
>  - 在C++中，更推荐使用 `new` 和 `delete` 运算符来动态分配和释放内存，因为它们提供了更好的类型安全性和异常处理机制。

---

>   **new 和 malloc 的区别** 
>
>   + 语法：
>       -  `malloc()` 是一个C语言函数，在C++中也可以使用，需要包含 `<cstdlib>` 头文件，并使用类型转换将返回的指针转换为适当的类型。
>       -  `new` 是一个C++运算符，不需要包含特定的头文件，并且返回的指针类型是自动推断的。
>
>   + 内存分配和初始化：
>       -  `malloc()` 只负责分配指定大小的内存块，不会对分配的内存进行初始化，即内存的内容是未定义的。
>       -  `new` 在分配内存的同时，会调用构造函数对内存进行初始化。这对于对象的构造是非常重要的，确保对象的成员变量处于有效状态。
>
>   + 内存大小：
>       -  `malloc()` 接收以字节为单位的内存大小作为参数。
>       -  `new` 接收对象类型作为参数，并根据类型自动计算所需的内存大小。
>
>   + 错误处理：
>       -  `malloc()` 在内存不足时返回一个空指针（ `NULL` ），需要手动检查返回值来判断内存分配是否成功。
>       -  `new` 在内存不足时抛出 `std::bad_alloc` 异常，可以使用异常处理机制来捕获和处理异常。
>
>   + 内存释放：
>       -  `malloc()` 分配的内存块需要使用 `free()` 函数手动释放。
>       -  `new` 分配的内存可以使用 `delete` 运算符来释放，对于数组形式的分配，使用 `delete[]` 。
>
>   + 类型安全：
>       -  `malloc()` 返回的是 `void*` 类型的指针，需要手动转换为实际的指针类型，可能存在类型错误的风险。
>       -  `new` 返回的指针类型是自动推断的，不需要手动转换，并且在编译时进行类型检查，提供了更好的类型安全性。
>

### 顺序表操作

#### 顺序表初始化

+ 静态顺序表因为数组部分在创建时就已经设置好了，所以初始化就直接设置数据长度就可以了。
  
  ![image-20230704224205453](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307042242503.png)
  
    ![image-20230704224301457](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307042243497.png)
  
    + 顺序表的表长刚开始确定后就**无法更改**（存储空间是静态的）
  
+ 动态顺序表不仅需要设置数据长度与最大 长度，还得分配数组初始空间。
  
    ![image-20230704225442071](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307042254114.png)
    
    ![image-20230704231125072](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307042311115.png)

#### 顺序表增长数据空间长度

只有**动态顺序表**才能增加。

![image-20230704231140027](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307042311067.png)

#### 顺序表插入

使用静态定义

![image-20230704231922311](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307042319351.png)

倒序移动元素，最后将数据插入对应索引并长度加一。（这是一个较好的方式，因为如果插入的话其他元素会被挤住，倒序移动元素可以正好空出位置）

![image-20230704231956301](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307042319344.png)



+ 空间复杂度为$O(1)$。

+ 时间复杂度

    + 最好时间复杂度O(1)$

        + 新元素插入到表尾，不需要移动元素
        + `i=n+1`，循环$0$次

    + 最坏时间复杂度$O(n)$

        + 新元素插入到表头，需要将原有的个元素全都向后移动
        + `i=1`，循环$n$次：

    + 平均时间复杂度$O(n)$

        + 假设$p_i$（$n_i=\dfrac{1}{n+1}$）是$i$位置上插入一个结点的概率，则在长度为$n$的线性表中插入一个结点时所需要移动结点的平均次数为

        + $\sum\limits_{i=1}^{n+1}p_i(n-i+1)=\sum\limits_{i=1}^{n+1}\dfrac{1}{n+1}(n-i+1)$

            $=\dfrac{1}{n+1}\sum\limits_{i=1}^{n+1}(n-i+1)=\dfrac{1}{n+1}\times\dfrac{n(n+1)}{2}=\dfrac{n}{2}$


---

> 注:**节点和结点**
>
> 1. 节点是一个实体，它具有处理的能力；
>     + 比如网络上的一台计算机
> 2. 结点是一个交叉点、一个标记，我们称它为结点。
>     + 结点则只是一个交叉点，像“结绳记事”，打个结，做个标记
>     + **一般算法中的点都称为结点**
>     + 数据集合中的每一个数据元素都用中间标有元素值的方框来表示

#### 顺序表删除

使用静态定义

![image-20230704231922311](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307042319351.png)

正序移动元素并长度减一。

![image-20230704232414045](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307042324090.png)

+ 空间复杂度为$O(1)$

+ 时间复杂度

    + 最好时间复杂度$O(1)$

        + 删除表尾元素，不需要移动其他元素
        + `i=n`，循环$0$次；

    + 最坏时间复杂度$O(n)$

        + 删除表头元素，需要将后续的$n-1$个元素全都向前移动
        + `i=1`，循环$n-1$次：

    + 平均时间复杂度$O(n)$

        + 假设$p_i$（$n_i=\dfrac{1}{n+1}$）是$i$位置上删除一个结点的概率，则在长度为$n$的线性表中插入一个结点时所需要移动结点的平均次数为

        + $\sum\limits_{i=1}^{n+1}p_i(n-i+1)=\sum\limits_{i=1}^{n+1}\dfrac{1}{n+1}(n-i+1)$

            $=\dfrac{1}{n+1}\sum\limits_{i=1}^{n+1}(n-i+1)=\dfrac{1}{n+1}\times\dfrac{n(n+1)}{2}=\dfrac{n}{2}$

#### 顺序表查找

使用动态定义

![image-20230704232842554](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307042328601.png)

+ 按位查找时间复杂度为$O(1)$。
  
    ![image-20230704232910427](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307042329466.png)
    
    + “随机存取”特性：由于顺序表的各个数据元素在内存中连续存放，因此可以根据起始地址和数据元素大小立即找到第$i$个元素
+ 按值查找时间复杂度为$O(n)$
  
    ![image-20230704233030094](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307042330135.png)
    
    + 一般都是找到第一个元素等于指定值的元素，返回其位序，如果没有找到就返回$-1$
    + 最好时间复杂度$O(1)$
        + 目标元素在表头，循环$1$次
    + 最坏时间复杂度$O(n)$
        + 目标元素在表尾，循环$n$次：
    + 平均时间复杂度$O(n)$
        + 假设目标元素出现在任何一个位置的概率相同，都是1
        + 目标元素在第1位，循环1次；在第2位，循环2次；...：在第n位，循环n次

> 408统考的《数据结构》试题中，手写代码可以直接用$==$，无论$ElemType$是基本数据类型还是结构类型

## 单链表

+   每个结点只包含一个指针域，也称为线性链表。
+   通常用头指针来标识一个单链表，如单链表$L$。
+   为了操作上的方便，在单链表第一个结点之前附加一个结点，称为==头结点==
+   头结点
    +   头结点的数据域可以不设任何信息，也可以记录表长等信息
    +   头结点的指针域指向线性表的第一个元素结点

+   头结点和头指针的区分

    +   不管带不带头结点，头指针都始终指向链表的第一个结点
    +   而头结点是带头结点的链表中的第一个结点，结点内通常不存储信息。 
+   引入头结点后，可以带来两个优点:

    1.   由于第一个数据结点的位置被存放在头结点的指针域中，因此在链表的第一个位置上的操作和在表的其他位置上的操作一致，无须进行特殊处理
    2.   无论链表是否为空，其头指针都是指向头结点的非空指针(空表中头结点的指针域为空)，因此空表和非空表的处理也就得到了统一

### 单链表特点

+ 不要求大量连续空间，改变容量(删除和插入)方便。
    + 使用==数组==的数据结构都需要大量连续空间

+ 不可随机存取。
    + 查找某个特定的结点时，需要从表头开始遍历， 依次查找。
    + 增删改都需要先查找结点，都需要$O(n)$的时间

+ 要花费多余空间存放指针。
    + 单链表附加指针域，浪费存储空间


是**非随机存取**的存储结构。

### 单链表定义

使用`LinkNode`表示一个单链表结点的结构体，而使用`LinkList`表示一个单链表，其实`LinkList`是一个指向`LinkNode`的指针变量。如定义`LinkList L`等价于`LinkNode* L`。

如，以下两种写法等价

![image-20230705094138531](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307050941593.png)

![image-20230705094202690](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307050942735.png)

要表示一个单链表时，只需声明一个**头指针**`L`，指向单链表的第一个结点

```c++
LNode *L;//声明一个指向单链表第一个结点的指针，强调指向链表中某结点的指针
LinkList L;//声明一个指向单链表第一个结点的指针，强调指向链表中头结点的指针(强调这是一个单链表)，一般不会变(不会再赋值)
```

---

>在C++中， `*` 和 `&` 运算符在内存操作方面具有以下含义：
>
>1.  `*` 运算符（解引用运算符）：
>   - 在指针类型前使用 `*` 运算符，可以访问指针所指向的内存地址的值。
>   - 例如，如果 `ptr` 是一个指向某个变量的指针， `*ptr` 表示该变量的值。
>   -  `*` 运算符用于读取或修改指针指向的内存中存储的值。
>
>2.  `&` 运算符（取地址运算符）：
>   - 在变量名前使用 `&` 运算符，可以获取该变量在内存中的地址。
>   - 例如，如果 `var` 是一个变量， `&var` 表示变量 `var` 的内存地址。
>   -  `&` 运算符用于获取变量的指针，可以用于传递变量的地址或创建指向变量的指针。
>
>下面是一些示例说明 `*` 和 `&` 运算符的使用：
>
>
>```cpp
>int var = 42;
>int* ptr = &var; // 获取变量 var 的地址，并将其赋值给指针 ptr
>int value = *ptr; // 通过解引用运算符获取指针 ptr 所指向的变量的值
>*ptr = 10; // 通过解引用运算符修改指针 ptr 所指向的变量的值
>int* anotherPtr = &(*ptr); // 使用解引用和取地址运算符进行连续操作
>int& ref = var; // 创建一个引用，ref 绑定到变量 var
>int& ref2 = *ptr; // 创建一个引用，ref2 绑定到指针 ptr 所指向的变量
>```

---

>再注：如何比较两个结构体`struct`是否相等
>
>1. 运算符重载：
>
>   + 可以重载 `==` 运算符，使其适用于比较两个 `struct` 类型的对象。
>
>     + 在重载函数中，根据自定义的比较规则，逐个比较结构体的成员变量是否相等。
>
>      + 如果所有成员变量都相等，则返回 `true` ，否则返回 `false` 。
>
>      + ```cpp
>        struct MyStruct {
>          int x;
>          int y;
>           bool operator==(const MyStruct& other) const {
>               return (x == other.x) && (y == other.y);
>           }
>        };
>        ```
>
>2. 比较函数：
>
>   + 可以编写一个函数，接受两个结构体对象作为参数，比较它们的成员变量是否相等。
>
>     + 函数返回 `bool` 类型，表示结构体对象是否相等。
>
>      + 例如：
>
>      + ```cpp
>        struct MyStruct {
>          int x;
>          int y;
>        };
>        
>        bool CompareStructs(const MyStruct& obj1， const MyStruct& obj2) {
>          return (obj1.x == obj2.x) && (obj1.y == obj2.y);
>        }
>        ```

### 单链表操作

#### 单链表初始化

+ 有带头结点与不带头结点的初始化的区别，带头结点代表第一个结点不存放数据，只是用于标识单链表的开始，但是区别不大，带头结点更好使用。
    + 不带头结点的单链表 
         ![image-20230705103454355](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307051034408.png)
    + 带头结点的单链表 
         ![image-20230705103522915](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307051035966.png)

+ 由于第一个数据结点的位置被存放在头结点的指针域中，因此在链表的第一个位置上的操作和在表的其他位置上的操作一致，无须进行特殊处理。
+ 无论链表是否为空，其头指针都指向头结点的非空指针（空表中头结点的指针域为空)，因此空表和非空表的处理也就得到了统一。

#### 单链表插入

插入方式一共分为下面几种：

+ 带头结点的位序插入
  
    ![image-20230705103746397](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307051037452.png)
+ 不带头结点的位序插入
  
    ![image-20230705105145070](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307051051123.png)

+ 指定结点插入：
    + 前插入。
      
        ![image-20230705105707653](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307051057704.png)
        
        ![image-20230705105812650](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307051058706.png)
    + 后插入。
      
        ![image-20230705105327320](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307051053372.png)
        
        ![image-20230705105500842](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307051055898.png)

假定从第一个结点开始就是第$0$索引的结点。

带头结点的单链表头结点就是$0$号结点，不带头结点的第一个数据结点就是$0$号结点。

带头结点的单链表只能往头结点之后插入，所以插入索引必须从$1$开始。

插入有/无头结点单链表元素函数的后面代码可以使用后插入单链表元素函数来替代。

使用前插入的方法插入元素，可以使用头指针来得到整个链表信息，从而就能找到链表中的这个结点，但是如果没有头指针那么就无法实现了。且这种遍历的时间复杂度是$O(n)$。

还有另一种方式实现头插法，先后插一个元素，把前面结点的数据移动到这个新加的结点，把要新加的数据放在原来的结点，这就实现了后插，虽然地址没有变化，但是从数据上看就是前插，且时间复杂度是$O(1)$。

#### 单链表删除

基本的方式和插入类似，都是转移$next$结点。

按位序删除带头结点的也只能删除从$1$开始的结点，$0$的头结点不能删除。

![image-20230705112030519](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307051120578.png)

时间复杂度为$O(n)$。

无头结点需要额外处理第一个结点

如果删除指定结点而不知道其前驱，也可以使用之前前插结点的方式，把该结点后继的结点的数据复制到本结点上，然后把后继结点删除，就相当于删除了本结点。时间复杂度为$O(1)$。

![image-20230705112302568](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307051123625.png)

所以单链表还是不算方便。

---

> 注:删除指定结点时若使用上述方法且传入结点指针`*p`为最后一个结点(无直接后继结点)时，在执行`p->data=p->next->data;`时会报错，因为此时`p`所指向的内存为`NULL`，此时优先使用传入头指针删除的方法
>
> ```cpp
> void deleteNode(LinkList list， element_type value) {
>     // 指向头结点的指针
>     LinkListNode* head = list;
> 
>     // 遍历链表，找到待删除结点的前一个结点
>     LinkListNode* prev = head;
>     while (prev->next != NULL && prev->next->data != value) {
>         prev = prev->next;
>     }
> 
>     // 判断是否找到待删除结点
>     if (prev->next == NULL) {
>         // 没有找到待删除结点，进行错误处理
>         // 或者直接返回
>         return;
>     }
> 
>     // 待删除结点的指针
>     LinkListNode* toDelete = prev->next;
> 
>     // 修改指针，将待删除结点从链表中断开
>     prev->next = toDelete->next;
> 
>     // 释放待删除结点的内存空间
>     free(toDelete);
> }
> ```

---

> 再注: **关于`p` ， `q` 指针变量的命名**
>
> 在单链表中，通常使用 `p` 和 `q` 作为指针变量来代表不同的结点。
>
> 1.  `p` 指针（或 `ptr` ）通常表示当前结点，用于遍历链表或执行操作。
>     - 例如，在遍历链表时，可以使用 `p` 指针从头结点开始，沿着 `next` 指针依次访问每个结点。
>     -  `p` 可以指向当前结点，进行数据读取、修改或其他操作。
>
> 2.  `q` 指针（或 `temp` ）通常表示临时结点或辅助结点，用于插入、删除或交换结点等操作。
>     - 例如，在插入结点时，可以使用 `q` 指针指向待插入位置的前一个结点，然后调整指针进行插入操作。
>     -  `q` 可以在链表中移动，帮助完成特定的插入、删除或其他修改操作。
>
> 命名 `p` 和 `q` 作为指针变量的含义是为了简洁、易读以及符合传统惯例。 `p` 通常表示 "$pointer$"（指针）或 "$current$"（当前），而 `q` 通常表示 "$temporary$"（临时）或 "$query$"（查询）。这种命名约定有助于理解和阅读代码，提高代码的可读性和可维护性。
>
> 当然，命名变量的方式可以因人而异，也可以选择其他的命名约定，只要能够清晰地表示变量的含义即可。重要的是保持一致性，并选择具有描述性的变量名，以便代码更易于理解和维护。

#### 单链表查找

按位查找时间复杂度为$O(n)$。

这样插入元素函数`InsertLinkListWithHead`只用`GetLinkListNode(list，i-1)`和`InsertNextLinkNode(p，elem)`两个函数完成。

+ 按位查找

     ![image-20230705114241679](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307051142737.png)

    + 王道书版本

        ![image-20230705114405330](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307051144384.png)

+ 按值查找

     ![image-20230705114603200](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307051146255.png)

#### 求单链表长度

求单链表长度时间复杂度为$O(n)$。

![image-20230705114743547](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307051147605.png)

#### 单链表建立

##### 尾插法建立单链表

+ 基本思想：使用**后插操作**从后面(**尾结点**)不断插入元素

    + 具体实现见上单链表插入处或见下
     ![image-20230705105327320](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307051053372.png)

+ 实现思路

    ![image-20240612190026733](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202406121900881.png)

    ![image-20240612190041635](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202406121900720.png)

+ 注意事项
    + 需要定义一个尾指针`r`来记录最后一位
    + 生成的链表中结点数据与输入数据顺序**一致**。
    + 总时间复杂度为$O(n)$。

##### 头插法建立单链表：

+ 基本思想:实际上也是使用**后插操作**，不过每一次后插的元素都是**头结点**，也不用使用尾指针

![image-20230705105327320](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307051053372.png)

+ 实现思路


![image-20230705121024656](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307051210713.png)
![image-20230705121211148](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307051212216.png)

+ 注意事项
    + 头插法可以实现链表的**逆置**，实现了输入数据的就地逆置。
        + 即头插法会导致生成的链表中结点数据与输入数据顺序**相反**。
    + 每个结点的插入时间为$O(1)$，设单链表长为$n$，则总时间复杂度为$O(n)$。

---

> 注:**实现对给定单链表的就地逆置**
>
> 1. 定义三个指针变量： `current` 、 `prev` 和 `next` ，分别表示当前节点、前一个节点和下一个节点。
>
>
> 2. 将 `current` 指针初始化为链表的第一个节点。
>
>
> 3. 遍历链表，重复以下步骤，直到 `current` 指针到达链表末尾（即 `current` 为空）：
>     - 将 `next` 指针指向 `current` 的下一个节点，以便在逆置后保留对下一个节点的引用。
>     - 将 `current` 的 `next` 指针指向 `prev` ，将 `current` 的指针方向逆转。
>     - 更新 `prev` 指针为 `current` ，以便在下一次迭代中将其作为前一个节点。
>     - 更新 `current` 指针为 `next` ，以便在下一次迭代中处理下一个节点。
>
> 4. 将链表的头指针指向 `prev` ，即完成了链表的就地逆置。
>
> 
>
> 下面是一个示例代码，展示了如何使用头插法实现单链表的就地逆置：
>
>
> ```cpp
> #include <iostream>
> 
> struct ListNode {
>     int data;
>     ListNode* next;
> };
> 
> void ReverseLinkedList(ListNode** head) {
>     if (*head == nullptr || (*head)->next == nullptr) {
>         return;  // 空链表或只有一个节点，无需逆置
>     }
> 
>     ListNode* current = *head;
>     ListNode* prev = nullptr;
>     ListNode* next = nullptr;
> 
>     while (current != nullptr) {
>         next = current->next;
>         current->next = prev;
>         prev = current;
>         current = next;
>     }
> 
>     *head = prev;  // 将链表的头指针指向逆置后的第一个节点
> }
> 
> void PrintLinkedList(ListNode* head) {
>     ListNode* current = head;
>     while (current != nullptr) {
>         std::cout << current->data << " ";
>         current = current->next;
>     }
>     std::cout << std::endl;
> }
> 
> int main() {
>     // 创建一个示例链表 1 -> 2 -> 3 -> 4 -> 5
>     ListNode* head = new ListNode{1， nullptr};
>     ListNode* second = new ListNode{2， nullptr};
>     ListNode* third = new ListNode{3， nullptr};
>     ListNode* fourth = new ListNode{4， nullptr};
>     ListNode* fifth = new ListNode{5， nullptr};
> 
>     head->next = second;
>     second->next = third;
>     third->next = fourth;
>     fourth->next = fifth;
> 
>     std::cout << "Original Linked List: ";
>     PrintLinkedList(head);
> 
>     ReverseLinkedList(&head);
> 
>     std::cout << "Reversed Linked List: ";
>     PrintLinkedList(head);
> 
>     return 0;
> }
> ```
> 输出结果：
>
>
> ```mathematica
> Original Linked List: 1 2 3 4 5
> Reversed Linked List: 5 4 3
> ```

## 双链表

为了解决单链表只能单一方向扫描而无法两项遍历的缺点，使用了两个指针，`prior`和`next`，分别指向前驱和后继。

![1689312566t-20230714-1326-583.87](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/1689312566t-20230714-1326-583.87.png)

### 双链表定义

基本上与单链表的定义一致。

![image-20230705225510118](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307052255185.png)

### 双链表操作

#### 双链表初始化

![image-20230705225549916](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307052255029.png)

#### 双链表插入

![1689312586t-20230714-1346-362.169](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/1689312586t-20230714-1346-362.169.png)

假如`p`的结点后要插入结点`s`，则基本代码如下：

![image-20230705225716653](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307052257711.png)

![image-20230705225821099](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307052258161.png)

操作上都是成对的，其中第①条第②条指令必须在第④条指令之前，否则`p`的后继就会丢掉。

#### 双链表删除

![1689312593t-20230714-1353-425.154](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/1689312593t-20230714-1353-425.154.png)

若删除结点`p`的后继结点`q`：

![image-20230705230117341](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307052301401.png)

删除整个双链表(销毁双链表)

![image-20230705230241731](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307052302784.png)



### 双链表遍历

双链表不可以随机存取

按位查找、按值查找操作都只能用遍历的方式实现

时间复杂度$O(n)$

![image-20230705230359003](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307052303063.png)

## 循环链表

分为循环单链表和循环双链表。基本上变化不大。

原来的单链表的尾部指向`NULL`，但是循环单链表的尾部是指向头部。

![1689312608t-20230714-1308-553.107](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/1689312608t-20230714-1308-553.107.png)

循环单链表即使没有头结点的地址，也可以通过循环得到整个单链表的信息。

从头到尾单链表需要遍历整个链表，而循环单链表只用移动一位就可以从头到尾。

---

![1689312621t-20230714-1321-613.141](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/1689312621t-20230714-1321-613.141.png)

循环双链表除此之外，头结点的`prior`指针还要指表尾结点（即某结点`*p`为尾结点时，`p->next==list`）。

循环双链表为空表时，头结点的`prior`和`next`域都等于`list`（即，指向自身）。

### 循环链表定义

循环链表和链表的结点定义是一致的。

![image-20230705230612315](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307052306369.png)



#### 循环单链表初始化

![image-20230705230634416](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307052306507.png)

#### 循环双链表初始化

+ 双链表：
    + 表头结点的`prior`指向`nullptr`
    + 表尾结点的`next`指向`nullptr`
     ![image-20230705230945021](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307052309077.png)
+ 循环双链表：
    + 表头结点的`prior`指向表尾结点；
    + 表尾结点的`next`指向头结点
     ![image-20230705230950149](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307052309211.png)

循环双链表初始化实现如下

![image-20230705231012466](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307052310524.png)

#### 循环双链表插入

![image-20230705231048171](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307052310227.png)

#### 循环双链表删除

![image-20230705231236732](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307052312818.png)

## 静态链表

+   静态链表借助==数组==来描述线性表的链式存储结构，结点也有数据域和指针域，这里的指针是结点的相对地址（数组下标），又称游标。
    +   静态链表内基本元素不是基本数据类型而是结构体类型
+   静态链表和顺序表一样需要预先分配一块**连续的内存空间**。
+   数组$0$号元素充当链表的头结点且不包含数据。
+   如果一个结点是尾结点，其游标设置为$-1$。
    +   具体的实现方式有多种，也可以$0$号元素数据保存头结点下标。

![image-20230705231707346](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307052317412.png)

![1689312710t-20230714-1350-488.215](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/1689312710t-20230714-1350-488.215.png)

### 静态链表特点

1. 增删改不需要移动大量数据元素。
2. 不能随机存取，只能从头结点开始。
3. 容量固定保持不变。
4. 由于使用了数组，需要分配连续的较大空间

适用场景：

1. 不支持指针的低级语言。

2. 数据元素数量**固定不变**，如操作系统文件分配表$FAT$。

---

> 注:**操作系统文件分配表$FAT$**
>
> 详见操作系统-文件管理-文件系统-文件物理结构-链接分配-显式分配
>
> ![image-20230626233424662](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202306262334718.png)
>
> + 操作系统把用于链接文件各物理块的指针显式地存放在一张表中.即文件分配表（$FAT$，$File\;Allocation\;Table$）
>     + $FAT$包含物理块号和下一块指针两项.
> + $FCB$目录中只需记录文件的起始块号.
> + 一个**磁盘**仅设置一张$FAT$.开机时，将$FAT$读入内存，并常驻内存.所以地址转换不需要读取内存，从而效率更高.
> + $FAT$的各个表项在物理上连续存储，且每一个表项长度相同，因此“物理块号”字段可以是隐含的.
> + 地址转换
>     + 目录项中找到起始块号，若$i>0$，则查询内存中的文件分配表$FAT$，往后找到$i$号逻辑块对应的物理块号
>     + 逻辑块号转换成物理块号的过程不需要**读磁盘**操作.
> + 优点：
>     + 很方便文件拓展.
>     + 不会有外部碎片问题，外存利用率高.
>     + 支持顺序访问也支持随机访问.
>         + 访问$i$号数据块时并不需要访问之前的$0\sim i-1$号数据块
>     + 相比于隐式链接来说，地址转换时不需要访问磁盘，因此文件的访问效率更高.
> + 缺点：文件分配表的需要占用一定的存储空间.

### 静态链表定义

![image-20230705231813612](https://cdn.jsdelivr.net/gh/WilliamTrouvaille/image_hosting@main/CS408_NOTE/202307052318655.png)

### 静态链表操作

#### 静态链表查找

需要从头结点往后**逐个**遍历结点，时间复杂度为$O(n)$。

#### 静态链表插入

如果要插入位序为`i`，索引为`i-1`的结点：

1. 找到一个空结点，存入数据元素。
    + 如何判断为空？可以先让`next`游标为某个特殊值如$-2$等
    + 可以使用`memset`函数
2. 然后从头结点出发扎到位序为`i-1`的结点。
3. 修改新结点的`next`。
4. 修改`i-1`位序结点的`next`。

---

> 注:**`memset`函数**
>
>  `memset` 是 C/C++ 标准库中的一个函数，用于将指定的内存区域设置为特定的值。它通常用于对数组、缓冲区或结构体等进行初始化或清零操作。
>
>  `memset` 函数的声明如下：
>
>
> ```cpp
> void* memset(void* ptr， int value， size_t num);
> ```
> 函数参数的含义如下：
>
> -  `ptr` ：指向要设置值的内存起始位置的指针。
> -  `value` ：要设置的值，作为 `int` 类型传递，但会被转换为 `unsigned char` 类型进行填充。
> -  `num` ：要设置的字节数，即要设置的内存区域的大小。
>
> 以下是使用 `memset` 的示例：
>
>
> ```cpp
> #include <iostream>
> #include <cstring>
> 
> int main() {
>     int arr[5];
> 
>     // 将整个数组设置为零
>     memset(arr， 0， sizeof(arr));
> 
>     // 输出数组元素
>     for (int i = 0; i < 5; ++i) {
>         std::cout << arr[i] << " ";
>     }
>     std::cout << std::endl;
> 
>     return 0;
> }
> ```
> 输出结果：
>
>
> ```
> 0 0 0 0 0
> ```
> 在上例中， `memset` 函数将整个数组 `arr` 的元素设置为零，使用了 `sizeof(arr)` 来获取数组的大小，确保了正确的字节数。注意， `memset` 函数的第一个参数是 `void*` 类型的指针，可以用于任何类型的内存区域。
>
> 需要注意的是， `memset` 通常用于处理原始的内存块，对于 C++ 对象或包含指针的结构体等复杂数据结构，使用 `memset` 可能会导致不正确的行为。对于 C++ 对象，推荐使用构造函数和赋值运算符来进行初始化。

## 顺序表与链表对比

+ 从**逻辑结构**来看
    + 都是**线性结构**的。
+ 从存储结构来看
    + 顺序表
        + 优点:可以**随机存取**，存储数据密度高
        + 缺点:大片连续空间分配不方便，改变容量不方便
    + 链表
        + 优点:空间离散，改变容量方便
        + 缺点:**不可随机存储**，存储数据密度低。
+ 从创建来看
    + 顺序表
        + 需要预分配大片连续空间
            + 若分配空间过小，则之后不方便拓展容量
            + 若分配空间过大，则浪费内存资源
    + 而链表
        + 只需分配一个头结点，之后方便拓展
        + 也可以不要头结点，只声明一个头指针
+ 从销毁来看
    + 顺序表
        + 需要将`length`设置为`0`，从逻辑上销毁，再从物理上销毁空间
        + 如果是静态分配的静态数组，系统会自动回收空间
        + 如果是动态数组，需要手动调用`free`函数
    + 链表
        + 依次删除每个结点
        + 使用`free`即可
+ 从增加删除操作来看
    + 顺序表都要对后续元素进行前移或后移
        + 时间复杂度为$O(n)$，主要来自于移动元素；
    + 而对于链表插入或删除元素只用修改指针就可以了
        + 时间复杂度也为$O(n)$，主要来自于查找目标元素
        + 链表的查找元素所花费的时间可能**远小于**移动元素的时间。
+ 从查找来看
    + 顺序表
        + 因为有顺序所以**按位查找**时间复杂度为$O(1)$
        + **按值查找**时间复杂度为$O(n)$
        + 如果值是有序的则可以通过二分查找等方式降低到在$O(\log_2n)$的时间内找到
    + 如果是链表的查找无论是按位还是按值都是$O(n)$的时间复杂度。

---

> + 表长难以预估、经常要增加/删哙元素一一链表
> + 表长可预估、查询（搜索）操作较多一一顺序表