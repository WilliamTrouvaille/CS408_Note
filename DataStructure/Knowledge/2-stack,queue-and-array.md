# 第三章 栈 、队列和数组

## 导读

### 【考纲内容】

1. 栈和队列的基本概念
2. 栈和队列的顺序存储结构
3. 栈和队列的链式存储结构
4. 多维数组的存储
5. 特殊矩阵的压缩存储
6. 栈、队列和数组的应用

### 【知识导图】



### 【复习提示】

+ 本章通常以选择题的形式考查，题目不算难，但命题的形式比较灵活
+ 其中栈（出入栈的过程、出栈序列的合法性)和队列的操作及其特征是重点
+ 由于它们均是线性表的应用和推广，因此也容易出现在算法设计题中
+ 此外，栈和队列的顺序存储、链式存储及其特点、双端队列的特点、栈和队列的常见应用，以及数组和特殊矩阵的压缩存储都是读者必须苹握的内容。

## 栈

+   栈结构与线性表类似，是只允许一端（表尾）进入或删除的==线性表==
    +   即后进先出$LIFO$。
+   栈顶就是允许插入和删除的一端，而另一端就是栈底。
+   进栈顺序：$A\rightarrow B\rightarrow C\rightarrow D$，出栈顺序：$D\rightarrow C\rightarrow B\rightarrow A$。
+   如果有$n$个不同的元素进栈，**合法的**出栈元素不同排列的个数为$\dfrac{1}{n+1}C_{2n}^n$，这就是卡特兰数。

### 顺序栈

#### 顺序栈定义

设置栈顶指针可以为$0$（代表栈顶元素的下一个存储单元）也可以为$-1$（代表栈顶元素当前未知）。

![GeePlayer_JTIfebBiOd](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307121928405.png)

栈满的条件：`top==MaxSize`

#### 顺序栈操作

![GeePlayer_A8AwrxyqwT](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307121939921.png)

##### 顺序栈初始化

+   栈顶指针初始化为$-1$，因为索引最小为$0$
    +   如果初始化为$0$也可以，不过其操作有所不同。



>   注:初值为$-1$时,栈顶指针指向当前元素
>
>   初值为$0$时,栈顶指针指的是当前元素的下一个位置

##### 进栈

![GeePlayer_15dNuYc5cH](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307121929248.png)

1.   首先要判满，然后才能进栈
2.   若栈顶指针指的是当前元素，即初值为$-1$，需要先自加再进栈
     +   如果不先自加就会覆盖在原来的栈顶元素上
3.   若栈顶指针指的是当前元素的下一个位置，即初值为$0$，则先进栈再自加
     +   因为指向的下一个位置，所以指向的位置是空的，所以可以存入然后自加，若先自加则中间就空了一格。

##### 出栈

![GeePlayer_QpxMvtMlNq](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307121934134.png)

1.   首先要判空，然后才能出栈
2.   若栈顶指针指的是当前元素，即初值为$-1$，需要先出栈再自减
     +   如果由于指的是当前元素所以要先将这个指向的元素弹出，然后自减，否则弹出的就是靠近栈底的下一个元素
3.   若栈顶指针指的是当前元素的下一个位置，即初值为$0$，则先自减再出栈
     +   因为指向的下一个位置，所以指向的位置是空的，要先自减指向有元素的一格才能出栈。

对于出栈元素的处理，既可以将原来的存储单元设置为`NULL`也可以不处理，因为栈顶指针不指向这些单元，用户是不知道里面是什么的，之后重新用到这些存储单元也会覆盖原来的数据。

#### 共享栈

即根据栈底不变，让两个顺序栈共享一个一维数组，将两个栈的栈底设在数组两端，栈顶向共享空间延申。

存取数据时间复杂度为$O(1)$。

![GeePlayer_jM1FgVc98Z](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307121940931.png)

栈满的条件：`top0+1==top1`

![GeePlayer_J5I1XUAv8W](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307121941663.png)

### 链栈

链栈基本上就是只能操作一头的链表，所以从定义上其基本上没有区别。基本上以表头为栈顶。

#### 链栈的定义

![GeePlayer_UkWQihlnkg](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/2023/07/12%3A1689162412%3AWjQdKrziL0tv93cX0cW6.png)

### 栈的应用

#### 括号匹配

即需要括号成双相对，且大小一样。

括号匹配时会发现最后出现的左括号最先被匹配$LIFO$。所以就可以通过栈来模拟这个匹配过程。

1.   自左至右扫描表达式，若遇左括号，则将左括号入栈
2.   若遇右括号，则将其与栈顶的左括号进行匹配
     1.   若配对，则栈顶的左括号出栈，否则出现括号不匹配错误
     2.   如果需要匹配但是栈空说明有单独的左或右括号，也匹配失败
     3.   如果结束，栈为空则正常结束，否则不匹配。

![1689167667t-20230712-2127-944.462](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689167667t-20230712-2127-944.462.png)

>   考试中可直接使用基本操作，需要使用注释简要说明接口

```cpp
#include <iostream>
#include <stack>
#include <string>

bool isMatchingPair(char opening, char closing) {
    return (opening == '(' && closing == ')') ||
           (opening == '[' && closing == ']') ||
           (opening == '{' && closing == '}');
}

bool isBalancedParentheses(const std::string& expression) {
    std::stack<char> parenthesesStack;

    for (char c : expression) {
        if (c == '(' || c == '[' || c == '{') {
            // 遇到左括号，将其入栈
            parenthesesStack.push(c);
        } else if (c == ')' || c == ']' || c == '}') {
            // 遇到右括号，检查栈顶元素是否匹配
            if (parenthesesStack.empty() || !isMatchingPair(parenthesesStack.top(), c)) {
                return false; // 括号不匹配
            }
            parenthesesStack.pop(); // 括号匹配，将栈顶元素出栈
        }
    }

    return parenthesesStack.empty(); // 括号全部匹配，栈应为空
}

int main() {
    std::string expression;

    std::cout << "Enter an expression with parentheses: ";
    std::getline(std::cin, expression);

    if (isBalancedParentheses(expression)) {
        std::cout << "Parentheses are balanced." << std::endl;
    } else {
        std::cout << "Parentheses are not balanced." << std::endl;
    }

    return 0;
}

```



#### 表达式求值

一般使用的都是中缀表达式，例如：$4+2\times3-10\div5$，按照运算法则，我们应当先算$2\times3$然后算$10\div5$ ，再算加法，最后算减法。

表达式分为三个部分：操作数、运算符、界限符。

如果不使用界限符，如中括号或小括号，可以使用后缀表达式（逆波兰表达式）或前缀表达式（波兰表达式）。

常用的中缀表达式是将运算符如加减乘除放在两个操作数中间，而后缀表达式就是放在两个操作数之后，前缀表达式就是放在两个操作数之前。

![1689168895t-20230712-2155-751.304](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689168895t-20230712-2155-751.304.png)

##### 后缀表达式

也叫逆波兰表达式

###### 中缀转后缀的手算方法：

1. 确定中缀表达式中各个运算符的运算顺序进行排序。
2. 选择下一个运算符，按照**左操作数 右操作数 运算符**的方式组合一个个新的操作数。
3. 如果还有运算符没有处理就重复步骤二。

如$A+B*(C-D)-E/F$就是$ABCD-*+EF/-$和$ABCD-*EF/-+$。中缀转后缀的结果可以有不同的结果。

可以发现**后缀表达式中运算符从左往右的先后顺序等于中缀表达式中的运算顺序**

![1689169225t-20230712-2125-425.228](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689169225t-20230712-2125-425.228.png)

即使有不同的转换结果，但是如果我们要通过计算机实现这种转换算法，就必须保证算法的唯一性，所以规定后缀表达式中运算符的顺序就是中缀表达式中运算符生效的顺序，即结果是第一个而不是第二个。

所以后缀表达式转换必须遵循左优先的原则，能先计算左边的运算符就计算左边的运算符。

###### 后缀表达式的计算(手算)

![1689169544t-20230712-2144-825.266](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689169544t-20230712-2144-825.266.png)

后缀表达式的手算方法：

+   从左往右扫描，每遇到一个**运算符**，就让运算符前面最近的两个操作数执行对应运算，合体为一个操作数
    +   先找运算符再找操作数

###### 后缀表达式计算的程序实现：

1. 从左往右扫描下一个元素，直到处理所有元素。
2. 若扫描到操作数则压入栈，并回到$1$，若扫描到运算符则执行$3$。
3. 扫描到运算符则弹出两个栈顶元素，执行相应操作，运算结果压入栈，回到步骤一。
    +   先出栈的是“右操作数”
    +   其实就是左边的数是左操作数,右边的数是右操作数
4. 先出栈的是右操作数，后出栈的是左操作数。

###### 后缀表达式转换的程序实现：

1. 初始化一个栈，用于==保存暂时不能确定运算顺序的运算符==。
2. 从左到右处理每个元素,可能遇到以下三种情况( • ̀ω•́ )✧
    1. 如果遇到==操作数==，直接加入后缀表达式。
    2. 如果遇到==界限符==，如果遇到左括号`'('`直接入栈，如果遇到右括号`')'`依次弹出栈内运算符并加入到后缀表达式中，直到遇到新的左括号为止。
        +   左括号`'('`不加入后缀表达式
    3. 遇到==运算符==，**依次弹出栈中优先级高于或等于当前运算符的所有运算符并加入后缀表达式**，若碰到左括号或栈空停止，之后再将当前运算符入栈。

3. 处理完所有字符后，将栈中剩余运算符依次弹出，并加入后缀运算符。

###### 使用栈进行中缀转后缀的手算方法：

1. 设定运算符栈。
2. 从左到右遍历中缀表达式的每个数字和运算符。
3. 若当前字符是数字，则直接输出成为后缀表达式的一部分。
4. 若当前字符为运算符，则判断其与栈顶运算符的优先级，若优先级大于栈顶运算符，则进栈；若优先级小于等于栈顶运算符，退出栈顶运算符成为后缀表达式的一部分，然后将当前运算符放入栈中。
5. 若当前字符为“(”，进栈。(不会影响其他非)符号的入栈出栈，在(前的符号不会因为(入栈而弹出，在)后的符号也不用判断是否弹出，直接入栈。
6. 若当前字符为“)”，则从栈顶起，依次将栈中运算符出栈成为后缀表达式的一部分，直到碰到“(”。将栈中“(”出栈，不需要成为后缀表达式的一部分，然后继续扫描表达式直到最终输出后缀表达式为止。

```cpp
#include <iostream>
#include <stack>
#include <string>
#include <unordered_map>

bool isOperator(char c) {
    return (c == '+' || c == '-' || c == '*' || c == '/');
}

int getPrecedence(char c) {
    static const std::unordered_map<char, int> precedence = {
        {'+', 1}, {'-', 1},
        {'*', 2}, {'/', 2}
    };

    auto it = precedence.find(c);
    if (it != precedence.end()) {
        //如果操作符存在于映射中，函数会返回对应的优先级值。
        return it->second;
    }
    //如果操作符不在映射中，函数会返回 -1。
    return -1;
}

std::string infixToPostfix(const std::string& infixExpression) {
    std::string postfixExpression;
    std::stack<char> operatorStack;

    for (char c : infixExpression) {
        if (std::isalnum(c)) {
            postfixExpression += c;  // 将操作数直接添加到后缀表达式中
        } else if (c == '(') {
            operatorStack.push(c);  // 遇到左括号，将其入栈
        } else if (c == ')') {
            // 遇到右括号，将栈中的操作符出栈，直到遇到左括号
            while (!operatorStack.empty() && operatorStack.top() != '(') {
                postfixExpression += operatorStack.top();
                operatorStack.pop();
            }
            operatorStack.pop();  // 将左括号出栈
        } else if (isOperator(c)) {
            // 遇到操作符，将栈中优先级大于或等于当前操作符的操作符出栈，再将当前操作符入栈
            while (!operatorStack.empty() && operatorStack.top() != '(' &&
                   getPrecedence(operatorStack.top()) >= getPrecedence(c)) {
                postfixExpression += operatorStack.top();
                operatorStack.pop();
            }
            operatorStack.push(c);
        }
    }

    // 将栈中剩余的操作符出栈
    while (!operatorStack.empty()) {
        postfixExpression += operatorStack.top();
        operatorStack.pop();
    }

    return postfixExpression;
}

int main() {
    std::string infixExpression;

    std::cout << "Enter an infix expression: ";
    std::getline(std::cin, infixExpression);

    std::string postfixExpression = infixToPostfix(infixExpression);

    std::cout << "Postfix expression: " << postfixExpression << std::endl;

    return 0;
}

```



###### 使用栈进行中缀表达式求值的程序实现：

1. 设定两个栈，一个用于存储运算符称之为==运算符栈==，另一个用于存储操作数称之为==操作数栈==。
2. 首先置操作数栈为空，表达式起始符“#”为运算符栈的栈底元素。
3. 依次读入表达式中每个字符
    1. 若是操作数则进==操作数栈==
    2. 若是运算符则和==运算符栈==栈顶元素比较优先级
        +   若栈顶元素优先级高于即将入栈的元素，则栈顶元素出栈（优先级高的先出栈，再把优先级低的放进来）
        +   操作数栈弹出两个操作数和运算符一起进行运算，将运算后的结果放入操作数栈，直至整个表达式求值完毕（即运算符栈顶元素和要放入元素均为“#”）。


```cpp
#include <iostream>
#include <stack>
#include <string>
#include <unordered_map>

bool isOperator(char c) {
    return (c == '+' || c == '-' || c == '*' || c == '/');
}

int getPrecedence(char c) {
    static const std::unordered_map<char, int> precedence = {
        {'+', 1}, {'-', 1},
        {'*', 2}, {'/', 2}
    };

    auto it = precedence.find(c);
    if (it != precedence.end()) {
        return it->second;
    }
    return -1;
}

int performOperation(int operand1, int operand2, char op) {
    switch (op) {
        case '+':
            return operand1 + operand2;
        case '-':
            return operand1 - operand2;
        case '*':
            return operand1 * operand2;
        case '/':
            if (operand2 != 0) {
                return operand1 / operand2;
            } else {
                throw std::runtime_error("Error: Division by zero");
            }
        default:
            throw std::runtime_error("Error: Invalid operator");
    }
}

int evaluateInfixExpression(const std::string& infixExpression) {
    std::stack<int> operandStack;//操作数栈 operand n.操作数
    std::stack<char> operatorStack;//运算符栈

    for (char c : infixExpression) {
        if (std::isspace(c)) {
            continue;  // 忽略空格
        } else if (std::isdigit(c)) {
            operandStack.push(c - '0');  // 将操作数入栈
        } else if (c == '(') {
            operatorStack.push(c);  // 遇到左括号，将其入栈
        } else if (c == ')') {
            // 遇到右括号，将栈中的操作符出栈，直到遇到左括号，并进行运算
            while (!operatorStack.empty() && operatorStack.top() != '(') {
                char op = operatorStack.top();
                operatorStack.pop();

                if (operandStack.size() < 2) {
                    throw std::runtime_error("Error: Invalid expression");
                }
                int operand2 = operandStack.top();
                operandStack.pop();
                int operand1 = operandStack.top();
                operandStack.pop();

                int result = performOperation(operand1, operand2, op);
                operandStack.push(result);
            }
            operatorStack.pop();  // 将左括号出栈
        } else if (isOperator(c)) {
            // 遇到操作符，将栈中优先级大于或等于当前操作符的操作符出栈，并进行运算
            while (!operatorStack.empty() && operatorStack.top() != '(' &&
                   getPrecedence(operatorStack.top()) >= getPrecedence(c)) {
                char op = operatorStack.top();
                operatorStack.pop();

                if (operandStack.size() < 2) {
                    throw std::runtime_error("Error: Invalid expression");
                }
                int operand2 = operandStack.top();
                operandStack.pop();
                int operand1 = operandStack.top();
                operandStack.pop();

                int result = performOperation(operand1, operand2, op);
                operandStack.push(result);
            }
            operatorStack.push(c);
        } else {
            throw std::runtime_error("Error: Invalid character");
        }
    }

    // 将栈中剩余的操作符出栈，并进行运算
    while (!operatorStack.empty()) {
        char op = operatorStack.top();
        operatorStack.pop();

        if (operandStack.size() < 2) {
            throw std::runtime_error("Error: Invalid expression");
        }
        int operand2 = operandStack.top();
        operandStack.pop();
        int operand1 = operandStack.top();
        operandStack.pop();

        int result = performOperation(operand1, operand2, op);
        operandStack.push(result);
    }

    if (operandStack.size() != 1) {
        throw std::runtime_error("Error: Invalid expression");
    }

    return operandStack.top();  // 返回最终结果
}

int main() {
    std::string infixExpression;

    std::cout << "Enter an infix expression: ";
    std::getline(std::cin, infixExpression);

    try {
        int result = evaluateInfixExpression(infixExpression);
        std::cout << "Result: " << result << std::endl;
    } catch (const std::exception& e) {
        std::cout << "Error: " << e.what() << std::endl;
    }

    return 0;
}

```



##### 前缀表达式

也叫波兰表达式

中缀转后缀的手算方法：

1. 确定中缀表达式中各个运算符的运算顺序进行排序。
2. 选择下一个运算符，按照**运算符 左操作数 右操作数**的方式组合一个个新的操作数。
3. 如果还有运算符没有处理就重复步骤二。

遵循右优先原则，只要能计算右边就优先计算右边。

前缀表达式计算的程序实现：

1. 从右往左扫描下一个元素，直到处理所有元素。
2. 若扫描到操作数则压入栈，并回到1，若扫描到运算符则执行3.
3. 扫描到运算符则弹出两个栈顶元素，执行相应操作，运算结果压入栈，回到步骤一。
4. 先出栈的是左操作数，后出栈的是右操作数。

#### 递归

+ 递归表达式（递归体）。
+ 边界条件（递归出口）。

函数调用的特点：最后被调用的函数最先执行结束$LIFO$。

函数调用时需要一个栈存储：

1.   调用返回地址
2.   实参
3.   局部变量

用栈来让递归算法转换为非递归算法。

+   每进入一层递归，就将递归调用所需信息压入栈顶
+   每退出一层递归，就从栈顶弹出相应信息

递归可以将原始问题拆分为属性相同、规模较小的问题。但是**如果太多层会造成栈溢出**。

#### 迷宫问题

+ 思想：以栈$S$记录当前路径，则栈顶中存放的是“当前路径上最后一个位置信息”。
+ 若当前位置“可通”，则纳入路径（入栈），继续前进。
+ 若当前位置“不可通”，则后退（出栈），换方向继续探索。
+ 若四周“均无通路”，则将当前位置从路径中删除出去。

## 队列

+   队列是只允许一端进行插入（入队或进队），另一端进行删除（出队或离队）的线性表。
    +   即先进先出$FIFO$。

+   队列允许插入的一端就是队尾，允许删除的一端就是队头。

+   队列和栈是逻辑结构和物理结构没有不同，只是操作方式不同。

### 顺序队列

![20230712-2055-732462](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/20230712-2055-732462.png)

分配一块连续的存储单元存放队列中的元素，并附设两个指针，队头指针和队尾指针，队头指针指向队头元素，队尾指针指向队尾元素的下一个位置。

![GeePlayer_UTTN0GIOKl](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307121953368.png)

>   $rear$   		$n.$后面；后方部队；屁股$adj.$后方的，后面的；背面的
>
>   $front$		$n.$前面；正面：前线$adj.$前面的；正面的

#### 顺序队列操作

##### 顺序队列插入

![20230712-2045-384196](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307122000378.bmp)

如果出队，则前面的空间会空闲，但是假如队尾指针会依照插入而不断加$1$，则我们的队尾指针最后会指向最后一个区域，计算机不知道前面是怎么样，所以就认为空间已经满了，实际上没有。这就是假溢出。

为了防止假溢出,可以使用取模运算使队尾指针超过了范围也仍能回到最开始插入数据,详细见下循环队列

![20230712-2001-490196](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307122004139.bmp)

##### 顺序队列删除

当如果我们必须保证所有的存储空间被利用，可以定义一个$size$表明队列当前的长度，就可以完全利用所有空间。

### 循环队列

对于顺序队列假溢出的解决的方法就是使用模运算，将队尾指针不仅仅是加一，而是加一后再取整个静态数组大小$MAXSIZE$的模，这样如果队列尾指针超过了范围也仍能回到最开始插入数据。这时候物理结构虽然是线性的，而逻辑上已经变成了环型的了。

所以与此同时，队列已满的条件也不再是队尾指针$=MAXSIZE$了，而是队尾指针的下一个指针是队头指针，这里最后一个存储单元是不能存储数据的，因为如果存储了数据那么头指针就等于尾指针，这在我们的定义中是空队列的意思。但是如何判断满队？

1. 牺牲最后的一个存储单元。入队少用一个队列单元
    +   队满条件`(rear+1)%MAXSIZE==front`
    +   队空条件`front==rear`
    +   队列元素个数`(rear+MAXSIZE-front)%MAXSIZE`
2. 增设一个用来表示数据个数的成员`length`
    +   队满条件`length==MAXSIZE`
    +   队空条件`length==0`
    +   队空和队满都是`front==rear`
3. 可以定义一个$int$类型的$tag$，当进行删除操作就置$tag$为$0$，插入操作时置$tag$为$1$，只有删除才可能队空，只有插入才可能队满，所以就可以根据这个来判断
    +   队空和队满都是`front==rear`

#### 循环队列入队

![20230712-2001-490196](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202307122004139.bmp)

#### 循环队列出队

![20230712-2003-465244](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/20230712-2003-465244.png)

### 链队

![20230712-2042-550237](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/20230712-2042-550237.png)

定义链队需要定义一个队头指针和一个队尾指针，队头指向队头结点，队尾指向队尾结点，即单链表最后一个结点，这跟顺序存储不同。

由于带头节点的链表对于出队比较简单，所以一般都定义为带头节点的链表。

#### 链队的初始化

![1689165033t-20230712-2033-593.162](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689165033t-20230712-2033-593.162.png)

#### 链队的入队

带头结点的队列,第一个元素入队和后来元素入队处理逻辑一致

刚开始,`rear`尾指针和`front`头指针都指向头结点

![1689165105t-20230712-2045-594.229](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689165105t-20230712-2045-594.229.png)

不带头结点的队列，第一个元素入队时需要特别处理

刚开始,`rear`尾指针和`front`头指针都指向`NULL`

![1689165377t-20230712-2017-617.366](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689165377t-20230712-2017-617.366.png)

#### 链队的出队

带头结点的队列

![1689165867t-20230712-2027-524.284](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689165867t-20230712-2027-524.284.png)

不带头结点的队列

![1689165918t-20230712-2018-563.365](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689165918t-20230712-2018-563.365.png)

#### 链队的判满

链式存储一一一般不会队满，除非内存不足

### 双端队列

双端队列：只允许从两端插入、两端删除的线性表。

输入受限的双端队列：只允许从一端插入，两端删除的线性表。

输入受限的双端队列：只允许从一端删除，两端插入的线性表。

![1689166158t-20230712-2018-661.429](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689166158t-20230712-2018-661.429.png)

### 队列应用

#### 树的层次遍历

1. 根结点入队。
2. 若队空（所有结点都已处理完毕)，则结束遍历，否则重复三操作。
3. 队列中第一个结点出队，并访问之。若其有左孩子，则将左孩子入队；若其有右孩子，则将右孩子入队，返回二。

#### 图的广度优先遍历

图的广度优先遍历（$Breadth-First Search$，简称$BFS$）是一种用于遍历图的算法。它从图的一个起始节点开始，逐层遍历图中的节点，先访问起始节点的邻居节点，然后再访问邻居节点的邻居节点，依次类推，直到遍历完图中所有可达节点。

#### 计算机系统

+   进程争用$FCFS$策略。

+ 解决$CPU$与外设速度不匹配问题。
+ 解决请求处理机问题。

## 数组

### 数组的定义

数组是由同类型的数据元素构成的有序集合，每个元素是数组元素，每个元素受$n$个线性关系的约束。其中每个元素在$n$个线性关系中的序号就是元素的下标，可以通过下标来访问元素。

数组是对线性表的推广。

数组一旦被定义其维数和维界就不能改变，所以数组只能对结构的初始化和销毁，以及元素的存取和修改。

所以数组的重点在于其存储。

### 数组的存储结构

#### 一维数组

+   各数组元素大小相同，且物理上连续存放。
+   数组元素$a[i]$的存放地址$=LOC+i\times sizeof(ElemType)$。数组下标从$0$开始。
    +   起始地址`LOC`

#### 二维数组

二维数组存储方式还是同一维数组一样连续的。已知二维数组$b[M][N]$。数组下标从$0$开始。

+ 行优先：一行一行存储。$b[i][j]$的存储地址$=LOC+(i\times N+j)\times sizeof(ElemType)$
+ 列优先：一列一列存储。$b[i][j]$的存储地址$=LOC+(j\times M+i)\times sizeof(ElemType)$

#### 十字链表法

每个结点中包含行数、列数、元素值，以及两个指针，向下域指针$down$指向同第$j$列的下一个个元素，向右域指针$right$指向同第$i$行的下一个元素。

### 特殊数组的压缩

压缩存储指为多个值相同的元素只分配一个存储空间从而节省存储空间。

特殊矩阵是指具有许多相同矩阵元素或零元素，并且这些相同矩阵元素或零元素的分布有一定规律的矩阵。

若是索引都从$0$开始，则公式不发生改变，用$i+1$和$j+1$替换公式的$i$和$j$。。

#### 对称矩阵

若对一个$n$阶方阵$A[1,n][1,n]$中的任意一个元素$a_{ij}$都有$a_{ij}=a_{ji}$，即主对角线对称元素相等的矩阵，就是对称矩阵。

其中元素可以分为上三角区域、主对角线和下三角区域，上下三角区域元素相等。

![1689171965t-20230712-2205-594.422](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689171965t-20230712-2205-594.422.png)

所以可以将$A$存放在一维数组$B[n(n+1)\div2]$中，从而$a_{ij}=b_k$，只存放下三角部分与主对角线部分元素。

对于$a_{ij}$而言，其在$i$行$j$列，第一行有$1$个元素，第二行有$2$个元素，所以第$i$行有$i=j$个元素，所以前$i$行共有$\dfrac{(1+n)\times n}{2}$个元素

从而$a_{ij}$对应数组大小$k_{max}=\dfrac{(1+n)\times n}{2}$

+ 当$i\geqslant j$时，即下三角区域与主对角线元素：$k = \dfrac{j \times (j-1)}{2} + (i - 1)$
+ 当$i<j$时，即上三角区域：$k = \dfrac{i \times (i-1)}{2} + (j - 1)$

>   不需要找存储地址而是做算法题时存储矩阵时一般采用从1开始存储的方式,方便按照行列号找到元素存储值

#### 三角矩阵

##### 下三角矩阵

![1689173090t-20230712-2250-469.385](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689173090t-20230712-2250-469.385.png)

下三角矩阵指上三角区域的元素均为同一常量，其存储思想与对称矩阵一样，但是需要最后多一个存储空间存储上三角的常量。所以可以将$A$存放在一维数组$B[n(n+1)\div2+1]$中。

+ 当$i\geqslant j$时，即下三角区域与主对角线元素：$k=\dfrac{i(i+1)}{2}+j$。
+ 当$i<j$时，即上三角区域：$k=\dfrac{n(n+1)}{2}$（最后一位）。

##### 上三角矩阵

![1689173110t-20230712-2210-489.381](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689173110t-20230712-2210-489.381.png)

上三角矩阵指下三角区域的元素均为同一常量，其存储思想与下三角矩阵一样，不过下标不同。

位于元素$a_{ij}\,(i\leqslant j)$前的元素个数有：第$0$行有$n$个元素，第$1$行有$n-1$个元素，第$i-1$行有$n-i+1$个元素，第$i$行有$j-i$个元素。

从而$a_{ij}$对应的$k=n+(n-1)+\cdots+(n-i+1)+(j-i)=\dfrac{i(2n-i+1)}{2}+(j-i)$。

+ 当$i\leqslant j$时，即上三角区域与主对角线元素：$k=\dfrac{i(2n-i+1)}{2}+(j-i)$。
+ 当$i>j$时，即下三角区域：$k=\dfrac{n(n+1)}{2}$。

##### 三对角矩阵

![1689173078t-20230712-2238-476.416](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689173078t-20230712-2238-476.416.png)

对角矩阵也称为带状矩阵。对于$n$阶方阵$A$的任意元素$a_{ij}$，当$\vert i-j\vert>1$时，有$a_{ij}=0$，则是三对角矩阵。

三对角矩阵除了以主对角线为中心的三条对角线的区域上的元素并不是完全为零外，其他元素都是零。

所以可以将三条对角线上的元素行优先地存储在一维数组中。第$0$行是两个元素，而$1$到$i-1$一共$i-1-1+1=i-1$行每行三个元素，所以前$i-1$行一共$2+(i-1)\times3$个元素，第$i$一共$j-i+1$个元素。即$k=2+(i-1)\times3+j-i+1$，所以得到$k=2i+j$。

##### 稀疏矩阵

+   若矩阵中非零元素很少，这个矩阵就是稀疏矩阵
+   三元组法:
    +   若使用普通一维数组存储则十分浪费空间，所以一般只存储非零元素。
    +   三元组(行标,列标,值)来存储
    +   ![1689173209t-20230712-2249-777.397](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689173209t-20230712-2249-777.397.png)

+   十字链表法
    +   ![1689173245t-20230712-2225-905.454](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689173245t-20230712-2225-905.454.png)
    +   ![1689173260t-20230712-2240-276.104](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/1689173260t-20230712-2240-276.104.png)

**稀疏矩阵压缩后就失去了随机存取的特性**