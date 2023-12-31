# EX-第四章 指令系统总结

## 导读

### 【考纲内容】

1. 指令格式的基本概念
2. 指令格式
3. 寻址方式
4. 数据的对齐和大/小端存放方式
5. $CISC$和$RISC$的基本概念
6. 高级语言程序与机器级代码之间的对应
    + 编译器、汇编器与链接器的基本概念
    + 选择结构语句的机器级表示
    + 循环结构语名的机器级表示
    + 过程(函数)调用对应的机器级表示

### 【复习提示】

选择题：各种寻址方式的特点及有效地址的计算，相对寻址的计算，$CISC$与$RISC$的特点与区别

综合题：机器级表示、指令格式、机器指令和指令寻址方式、相对寻址的计算

## 考点总结

### 指令概述

1.   **指令**（又称机器指令）：是指示计算机执行某种操作的命令，是计算机运行的最小功能单位。
2.   **指令系统/指令集**：指令系统指的是计算机执行的机器指令的集合。指令系统是**指令集体系结构**$ISA$中最核心的部分。指令系统是计算机软硬件的界面。
3.   ※**指令集体系结构**$ISA$规定的内容主要包括：**指令格式，数据类型及格式，操作数的存放方式，程序可访问的寄存器个数、位数和编号，存储空间的大小和编址方式，寻址方式，指令执行过程的控制方式**等。$ISA$完整定义了软件和硬件之间的接口，是机器语言或汇编语言程序员所应熟悉的。
4.   指令的基本格式：一条指令通常要包括操作码字段$OP$和地址码字段$A$两部分。其中，指令地址由程序计数器$PC$给出。
5.   指令的长度：一条指令中所包含的二进制代码的位数。指令字长取决于操作码的长度、操作数地址码的长度和操作数地址的个数。与机器字长没有固定的关系。
     1.   定长指令字结构：在一个指令系统中，若所有指令的长度都是相等的，则称为定长指令字结构。指令字长=存储字长。
     2.   变长指令字结构：按字节的倍数变化，指令长度随指令功能而异。

6.   指令类型：根据指令中操作数地址码的数目的不同，可将指令分成以下几种格式。
     1.   零地址指令：只有操作码，不需要操作数。如**空操作、停机、关中断**等。零地址的运算类指令仅用在堆栈计算机中。通常参与运算的两个操作数隐含地从栈顶和次栈顶弹出，送到运算器进行运算，运算结果再隐含地压入堆栈。
     2.   一地址指令（单地址指令）：操作码$+$操作数地址$A_1$。
          1.   只需要一个操作数的指令操作，如加一、减一、取反、求补等：$OP(A_1)\rightarrow A_1$。只需要三次访存（取出指令、取出数、放回）。
          2.   隐含约定的目的地址的双操作数指令，如目的地址为累加器$ACC$的地址：$(ACC)OP(A_1)\rightarrow ACC$。只需要两次访存（取指令和取数）。
     3.   二地址指令：操作码$+$操作数$A_1$地址$+$操作数$A_2$地址。
          1.   将结果存到操作数$A_1$地址或操作数$A_2$地址中：$(A_1)OP(A_2)\rightarrow A_1$。一共访存四次（取指令，取数$A_1$和$A_2$，存放到$A_1$或$A_2$）。
          2.   若使用累加器$ACC$暂存结果则只用访存三次（取指令，取数$A_1$和$A_2$，暂存到$ACC$）。$(A_1)OP(A_2)\rightarrow ACC$。
     4.   三地址指令：操作码$+$操作数$A_1$地址$+$操作数$A_2$地址$+$结果地址$A_3$。
          +   $A_1$与$A_2$运算，结果存到$A_3$中：$(A_1)OP(A_2)\rightarrow A_3$。一共访存四次（取指令，取数$A_1$和$A_2$，存放到$A_3$）。
     5.   四地址指令：操作码$+$操作数$A_1$地址$+$操作数$A_2$地址$+$结果地址$A_3$$+$下指令地址$A_4$。$(A_1)OP(A_2)\rightarrow A_3$，$A_4=$下一条将要执行指令的地址
7.   定长操作码：指令字的最高位部分分配固定的若干位表示操作码。若有$n$位操作码，则有$2^n$条指令。
     1.   优点：能简化计算机硬件设计，提高指令译码和识别速度。一般这种操作码用于指令字长较长的情况。当计算机字长为32位或更长时常采用定长操作码。
     2.   缺点：指令数量增加时会占用更多固定位，留给表示操作数地址的位数有限。
8.   可变长操作码，最常见的是拓展操作码：定长指令字结构+可变长操作码。操作码的位数至少为当种指令总条数的二对数。
     1.   在设计扩展操作码指令格式时，必须注意以下两点：不允许短码是长码的前缀（即不允许存在**前缀编码**）；各指令的操作码一定不能重复。
     2.   **假设地址长度为$n$，上一层留出$m$种状态，则下一层可以拓展出$m\times2^n$种状态**。

### 指令寻址

1.   顺序寻址，即`(PC)+"1"→PC`：加上的$1$实际上表示的是**当前指令占用的存储单元数**。需要先确定编址单位并利用指令字长得到加上的$1$。
2.   跳跃寻址：由转移指令`jmp`或`jxxx`指出。跳跃地址分为绝对地址（标记符直接得到）和相对地址（相对当前地址的偏移量，**偏移量使用补码表示**，方便往回跳）。跳跃的结构是当前指令修改$PC$值，所以下一条指令仍通过$PC$给出。程序跳跃后，按新的指令地址开始顺序执行。

### ※※数据寻址

**主存寻址**：数据都存储在主存中。

1.   直接寻址：$EA=A$。共访存$2$次（取指令，取操作数）。
     1.   优点：简单，指令执行阶段仅访问一次主存，不需专门计算操作数的地址。
     2.   缺点：形式地址$A$的位数决定了该指令操作数的寻址范围；操作数的地址不易修改。
2.   间接寻址：$EA=(A)$。共访存$3$次（取指令，取操作数地址，取操作数）。
     1.   优点：扩大了寻址范围；便于编制程序，易于完成子程序返回。
     2.   缺点：指令在执行阶段要多次访存，由于访问速度过慢，所以一般都不使用。
3.   隐含寻址：累加器$ACC$对单地址指令格式来说是隐含寻址。
     1.   优点：有利于缩短指令字长。
     2.   缺点：需增加存储操作数或隐含地址的硬件。
4.   立即寻址：形式地址指出的不是数据地址，而是数据本身，称为立即数，**采用补码形式存储**。共访存$1$次（取指令）。
     1.   地址形式为：操作码`OP+#+`立即数$A$，其中`#`表示立即寻址特征。
     2.   优点：采用**定长指令码**格式时，指令执行阶段不访问主存，**指令执行时间最短**。
     3.   缺点：形式地址$A$的位数限制了立即数的范围。如$A$的位数为$n$，且立即数采用补码时，可表示的数据范围为$[-2^{n-1}，2^{n-1}-1]$。

**寄存器寻址**：数据都存储在寄存器中。

5.   寄存器寻址：$EA=R_i$，其操作数在由$R_i$所指的寄存器内。共访存$1$次（取指令）。
     1.   优点：指令字短；寄存器位于$CPU$内部，采用**变长指令码**格式时，**指令执行时间最短**；**支持向量/矩阵运算**。
     2.   缺点：寄存器价格昂贵；计算机中寄存器个数有限。
6.   寄存器间接寻址：$EA=(R_i)$。共访存$2$次（取指令，取操作数）。

**※※偏移寻址**：都需要加一个偏移量。

7. 基址寻址：**以程序的起始存放地址作为“起点”**。$EA=(BR)+A$，其中$BR$表示$CPU$中的基址寄存器，基址寄存器可使用专用寄存器和某通用寄存器。
    1. 基址寄存器是面向操作系统的，其**内容**由操作系统或管理程序确定，用户不可改变。
    2. 当采用通用寄存器作为基址寄存器时，**可由用户决定哪个寄存器作为基址寄存器**，但其内容仍由操作系统确定。
    3. 优点：可扩大寻址范围（基址寄存器的位数大于形式地址$A$的位数）；**便于程序“浮动”**，用户不必考虑自己的程序存于主存的哪一空间区域，故**有利于多道程序并发运行**，以及可用于**编制浮动程序**。
8. 变址寻址：**程序员自己决定从哪里作为“起点”**。$EA=(IX)+A$，其中$IX$为变址寄存器（专用），也可用通用寄存器作为变址寄存器。
    1. 与基址寄存器不同的是，变址寄存器是面向用户的，在程序执行过程中，变址寄存器的内容可由用户改变（作为偏移量）。

    2. 优点：可扩大寻址范围；在**数组处理**过程中，可设定$A$为数组的首地址，不断改变变址寄存器$IX$的内容，便可很容易形成数组中任一数据的地址，特别适合**编制循环程序**。

9. ※※相对寻址：**以程序计数器$PC$所指地址作为“起点”**。$EA=(PC)+A$，其中$A$是相对于$PC$所指地址即下一条指令的地址的位移量，使用补码表示，方便向前向后跳转。
    1. **转移指令基本上使用相对寻址方式**。
    2. ※由于取指时会默认$(PC)+1\rightarrow PC$，所以**相对寻址中的$A$需要将$1$计算进去**。
    3. 优点：操作数的地址与指令地址之间总是相差一个固定值，因此便于一段代码在**整个程序内部的浮动**；相对寻址广泛应用于转移指令。

---

速度快慢

1.   **间接寻址**方式的执行速度最慢：需要读内存两次（第一次由操作数的间接地址读到操作数的地址，第二次再由操作数的地址读到操作数）。
2.   采用**变长指令码**格式，**寄存器寻址**方式的执行速度最快：通用寄存器位于$CPU$内部，无须到内存读取操作数。
     +   采用变长指令码格式时，**立即数寻址虽然无须取操作数，但因指令码长度最长**，取指令访存花费的时间较多。
3.   如果采用**定长指令码**格式，那就是**立即数寻址**最快：所有指令（包括采用立即寻址方式的指令）所包含的二进制位数均相同，由于读到指令的同时，便立即取得操作数，所以立即寻址方式执行速度最快。

指令码长度长短

1.   **寄存器寻址**方式和**寄存器间接寻址**方式的指令码长度最短。
2.   **立即寻址**方式、**直接寻址**方式和**间接寻址**方式的指令码长度最长。
     + 若指令码长度太短，则无法表示范围较大的立即数和寻址到较大的内存地址空间。

### ※※程序的机器代码表示

#### 应试技巧

首先需要分辨是$x86$还是$MIPS$：

1.   观察是否有注释：通常来说，真题中$x86$汇编语⾔不会给太多注释，考研中默认⼤家懂$x86$。
2.   观察指令⻓度是否固定
     1.   $x86$属于$CISC$，指令⻓度不固定。
     2.   $MIPS$属于$RISC$，需要使用指令流水线，指令⻓度固定。
3.   观察寄存器名
     1.   $x86$的寄存器名为$eax$、$ebx$、$ecx$、$edx$。
     2.   $MIPS$的寄存器名为$R[0]$、$R[1]$、$R[2]$。

---

$x86$：重点关注$Intel$格式

1.   首先先搞懂题上的C语言代码，分析该段代码中的代码结构，带着分析出来的代码结构看汇编代码做题。
2.   分析分支结构：`jmp，jxxx`，$x86$的转移指令都是**相对寻址**

     1.   无条件转移指令：`jmp`。

     2.   条件转移指令：`jxxx`，一般会前置`cmp`指令。

     3.   分析循环结构：`jxxx`，`loopxxx`。

     4.   分析函数调用：`call`，`ret。`

          1.   函数调⽤参数：函数调⽤参数⼀般在[ebp+8]、[ebp+12]等位置。
          2.   局部变量：局部变量的存储地址⼀般在[ebp-4]、[ebp-8]、[ebp-12]等位置。

     5.   算数、逻辑运算类指令：加、减、乘、除、左移、右移等指令。

     6.   **$x86$默认小端存储**（反过来），实在写不出来可以蒙，尽量验证。

#### 机器级代码概述

1.   相关寄存器
     1.   $EAX,EBX,ECX,EDX$：$E=Extended=32bit$，看到$E$表示是$32$位。
     2.   $AX,BX$：$16bit$。
     3.   $AH,AL,BH,BL$：$8bit$。
     4.   变址寄存器$ESI,EDI$：用于线性表和字符串的处理。$S=Source,D=Destination$。
     5.   堆栈寄存器$EBP,ESP$：用于实现函数调用
2.   汇编指令格式`AT&T`$V.S.$`Intel`，重点关注$Intel$格式
     1.   `AT&T`格式的指令只能用小写字母；而$Intel$格式的指令对大小写不敏感。
     2.   在`AT&T`格式中，第一个为源操作数，第二个为目的操作数，方向从左到右，合乎自然；在$Intel$格式中，第一个为目的操作数，第二个为源操作数，方向从右向左。
     3.   在`AT&T`格式中，寄存器需要加前缀`%`，立即数需要加前缀`$`；在$Intel$格式中，寄存器和立即数都不需要加前缀。
     4.   在内存寻址方面，`AT&T`格式使用`(`和`)`；而$Intel$格式使用`[`和`]`。
     5.   在处理复杂寻址方式时

          +   例如`AT&T`格式的内存操作数`disp(base,index,scale)`分别表示偏移量、基址寄存器、变址寄存器和比例因子
              +   如`8(%edx,%eax,2)`表示操作数为`M[R[edx]+R[eax]*2+8]`
          +   其对应的$Intel$格式的操作数为`[edx+eax*2+8]`
     6.   在指定数据长度方面
          1.   `AT&T`格式指令操作码的后面紧跟一个字符，表明操作数大小。**b**：表示字节（8位）；**w**：表示字（16位）；**l**：表示双字（32位）；**q**：表示四字（64位）。
          2.   $Intel$格式也有类似的语法，它在操作码后面显式地注明`byte ptr`，`word ptr`或`dword ptr`。$dword\;ptr$：双字，$32bit$；$word\;ptr$：单字，$16bit$；$byte\;ptr$：字节，$8bit$；若未指明主存读写长度，默认$32bit$。
3.   常见表示
     1.   `<reg>`：表示任意寄存器，若其后带有数字，则指定其位数。如`<reg32>`表示32位寄存器（eax、ebx、ecx、edx、esi、edi、esp或ebp）。
     2.   `<mem>`：表示内存地址。如`[eax]`、`[var+4]`或`dword ptr[eax+ebx]`。
     3.   `<con>`：表示8位、16位或32位常数。`<vcon8>`表示8位常数；`<vconl6>`表示16位常数；`<vcon32>`表示32位常数。
     4.   例如`af996h`中的$h$表示16进制。
     5.   `[]`表示里面是个地址，并且一般前面还会指明数据长度
          1.   `[]`里面如果没有写直接的地址就表示寄存器($EAX\;EBX\;ECX\;EDX$等)，即表示寄存器间接寻址
               + 即先把寄存器中存放的主存地址取出在去主存中取数据
          2.   `[]`里面还可以加减表示地址偏移
     6.   $d：destination$（$n.$目的地）；$s：source$（$n.$来源，来源地）

#### 常用指令

数据传送指令：

1.   数据转移指令`mov`：【`mov `目标操作数$d$ 源操作数$s$】。将源操作数s复制到目的操作数d所指的位置，但不能用于直接从内存复制到内存。
     1.   `mov eax,ebx`：将寄存器$ebx$的值复制到寄存器$eax$。寄存器寻址→寄存器寻址。
     2.   `mov eax，5`：将立即数$5$复制到寄存器$eax$。立即数寻址→寄存器寻址
     3.   `mov eax,dwordptr[af996h]`：将内存地址`af996h`所指的$32bit$值复制到寄存器$eax$。

2.   入栈指令`push`：【`push`  源操作数$s$】，其中源操作数$s$表示要推入栈的数据源。`push`指令的作用是将源操作数的值放入栈顶，并将栈指针$-1$（此处的$1$同$PC$，毕竟是**指向指令的数**），以便指向新的栈顶。**常用于函数调用**。

     ```assembly
     push %eax    ; 将 eax 寄存器的值推入栈中
     push $42     ; 将立即数 42 推入栈中
     push (%esi)  ; 将 esi 寄存器指向的内存位置的值推入栈中
     ```

3.   出栈指令`pop`：【`pop` 目标操作数$d$】，目标操作数$d$表示弹出的值将要存储的位置。`pop`指令从栈顶弹出一个值，将其存储到目标操作数中，并将栈指针$+1$（同上），以指向新的栈顶。

     ```assembly
     pop %eax    ; 从栈中弹出一个值，存储到 eax 寄存器中
     pop %ebx    ; 从栈中弹出一个值，存储到 ebx 寄存器中
     pop (%esi)  ; 从栈中弹出一个值，存储到 esi 寄存器指向的内存位置中
     ```

运算/逻辑运算指令：

1.   加运算指令`add`：【`add` 目标操作数$d$ 源操作数$s$】，两数相加，结果保存在第一个数（目标操作数$d$）中。

     ```assembly
     add ebx, eax    ; 将 eax 寄存器的内容与 ebx 寄存器的内容相加，并将结果存储在 ebx 中
     add ecx, 42     ; 将立即数 42 与 ecx 寄存器的内容相加，并将结果存储在 ecx 中
     add edx, [esi]  ; 将 esi 寄存器指向的内存位置的内容与 edx 寄存器的内容相加，并将结果存储在 edx 中
     ```

2.   减运算指令`sub`：【`sub` 目标操作数$d$ 源操作数$s$】，两数相减，结果保存在第一个数（目标操作数$d$）中。

     ```assembly
     sub ebx, eax    ; 将 ebx 寄存器的内容减去 eax 寄存器的内容，并将结果存储在 ebx 中
     sub ecx, 10     ; 将 ecx 寄存器的内容减去立即数 10，并将结果存储在 ecx 中
     sub edx, [esi]  ; 将 edx 寄存器的内容减去 esi 寄存器指向的内存位置的内容，并将结果存储在 edx 中
     ```

3.   自增指令`inc` ：【`inc` 目标操作数$d$】，将目标操作数的值增加$1$。这里的$1$就是数值上的$1$了，表示的是**数值的增减而不是指令地址的变化**，下同。

4.   自减指令`dec` ：【`dec` 目标操作数$d$】，将目标操作数的值减少$1$。

5.   带符号整数乘法指令`imul`：第一个操作数必须为寄存器（不可以是立即数），其他操作数可以是立即数或寄存器。乘法操作结果可能溢出，则编译器置溢出标志$OF =1$，以使$CPU$调出溢出异常处理程序。

     1.   【`sub` 目标操作数$d$ 源操作数$s$】：两个操作数，将两个操作数相乘，将结果保存在第一个操作数中，**第一个操作数必须为寄存器**
     2.   【`sub` 目标操作数$d$ 源操作数$s$ 乘法因子】：三个操作数，将第二个和第三个操作数相乘，将结果保存在第一个操作数中，第一个操作数必须为寄存器。 

     ```assembly
     imul eax, [esi]    ; 将 eax 寄存器的内容与 esi 寄存器指向的内存位置的内容相乘，并将结果存储在 ebx 中
     imul ecx, [esi], edx  ; 将 esi 寄存器指向的内存位置的内容与 edx 寄存器的内容相乘，结果存储在 ecx 中
     ```

6.   带符号整数除法指令`idiv`：【`idiv` 目标操作数$d$】，只有一个操作数，即除数，而被除数则为edx:eax中的内容（64位整数）。操作结果有两部分：商和余数，商送到eax，余数则送到edx。 

     ```assembly
     idiv ecx    ; 用 ecx 寄存器的值除以 AX，商存储在 AL，余数存储在 AH
     idiv dword ptr [esi]  ; 用 esi 寄存器指向的内存位置的值除以 AX，商存储在 AL，余数存储在 AH
     ```

7.   逻辑与运算指令`and`：【`and `  目标操作数$d$ 源操作数$s$ 】，将$d、s$逐位相与，结果放回$d$。

8.   逻辑或运算指令`or`：【`or `  目标操作数$d$ 源操作数$s$ 】，将$d、s$逐位相或，结果放回$d$。

9.   逻辑非运算指令`not`：【`not`  目标操作数$d$ 】，将$d$逐位取反，结果放回$d$。

10.   逻辑异或运算指令`xor`：【`xor`  目标操作数$d$ 源操作数$s$ 】， 将$d、s$逐位异或，结果放回$d$。

11.   取负运算指令`neg`：【`neg`  目标操作数$d$ 】，将$d$取负，结果放回$d$。

12.   左移运算指令`shl `：【`shl`  目标操作数$d$ 源操作数$s$ 】，将$d$逻辑左移$s$位，结果放回$d$。通常$s$是常量。

13.   右移运算指令`shr`：【`shr`  目标操作数$d$ 源操作数$s$ 】，将$d$逻辑右移$s$位，结果放回$d$。通常$s$是常量。

----

控制流指令：注意条件转移指令普遍使用的相对寻址进行跳转时程序计数器$PC$会（在取本条指令时）默认加一（在强调一遍加的是指令字长）

1.   无条件跳转指令`jmp`：【 `jum ` <地址>】使用 `jmp` 无条件跳转到指定地址，通常在循环中用于控制跳转。其中地址可以

     1.   `jmp 128`：<地址>可以用常数给出
     2.   `jmp eax`：<地址>可以来自于寄存器

     3.   `jmp[999]`：<地址>可以来自于主存

     4.   `jum NEXT`：<地址>可以用‘标号’锚定

2.   条件转移指令`jxxx`：【 `jxxx ` <地址>】。一般与`cmp`指令一起使用。地址的说明同上。

     1.   $je$：$jump \;when \;egual$，若$a==b$则跳转。
     2.   $jne$：$jump \;when\;not\;egual$，若$a\ne b$则跳转。
     3.   $jg$：$jump\;when\;greater\;than$，若$a>b$则跳转。
     4.   $jge$：$jump\;when\;greater\;than\;o\;requal\;to$，若$a\ge b$则跳转。
     5.   $jl$：$jump\;when\;less\;than$，若$a<b$则跳转。
     6.   $jle$：$jump\;when\;less\;than\;or\;equal\;to$，若$a\le b$则跳转。

3.   操作数比较指令`cmp`：【`cmp`  目标操作数$d$ 源操作数$s$ 】，其中，目标操作数$d$表示比较的目标操作数，源操作数$s$表示比较的源操作数。两个操作数可以是寄存器、内存地址或立即数。不保存操作结果，仅根据运算结果设置$CPU$状态字中的条件码。 

4.   循环控制指令`loop`：【`loop` `target`】，其中，target表示循环的目标位置，可以是标签或相对/绝对的地址。`loop`会根据$CX$寄存器的计数值进行循环迭代。`loop`指令通常与**递减指令**`dec` 结合使用，用于实现循环计数器的递减。在每次循环迭代中，`dec`指令会递减$CX$寄存器中的计数值，然后根据计数值是否为零来决定是否跳转到目标位置。

     ```assembly
     MOV CX， 5       ; 初始化CX寄存器为5，作为循环计数器
     LoopStart:
         ; 循环内的指令
         LOOP LoopStart   ; 循环迭代，跳转回LoopStart标签
     ```

5.   调用指令`call`：【`call` `<label>`】，用于调用子程序（函数），将当前执行位置的返回地址推入栈中，并跳转到子程序入口点。

6.   返回指令`ret`：【`ret`】，用于从子程序返回，弹出返回地址并跳转回调用点。

#### 过程调用的机器级表示

1.   假定过程$P $（调用者）调用过程$Q $（被调用者），过程调用的执行步骤如下：

     1.   $P$将入口参数（实参）放在$Q$能访问到的地方。
     2.   $P$将返回地址存到特定的地方，然后将控制转移到$Q$。（由`call`指令实现）
     3.   $Q$保存P的现场（通用寄存器的内容），并为自己的非静态局部变量分配空间。
     4.   执行过程$Q$。
     5.   $Q$恢复$P$的现场，将返回结果放到$P$能访问到的地方，并释放局部变量所占空间。
     6.   $Q$取出返回地址，将控制转移到$P$。（通过`ret`指令返回到过程$P$）

2.   每个过程都有自己的栈区，称为**栈帧**。帧指针寄存器$EBP$指示栈帧的起始位置（栈底），栈指针寄存器$ESP$指示栈顶，栈从高地址向低地址增长。因此，当前栈帧的范围在帧指针$EBP$和$ESP$指向的区域之间。 

3.   如图，从上往下是从高位（栈底）到低位（栈顶），局部变量存放在低位（栈顶）下（在栈帧中，通过`ebp-4k`取得数据），函数调⽤参数存放在低位（栈顶）下（在栈帧外，上一层函数中，通过`ebp+4k`取得数据）

     ![image-20230919214045952](https://trouvaille-oss.oss-cn-beijing.aliyuncs.com/picList/202309192140106.png)

4.   使用条件转移指令实现循环，一般有四个步骤

     1. 循环前的初始化
     2. 是否直接跳过循环
     3. 循环主体
     4. 是否继续循环

### 指令集计算机

1.   复杂指令集计算机$CISC(Complex\;Instruction\;Set\;Computer)$：一条指令完成一个复杂的基本功能。代表为$x86$架构，主要用于笔记本和台式机。$CISC$主要特点如下：
     1.   **指令系统复杂庞大**，指令数目一般为200条以上。
     2.   指令的**长度不固定**，指令格式多，寻址方式多。
     3.   **可以访存的指令不受限制**。
     4.   **各种指令执行时间相差很大**，大多数指令需多小时钟周期才能完成。
     5.   控制器大多数**采用微程序控制**。有些指令非常复杂，以至于**无法采用硬连线控制**。
     6.   从指令系统兼容性看，**$CISC$大多能实现软件兼容**，即高档机包含了低档机的全部指令，并可加以扩充。
2.   精简指令集计算机$RISC(Reduced\;Innstruction\;Set\;Computer)$：一条指令完成一个基本动作，多条指令组合完成一个复杂的基本功能。代表为$ARM$架构，主要用于手机和平板。$RISC$的主要特点如下：
     1.   **选取使用频率最高的一些简单指令**，复杂指令的功能由简单指令的组合来实现。
     2.   **指令长度固定**，指令格式种类少，寻址方式种类少。
     3.   **只有`Load/Store`（取数/存数）指令访存**，其余指令的操作都在寄存器之间进行。
     4.   **$CPU$中通用寄存器的数量相当多**。
     5.   **$RISC$一定采用指令流水线技术**，大部分指令在一个时钟周期内完成。
     6.   **以硬布线控制为主**，不用或少用微程序控制。
     7.   从指令系统兼容性看，$RISC$简化了指令系统，**指令条数少**，格式也不同于老机器，因此**大多数$RISC$机不能与老机器兼容**。
3.   $CISC$和$RISC$比较：和$CISC$相比，$RISC$的优点主要体现在以下几点
     1.   $RISC$更能充分利用$VLSI$芯片的面积。
          1.   $CISC$的控制器大多采用微程序控制，其控制存储器在$CPU$芯片内所占的面积达$50%$以上
          2.   而$RISC$控制器采用组合逻辑控制，其硬布线逻辑只占$CPU$芯片面积的$10%$左右。
     2.   $RISC$更能提高运算速度：$RISC$的指令数、寻址方式和指令格式种类少，又设有多个通用寄存器，采用流水线技术，
     3.   $RISC$便于设计，可降低成本，提高可靠性。$RISC$指令系统简单，因此机器设计周期短；其逻辑简单，因此可靠性高。
     4.   $RISC$有利于编译程序代码优化。
          +   $RISC$指令类型少，寻址方式少，使编译程序容易选择更有效的指令和寻址方式，并适当地调整指令顺序，使得代码执行更高效化。