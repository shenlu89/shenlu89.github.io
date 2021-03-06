---
layout: post
title: "一个程序是怎么在计算机中执行的"
categories: [Computer]
tags: [rudiment]
---

>All computer software is built up of sets of instructions. Instructions are encoded in binary. The fetch-decode-execute cycle is the sequence of steps that the CPU follows to process instructions.

为了搞清背后的工作原理，没有章法的了解一堆概念：

冯.诺依曼结构的计算机是由五个组成部分：**运算器**，**控制器**，**存储器**，**输入设备**，**输出设备**。这5部分比较容易理解，就不多解释了。

![](./assets/images/Von-Neumann-Architecture-Diagram.jpg)

**中央处理器 (Central Processing Unit)**的组成部分

- 控制器(Control Unit, CU): 
	- **指令计数器** (Program Counter, PC): 又称程序计数器，它包含当前正在执行的指令的地址（位置）。在每个指令被获取之后（指令寄存器将指令提交到控制器解码前），程序计数器指向顺序中的下一个指令（存储地址加1）。当计算机重启或复位时，程序计数器通常恢复到0。
	- **地址寄存器** (Memory Address Register, MAR): 是指存放指令地址的寄存器。控制器根据地址寄存器中存放的指令地址，从内存中将相应地址的指令取出到数据寄存器中。
	- **数据寄存器** (Memory Data Register, MDR): 暂存由计算机主存储器读取后的数据胡或将要写入到计算机主存储器（RAM）的数据。它就像缓冲器，持有从内存复制的数据，以准备给CPU使用。
	- **指令寄存器** (Instruction Register, IR): 
	- 指令译码器(Instruction Decoder, ID): 能将存储在指令寄存器或微程序指令中的位元(二进制位，0或1)转换为能控制CPU其他部分的控制信号(signals)。指令由操作码 (Opcode)和地址码 (Operand)组成。操作码表示要执行的操作性质，即执行什么操作，或做什么；地址码是操作码执行时的操作对象的地址。计算机执行一条指定的指令时，必须首先分析这条指令的操作码是什么，以决定操作的性质和方法，然后才能控制计算机其他各部件协同完成指令表达的功能。这个分析工作由指令译码器来完成。
- 运算器(Arithmetic Unit, AU)：
	- 算术逻辑单元(Arithmetic Logic Unit, ALU): 算术逻辑运算单元（ALU）的基本功能为加、减、乘、除四则运算，与、或、非、异或等逻辑操作，以及移位、求补等操作。
	- **累加器** (Accumulator, ACC, AC): 是一个通用寄存器。当运算器的算术逻辑单元(ALU)执行算术或逻辑运算时(加法，乘法，移位等等)，累加器为ALU提供一个工作区，可以为ALU暂时保存一个操作数或运算结果。显然，运算器中至少要有一个累加寄存器。
	- **状态字寄存器** (Status Register, Flag Register, Condition Code Register, CCR)，是一个16位(bit)寄存器,由条件码标志(flag)和控制标志构成，主要用于存放控制信息和体现当前指令执行结果的状态信息。

**寄存器 (Register)**

上面列出的**加粗名词**都是**寄存器**。在计算机架构中，寄存器(Register)是中央处理器(CPU)内用来暂存指令、地址、中间运算结果等数据的存储器，存储容量有限，读写速度非常快。寄存器位于存储阶层（Memory Hierarchy）的最顶端，是读写速度最快的存储器。

![](./assets/images/Slide21.png)

**总线 (Bus)**

在计算机系统中，各个部件之间传送信息的公共通路叫总线(bus)。总线是一种内部结构，它是cpu、内存、输入、输出设备传递信息的公用通道，主机的各个部件通过总线相连接，外部设备通过相应的接口电路再与总线相连接，从而形成了计算机硬件系统。如果说主板（Mother Board）是一座城市，那么总线就像是城市里的公共汽车（Bus），能按照固定行车路线，传输来回不停运作的比特（bit）。这些线路在同一时间内都仅能负责传输一个比特。因此，必须同时采用多条线路才能传送更多数据，而总线可同时传输的数据数就称为宽度（Width），以比特为单位，总线宽度愈大，传输性能就愈佳。

总线的带宽（即单位时间内可以传输的总数据数）为：总线带宽 = 频率 x 宽度（Bytes/Second）。

指令周期主要涉及的总线有：

- 数据总线 (Data Bus)：在CPU与RAM之间来回传送需要处理或是需要储存的数据。
- 地址总线 (Address Bus)：用来指定在RAM（Random Access Memory）之中储存的数据的地址。
- 控制总线 (Control Bus)：将控制器（Control Unit）的信号，传送到周边设备。

![](./assets/images/400px-Computer_system_bus.svg.png)

存储器主要就是内存和外存：

- 内部存储器 (Internal Memory) : 
	- 寄存器 (Register) : 在中央处理器的组成部分已经说过了。
	- CPU高速缓存 (CPU Cache) : 是用于减少处理器访问内存所需平均时间的部件。也就是平常看到的一级缓存(L1 Cache）、二级缓存(L2 Cache）和三级缓存(L3 Cache）。位于CPU与内存之间，其容量远小于内存，读写速度比内存更快，可以接近处理器的频率，仅次于寄存器。当CPU向内存中写入或读出数据时，这些数据也被存储进高速缓冲存储器中。当CPU再次需要这些数据时，CPU就从高速缓冲存储器读取数据，而不是访问较慢的内存，当然，如需要的数据没有在缓存中找到，CPU会再次读取内存中的数据。
	- 内存 (Memory) : 又称主存 (Main Memory)，包括随机存储器 (RAM)，只读存储器 (ROM)等。它是计算机外存与CPU进行沟通的桥梁，所有的计算机程序都要读入内存后才能运行。
		- 只读存储器 (ROM) : 在制造ROM的时候，信息（数据或程序）就被存入并永久保存。这些信息只能读出，一般不能写入，即使机器停电，这些数据也不会丢失。ROM一般用于存放计算机的基本程序和数据，如BIOS ROM。
		- 随机存储器 (RAM) : 我们日常说的内存(Memory)通常就是指(RAM)，它是外存与CPU进行沟通的桥梁。其作用是用于暂时存放CPU中的运算数据，以及与硬盘等外部存储器交换的数据。只要计算机在运行中，操作系统就会把需要运算的数据从内存调到CPU中进行运算，当运算完成后CPU再将结果传送出来，内存的运行也决定了计算机的稳定运行。当机器电源关闭时，存于其中的数据就会丢失。我们通常购买或升级的内存条就是用作电脑的内存。

- 外部存储器 (External Memory) : 比如，硬盘，U盘，磁盘，光盘等等。

![](./assets/images/v2-6cbe0ce1ed6a7336de891e9f83ad4269_r.jpg)

**补充材料:** 

1. [为什么寄存器比内存快？](https://www.ruanyifeng.com/blog/2013/10/register.html){:target="_blank"}

### 内存容量

计算机的内存容量通常是指随机存储器（RAM) 的容量，是内存条的关键性参数。内存的容量一般都是2的整次方倍，比如64MB、128MB、256MB等，一般而言，内存容量越大越有利于系统的运行。系统对内存的识别是以Byte（字节）为单位，每个字节由8位二进制数组成，即8bit（比特，也称“位”）。

购买电脑和下载软件的时候，经常可以看到64位(64bit)和32位(32bit)两种描述，这里的64位技术是相对于32位而言的，计算机CPU的GPRs（General-Purpose Registers，通用寄存器）的数据宽度为64位，64位指令集就是运行64位数据的指令，也就是说处理器一次可以运行64bit数据。由此可以推断，总线的宽度也至少是64bit。

32位的计算机就是可以一次处理32bit的数据，如果这个数据是地址信息，32bit最大寻址空间为$2^{32} = 2^{(10+10+10+2)} = 2^2 \times 2^{30}$个地址，刚刚说了内存以字节(Byte)为单位。也就是意味着32位系统最多只能在4GB的内存里找数据。而64位系统的最大寻址空间为$2^{64} = 2^{(60+4)} = 2^{60} \times 2^4$个地址，最大寻址范围达到了16EB。不过那也只是理论值而已，实际上是达不到的。

搞清楚这些概念后，就可以开始理解一个程序被执行的时候都经历了什么。

最近在看[Computer System. A Programmer's Perspective [3rd ed.](2016 Person)](http://csapp.cs.cmu.edu/3e/labs.html)，里面清楚的描述了C语言程序在计算机的执行过程。

### 计算机眼中的C语言源文件

一个包含如下内容的`hello.c`文本文件(text file)，也是C语言程序的源文件(source program or source file)

```c
#include <stdio.h>

int main()
{
	printf("hello, world\n");
	return 0;
}
```

大部分的计算机都使用[ASCII](https://en.wikipedia.org/wiki/ASCII){:target="_blank"}码，什么是ASCII码?

计算机中储存的所有信息都是用二进制数表示的(0或1)。而我们在屏幕上看到的英文、汉字等字符是二进制数通过字符编码(Charactor Encoding)转换之后的结果。

>字符集 (Charset): 是一个系统所支持的抽象字符集合。字符编码 (Character Encoding): 就是字符集与数字系统之间建立对应关系的一套法则。

ASCII码就是其中一种计算机常用字符集。上个世纪60年代，美国制定了一套字符编码，对英语字符与二进制位之间的关系，做了统一规定。这被称为 ASCII码，一直沿用至今。

![](./assets/images/800px-USASCII_code_chart.png)

每一个二进制位（bit）有0和1两种状态，因此8个二进制位就可以组合出256 ($2^8$)种状态，这被称为一个字节(Byte)。也就是说，一个字节(Byte)一共可以用来表示256种不同的整数，每一个整数对应一个符号。ASSCII标准码就是256个符号。例如，`hello.c`中的第一个字符`#`的整数值是35，第二个字符`i`是105，每一行结束的换行符`\n`是10。

```
# i n c l u d e S P < s t d i o .
35 105 110 99 108 117 100 101 32 60 115 116 100 105 111 46
h > \n \n i n t SP m a i n ( ) \n {
104 62 10 10 105 110 116 32 109 97 105 110 40 41 10 123
\n SP SP SP SP printf("hel
10 32 32 32 32 112 114 105 110 116 102 40 34 104 101 108
lo, SP w o r l d \ n " ) ; \n SP
108 111 44 32 119 111 114 108 100 92 110 34 41 59 10 32
SP SP SP return SP 0 ; \n } \n
32 32 32 114 101 116 117 114 110 32 48 59 10 125 10
```

计算机上存储的所有信息对计算机而言本质上都是0或1。在不同的上下文(Context)中，一串相同的字节可以被看做整数(integer)，浮点数(floating-point number)，字符串(character string)或者机器指令(machine instructions)。

### 将C语言源文件编译成可执行文件

1.2 Programs Are Translated by Other Programs into Different Forms

C语言是一种高级语言(high-level programming language)，由C语言编写的程序由固定的词汇按照固定的格式组织起来，简单直观，容易识别和理解。但是计算机只能识别二进制的机器语言(machine-language)，并不认识C语言源文件(source program)中的字符。这就需要另一个程序，将C语言原文近(source program)转换成计算机CPU可以识别的二进制指令。这个程序就叫编译器(Compiler)。编译器能够识别代码中的词汇、句子以及各种特定的格式，并将他们转换成计算机能够识别的二进制形式，这个过程称为编译（Compile）。

之前的`hello.c`需要通过`gcc`编译器(complier)编译成二进制的可执行程序
(executable object file, executable file or object file)后才能在计算机上正常运行。

```c
gcc -o hello hello.c
```

另外，在Windows系统上，以`.exe`结尾，双击后可直接运行的文件都是可执行程序(executable file)。

>C语言标准主要由两部分组成：一部分描述C的语法，另一部分描述C标准库。C标准库定义了一组标准头文件，每个头文件中包含一些相关的函数、变量、类型声明和宏定义，比如，如常见的`printf`函数便是一个C标准库函数，其原型定义在`stdio`头文件中。C语言标准仅仅定义了C标准库函数原型，并没有提供实现。因此，C语言编译器通常需要一个C运行时库（C Run Time Libray，CRT）的支持。C运行时库又常简称为C运行库。C运行库，是和平台相关的，即和操作系统相关的。它由不同操作系统不同开发平台提供不同的C运行库。但是C运行库的部分实现是基于C标准库的，即C运行库是各个操作系统各个开发工具根据自身平台开发的库，某种程度上，可以说C运行库是C标准库的一个扩展库，只是加了很多C标准库所没有的与平台相关的或者不相关的库接口函数。与C语言类似，C++也定义了自己的标准，同时提供相关支持库，称为C++运行时库。

编译也可以理解为“翻译”，类似于将中文翻译成英文、将英文翻译成象形文字，它是一个复杂的过程，大致包括词法分析、语法分析、语义分析、性能优化、生成可执行文件五个步骤，期间涉及到复杂的算法和硬件架构，这里不做展开了。

以C语言为例，C语言源代码到被`GCC`编译器编译成二进可制行程序都经历了四个阶段：

![](./assets/images/c-compling-process.png)

**1.预处理(Preprocessing)**

预处理过程实质上是处理“#”，将`#include`包含的头文件直接拷贝到`hello.c`当中；将`#define`定义的宏进行替换，同时将代码中没用的注释部分删除等。[点击查看编译后的hello.i文件](./public/2019-10-01-how-does-a-program-run-in-your-computer/hello.i)

```sh
gcc -E hello.c -o hello.i
```

**2.编译(Compilation)**

编译器(compiler)将预编译得到的文件进行词法、语法、语义分析等，使之转化为功能等价的汇编语言(assembly lauange)代码。对代码进行优化，减小代码的时间和空间，提高执行效率。不同的高级语言（如 C、Fortran、Pascal 等等）有不同的预处理器和编译器，但它们在经过编译之后都变成了同一种汇编语言，于是后面的步骤和工具大家都是通用的了。

```sh
gcc -S hello.i -o hello.s
```

`hello.s`内容如下

```
	.file	"hello.c"
	.text
	.section	.rodata
.LC0:
	.string	"hello, wrold"
	.text
	.globl	main
	.type	main, @function
main:
.LFB0:
	.cfi_startproc
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register 6
	leaq	.LC0(%rip), %rdi
	call	puts@PLT
	movl	$0, %eax
	popq	%rbp
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
.LFE0:
	.size	main, .-main
	.ident	"GCC: (Ubuntu 7.5.0-3ubuntu1~18.04) 7.5.0"
	.section	.note.GNU-stack,"",@progbits
```

**3.汇编(Assemble)**

汇编就是通过汇编器(assembler)将汇编语言程序真正翻译成由`0`和`1`组成的二进制机器指令，并把它们打包输出成一个可重定位文件(relocatable object program)。

```sh
gcc -c hello.s -o hello.o 
```

>可以通过`od`命令查看`hello.o`内容

**4.链接(Linking)**

链接程序运行的所需要的目标文件，以及依赖的库文件，最后生成可执行文件。比如，`hello.c`程序中调用了C语言标准库里的 `printf()`函数，用来打印输出`hello，world`。`hello.c`文件中的主函数(main)和`printf()`函数存放在两个不同的目标文件里，经过连接器(linker)处理之后这两文件就被合并到一起了（静态链接)。

```sh
gcc hello.o -o hello
```

<!-- The shell is a command-line interpreter that prints a prompt, waits for you
to type a command line, and then performs the command. If the first word of the
command line does not correspond to a built-in shell command, then the shell
assumes that it is the name of an executable file that it should load and run. So
in this case, the shell loads and runs the hello program and then waits for it to
terminate. The hello program prints its message to the screen and then terminates.
The shell then prints a prompt and waits for the next input command line. -->

编译完成后，在当前目录执行`./hello`，就可以看到输出结果了。

```sh
./hello
hello，world
```

### 指令周期 (Instruction Cycle, Fetch-decode-execute Cycle)

>The main job of the CPU is to execute programs using the fetch-decode-execute cycle (also known as the instruction cycle). This cycle begins as soon as you turn on a computer.

一个程序在执行前，先要读入内存(RAM)中，内容包含这个程序需要CPU处理的机器语言指令和数据。CPU执行一条指令一般分为四个阶段，每个阶段是一个机器周期 (Machine Cycle)：**提取（Fetch）**、**解码（Decode）**、**执行（Execute）**和**写回（Writeback）**

>指令周期（Instruction Cycle）是指取出并执行一条指令的时间，每个指令周期 (Instruction Cycle)由若干个机器周期组成。

![](./assets/images/1.2+微机的工作过程+计算机的工作原理是：+存储程序+++程序控制+CPU+总线+内存+程序+数据+寄存器组+指+令+地+寄存.jpg)

1. 提取 (Fetch)：也就是从程序计数器(PC)里找到对应的指令地址，根据指令地址从内存里把具体的指令，加载到指令寄存器(IR)中，然后把程序计数器(PC)寄存器自增，好在未来执行下一条指令。具体过程：
 - 将程序计数器 (PC)中的内容送至存储器地址寄存器 (MAR)，并送地址总线 (AB)
 - 由控制单元 (CU)经控制总线（CB）向存储器发读命令
 - 从主存中取出的指令通过数据总线 (DB) 送到存储器数据寄存器 (MDR)
 - 将存储器数据寄存器 (MDR) 的内容送至指令寄存器 (IR) 中
 - 将程序计数器 (PC)的内容递增，为取下一条指令做好准备
2. 解码 (Decode)：将指令寄存器 (IR)中的指令提交到控制器 (CU)中的指令解码器(ID)，指令译码器 (ID)对提交的指令进行检测和译码，解析成不同的操作信号，地址和操作数等，通过控制总线(CB)将操作信号和数据传递到算数逻辑单元(ALU)。
3. 执行 (Execute)：算数逻辑单元(ALU)根据输入的操作信号和数据进行实际的运算，状态字寄存器 (SR)存放控制信息和记录当前指令的执行状态，累加器(ACC)暂存计算中间结果和数据。
4. 写回 (Writeback)：程序执行后需要输出的结果，通过数据总线(DB)传回内存，之后可通过输出设备输出结果或将执行结果保存到外存中。

>时钟频率（Clock Rate）是指同步电路中时钟的基础频率，它以“每秒时钟周期”（clock cycles per second）来度量，量度单位採用SI单位赫兹（Hz）。一个时钟脉冲所需要的时间。在计算机组成原理中又叫T周期或节拍脉冲。是CPU和其他单片机的基本时间单位。它可以表示为时钟晶振频率（1秒钟的时钟脉冲数）的倒数（也就是1s/时钟脉冲数，比如1/12MHz），对CPU来说，在一个时钟周期内，CPU仅完成一个最基本的动作。时钟脉冲是计算机的基本工作脉冲，控制着计算机的工作节奏。时钟频率越高，时钟周期就越短，工作速度也就越快。

>时钟周期 (Clock Cycle)也称为振荡周期，定义为时钟频率的倒数。时钟周期是计算机中最基本的、最小的时间单位。在一个时钟周期内，CPU仅完成一个最基本的动作。时钟周期是一个时间的量。时钟周期表示了SDRAM所能运行的最高频率。更小的时钟周期就意味着更高的工作频率。

>机器周期 (Machine Cycle)也称为CPU周期。在计算机中，为了便于管理，常把一条指令的执行过程划分为若干个阶段（如取指、译码、执行等），每一阶段完成一个基本操作。完成一个基本操作所需要的时间称为机器周期。一般情况下，一个机器周期由若干个时钟周期组成，不同的机器分解指令周期的方式也不同。

<!-- 1. High Level lauguage source code

```
z = x + y
```

2. Assembly language source code 

```
LOAD [10]
ADD [11]
STORE [12]
``` 

Assembled Machine Code (Object Code)

```
100110	0000001010
110011	0000001011
111010	0000001100
```

16个比特(bit)，2字节(Byte)。前6位是operation code，后10位是operand(s)，operand可以是一个CPU寄存器或一个内存地址。 -->

随着知识的增加，不断完善上述内容。

**参考文献：**

- [1] [Instructions](https://www.bbc.co.uk/bitesize/guides/z2342hv/revision/5){:target="_blank"}
- [2] [The Fetch Execute Cycle](https://www.youtube.com/watch?v=xfJbpCJSpd8){:target="_blank"}
- [3] [Fetch decode execute cycle](https://www.youtube.com/watch?v=IL44-Mfp8x4){:target="_blank"}
- [4] [Fetch Decode Execute Cycle in more detail](https://www.youtube.com/watch?v=jFDMZpkUWCw){:target="_blank"}
- [5] [Processor Addressing Modes](https://www.youtube.com/watch?v=TGcjn8zMhfM){:target="_blank"}
- [6] [机器周期](https://baike.baidu.com/item/%E6%9C%BA%E5%99%A8%E5%91%A8%E6%9C%9F){:target="_blank"}
- [7] [时钟周期、振荡周期、机器周期、CPU周期、状态周期、指令周期、总线周期、任务周期](https://blog.csdn.net/yangtalent1206/article/details/5853017)

- [8] [简单介绍 CPU 的工作原理](https://www.cnblogs.com/onepixel/p/8724526.html)
- [9] [字符编码那点事：快速理解ASCII、Unicode、GBK和UTF-8](https://zhuanlan.zhihu.com/p/38333902)
- [10] [c运行库、c标准库、windows API的区别和联系](https://www.cnblogs.com/renyuan/p/5031100.html)
- [11] [C Tutor - Visualize C code execution to learn C online](http://pythontutor.com/c.html#mode=display)
- [12] [C语言编译和链接详解（通俗易懂，深入本质）](http://c.biancheng.net/view/1736.html)