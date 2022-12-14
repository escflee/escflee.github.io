# C基础

# 标识符

C语言规定（语法）：标识符由大写字母A到Z、小写字母a到z、数字0到9和下划线组成，且第一个字符必须是字母或下划线，随后的字符必须是字母、数字或下划线。且大小写敏感，如age和Age是两个不同的标识符。

# 表达式

优先级：算术运算符 > 关系运算符( < / > / == ) > 逻辑运算符( && / || ) > 赋值运算符 > 逗号表达式

for结构中的三个表达式是可有可无的：如果在程序的其他地方初始化了控制变量，则可以省去表达式1；如果省略了表达式2，则假定条件为真，建立了一个“无限循环”；如果在for结构体中计算了递增（递减）表达式或者不需要递增（递减）表达式，则可以省去表达式3。

逗号表达式：**表达式1，表达式2，…，表达式n**

执行过程 先计算表达式1，然后依次计算其后的各个表达式的值，并将最右边那个表达式的值作为逗号表达式的值。

# 算法的描述方法

## 流程图

![Untitled](media/C%E5%9F%BA%E7%A1%80/Untitled.png)

## NS图

![Untitled](media/C%E5%9F%BA%E7%A1%80/Untitled%201.png)

![Untitled](media/C%E5%9F%BA%E7%A1%80/Untitled%202.png)

# 函数

## 函数原型

函数原型一般格式：返回值类型 函数名(数据类型[参数名1], 数据类型[参数名2]…)；
**注意**：参数名可写可不写。

函数原型可以放置在任何函数之外，出现在该函数原型之后的所有函数都可以调用对应的函数；也可以放在某个函数中，此时只有该函数能够调用它。从程序设计风格考虑，最好是将函数原型集中在一起，放在主函数之前。

当被调用函数的函数定义出现在该函数调用之前时，可以省略函数原型。因为在调用之前，编译系统已经知道了被调用函数的函数类型、参数个数、类型和顺序。

## 返回值

如果不指明返回值类型，编译器将假定返回值是int型（最好明确指定）

如果函数不返回任何值（即函数功能实现部分无return语句），则返回值类型定义成void。若既无return语句，返回值又不是void类型，则函数将返回一个不确定的值

# 数据存储

## 变量作用域

若变量在复合语句中定义，则其具有块作用域：只在复合语句范围内才能引用该变量。

若变量在函数外部定义，则该变量具有文件作用域：从变量的定义位置开始，到本文件结束为止的区域可以引用该变量。

内部变量屏蔽同名外部变量。

如果文件中外部变量定义点之前的函数需要引用这些外部变量时，需要在*函数内*对被引用的外部变量进行说明。外部变量说明的一般形式为：

extern 数据类型 外部变量1，外部变量2……

## 变量的存储类别

在Ｃ语言中，变量有以下四种存储类别：自动 (auto)、寄存器(register)、静态 (static)、外部 (extern)

局部变量的存储类别可以是：自动(auto)，寄存器 (register) ，静态 (static)

全局变量的存储类别可以是：静态(static) ，外部 (extern)

- 静态局部变量：（函数内定义，前加static）

存储空间在静态存储区分配。在程序开始运行时分配空间，程序执行期间，静态局部变量始终存在。即使所在函数不被调用、或者所在函数调用结束也不释放。但其它函数不能访用它们。若定义静态局部变量但不初始化，则系统自动赋以０（整型或实型）或‘\0’（字符型）。**每次调用它们所在的函数时，不再重新赋初值，只是保留上次调用结束时的值。**

- 静态全局变量：（函数外定义，前加static）

存储空间在静态存储区分配。在程序开始运行时分配空间，程序执行期间，静态全局变量始终存在。**作用域：文件作用域，不能被其他文件中的函数访问。**

- 非静态全局变量：（函数外定义，前面不加static）

存储空间在静态存储区分配。在程序开始运行时分配空间，程序执行期间，非静态全局变量始终存在。**作用域：文件作用域，可以被其他文件中的函数访问**。其它源文件中的函数，引用非静态外部变量时，需要在引用函数所在的源文件中（通常在文件开头）进行说明：extern 数据类型 全局变量表

## 常量类型

- 定义变量、数组时用关键字const修饰：该变量被定义为常量类型，后续无法更新。常量类型必须在定义时初始化。
- 指针常量和常量指针：根据定义变量时关键字和*号的位置关系可以区分指针常量和常量指针。常量指针：指针指向的变量是常量，不能通过这个指针来改变，但指针可以被改变。指针常量：指针不可以被改变，但指针指向的内容可以被改变。

```c
//常量指针：const在*左侧，如：
const int* n;
int const* n;
//无法通过常量指针改变其指向对象的值（不影响其他方式）
int a=5;
const int* n=&a;
*n =6;//错误
a=6;//正确

//指针常量：const在*右侧，如：
int* const n;
//指针无法被改变，但指针指向的内容可以被改变

//*左右使用两个const关键字时指针、指针指向的变量均不能被改变
const int* const p;
```

# 输入输出

## scanf函数

scanf函数返回值为成功读取到的数据的个数。读取到EOF时返回-1

# 编译命令

## 编译预处理指令

- include包含指令：#include<文件名>表示在C标准库中寻找，#include"文件名"表示在当前文件夹中寻找。
- define宏定义指令
- undef删除由#define定义的指宏

## 条件编译指令

```c
#if 常量表达式1
	程序正文
#elif 常量表达式2
	程序正文
#else
	程序正文
#endif
```

```c
#ifdef 标识符//或者#ifndef 标识符
	程序段1
#else
	程序段2
#endif
```

