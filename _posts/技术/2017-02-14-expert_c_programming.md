---
title: C专家编程--笔记
layout: post
author: WenfengShi
category: 技术
tags: [读书笔记 c/c++]
---
博客链接: [http://codeshold.me/2017/02/expert_c_programming.html](http://codeshold.me/2017/02/expert_c_programming.html)

`typedef struct bar {int bar;} bar;` 的含义
为什么`extern char *p`和另一个文件中的`char p[100]`不能匹配？
什是总线错误（bus error）？什么是段违规（segmentation violation）？
`har *foo[]`和`char (*foo)[]`有何不同？

计算机日期
- UNIX的系统时间是从1970年1月1日(UTC)起按秒算的
- timestamp 是从1970年开始的
- 先有UNIX， 再有C
- 关于`time_t`什么时候其会重新回到开始？即达到尽头，运行如下代码即可（2038年）

        #include <stdio.h>
        #include <time.h>

        int main(){
            // time_t 是 long 的typedef形式（有符号）
            time_t biggest = 0x7FFFFFFF;
            // ctime 把时间转化为当地时间（含时区）
            printf("biggest = %s \n", ctime(&biggest));
            // gmtime 获取对应的UTC时间，但返回的不是一个可打印的字符串，故使用asctime
            printf("biggest = %s \n", asctime(gmtime(&biggest)));
            return 0;
        }

C语言排斥强类型，即其是弱类型
C语言的许多特性是为了方便编译器设计者而建立的：
- 数组下表从0开始
- 基本数据类型直接和底层硬件县官
- auto 关键字是摆设（它是缺省的内存分配模式，其只对创建符号表入口的编译器设计者有用）
- float被自动扩展为double（但在ANSI C中不在如此）
- 不允许嵌套，即函数内部包含另外一个函数

Steve Bourne编写的UNIX shell时，创立了一个C语言的变型， 其使用了很多“显示的结束语句”，如`if...fi`，且shell不用malloc，而使用`sbrk`自行负责堆存储管理，提高字符串处理效率。
Broune 事实上促成了国际C语言混乱代码大赛（The International Obfuscated C Code Competition）

应使用ANSI C 而不是 K&R C
ANSI C 对编译器做要求，如
- 在函数定义中形参个数的上限至少可以达到31个
- 在函数调用中实参个数的上限至少可以达到31个
ANSI C 与 K&R C 的不同
1. 新的、非常不同，且重要的东西(仅一个)
- ANSI C 把函数原型作为函数声明的一部分，原型的形式两者也有了很大的变化
2. 新增的关键字
- ANSI C 增加了 enum, const, volatile, signed, void等关键字
- 弃掉了K&R C中的entry等关键字
3. “安静的改变”
- 相邻字符串的面值会被自动连接在一起
- 寻常算数转换（usual arithmetic conversation）
    - K&R C 采用的是无符号保留（unsigned preserving）原则，即当一个无符号类型与int或更小的整型混合使用时，结果类型是无符号类型。
    - ANSI C 采用的是值保留（value preserving）原则，即当执行算数运算时，如果类型不同，就会发生转换。数据类型朝着浮点精度更高、长度更长的方向转换，整形数如果转换为signed不会丢失信息，就转换为signed，否则转换为unsigned —— 包括整型升级和寻常算数转换

            main(){
                if(-1 < (unsigned char)1)
                    printf("-1 is less than (unsigned char)1: ANSI semantics");
                else
                    printf("-1 NOT less than (unsigned char)1: K&R C semantics");
            }
4. 除上面之外的其他区别
- 符号粘贴（token-pasting）
- 三字母词（trigraph），即用3个字符表示一个单独的字符，如两字母词`\t`表示“tab”， 三字母词`??<`表示“开放的花括号”


    foo(const char **p) {}

    main(int argc, char **argv) {
        foo(argv);
    }

该代码会报一条warning，“argument #1 is imcompatible with prototype...”
原因分析(摘自ANSI C标准):
- 每个实参都应该具有自己的类型，这样它的值就可以复制给与它所对应的形参类型的对象（该对象的类型不能含有限定符）
- 要使得上述赋值形式合法，必须满足下列条件之一：
两个操作数都是指向有限定符或无限定符的相容类型指针，左边指针所指向的类型必须具有右边指针所指向类型的全部限定符
`故实参char *能和型参const char*匹配`
- `const float *`类型并不是一个有限定符的指针类型，它的类型是“指向一个具有const限定符的float类型的指针，即const修饰的是指针指向的类型而不是指针本身
`故char ** 和 const char ** 都是没有限定符的执政类型，但它们所指向的类型不一样，进而不相容`

`const`和`＊`通常只用于数组形式的参数中模拟传值调用！

`#pragma`用于向编译器提示一些信息，诸如希望把某个特定函数扩展为内敛函数，或者取消边界的检查。

一个‘L’的 NUL 用于结束一个ACSII 字符串；两个‘LL’的 NULL 用于表示什么也不指向（空指针）
一个遵循标准的C编译器至少允许一条switch语句中有257个case标签
使用`switch... case...break...`时，养成添加`/* fall through */`的习惯
字符串数组初始化（枚举声明、单行多变量声明等），最后一个尾巴`,`，ANSI C rationel对其的解释是：它使得C语言在自动生成时更容易些！
几乎没有人习惯在函数明前添加存储类型说明符，所以绝大多数函数都是全局可见

C语言的符号重载
- `static` 在函数内部时，表示该变量的值在各个调用间一直保持着连续性；……
- `void` 位于参数列表中，表示没有参数；……
- `()` 调用一个函数；定义带参数的宏；包围sizeof操作符的操作数（如果它是类型名）

当sizeof操作数是个类型时，其必须加上`()`，若是变量则不必加括号（建议加）
`i = 1, 2;` 中 i 的结果是？（1）
    - 优先级
    - 结合性

`/`可对一些字符转义，包括newline（即回车键，表示连接）


不充分的参数解析，shell参数解析
- 找出目录中的链接文件
    - `ls -l | grep ->`，`ls -l | grep "->"` 均不行
    - `ls -AF | grep "@"` 或者 `file -h | grep link`

`ratio = *x/*y;` 会报错

错误检查程序，lint程序。cstyle

存储类型说明符(storage-class)：extern static register auto typedef
类型限定符(type-qualifier): const volatile

“在函数调用时，参数按照从右到左的次序压到堆栈里”这种说法过于简单，参数在传递时首先尽可能地存放到寄存器中（追求速度）

结构体
- 一般形式
        struct 结构标签 (可选) {
            类型1 标志符1；
            类型2 标志符2；
            ……
        } 变量定义(可选);
- 结构中允许存在位段、无名字段以及字对其所需的填充字段
        struct pid_tag {
            unsigned int inactive : 1;
            unsigned int : 1; // 1个位的填充
            unsigned int refcount : 6;
            unsigned int : 0; //填充到下一个字边界
            short pid_id;
            struct pid_tag *link;
        }
- 位段的类型必须是int，unsigned int 或 signed int（或加上限定词）

联合
- 一般形式
        union 结构标签 (可选) {
            类型1 标志符1；
            类型2 标志符2；
            ……
        } 变量定义(可选);
- 节省存储空间 && 提取单独的字节字段（联合不需要额外的赋值和强制类型转换，同一个数据可解释为两个不一样的东西），如下`value.byte.c0`
        union bits32_tag {
            int whole;  /* 一个32位的值 */
            struct {char c0, c1, c2, c3;} byte;  /* 4个8位的字节 */
        } value;

枚举
- 把一串名字和一串整型值联系在一起

如果const（或）volatil关键字的后面紧跟着类型说明符（如int，long等），它作用于类型说明符。在其他情况下，const和（或）volatil 关键字作用于它左边紧邻的指针星号

分析以下声明：
- `char * const *(*next)();`
- `char *(* c[10])(int **p);`

typedef关键字并不是创建一个变量，而是宣称“这个名字是指定类型的同义词”
`typedef struct foo{...foo;}`的含义
C语言中存在多种名字空间：
    - 标签名（label name)
    - 标签（tag）
    - 成员名
    - 其他
对于`typedef struct baz {int baz;} baz;` 即相当于 `typedef struct baz {int baz;} baz_type;` 即typedef声明引入了baz_type作为`struct baz {int baz;}`的简写形式
    - `struct baz xxxxx; `使用的是结构标签
    - `baz yyyyy;` 使用的是结构类型

C语言声明解释程序


数组和指针并不相同
`extern int *x;`和`extern int x[]`
声明相当于普通的声明：它所说明的并非本身，而是描述其他地方的创建的对象
定义相当于特殊的声明：它为对象分配内存

左值：可修改的左值（允许出现在复制语句左边）和不可修改的左值
数组名是左值，但不能作为赋值的对象（左值即为可取地址的值），编译器为每隔对象分配一个地址（左值）
`char *p = "abcdefgh"; ...p[3]` 先取符号表中p的地址；提取存储于此处的指针；把偏移量和指针相加，产生一个地址；访问这个地址，取得内容
`char a[] = "abcdefgh"; ....a[3]` 先取符号表中a的地址；把偏移量和这个地址相加，产生一个地址；访问这个地址，取得内容
初始化指针时所创建的字符串敞亮被定义为只读！
回文！

定义是声明的特殊情况，它分配内存空间，并可能提供一个初始值
对编译器而言，一个数组就是一个地址，一个指针就是一个地址的地址。

数组和指针相同的情况：
1. 表达式中数组名（与声明不同）被编译器当作一个指向该数组第一个元素的指针
    - “表达式中的数组”就是指针
    - `a[6] = ...; 6[a] = ...;` 两种形式都正确
    
            fun1(int arr[]) {
                int tmp[] = {1, 2, 3};

                printf("%#x\n", &arr);
                printf("%#x\n", arr);
                printf("%#x\n", &(arr[0]));

                printf("%#x\n", &tmp);
                printf("%#x\n", tmp);
                printf("%#x\n", &(tmp[0]));
            }

2. 下标总是与指针的偏移量相同
    - C语言把数组下标作为指针的偏移量
    - 处理以一维数组时，指针并不见的比数组快
3. 在函数参数的声明中，数组名被编译器当作指向该数组第一个元素的指针
    - “作为函数参数的数组名”等同于指针
    - 下面代码运行正常

            fun2(int arr[]) {
                arr[1] = 3;
                *arr = 3;
                arr = array2;
            }

在C语言中，所有非数组形式的数据均以传值形式调用

指针就是指针，只是可以通过下表的形式对其进行访问
用a[i]这样的形式对数组进行访问总是被编译器“改写”或解释为像`*(a+1)`这样的指针访问

多维数组初始化时，可省略最左边下标的长度（也只能是最左边），如`int rhubarb[][3] = {{0, 0, 0}, {1, 1, 1},};`

sizeof(数组名)返回的是数组总的字节数
- 下面代码，运算结果如下 `sizeof(str):15 func sizeof(str):8`

        #include <stdio.h>

        int func(char str[]) {
            return sizeof(str);
        }
        int main(){
            char str[] = "abcdefghijklmn";

            printf("sizeof(str):%lu\n", sizeof(str));
            printf("func sizeof(str):%d\n", func(str));
        }


指针数组就是Iliffe向量, `char *pea[4]`
指针数组必须用指向为字符串而分配的内存的指针进行初始化

“数组名被改写成一个指针参数”规则并不是递归定义的。数组的数组会被改写成为“数组的指针”，而不是“指针的指针”

        |实参｜所匹配的形式参数|
        |:-----:|:-------:|
        |数组的数组`char c[8][10];`| `char (*)[10]` 数组指针|
        |指针数组`char *c[15]`| `char **c` 指针的指针|
        |数组指针（行指针）`char (*c)[64]`| `char (*c)[64]` 不改变|
        |指针的指针 `char **c`| `char **c` 不改变|


        func1(int fruit[2][3][4]) { ; }
        func2(int fruit[][3][4]) { ; }
        func3(int (*fruit)[3][4]) { ; }

向函数传递一个一位数组：增加一个额外的参数或者赋予数组最后一个元素一个特殊的值
有没有办法向函数传递一个普通的多维数组：必须提供除了最左边一维以外多有维的长度。即多维数组最主要的一维长度不必显式书写。

strings实用程序可帮助从二进制文件内部查看程序可能产生的错误。

不使用临时变量交换两个值

    a ^= b;
    b ^= a;
    a ^= b;


`cc -S -Xc banana.c`, `-S`选项使编译器停在汇编阶段，`-Xc`选项告诉编译器拒绝任何不符合ANSI C的代码结构



1. 怎样检测到链表中存在循环
2. C语言中不同增值语句的区别（考虑变量和指针等多种情况）

        x = x + 1;
        ++x;
        x++;
        x += 1;

3. 库函数调用和系统调用的区别
    - 典型的C函数库：system, fprintf, malloc
    - 典型的系统调用：fork, chdir, write, brk
    - 函数库调用：在所有ANSI C编译器版本中，C函数库是相同的；语言或应用程序的一部分；用户地址空间执行；调用函数库中的一个程序；运行时间属于用户时间；属于过程调用，开销较小；C函数库libc中大约300个程序；记录于unix手册第三节
    - 系统调用：各个操作系统的系统调用不相同；操作系统的一部分；内核地址空间执行；调用系统内核的服务；运行时间属于系统时间；需要内核上下文切换，开销较大；在UNIX中大约有90个系统调用
4. 文件描述符和文件指针有何不同
    - 文件描述符（用于索引开放文件的每个进程表 per-process table-of-open-files）就是开放文件的每个进程表的一个偏移量（如3），其用于unix系统调用中，用于标识文件
    - FILE 指针保存了一个FILE结构的地址。FILE结构用于表示开放的I/O流。它用于ANSI C标准I/O库中，用于标识文件 (stdio.h)，结构内容依平台而不一样，在unix中通常是开放文件的每个进程表的一个条目！
5. 编写代码，确定一个变量是有符号数还是无符号数
    - 如下代码能适用于K&R C，但由于类型提升无法适用于ANSI C

            #define ISUNSIGNED(a) (a >= 0 && ~a >= 0)
            或
            #define ISUNSIGNED(type) ((type)0 - 1 > 0)

6. 打印一颗二叉树的时间复杂度是多少？
    - 如果execvc 系统调用成功，它将返回什么？（失败才会返回）
7. 从文件中随机提取一个字符串
    - 对字符串进行计数，并记录每个字符串的偏移位置
    - 如果只能顺序遍历文件一次，且不能实用表格来存储字符串的偏移位置，怎么计算？（提示：概率）
8. 如何用气压计测量建筑物的高度
