---
layout: post
title: Write a simple C program
tags: [C/C++] 
---

### 2.1 编写一个简单的C程序

> 某个人的常量可能是其他人的变量.

**` 程序 ` 显示双关语**

*To C, or not to C: that is the question.*

下面这个名为pun.c的程序会在每次运行时显示上述消息.

```c
/* pun.c */
// 指令
#include <stdio.h>
#include <stdlib.h>

// 函数入口
int main(void)
{
    printf("To C, or not to C: that is the question.\n");    // 输出函数,输出字符串内容,使用""包裹
    system("pause");    // 调用系统函数,使程序暂停不关闭窗口.
    return 0;    // 返回0,表示程序成功执行结束.
}
```

#### 2.1.1 编译和链接

- **预处理(preprocessor)**

- **编译(compiler)**

- **链接(linker)**        

UNIX下把文件pun.c生成可执行文件命名为pun,只需输入命令: cc -o pun pun.c

上述过程往往是自动实现的.

使用gcc: gcc -o pun pun.c

#### 2.1.2 集成开发环境

#### 2.2 简单程序的一般形式

*指令*

int main(void)

{

    *语句*

}

- **指令**

- **函数**

- **语句**

#### 2.3 **注释**

/* */或者//

#### 2.4 变量和赋值

##### 2.4.1 类型

int,float,...

##### 2.4.2 声明

**变量声明:** "*类型 变量名*" 

变量名遵循命名规则(见2.7节).

完整的语句后面要以分号结尾.

变量被使用之前必须先声明以及赋值(避免出现错误)
就书写格式而言，建议在声明和语句之间留出一个空行.

##### 2.4.3 赋值

变量通过**赋值**（assignment）的方式获得值.

eg.

height = 8; 
length = 12; 
width = 10;

float类型赋值: profit = 2150.48f;

##### 2.4.4 显示变量的值

用printf 可以显示出变量的当前值.

printf("Height: %d\n", height);

%d为整型int的占位符.

**`程序` 计算箱子的空间重量**

> 运输公司特别不喜欢又大又轻的箱子，因为箱子在卡车或飞机上运输时要占据宝贵的空间。
> 事实上，对于这类箱子，运输公司常常要求按照箱子的体积而不是重量来支付额外的费用。在
> 美国，通常的做法是把体积除以 166（这是每磅允许的立方英寸①数）。如果除得的商（也就是
> 箱子的“空间”重量或“体积”重量）大于箱子的实际重量，那么运费就按照空间重量来计算。
> （除数166是针对国际运输的，计算美国国内运输的空间重量时通常用194代替。） 
> 假设运输公司雇你来编写一个计算箱子空间重量的程序。因为刚刚开始学习C语言，所以
> 你决定先编写一个计算特定箱子空间重量的程序来试试身手，其中箱子的长、宽、高分别是12
> 英寸、10英寸和8英寸。C语言中除法运算用符号/表示。所以，显然计算箱子空间重量的公式
> 如下： 
> weight = volume / 166;
> 
> 这里的weight和volume都是整型变量，分别用来表示箱子的重量和体积。但是上面这个公式
> 并不是我们所需要的。在 C语言中，如果两个整数相除，那么结果会被“截短”：小数点后的
> 所有数字都会丢失。12英寸×10英寸×8英寸的箱子体积是960立方英寸，960除以166的结
> 果是5而不是5.783，这样使得重量向下舍入，运输公司则希望结果向上舍入。一种解决方案是
> 在除以166之前把体积数加上165： 
> weight = (volume + 165) / 166;
> 
> 这样，体积为166立方英寸的箱子的空间重量就为331/166，舍入为1；而体积为167立方英寸的
> 箱子的空间重量则为332/166，舍入为2。下面给出了利用这种方法编写的计算空间重量的程序。

```c
/* dweight.c */
/* Computes the dimensional weight of a 12" x 10" x 8" box */

#include <stdio.h>
int main(void)
{
    int height, length, width, volume, weight;

    height = 8;
    length = 12;
    width = 10;
    volume = height * length * width;
    weight = (volume + 165) / 166;

    printf("Dimensions: %dx%dx%d\n", length, width, height);
    printf("Volume (cubic inches): %d\n", volume);
    printf("Dimensional weight (pounds): %d\n", weight);

    return 0;
}
```

插曲:

<mark>vim复制内容到剪切板:</mark>

查看是否支持系统剪切板: `vim --version | grep clipboard`

总结快捷命令

- **复制整个文件**：*: %y+*

- **复制部分行**：*:10,20y+*

- **可视模式复制**：选中后按 *"+y

` 程序 `计算箱子的空间重量(改进版)

```c
/* Computes the dimensional weight of a box form input provided by the user */
/* dweight2. */
#include <stdio.h>
#include <stdlib.h>

#define INCHES_PER_POUND 166

int main(void)
{
    int height, length, width, volume, weight;
    printf("Enter height of box: ");
    scanf("%d", &height);
    printf("Enter length of box: ");
    scanf("%d", &length);
    printf("Enter width of box: ");
    scanf("%d", &width);
    volume = height * length * width;
    weight = (volume + INCHES_PER_POUND - 1) / INCHES_PER_POUND;
    printf("Volume (cubic inches): %d\n", volume);
    printf("Dimensinoal weight (pounds): %d\n", weight);

    system("pause");
    return 0;
}
```

