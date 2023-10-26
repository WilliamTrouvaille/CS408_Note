# EX-算法总结

## 数据结构及其操作总结

### 数组

#### 将一个数组前后翻转

![d6eefad9161c404ba2b7a72f8f2b7f4c.png](https://ucc.alicdn.com/pic/developer-ecology/e5679e21020d4d459d68227732124238.png)

```cpp
bool Delete_Min(int A[], n, &min) {
    if (!n) return false;    //数组长度为0，返回false
    int temp = INT_MAX, m;    //INT_MAX为int类型的最大值
    for (int i = 0; i < n; i++) {    //遍历数组，找到数组当前的最小元素
        if (A[i] < temp) {
            temp = A[i];    //更新数组最小值
            m = i;    //记录数组下标
        }
    }
    min = temp;    //min保存最小值
    A[m] = A[n - 1];    //m用数组中最后一个元素替换
    return true;
}
```

#### 删除数组中值为x的元素

![5d0005dde27f442cb56bea3cfae7c26a.png](https://ucc.alicdn.com/pic/developer-ecology/3955e7b5b3374b7190cd1deec5d21e71.png)

```cpp
int DeleteX(int A[], x, n){
    int i = 0, j = 0;
    for (i = 0; i < n; i++) {
        if (A[i] != x) {    //当前元素的值不为x
            A[j] = A[i];    //将其保存到数组下标为j的元素中
            j++;
        }
    }
    n = j;    
    return n;    //返回删除x后的数组元素个数
}
```

#### 将两个有序数组合并成一个有序数组

![9b3df95c179542c994fc309f9783494e.png](https://ucc.alicdn.com/pic/developer-ecology/29f915168e7c42d8872298bc0c9406df.png)

```cpp
int* Merge(int A[], B[], lenA, lenB) {
    int *C = (int*)malloc((lenA + lenB) * sizeof(int));
    int a = 0, b = 0, c = 0;
    for (c = 0; a < lenA && b < lenB; c++) {    //选择两个数组中的较小值放入数组C中
        if (A[a] <= B[b]) C[c] = A[a++];
        else C[c] = B[b++];
    }
    while (a < lenA) C[c++] = A[a++];    //将剩余数组放入C中
    while (b < lenB) C[c++] = B[b++];
    return C;
}
```

#### 真题

![bd99ceb964a943a8b2cf8fb35e95363e.png](https://ucc.alicdn.com/pic/developer-ecology/f1f29528f97e49629b521850a2496551.png)

```cpp
void ans(int A[], n, p){
    int B[n], i, j;
    for (i = 0, j = p; j < n; i++, j++) B[i] = A[j];    //数组后部分前移
    for (j = 0; j < p; i++, j++) B[i] = A[j];    //数组前部分后移
    for (i = 0; i < n; i++) cout << B[i];    //输出循环前移后的数组
}
```

![63e9f7fd08b448d08b08ae0714bc92d1.png](https://ucc.alicdn.com/pic/developer-ecology/1d77b89b86034d13b1620d4e35120e3e.png)

```cpp
int res(int A[], int B[], int n){
    int i, j, k, mid;
    for (i = 0, j = 0, k = 0; k < n; k++){
        if (A[i] <= B[j]) {    //当前A数组的元素小，保存A[i]
            mid = A[i];
            i++;    
        } else {    //当前B数组的元素小，保存B[j]
            mid = B[j];
            j++;
        }
    }
    return mid;
}
void Qsort(int A[], L, R){
    if (L >= R) return;    //当前数组区间<= 1，返回
    随机选择数组中一个元素和A[L]交换    //快速排序优化，使得基准元素的选取随机
    int key = A[L], i = L, j = R;    //选择A[L]作为基准元素，i和j分别为左右指针
    while(i < j) {
        while (i < j && key < A[j]) j--;
        while (i < j && A[i] <= key) i++;
        if (i < j) swap(A[i], A[j]);    //交换A[i]和A[j]
    }
    swap(A[i], A[L]);
    Qsort(A, L, i - 1);    //递归排序左区间
    Qsort(A, i + 1, R);    //递归排序右区间
}
 
int ans(int A[], int B[], int n) {
    int C[2n], i, j;
    for (i = 0; i < n; i++) C[i] = A[i];    //复制数组A和数组B的元素
    for (j = 0; j < n; i++, j++) C[i] = B[j];
    Qsort(C, 0, 2n - 1);    //对数组C进行快速排序
    return C[n - 1];    //返回中位数
}
```

![e24f9027d944449f9f267da69c9a9428.png](https://ucc.alicdn.com/pic/developer-ecology/63f78de6f77b4127bef2f5d7eadbf54c.png)

```cpp
int ans(int A[], n){
    int count[n];
    for (int i = 0; i < n; i++) count[i] = 0;    //初始化count数组
    //遍历A数组，其元素的值作为count数组下标的元素+1，表示该元素在A数组中出现次数
    for (int i = 0; i < n; i++) count[A[i]]++;   
    for (int i = 0; i < n; i++) {    //当前元素出现次数符合主元素定义
        if (count[i] > n / 2) return i;    //返回i，即该元素的值
    }
    return -1;    //没有元素符合主元素定义
}
```

![9fd06f5530b2430da410e14815da0fdd.png](https://ucc.alicdn.com/pic/developer-ecology/53d2687772284ef7b135b2610b6665cc.png)

```cpp
int ans(int A[], n) {
    bool B[n + 2];    //B用来标记数组中出现的正整数
    for (int i = 0; i < n; i++) B[i] = false;    //初始化B数组
    for (int i = 0; i < n; i++) {
        if (A[i] > 0) B[A[i]] = true;    //该元素大于0，则在B数组中标记为已经出现
    }
    for (int i = 1; i < n; i++) {
        if (B[i]  false) return i;    //返回数组B中第一个false的元素下标
    }
}
void Qsort(int A[], L, R) {
    if (L >= R) return; //数组区间<= 1，返回
    随机选择数组中一元素和A[L]交换;//快排优化，使得基准元素的选取随机
    int key = A[L], i = L, j = R;   //A[L]为基准元素，ij为左右指针
    while (i < j) {
        while (i < j && key < A[j]) j--;
        while (i < j && A[i] <= key) i++;
        if (i < j) Swap(A[i], A[j]);    //交换A[i]和A[j]
    }
    Swap(A[L], A[i]);
    Qsort(A, L, i - 1); //递归排序左区间
    Qsort(A, i + 1, R); //递归排序右区间
}
 
int ans(int A[], n) {
    Qsort(A, 0, n - 1); //快速排序
    int i = 0;
    while (A[i] <= 0) i++;  //找到数组中第一个大于0的元素
    if (n  i) return 1;   //数组中没有元素大于0，返回1
    else {
        if (A[i] != 1) return 1;    //第一个整数不是1，则返回1
        else {    //第一个整数为1，找到数组中正整数第一个间断点
            int j = i + 1;
            while (j < n) {
                if (a[j]  a[j - 1]) j++;    //相邻元素相等
                else if (a[j]  a[j - 1] + 1) j++;    //相邻元素是连续数
                else return A[j - 1] + 1;    //相邻元素是间断点
            }//while
        }//else
    }//else
}
```

![6ebfa00824c04488a0e1538e123a20f8.png](https://ucc.alicdn.com/pic/developer-ecology/d78bbb3d6e5a475da13757ccc2d0224c.png)

```cpp
int Dis(int a, b, c){
    int res = abs(a - b) + abs(a - c) + abs(b - c);    //计算绝对值
    return res;
}
 
int Ans(int A[], int B[], int C[], int n1, int n2, int n3){
    int min = INT_MAX, i, j, k, temp;    //min取整型的最大值
    for (int i = 0; i < n1; i++) {    //循环遍历数组A
        for (int j = 0; j < n2; j++) {    //循环遍历数组B
            for (int k = 0; k < n3; k++) {    //循环遍历数组C
                temp = Dis(A[i], B[j], C[k]);
                if (temp < min) min = temp;    //当前元素之间的距离更小，更新最小距离
            }//for
        }//for
    }//for
    return min;    //返回最小距离
}
```

### 链表

#### 链表的数据结构定义

##### 单链表

```cpp
typedef struct LNode {
    struct LNode *next;
    int data;
}LNode, *LinkList;
```

##### 双链表

```cpp
typedef struct LNode {
    struct LNode *prior, *next;
    int data;
}LNode, *LinkList;
```

#### 链表的操作

##### 头插法（插入到链表头）

```cpp
void HeadInsert(LinkList &L, int key) {
    LNode *p = (LNode*)malloc(sizeof(LNode));
    p->data = key;
    p->next = L->next;
    L->next = q;
}
```

##### 尾插法（插入到链表尾）

```cpp
void TailInsert(LinkList &L, int key) {
    LNode *q = (LNode*)malloc(sizeof(LNode);
    q->data = key;
    q->next = NULL;
    LNode *p = L->next, *pre = L;
    while (!p) {
        pre = p;
        p = p->next;
    }
    pre->next = q;
}
```

##### 链表逆置（头插法实现）

```cpp
void Reverse(LinkList &L) {
    LNode *p = L->next, *q = NULL;
    L->next = NULL;    //将L表断开
    while (!p) {
        q = p->next;    //q指向p的下一个结点
        p->next = L->next;    //头插法
        L->next = p;
        p = q;
    }
}
```

##### 链表的遍历

```cpp
LNode *p = L->next;
while (!p) {
    visit(p);
    p = p->next;
}
```

##### 链表的删除

```cpp
void Delete(LinkList &L, int &key) {
    LNode *p = L->next, *pre = L;
    移动p和pre到指定结点    //pre指向p的前驱结点
    key = p->data;    //key保存p的data领
    pre->next = p->next;    //pre的next指针指向p的后继节点
    free(p);
}
```

#### 链表算法题

##### 删除值为x的结点

```cpp
void DeleteX(LinkList &L, int x){
    LNode *p = L->next, *pre = L;
    while (p) {
        if (p->data  x) {    //当前元素值为x
            pre->next = p->next;
            free(p);
            p = pre->next;
        } else {    //当前元素值非x，p和pre向后移动
            p = p->next;
            pre = pre->next;
        }
    }
}
```

##### 单链表就地逆置

![99e7afe97aa04a699c22f929a0b6f198.png](https://ucc.alicdn.com/pic/developer-ecology/cf008a3081864eed851320ddd03c840b.png)

```cpp
void reverse(LinkList &L){
    LNode *p = L->next, *q;
    L->next = NULL;    //断链
    while (p) {
        q = p->next;    //q指向p的下一个结点
        p->next = L->next;    //头插法
        L->next = p;
        p = q;
    }
}
```

##### 将链表排序

![83649d0dd1504382b5f534f74b959ff0.png](https://ucc.alicdn.com/pic/developer-ecology/0ad80b8289804ac0896e0f83064c76ff.png)

```cpp
LNode *Sort(LinkList L) {
    LNode* p = (LNode*)malloc(sizeof(Lnode));
    p->next = NULL;
    LNode* t = L->next, * tpre = L, *min, *minpre, *r = p;
    int m = INT_MAX;
    while (t) {
        while (t) {    //遍历链表
            if (t->data < m) {  //更新最小值结点
                min = t;
                minpre = tpre;
                m = t->data;
            }//if
            tpre = t;
            t = t->next;
        }//while
        minpre->next = min->next;   //将min从L中删除
        r->next = min;  //将min插入p
        r = min;    //r后移
        m = INT_MAX;    //重新初始化
        t = L->next;
        tpre = L;
    }//while
    r->next = NULL;
    return p;
}
```

##### 拆分链表

![87bccb5146f44a919002d0af76a41f96.png](https://ucc.alicdn.com/pic/developer-ecology/e90ff6bfebfb4654a0024ed2e615fc0b.png)

```cpp
LNode* (LinkList &L) {
    LNode *p = (LNode*)malloc(sizeof(LNode);
    p->next = NULL;    //p为新链的头结点
    LNode *q = L->next, *t = NULL, *r = L;
    r->next = NULL;    //r结点始终指向L的最后一个结点
    while (q) {
        t = q->next;
        r->next = q;    //奇数结点尾插法
        r = q;
        q = t;
        t = q->next;
        q->next = p->next;    //偶数节点头插法
        p->next = q;
        q = t;
    }
    r->next = NULL;    //将r的next指针置空
    return p;
}
```

##### 删除链表中的重复元素

![897f91ea488e437280ebd1fddfc4c064.png](https://ucc.alicdn.com/pic/developer-ecology/c7447168a1df4a3ca4568dc3dc3b9890.png)

```cpp
void Delete(LinkList &L) {
    LNode *p = L->next;
    while (p) {
        LNode *post = p->next;    //post指向p的下一个结点
        while (post && post->data  p->data) {    //post存在并且值和p相等时
            LNode *temp = post;    //temp指向post
            post = post->next;    //post向后移动
            p->next = post;    //将p的下一个结点修改为post
            free(temp);
        }
        p = p->next;
    }
}
```

##### 将两个递增链表合并成一个递减链表

![31bcee4271af4b03b808c33a52267356.png](https://ucc.alicdn.com/pic/developer-ecology/285714f360c9427cb0aecad9257ab59d.png)

```cpp
void Merge(LinkList &L1, LinkList L2) {
    LNode *p = L1->next, *q = L2->next, *temp;
    L1->next = NULL;    //L1断链
    while (p && q) {
        if (p->data <= q->data) {    //当前p指向的元素更小
            temp = p->next;    //temp指向p的下一个结点
            p->next = L1->next;    //将p用头插法插入L1
            L1->next = p;
            p = temp;    //p指向temp
        } else {    //当前q指向的元素更小
            temp = q->next;
            q->next = L1->next;
            L1->next = q;
            q = temp;
        }
    }//while
    while (p) {    //将剩余节点插入L1
        temp = p->next;
        p->next = L1->next;
        L1->next = p;
        p = temp;
    }
    while (q) {
        temp = q->next;
        q->next = L1->next;
        L1->next = q;
        q = temp;
    }
    return;
}
```

##### 将两个递增链表合并为一个递增链表

```cpp
LNode *Merge(LinkList L1, LinkList L2) {
    LNode *p = L1->next, *q = L2->next, *r, *temp;
    LNode *L = (LNode*)malloc(sizeof(LNode));
    L->next = NULL;
    r = L;
    while (p && q) {
        if (p->data <= q->data) {    //当前p指向的结点小于等于q
            temp = p->next;
            r->next = p;    //p尾插法插入L中
            r = p;
            p = temp;
        } else {
            temp = q->next;
            r->next = q;
            r = q;
            q = temp;
        }
    }
    while (p) {    //插入剩余结点
        temp = p->next;
        r->next = p;
        r = p;
        p = temp;
    }
    while (q) {
        temp = q->next;
        r->next = q;
        r = q;
        q = temp;
    }
    r->next = NULL;    //将r的next指针置空
    return L;
}
```

##### 判断链表是否对称

![68d0f4f27e6d4417a2c3776c06645b4f.png](https://ucc.alicdn.com/pic/developer-ecology/0dd249a58f8a4f669dd53e007a2381ce.png)

```cpp
bool ans(LinkList L) {
    LNode* post = L->prior, * pre = L->next;    //前后指针
    //表中元素为奇数时，终止条件为两者移动到同一结点
    //表中元素为偶数时，终止条件为两者后指针的next指向前指针
    while (post != pre && post->next != pre) {    
        if (post->data != pre->data) return false;    //前后指针的指针域不相等
        pre = pre->next;    //前指针前移
        post = post->prior;    //后指针后移
    }
    //表对称
    return true;
}
bool ans(LinkList L) {
    LNode* p = L->next;
    int len = 0;    //记录表中的元素个数
    while (p != L) {
        p = p->next;
        len++;
    }
    int a = (int*)malloc(len * sizeof(int));    //定义跟链表结点个数相等的长度的数组
    len = 0;
    p = L->next
    while (p != L) {    //遍历链表，用数组保存链表中每个结点的值
        a[len] = p->next;
        p = p->next;
    }
    //遍历数组，前后指针指向元素的值不相等，返回false
    for (int i = 0, j = len - 1; i < j; i++, j--) {
        if (a[i] != a[j]) return false;
    }
    return true;
}
```

##### 依次输出链表中结点值最小的元素

![da5a120bd343499eb0f1dd383d35b0a1.png](https://ucc.alicdn.com/pic/developer-ecology/cfef721ffa074dbf896ef02ca1c10b0e.png)

```cpp
void DeleteMin(LinkList &L) {
    LNode *p = L->next, *ppre = L->next, *min, *minpre;
    while (L->next != L) {
        p = L->next;
        ppre = L;
        int tempMin = INT_MAX;    //当前最小值
        while (p != L) {
            if (p->data < tempMin) {    //当前结点值更小，更新最小结点
                min = p;
                minpre = ppre;
            }    //p向后移动
            ppre = p;
            p = p->next;
        }
        cout << min->data;    //输出最小结点的值
        minpre->next = min->next;    //删除min结点
        free(min);
    }//while
    free(L);    //删除头结点
}
```

#### 真题

![a822bd0933d74051bf109284bedcdd31.png](https://ucc.alicdn.com/pic/developer-ecology/002ad8290b744e4d892d20daa0589d5d.png)

```cpp
void ans(LinkList L, int k){
    LNode *p = L->link, *q = L->link;
    for (int i = 0; i < k; i++) p = p->link;    //p指针向后移动k个结点
    while (p) {
        p = p->link;
        q = q->link;
    }
    cout << q->data;
}
```

![0d293bb78b334ecf8ee9dfdc7392f538.png](https://ucc.alicdn.com/pic/developer-ecology/09252317536646d0b524d602b6648445.png)

```cpp
void ans(LinkList str1, LinkList str2) {
    LNode *p = str1->next, *q = str2->next;
    int len1 = 0, len2 = 0;
    while (p) {    //遍历str1，得到str1的长度
        len1++;
        p = p->next;
    }
    while (q) {    //遍历str2，得到str2的长度
        len2++;
        q = q->next;
    }
    int len = abs(len1 - len2);    //得到两表长度之差
    p = str1->next;    //重置pq指向第一个结点
    q = str2->next;
    if (len1 >= len2) {    //长表向前移动，使得两表剩余元素相等
        for (int i = 0; i < len; i++) p = p->next;
    }
    else {
        for (int i = 0; i < len; i++) q = q->next;
    }
    while (p) {    //遍历剩余结点，找到两者指向的第一个共同结点
        if (p  q) return p;
        p = p->next;
        q = q->next;
    }
    return NULL;    //两者没有共同后缀
}
```

![c0f0efeb281542c5863afd80a37ddea1.png](https://ucc.alicdn.com/pic/developer-ecology/f114e51e74ec41bfaba85208e8a06d54.png)

```cpp
void ans(LinkList &L){
    bool A[n + 1];    //长度为n + 1的数组，用来标记该数是否出现过
    for (int i = 0; i < n + 1; i++) A[i] = false;    //初始化A数组
    LNode *p = head->next, *pre = head;
    while (p) {
        int t = abs(p->data);    //取当前结点值的绝对值
        if (A[t]) {    //该值出现过，删除该结点
            LNode *r = p->next;
            pre->next = r;
            free(p);
            p = r;
        } else {    //该值没有出现过，在数组A中标记该值，p和pre向后移动
            A[t] = true;
            pre = p;
            p = p->next;
        }
    }//while
}
```

![6e9952902c0a498ab760546c23af4047.png](https://ucc.alicdn.com/pic/developer-ecology/a441f435d41547eca3cb564a48fc2cc8.png)

```cpp
void ans(NODE *L) {
    NODE* p = L->next, *f = L->next, *s = L->next, *q, *t;
    while (f->next->next && f->next) {  //找到前半链的最后一个结点
        f = f->next->next;    //快指针移动两个结点
        s = s->next;    //慢指针移动一个结点
    }
    q = s->next;    //q指向后半链的第一个结点
    s->next = NULL; //前半链后半链断开
    LNode* post = (NODE*)malloc(sizeof(NODE));
    post->next = NULL;
    while (q) { //后半链逆置
        t = q->next;
        q->next = post->next;
        post->next = q;
        q = t;
    }
    q = post->next; //q指向逆置后的后半链的第一个结点
    while (q) {
        r = q->next;    //r指向后半链的下一个结点
        t = p->next;    //t指向前半链下一个插入位置
        q->next = p->next;
        p->next = q;
        q = r;  //重置pq
        p = t;
    }
}
```

### 栈

#### 栈的数据结构定义

##### 顺序栈

```cpp
#define MAXSIZE 100
 
typedef struct Stack {
    int data[MAXSIZE];
    int top = -1;
}Stack;
```

##### 链栈

```cpp
typedef struct LStack {
    int data;
    struct LStack *next;
}SNode, *LStack;
```

#### 顺序栈的操作

##### 栈的初始化

```cpp
void InitStack (Stack &S) {
    S.top = -1;
}
```

##### 入栈

```cpp
bool Push(Stack &S, int key) {
    if (S.top  MAXSIZE - 1) return false;    //栈满
    S.data[++top] = key;
    return true;
}
```

##### 出栈

```cpp
bool Pop (Stack &S, int &key) {
    if (S.top  -1) return false;    //栈空
    key = S.data[top--];
    return true;
}
```

##### 判断栈空

```cpp
bool IsEmpty (Stack S) {
    if (S.top  -1) return true;
    else return false;
}
```

#### 链栈的基本操作

##### 初始化

```cpp
void InitStack (LStack &S) {
    SNode *s = (SNode*)malloc(Sizeof(SNode));
    S->next = NULL;
}
```

##### 入栈

```cpp
void Push (LStack &S, int key) {
    SNode *p = (SNode*)malloc(sizeof(SNode));
    p->data = key;
    p->next = S->next;    //头插法
    S->next = p;
}
```

##### 出栈

```cpp
bool Pop (LStack &S, int &key) {
    if (S->next  NULL) return false;    //栈空
    SNode *p = S->next;
    key = p->data;    //key保存栈顶元素的值
    S->next = p->next;
    free(p);
}
```

##### 判断栈空

```cpp
bool IsEmpty(LStack &S) {
    if (S->next  NULL) return true;
    else return false;
}
```

### 队列

#### 队列的数据结构定义

##### 顺序队列

```cpp
#define MAXSIZE 100
typedef struct Queue {
    int data[MAXSIZE];
    int front, rear;
}Queue;
```

##### 链式队列

```cpp
typedef struct LNode{
    struct LNode *next;
    int data;
}LNode;
 
typedef struct Queue{
    LNode *front, *rear;
}Queue;
```

#### 顺序队列的基本操作

##### 初始化

```cpp
void InitQueue(Queue &Q){
    Q.front = Q.rear = 0;
}
```

##### 入队

```cpp
bool EnQueue(Queue &Q, int key){
    if (Q.front  (Q.rear + 1) % MAXSIZE) return false;    //队满
    Q.data[rear] = key;
    Q.rear = (Q.rear + 1) % MAXSIZE;
    return true;
}
```

##### 出队

```cpp
bool DeQueue(Queue &Q, int &key){
    if (Q.rear  Q.front) return false;    //队空
    key = Q.front;
    Q.front = (Q.front + 1) % MAXSIZE;
    return true;
}
```

##### 判断队空

```cpp
bool IsEmpty(Queue Q){
    if (Q.front  Q.rear) return true;
    else return false;
}
```

#### 链式队列的基本操作

##### 初始化

```cpp
void InitQueue(Queue &Q){
    Q.front = Q.rear = (LNode*)maloc(sizeof(LNode));
    Q.front->next = NULL;
}
```

##### 入队

```cpp
void Queue(Queue &Q, int key){
    LNode *p = (LNode*)malloc(sizeof(LNode));    //申明一个新结点
    p->data = key;
    p->next = NULL;
    Q.rear->next = p;    //尾插法插入到rear后
    Q.rear = p;    //更新rear
}
```

##### 出队

```cpp
bool DeQueue(Queue &Q, int &key){
    if (Q.front  Q.rear) return false;    //队空
    LNode *p = Q.front->next;
    key = p->data;    //保存队首元素的数据
    Q.front->next = p->next;
    if (Q.rear  p) Q.rear = Q.front;    //队列中只有一个元素
    free(p);
    return true;
}
```

##### 判断队空

```cpp
bool IsEmpty(Queue Q){
    if (Q.rear  Q.front) return true;
    else return false;
}
```

### 树

#### 树的数据结构定义

##### 二叉树的链式存储

```cpp
typedef struct BiTree{
    sturct BiTree *lchild, *rchild;    //左右孩子指针
    int value;    //结点数据
}BiTNode, *BiTree;
```

##### 二叉树的顺序存储

```cpp
#define MAXSIZE 100
 
typedef struct TreeNode{
    int value;    //结点数据
    bool IsEmpty;    //该结点是否存在
}TreeNode;
 
void InitTree(TreeNode T[], int len){
    for (int i = 0; i < len; i++) T[i].IsEmpty = true;    //将该结点初始化为空结点
}
 
 
int main(){
    TreeNode T[MAXSIZE];    //申明一个长度为MAXSIZE的TreeNode数组
    InitTree(T);    //初始化树
    ...
}
```

##### 双亲表示法

```cpp
#define MAXSIZE 100    //树中最多结点数
 
typedef struct TreeNode{
    int data;    //结点数据
    int parent;    //该结点的双亲结点在数组的下标
}TreeNode;
 
typedef struct Tree{
    TreeNode T[MAXSIZE];    //长度为MAXSIZE的TreeNode类型的数组
    int treeNum;    //结点数
}Tree;
```

##### 孩子表示法

```cpp
#define MAXSIZE 100
 
//孩子结点
typedef struct Child{
    int index;    //该结点的编号
    struct Child *next;    //指向该结点的下一个孩子结点的指针
}Child;
 
//结点信息
typedef struct TreeNode{
    Child *firstTNode;    //指向该结点的第一个孩子结点的指针
    int data;    //该结点数据
}TreeNode;
 
TreeNode T[MAXSIZE];    //定义一个长度为MAXSIZE的树
```

##### 孩子兄弟表示法

```cpp
#define MAXSIZE 100
 
typedef struct CSNode{
    struct CSNode *firstChild, *nextSibling;    //指向第一个孩子和右兄弟节点
    int data;    //该结点数据
}CSNode;
```

##### 线索二叉树

```cpp
typedef struct ThreadNode{
    struct ThreadNode *lchild, *rchild;    //左右孩子指针
    int ltag, rtag;    //左右线索标志
    int data;    //结点数据    
}ThreadNode, *ThreadTree;
```

#### 二叉树的基本操作

##### 先序遍历

```cpp
void PreOrder(BiTree T){
    if (T) {
        visit(T);
        PreOrder(T->lchild);
        PreOrder(T->rchild);
    }
}
```

##### 中序遍历

```cpp
void InOrder(BiTree T){
    if (T) {
        InOrder(T->lchild);
        visit(T);
        InOrder(T->rchild);
    }
}
```

##### 后序遍历

```cpp
void PostOrder(BiTree T){
    if (T) {
        PostOrder(T->lchild);
        PostOrder(T->rchild);
        visit(T);
    }
}
typedef struct Stack{
    BiTNode *Node[MAXSIZE];
    int top;
}Stack;
 
void PostOrder(BiTree T){
    Stack S;
    InitStack(S);
    BiTNode *p, *pre;
    while (p || !IsEmpty(S)）{
        if (p) {    //往左下走到尽头
            Push(p);    //p入栈
            p = p->lchild;    //进入其左子树
        } else {
            GetTop(S, p);    //查看栈顶元素
            //栈顶元素的右孩子存在，并且不是上一个访问的结点
            if (p->rchild && p->rchild != pre) {
                p = p->rchild;    //进入栈顶元素的右子树
                Push(p);    //该结点入栈
                p = p->lchild;    //进入该结点左子树
            } else {    //栈顶元素的右孩子被访问过
                Pop(S, p);    //弹出栈顶元素
                visit(p);    //访问该结点
                pre = p;    //用pre标记该结点
                p = NULL;    //将p置空
            }//if
                
        }//if
    }//whil
}
```

##### 层次遍历

```cpp
void LevelOrder(BiTree T){
    InitQueue(Q);
    EnQueue(Q, T);
    BiTNode *p;
    while (!IsEmpty(Q)) {
        DeQueue(Q, p);
        visit(p);
        if (p->lchild) EnQueue(Q, p->lchild);
        if (p->rchild) EnQueue(Q, p->rchild);
    }
}
```

#### 二叉树算法题

##### 计算二叉树中双分支结点的个数

![cafb376f570b4fada2460af133fee7fa.png](https://ucc.alicdn.com/pic/developer-ecology/fe58cbb22be1451089e0d4f421fb4f0f.png)

```cpp
int count = 0;    //双分支结点个数
 
void PreOrder(BiTree T){
    if (T) {
        //当前结点的左右孩子都存在，count++
        if (T->lchild && T->rchild) count++;
        if (T->lchild) PreOrder(T->lchild);    //递归遍历左子树
        if (T->rchild) Preorder(T->rchild);    //递归遍历右子树
}
 
void ans(BiTree T) {
    PreOrder(T);    //先序遍历该树
    cout << count;    //输出双分支结点个数
}
```

##### 交换二叉树中所有左右子树

![a64f890e722b45f78fe588f23ecca770.png](https://ucc.alicdn.com/pic/developer-ecology/29f484a107f9425d931ee48d8c82403a.png)

```cpp
void PostOrder(BiTree &T){
    if (T) {
        PostOrder(T->lchild);
        PostOrder(T->rchild);
        BiTNode *t = T->lchild;
        T->lchild = T->rchild;
        T->rchild = t;
    }
}
 
void ans(BiTree &T){
    Post(Order(T));
    return;
}
```

##### 求先序遍历第k个元素

![6805b416eefa4dc9a7aa4ea3b053c7c2.png](https://ucc.alicdn.com/pic/developer-ecology/a56365e020874255960f2fc2851f6694.png)

```cpp
int t = 0;
int res = 0;
 
void PreOrder(BiTree T) {
    if (T) {
        t--;
        if (t  0) res = T->data;    //第k个结点，用res保存当前结点的值
        PreOrder(T->lchild);    //递归访问左子树
        PreOrder(T->rchild);    //递归访问右子树
    }
}
 
void ans(BiTree T, int k) {
    t = k;
    PreOrder(T);
    cout << res;    //输出第k个结点的值
}
```

##### 删去值为x的子树

![54aaa4bce6424c299e487ebbaa87ae16.png](https://ucc.alicdn.com/pic/developer-ecology/6e7c68c24258467982ad6912c3813bf8.png)

```cpp
int k;
 
void Delete(BiTree &T){    //后序遍历的方式删除结点
    if (T) {
        DeleteX(T->lchild);
        DeleteX(T->rchild);
        free(T);
    }
}
 
void PreOrder(BiTree &T) {
    if (T) {
        BiTNode *t;
        if (T->lchild->data  k) {    //左子树的值为x，删除左子树
            t = T->lchild;
            T->lchild = NULL;
            Delete(t);
        }
        if (T->rchild->data  k) {    //右子树的值为x，删除右子树
            t = t->rchild;
            T->rchild = NULL;
            Delete(t);
        }
        if (T->lchild) PreOrder(T->lchild);    //递归遍历左子树
        if (T->rchild) PreOrder(T->rchild);    //递归遍历右子树
    }//if
} 
 
 
void ans(BiTree &T, int x){
    k = x;
    if (T->data  x) {    //根节点的值为x，删除整个树并返回
        Delete(T);
        return;
    } else PreOrder(T);    //先序遍历x
}
void Delete(BiTree &T) {    //后序遍历，并删除结点
    if (T) {
        Delete(T->lchild);
        Delete(T->rchild);
        free(T);
    }
}
 
void LevelOrder(BiTree &T, int x){
    if (T->data  x) {    //根节点的值为x，删除整个树，并返回
        Delete(T);
        return;
    }
    Queue Q;
    InitQueue(Q);    //初始化队列
    BiTNode *p = T;
    EnQueue(Q, p);    //根节点入队
    while (!IsEmpty(Q)) {
        DeQueue(Q, p);
        if (p->lchild) {
            if (p->lchild->data  x) {
                BiTNode *q = p->lchild;
                p->lchild = NULL;    //左孩子指针置空
                Delete(q);    //以q为根节点的子树
            } else EnQueue(Q, p);
        }
        if (p->rchild) { {}
            if (p->rchild->data  x) {
                BiTNode *q = p->rchild;
                p->rchild = NULL;
                Delete(q);
            } else EnQueue(Q, p);
        }
    }//while
}
```

##### 查找二叉树中两个结点的公共祖先结点

![59da31125bff423a875a1b8ce85b8673.png](https://ucc.alicdn.com/pic/developer-ecology/299b6200ef5a448286679ea62db7933e.png)

```cpp
BiTNode *ans(BiTree ROOT, BiTNode *p, BiTNode *q) {
    Stack S, Sp, Sq;    //Sp和Sq分别用来保存p和q的祖先结点
    S.top = -1; //初始化队列
    BiTNode* t = ROOT, *pre = NULL;
    while (t || S.top >= 0) {
        if (t) {    //t结点非空
            S.data[++S.top] = t;    //t结点入队
            t = t->lchild;  //进入t的左子树
        }
        else {  //t结点为空
            t = S.data[S.top];  //查看栈顶元素
            //t的右子树存在，并且上一个访问的并不是其右子树
            if (t->rchild && t->rchild != pre) {    
                t = t->rchild;  //进入右子树
                S.data[++S.top] = t;    //入栈该结点
                t = t->rchild;  //进入左子树
            }
            else {  //右子树不存在，或者存在但是上一个访问的是右子树
                S.top--;    //出栈该结点，并访问
                if (t  p) {   //当前结点为p，保存栈中内容，即其所有祖先结点
                    for (int i = 0; i < S.top; i++) Sp.data[i] = S.data[i];
                    Sp.top = S.top;
                }
                if (t  q) {   //当前结点为q，保存栈中内容，即其所有祖先结点
                    for (int i = 0; i < S.top; i++) Sq.data[i] = S.data[i];
                    Sq.top = S.top;
                }
            }//if
        }//if
    }//while
    //自栈顶到栈顶分别遍历Sp和Sq找到最接近栈顶的相同祖先结点
    for (int i = Sp.top; i >= 0; i--) { 
        for (int j = Sq.top; i >= 0; j--) {
            if (Sp.data[i]  Sq.data[j]) return Sp.data[i];
        }
    }
    return NULL;    //无相同祖先顶点
}
```

##### 求二叉树的宽度![ccb1ae7ce764475ea764c119315612e4.png](https://ucc.alicdn.com/pic/developer-ecology/4a51f8d8033948fcb5e2bf0eea257214.png)

```cpp
typedef struct Queue{
    BiTNode *data[MAXSIZE];    //足够大的数组
    int front, rear;
}Queue;
 
int ans(BiTree T){
    if (!T) return 0;    //空树，返回0
    BiTNode *p = T;
    Queue Q;
    InitQueue(Q);    //初始化队列
    EnQueue(Q, p);    //将p入队
    //rear指向当前层的最后一个结点，count记录当前层的结点数，max记录最大结点数
    int last = Q.rear, count = 0, max = INT_MIN;    
    while (!IsEmpty(Q) {
        DeQueue(Q, p);
        count++;    //结点数+1
        if (p->lchild) EnQueue(Q, p->lchild);    //p的左子树存在，左子树入队
        if (p->rchild) EnQueue(Q, p->rchild);    //p的右孩子存在，右孩子入队
        if (last  Q.front) {    //当前结点是本层的最后一个节点
            last = Q.rear;    //last指向下一层的最后一个结点
            if (count > max) max = temp;    //更新最大结点数
            count = 0;    //结点数归零
        }
    }//while
    return max;
}
typedef struct Queue {  //足够大的非循环数组
    BiTNode *data[MAXSIZE]; //结点数组，保存每个结点
    int level[MAXSIZE]; //层数数组，记录每个结点的层数
    int front, rear;    //头尾指针
}Queue;
 
int ans(BiTree T) {
    BiTNode* p = T;
    Queue Q;
    Q.rear = Q.front = 0;   //初始化队列
    if (T) {
        Q.data[Q.rear] = T; //根节点入队
        Q.level[Q.rear] = 1;
        Q.rear++;
        while (Q.front < Q.rear) {
            p = Q.data[Q.front];    //出队队首元素
            int level = Q.level[Q.front];   //保存当前结点的层数
            Q.front++;
            if (p->lchild) {    //左孩子入队
                Q.data[Q.rear] = p->lchild;
                Q.level[Q.rear] = level + 1;
                Q.rear++;
            }
            if (p->rchild) {    //右孩子入队
                Q.data[Q.rear] = p->rchild;
                Q.level[Q.rear] = level + 1;
                Q.rear++;
            }
        }//while
        int max = INT_MIN, i = 0, level = 1;
        while (i <= Q.rear) {
            int count = 0;  //记录当前层的结点数
            while (i <= Q.rear && level  Q.level[i]) {
                count++;
                i++;
            }
            if (count > max) max = count;   //更新每层的最大结点数
            level = Q.level[i]; //更新层数，while循环结束时，i指向下一层的第一个结点
        }//while
        return max; //返回最大结点数
    }
    else return 0;  //空树，返回0
}
```

##### 求二叉树的高度

```cpp
int Get_Heigh(BiTree T) {
    int front = 0, rear = 0;    //前后指针
    BiTNode* p = T;
    BiTNode *data[MAXSIZE]; //足够大的队列，元素是二叉树结点
    data[rear++] = p;   //根节点入队
    int last = rear, level = 0; //rear标记本层最后一个结点, level记录当前层数
    while (front < rear) {  //循环直到队空
        p = data[front++];  //出队队首结点
        if (p->lchild) data[Q.rear++] = p->lchild;  //左右孩子入队
        if (p->rchild) data[Q.rear++] = p->rchild;
        if (last  front) {    //当前结点为本层的最后一个结点
            last = rear;    //标记下层的最后一个结点
            level++;    //层数+1
        }
    }//while
    return level;
}
int Get_High(BiTree T){
    if (!T) return 0;    //空树返回0
    else {
        int hl = Get_High(T->lchild);    //递归求左右子树高度
        int hr = Get_High(T->rchild);
        int maxH = max(hl, hr) + 1;    //树高等于左右子树更高的那个+1
        return maxH;
    }
}
```

##### 排序二叉树的判定

```cpp
int pre = INT_MIN;  //标记上一个结点的值，初始值为INT类型的最小值
 
int JudgeBST(BiTree T) {
    if (!T) return 1;   //空树，是排序二叉树
    else {
        int l = JudgeBST(T->lchild);    //判断左子树
        //当前值小于等于pre的值，或左子树不满足排序二叉树定义，返回0
        if (T->data <= pre|| l  0) return 0;
        pre = T->data;  //更新pre为当前结点值
        int r = JudgeBST(T->rchild);    //判断右子树
        return r;
    }
}
int A[n];   //足够大的数组，保存每个节点的值
int count = 0;  //记录结点个数
 
void InOrder(BiTree T) {
    if (T) {
        InOrder(T->lchild); 
        A[count++] = T->data;   //记录当前结点值，并且count+1
        InOrder(T->rchild);
    }
}
 
bool ans(BiTree T) {
    if (!T) return true;    //空树为排序二叉树
    else if (!T->lchild && !T->rchild) return true; //只有根节点，是排序二叉树
    else {
        InOrder(T); //中序遍历二叉树，并且记录每个结点的值
        for (int i = 0; i < count - 1; i++) {
            if (A[i] >= A[i + 1]) return false; //非排序二叉树
        }
        return true;    //排序二叉树
    }
}
```



##### 平衡二叉树的判定

```cpp
int Get_Height(BiTree T) {
    if (!T) return 0;
    else {
        int hl = Get_Height(T->lchild); //递归求左子树高度
        int hr = Get_Height(T->rchild); //递归求右子树高度
        int maxH = max(hl, hr) + 1; //树高为左右子树更高的那个 + 1
        return maxH;    
    }
}
 
bool JudgeBalance(BiTree T) {
    if (!T) return true;    //空树为平衡二叉树
    else {
        int hl = Get_Height(T->lchild); //左子树高度
        int hr = Get_Height(T->rchild); //右子树高度
        //当前结点的左右平衡因子小于等于1，递归判断其左右子树是否满足平衡二叉树
        if (abs(hl - hr) <= 1) {
            return JudgeBalance(T->lchild) && JudgeBalance(T->rchild);
        }
        //当前节点左右平衡因子大于1，则已不满足平衡二叉树，无需判断左右子树，返回false
        else return false;
    }
}
```

##### 完全二叉树的判定

```cpp
bool JudgeComplete(BiTree T) {
    BiTNode* data[MAXSIZE]; //足够大的队列
    int front = 0, rear = 0;    //头尾指针
    BiTNode* p = T; 
    data[rear++] = T;   //根节点入队
    while (front < rear) {  //循环直到队空
        p = data[front++];  //出队队首元素
        if (p) {    //p结点存在，入队左右孩子
            data[rear++] = p->lchild;
            data[rear++] = p->rchild;
        }
        else {  //p结点不存在，出队剩余元素
            while (front < rear) {
                p = data[front++];
                if p  NULL return false;  //当前元素为空，则为非完全二叉树
            }
        }
    }
    return true;
}
```

#### 真题

![6f2f5d0118df435db4b92a0872d5d5ce.png](https://ucc.alicdn.com/pic/developer-ecology/71468bc79184427c92cee88049035c67.png)

```cpp
int WPL = 0;
 
void InOrder(BiTree T, int deep){
    if (T) {
        if (!T->left && !T->right) {    //叶子结点
            WPL = WPL + T.weight * deep;    //更新WPL
        }
        if (T->left) InOrder(T->left, deep + 1);
        if (T->right) InOrder(T->right, deep + 1);
    }
}
 
int ans(BiTree T){
    InOrder(T, 0);
    return WPL;
}
int Get_WPL(BiTree root) {
    BiTNode* data[MAXSIZE]; //足够大的非循环数组
    BiTNode* p = root;
    int f = 0, r = 0, level = 0, WPL = 0, last = 0; 
    data[r++] = p;  //根节点入队
    last = r;   //last标记本层的最后一个元素
    while (f < r) {
        p = data[f++];  //队首元素出队
        //该结点为叶子结点，计算WPL
        if (!p->lchild && !p->rchild) WPL = WPL + level * p->weight;
        if (p->lchild) data[r++] = p->left; //左右孩子入队
        if (p->rchild) data[r++] = p->right;
        if (last  f) {    //该结点为本层的最后一个结点
            last = r;   //更新last和level
            level++;
        }
    }
    return WPL;
}
```

![f184da9060ed4f348f5a5207d442c7f6.png](https://ucc.alicdn.com/pic/developer-ecology/bd565a18a78a4fc590c28f96d81cd669.png)

```cpp
void InOrder(BiTree T, int deep){
    if (T) {
        if (deep > 1 && (T->lchild || T->rchild)) cout << '(';    //分支节点打印左括号
        if (T->lchild) InOrder(T->lchild, deep + 1);
        cout << T->data;
        if (T->rchild) InOrder(T->rchild, deep + 1);
        if (deep > 1 && (T->lchild || T->rchild)) cout << ')';    //分支节点打印右括号
    }
}
 
void ans(BiTree T){
    InOrder(T, 1);
}
```

### 图

#### 图的数据结构定义

##### 邻接矩阵

```cpp
#define MAXVEX 100
typedef struct Graph {
    int data[MAXVEX];    //一维数组，存放顶点数据
    int edge[MAXVEX][MAXVEX];    //二维数组，存放边数据（权值）
    int vexNum, edgeNum;    //顶点总数和边总数
}Graph;
```

##### 邻接表

```cpp
#define MAXVEX 100
typedef struct edgeNode {    //边
    struct edgeNode *next;    //指向下一条邻接边的指针
    int weight;    //该邻接边权值
    int adjVex;    //该邻接边指向的顶点编号
}edgeNode;
 
typedef struct vexNode {    //顶点
    edgeNode *firstEdge;    ///指向该顶点的第一条邻接边
    int vexData;    //该顶点数据
}vexNode;
 
typedef struct Graph {    //图
    int vexNum, edgeNum;    //顶点数，边数
    vexNode vex[MAXVEX];    //vexNode类型的一维数组vex
}Graph;
```

#### 图的遍历

##### 深度优先遍历

```cpp
#define MAXVEX 100
bool visited[MAXVEX];    //visited数组记录该顶点是否被访问过
 
void DFSTraverse (Graph G) {
    for (int i = 0; i < G.vexNum; i++) {
        visited[i] = false;    //初始化visited数组
    }
    for (int i = 0; i < G.vexNum; i++) {
        if (!visited[i]) DFS (G, i);    //当前顶点未被访问过，则访问
    }
}
 
void DFS (Graph G, int v) {
    visit (v);    //访问顶点v（具体操作）
    visited[v] = true;    //更新visited数组
    for (int w = FirstNeighbor (G, v); w >= 0; w = NextNeighbor (G, v, w)){
        if (!visited[w]) DFS(G, w);    //递归调用DFS
    }
}
```

##### 广度优先遍历

```cpp
#define MAXVEX 100
Queue Q;
bool visited[MAXVEX];
 
void BFSTraverse (Graph G) {
    for (int i = 0; i < G.vexNum; i++) {    //初始化visited数组
        visited[i] = false;    
    }
    InitQueue Q;    //初始化队列Q
    for (int i = 0; i < G.vexNum; i++) {    //遍历图
        if (!visited[i]) BFS(G, i);
    }
}
 
void BFS (Graph G, int v) {
    visit(v);    //访问该顶点（具体操作）
    visited[v] = true;    //更新visited数组
    EnQueue(Q, v);    //将v结点入队
    int w;
    while(!IsEmpty(Q)) {
        DeQueue(Q, v);    //队首元素出队
        for (w = FirstNeighbor(G, v); w >= 0; w = NextNeighbor(G, v, w)) {
            if (!visited[w]) {    //顶点未被访问过
                visit(w);
                visited[w] = true;
                EnQueue(Q, w);
            }
        }//for
    }//while
}
```

##### 单源最短路径

```cpp
#define MAXVEX 100
bool visited[MAXVEX];
int dis[MAXVEX];
Queue Q;
 
void Min_Dis (Graph G, int v) {
    for (int i = 0; i < G.vexNum; i++) {    //初始化visited数组和dis数组
        visited[i] = false;
        dis[i] = INT_MAX;
    }
    visited[v] = true;
    dis[v] = 0;
    InitQueue(Q);
    EnQueue(Q, v);
    int w;
    while (!IsEmpty(Q)) {
        DeQueue(Q, v);
        for (w = FisrtNeighbor(G, v); w >= 0; w = NextNeighbor(G, v, w) {
            if (!visited[w]) {
                visited[w] = true;
                dis[w] = dis[v] + 1;
            }
        }//for
    }//while
}
```

#### 真题

![fa4a011572674952bb92acaee9c519c5.png](https://ucc.alicdn.com/pic/developer-ecology/35c416525ffc43958b0a9e00f4359ae2.png)

```cpp
int IsExistEL(MGraph G){
    int count = 0;    //记录该图中度为奇数的顶点个数
    int i, j;
    for (i = 0; i < G.numVertices; i++){    //行遍历邻接矩阵
        int degree = 0;
        for (j = 0; j < G.numVertices; j++){    //列遍历当前行
            if (Edge[i][j] > 0) degree++;    //当前数组元素不为0，则度+1
        }
        if (degree % 2) count++;    //当前顶点的度为奇数，count++
    }
    if (count  0 || count  2) return 1;    //奇数顶点个数为0或者2，有EL路径
    else return 0;    //奇数顶点个数不为0或者2，没有EL路径
}
```

### 排序

#### 快速排序

![image-20231007193737798](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202310071937159.png)

#### 归并排序

![image-20231007193752924](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202310071937365.png)

### 查找

#### 折半查找

![image-20231007193809273](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202310071938561.png)

