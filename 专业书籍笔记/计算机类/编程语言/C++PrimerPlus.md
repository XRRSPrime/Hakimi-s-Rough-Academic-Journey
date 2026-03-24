# C++PrimerPlus 笔记
## 目录

## 一、我的第一个程序——基础知识<a id="基础知识"></a>
```cpp
// first.cpp --print "Hello, world."
#include <iostream>
using namespace std;
int main(){
    cout << "Hello, world." << endl;
    return 0;
}
```
这是一个非常简短的一个程序，我第一个接触到的C++程序是这个（我在python语言也差不多都是从打印"Hello, world."开始）。这个程序有下面的要素：
- 注释：`//`
- 预处理指令：`#include`
- 头文件：`<iostream>`
- 关键字：`using`
- 名称空间：`namespace`
- 函数：`int main(){···}`
- cout(cin)；`cout`
- 语句：`cout << ···` 和 `return`

我们一个一个看：
### 1.1 C++注释<a id="注释"></a>
C++ 继承了 C 语言的注释方法，同时也有着自己的注释方式：
   - `//`：C++ 风格注释，适用于**单行注释**。
   - `/**/`：C 风格注释，适用于**多行注释**。
   - 两者可以混用，在 VS 和 VScode 中快捷键`Ctrl + /`将选中的部分进行注释。
### 1.2 预处理指令`#include` <a id="预处理指令"></a>
几乎所有的 C++ 文件都会有`#include`语句<a id="语句"></a>，这种语句叫做**预处理指令**，我们通过这个命令在程序运行之前进行一些操作（比如联编一些我们的文件和我们的库）。
   **只要是以`#`开头的都是预处理指令**。
### 1.3 库和头文件`<iostream>` <a id="库和头文件"></a>
`<iostream>` 是一种文件，他被包含在其他文件中，又称包含文件，也叫**头文件**。C 语言和 C++ 的头文件有所区别，C 语言中的头文件都是有扩展名 `.h` 的[^1]，但是 C++ 中的头文件是没有扩展名的，当然对于一些老式的 C 语言的头文件 C++ 保留了扩展名。
`<iostream>`包含在**C++标准库**中，头文件中包含[声明](#声明语句)，而库是**实现**，库中包含了头文件中的声明的具体实现。也就是说：**头文件是接口[^2]，而库是实现**。
两者还是有区别的：头文件是**源码格式（文本格式）**，库是**经过编译的二进制文件**。库是一个庞大的集合，里边包含了头文件中所包含的声明的实现。
### 1.4 关键字`using`：<a id="using关键字"></a>
`using` 是一种起**引入**或着是**起别名**功能的关键字。
1. 引入名称空间，或者是指定的成员：
    ```cpp
    using namespace std;
    using std::cout;
    using Base::func;
    ```
    这三个语句都是合理的。第一个是引入名称空间`std`，第二个是引入一个成员`std::cout`。第三个与类有关：
    - 恢复被遮蔽的函数：
    如果子类重写了父类的一个函数，父类的同名重载函数会被“隐藏”。用 using 可以把它们找回来
    ```cpp
    class Base {
    public:
        void show(int i) { ... }
    };

    class Derived : public Base {
    public:
        using Base::show; // 告诉编译器，把父类的 show 也包含进来
        void show(string s) { ... } // 否则这里会把父类的 show(int) 遮蔽掉
    };
    ```
    - 继承构造函数 (C++11)
    ```cpp
    class Base {
    public:
        void show(int i) { ... }
    };

    class Derived : public Base {
    public:
        using Base::show; // 告诉编译器，把父类的 show 也包含进来
        void show(string s) { ... } // 否则这里会把父类的 show(int) 遮蔽掉
    };
    ```
2. 类型别名
    在C++11以后我们可以给一些类型起外号：
    ```cpp
    // 以前的方法 (C语言风格)
    typedef unsigned long ulong;

    // 现代 C++ 方法 (更清晰)
    using ulong = unsigned long;
    using vec_int = std::vector<int>;

    // 甚至可以给函数指针起别名
    using FuncPtr = void(*)(int, double);
    ```
### 1.5 名称空间`namespace`<a id="名称空间"></a>
名称空间是用来限定类、函数、变量等C++组件的。由于命名的自由性，我们很容易就会发生命名重复（这个在C++中有其他的含义），就会导致很混乱。因此，我们需要引入名称空间来限定名称的适用范围。要使用名称空间的名称，我们要用`namespace::func_name`的格式来使用，其中`::`被称为**域名解析符**。

### 1.6 语句<a id="语句"></a>
在C++中，每条完整的指令都被称为**语句**，且所有的语句都以**分号**结束。
#### 1.声明语句 <a id="声明语句"></a>
在C/C++中我们需要明确信息的存储位置和所需的内存空间，这个时候就需要我们的**声明语句**。声明语句的结构：`Typename + Variable-name`。这里的`typename`是类型名，可以是任何一种类型，`variable-name`是变量名/函数名，总之是一种标签。
###### 声明与定义：
程序中的声明语句叫做定义声明语句，简称**定义**。定义意味着他将命令编译器为变量分配内存空间。但是声明还有不分配空间的情况，称为**引用声明**。
声明和定义的关系：定义 = 声明 + 分配内存
| 关键词/特征 | 声明 (Declaration) | 定义 (Definition) |
| :--- | :---: | :---: |
| `extern` (无初始化) | ✅ | ⬜ |
| `extern` (有初始化) | ✅ | ✅ |
| `static` 全局变量 | ⬜ | ✅ |
| `struct` 标签 | ✅ | ✅ |
| 函数原型 (无 `{ }`) | ✅ | ⬜ |
| 函数体 (`{ ... }`) | ⬜ | ✅ |
| `typedef` / `using` | ✅ | ⬜ |
| `&` 引用 | ✅ | ✅ |
##### 2.赋值语句<a id="赋值语句"></a>
符号 `=` 叫做赋值运算符，与数学中的`=`不同，他表示**将值绑定到该标签**。我们允许连续赋值，比如下面的例子
```cpp
int a, b, c;
a = b = c = 0;
```
这个时候的赋值顺序是`0->c->b->a`，即从右向左赋值。这种连续赋值在C中是不允许的，是C++的特性之一。
```cpp
int a = 10;
a = a - 1; // a = 9
```
由于赋值语句是将值绑定到标签上。所以`a = a - 1`实际上是计算了表达式`a - 1`后，将评估结果（值）绑定到了`a`上（尽管在数学上这看起来很奇怪）。

### 1.7 关键字
关键字是计算机语言中的词汇。比如 int、void、return和double。由于这些关键字都是C++专用的，因此不能用作他用。也就是说，不能将return用作变量名，也不能把double用作函数名。不过可以把它们用作名称的一部分，如painter（其中包含int）或return_aces。
另外，main不是关键字，由于它不是语言的组成部分。然而，它是一个必不可少的函数的名称。可以把main用作变量名。同样，其他函数名和对象名也都不能是关键字。然而，在程序中将同一个名称（比如cout）用作对象名和变量名会把编译器搞糊涂。也就是说，在不使用cout对象进行输出的函数中，可以将cout用作变量名，但不能在同一个函数中同时将cout用作对象名和变量名。

### 1.8 函数和`cin/cout`
这两个在后面有单独的记录，此处先略过，也可以直接跳转：[函数](#函数)，[cin/cout](#输入/输出)

## 二、简单数据<a id="简单数据"></a>
### 2.1 变量简介<a id="变量简介"></a>
在编程中，变量是程序为了存储数据而申请的一块内存空间。
为了把信息寻出在计算机中，程序必须记录3个基本属性：
- 信息将存储在哪里（内存位置）
- 要存储什么值（变量内容）
- 存储何种类型的信息（变量类型）

### 2.2 变量名<a id="变量名"></a>
C++ 中的变量名既自由又有约束。C++变量名需要遵循以下规则：
- 在名称中只能使用字母字符、数字和下划线(_)
- 名称的第一个字符不能是数字
- 区分大小写
- 不能将C++关键字用作名称
- 以两个下划线或下划线和大写字母打头的名称被保留给实现（编译器及其使用的资源）使用。以一个下划线开头的名称被保留给实现，用作全局标识符。
- C++对名称的长度是没有限制的，名称中的所有字符都有意义。但有些平台有长度限制。

###### 对于上方的倒数第二点，C/C++规定：任何以`__`和`_大写字母`开头的名字，保留给**实现**使用。普通程序员不要定义这种名字，避免与编译器或者标准库冲突。[^3]
```cpp
int __myVar;  // ❌ 不要用，保留给编译器
int _Xyz;     // ❌ 不要用
```
下面是一些命名例子:
```cpp
int poodle;      // valid
int Poodle;      // valid and distinct from poodle
int POODLE;      // valid and even more distinct
Int terrier;     // invalid -- has to be int, not Int
int my_stars3;   // valid
int _Mystars3;   // valid but reserved -- starts with underscore
int 4ever;       // invalid because starts with a digit
int double;      // invalid -- double is a C++ keyword
int begin;       // valid -- begin is a Pascal keyword
int __fools;     // valid but reserved -- starts with two underscores
int the_very_best_variable_i_can_be_version_112;  // valid
int honky-tonk;  // invalid -- no hyphens allowed
```
C/C++中的命名方式是自由的，我们可以在不违反规定的情况下随意命名，但是一个好的命名习惯是必要的，这不仅是工程实践的经验，也是为了自己看着舒服。我们也可以在我们的变量名前加入一些前缀，这方便我们进行判读这些我们写的变量的用途（包会忘的）例如，可以将整型变量myWeight命名为nMyWeight，其中前缀n用来表示整数值，在阅读代码或变量定义不是十分清楚的情况下，前缀很有用。另外，这个变量也可以叫做intMyWeight，这将更精确，而且容易理解，不过它多了几个字母（对于很多程序员来说，这是非常讨厌的事）。常以这种方式使用的其他前缀有：str或sz（表示以空字符结束的字符串）、b（表示布尔值）、p（表示指针）和c（表示单个字符）。

### 2.3 整形变量<a id="整形变量"></a>
整型就是我们在数学上所说的整数（没有小数部分）有些语言只提供一种整型（一种类型满足所有要求！），而C++则提供好几种，这样便能够根据程序的具体要求选择最合适的整型。
不同C++整型使用不同的内存量来存储整数。使用的内存量越大，可以表示的整数值范围也越大。另外，有的类型（符号类型）可表示正值和负值，而有的类型（无符号类型）不能表示负值。术语宽度
（width）用于描述存储整数时使用的内存量。使用的内存越多，则越宽。
C++中的整型(按照宽度递增排列)分别是`char, short, int, long, long long(C++11新增)`，这些都有器有符号版本(signed)和无符号版本(unsigned)。其中的`char`最常用来表示字符。对于 char 我们放在后面单说，也可以直接[跳转](#char类型)
#### 2.3.1 整型short、int、long和long long<a id="整型short、int、long和long long"></a>
计算机内存由一些叫做位（bit）的单元组成。C++的short、int、long和long long类型通过使用不同数目的位来存储值，最多能够表示4种不同的整数宽度。但是内存是有限的，即他们的上线是被锁死的，但是C++保证了他们的最小长度。如下所示：
- short >= 16bit
- int >= short
- long >= 32bit and long >= int
- long long >= 64bit and long long >= long

计算机内存的基本单元是位（bit）。可以将位看作电子开关，可以开，也可以关。关表示值0，开表示值1。字节（byte）通常指的是8位的内存单元。从这个意义上说，字节指的就是描述计算机内存量的度量单位，在C++中，字节也是最小的可寻址的内存单位。且其中的位因为是连续的，不会分散在内存里。C++ 中的字节是连续的位组成的存储单位，它必须至少有足够的不同取值来表示 C++ 的基本字符集。
当前的系统搜使用的是最小长度。笔者所使用的windows系统对应的他们的所占字节如下：
| 名称 | `short` | `int` | `long` | `long long` |
| :--: | :--: | :--: | :--: | :--: |
| 字节数 | 1 | 4 | 4 | 8 |
| 表示范围 | 一般用来表示字符 | $-2^{31}$ 到 $2^{31}-1$ | $-2^{31}$ 到 $2^{31}-1$ | $-2^{63}$ 到 $2^{63}-1$ |

这些类型都是C++中内置的类型，使用方法也和int一样。
```cpp
short score;             // creates a type short integer variable
int temperature;         // creates a type int integer variable
long position;           // creates a type long integer variable
```

实际上short是short int的简称，而long是long int的简称。

要知道系统中整数的最大长度，可以在程序中使用C++工具来检查类型的长度。首先，sizeof运算符返回类型或变量的长度，单位为字节。其次，头文件climits（在老式实现中为limits.h）中包含了关于整型限制的信息。具体地说，它定义了表示各种限制的符号名称。例如，`INT_MAX`为int的最大取值，`CHAR_BIT`为字节的位数。

#### 2.3.2 运算符sizeof和头文件climits<a id="sizeof和climits"></a>
在C++文件中，我们可以使用`sizeof`运算符来获取简单类型对象的内存长度（对于数组等则是获取对应的元素个数）。**运算符**是内置的语言元素，对一个或多个数据进行运算，并生成一个值。`sizeof`可以计算类型的所占内存（单位：byte）。对于单纯的类型名，我们要将他放在`()`中，但是如果是变量名，我们可以省略掉`()`
```cpp
sizeof(int); // Windows中会输出 4
sizeof n_short; //显示n_short对应类型的内存大小
```
头文件climits定义了符号常量来表示类型的限制。下面是对应的符号常量和其对应的含义：
| 符号常量 | 表示 |
| :--- | :--- |
| CHAR_BIT | char的位数 |
| CHAR_MAX | char的最大值 |
| CHAR_MIN | char的最小值 |
| SCHAR_MAX | signed char的最大值 |
| SCHAR_MIN | signed char的最小值 |
| UCHAR_MAX | unsigned char的最大值 |
| SHRT_MAX | short的最大值 |
| SHRT_MIN | short的最小值 |
| USHRT_MAX | unsigned short的最大值 |
| INT_MAX | int的最大值 |
| INT_MIN | int的最小值 |
| UNIT_MAX | unsigned int的最大值 |
| LONG_MAX | long的最大值 |
| LONG_MIN | long的最小值 |
| ULONG_MAX | unsigned long的最大值 |
| LLONG_MAX | long long的最大值 |
| LLONG_MIN | long long的最小值 |
| ULLONG_MAX | unsigned long long的最大值 |

#### 2.3.3 整形类型的使用<a id="整形类型的使用"></a>
C++提供了大量的整型，通常我们是使用`int`，对于其他的C++整型也有一些使用的原因。
1. long和long long的使用
如果知道变量可能表示的整数值大于16位整数的最大可能值，则使
用long。即使系统上int为32位，也应这样做。这样，将程序移植到16位
系统时，就不会突然无法正常工作（这里是因为在Windows系统中int为32位和long相同，但是Linux系统中int为16位。）如果要存储的值超过20亿，可使用long long。
2. short的使用
short比int小我们可以用short来节省内存（这是一个非常好的编程习惯！）通常，仅当有大型整型数组时，才有必要使用short来节省内存。
3. int的使用
遇事不决用int就行(doge)，具体原因如下：
   1. **CPU 的“原生”处理能力 (最核心原因)**
   现代 CPU（尤其是 32 位和 64 位架构）在处理`int`类型时是最快的。
   - 字对齐（Word Alignment）： CPU 读取内存通常是一次读取 32 位或 64 位。如果你定义一个 int，它正好适配 CPU 的总线宽度。
   - 大多数 CPU 的算术指令（加减乘除）是直接针对“寄存器大小”优化的。即便你定义一个 char（8 位）变量进行加法运算，CPU 往往也会先把它提升为 int，做完计算后再截断存回内存。这就使得我们用char反而还增加了运算的任务。
   1. **数值溢出的风险**
   char 的表示范围非常小（通常是 -128 到 127）。在实际编程中，稍微大一点的计数器或者循环变量，甚至是一次简单的加法，都很容易造成溢出（Overflow）。
   2. **代码的可读性和可维护性**
   当你在代码中看到 int 时，潜意识里会认为这是一个“正常的数字”。如果你看到 char，大脑会下意识地去想：“这里是不是要处理字符？是不是要处理 ASCII 码？还是这里有极端的内存限制？”使用 int 可以减少沟通成本，避免误解。

#### 2.3.4 整形字面值<a id="整形字面值"></a>
字面值[^4]是指你在代码里直接写出来的值。之所以叫“字面值”，是因为你一眼就能从“字面上”看出它的值和类型，而不需要通过变量名去查找。
|进制(base)|10|8|16|
|:--:|:--:|:--:|:--:|
|格式|正常表示|第一位用0第二位为1到7|前两位为0x或0X|
|例子| 114514 | 042(等同于34) | 0xF(等同于15)、0XA5(等同于165) |
在默认情况下，cout使用十进制格式显示整数（方便我们的日常使用）[^5]。虽然我们眼中这些进制有所区别，但是对于计算机来说他们都一样，因为他们都要被以二进制存储。

#### 2.3.5 常量<a id="常量"></a>
**常量**是不允许修改的值，我们在进行初始化以后就不允许再次进行修改。
我们使用**声明**将整型变量的类型告诉了C++编译器，但是对于像114514这样的常量，除非有理由存储为其他类型（如使用了特殊的后缀来表示特定的类型，或者值太大，不能存储为int），否则C++将整型常量存储为int类型。
先来看看后缀。后缀是放在数字常量后面的字母，用于表示类型。
|后缀| u / U |l / L | ul(顺序任意，大小写任意) | ll / LL | ull等（同ul要求）|
|:--:|:--:|:--:|:--:|:--:|:--:|
|含义|unsinged int|long|unsigned long|long long|unsigned long long|
再来看看长度。在C++中，对十进制整数采用的规则，与十六进制和八进制稍微有些不同。对于不带后缀的十进制整数，将使用下面几种类型中能够存储该数的最小类型来表示：int、long或long long。对于不带后缀的十六进制或八进制整数，将使用下面几种类型中能够存储该数的最小类型来表示：int、unsigned int/long、unsigned long、long long或unsigned long long。这是因为十六进制常用来表示内存地址，而内存地址是没有符号的，因此，usigned int比long更适合用来表示16位的地址。

### 2.4 初始化<a id="初始化"></a>
#### 2.4.1 传统初始化<a id="初始化1"></a>
初始化是将赋值和声明合并在一起。
`int x = 0;`
我们也可以用字面值常量来初始化，用另一个已经定义过的变量来初始化另一个变量。
```cpp
int x = 0;
int y = x;
cout << y;  //output: 0
```
当然我们也可以用表达式来这么做
```cpp
int x1 = 0;
int x2 = 1;
int x3 = x1 + x2;
cout << x3;  //output: 1
```
这种初始化应注意我们用来初始化的变量或者内容是先前定义好的。如果出现了未声明或定义的变量，编译器就会报错。
除此之外，我们还有另外的一种C++独有的初始化方式：
```cpp
int owls = 101;   //traditional C initialization, sets owls to 101.
int wrens(432);   //alternative C++ syntax, set wrens to 432.
```
###### 这里的`wren(432)`类似于我们定义类时用到的**构造函数**
如果我们知道变量的初始值应该是什么，对他进行初始化是一个非常好的变成习惯，可以避免忘记给变量赋值的情况发生。
#### 2.4.2 C++11 风格的初始化<a id="初始化2"></a>
除了上面的初始化方式，C++还有一种，这种方式用于数组和结构，但在C++98中我们还可以用于单值变量：
`int hamburgers = {24}; // set hamburgers to 24`
在C++11中我们使用这种情形就变多了，首先在这种方式下，我们既可以使用等号也可以不使用。
```cpp
int emus{7};    //set emus to 5
int rheas = {12};    //set rheas to 12
```
在这种情况下我们的大括号中可以包含数据可以为空，为空的时候我们就会将变量初始化为0
```cpp
int psychics = {};     //set psychics to 0
int rocs{}    //set rocs to 0
```
这样的初始化有一个好处：**更好的防范类型转换错误。**
###### 以前，C++使用不同的方式来初始化不同的类型：初始化类变量的方式不同于初始化常规结构的方式，而初始化常规结构的方式又不同于初始化简单变量的方式；通过使用C++新增的大括号初始化器，初始化常规变量的方式与初始化类变量的方式更像。C++11使得可将大括号初始化器用于任何类型（可以使用等号，也可以不使用），这是一种通用的初始化语法。

### 2.5 char类型<a id="char类型"></a>
char很特殊，**内存仅占有 1 byte**，他的**大小范围是-128到127**。虽然char经常用来处理字符，但是我们也可以用它来表示比short更小的整型。对于字符表示，我们有不同的表示方法。目前我们常接触到的ASCII是美国制定定的标准。但是这只是一个方言，并不能很好的实现国际化（例如IBM大型机使用的EBCDIC编码也是一种表示方法）。为了让全世界的文字都能在电脑里显示，人们发明了 Unicode（万国码）。它给世界上的每一个字符都分配了一个唯一的数字编号。但是Unicode需要表示的字符太多，传统的 char 类型（通常只占 1 个字节，即 8 位）装不下这么大的数字。所以，C++ 引入了 `wchar_t`（宽字符类型），它的内存空间更大（通常占 2 字节或更多），专门用来存储这些“国际化”的大数字。
在程序中，我们使用char来表示字符，但是在内存中，我们是以数字的形式进行保存的，比如‘M’，我们查看内存可知是77，但是在输出和输入的时候我们看到或使用的却是M，这是因为智能的cin和cout。输入时，cin将键盘输入的M转换为77；输出时，cout将值77转换为所显示的字符M。在程序中，我们书写字符是应使用单引号`''`而不是双引号`""`（双引号代表的是**字符串**，两者有很大区别）。
由于char是一个整数，我们也可以对其进行整数操作。
```cpp
char ch = 'A'
cout << ch + 1;   //输出66
```
这里的输出设计了[自动类型转换](#类型转换)，在C++中，当char类型数据参与算术运算的时候，编译器会自动将char转换成int型。此时的cout检测到后面的表达式是`int`型，所以输出的是66。
#### 2.5.1 char字面值<a id="char字面值"></a>
在C++中，书写字符常量的方式有多种。对于常规字符（如字母、标点符号和数字），最简单的方法是将字符用单引号括起。这种表示法代表的是字符的数值编码。这种表示法更加清晰，且不需要知道编码方式，在不同的编译器中都能用。
但是总有一些例外，比如我们的`""`，他们不能简单的用这种表示，因为C++赋予了他们新的含义——用来表示字符串，如果强制用的话会使编译器报错。因此，C++使用了一种特殊的表示方法——**转义序列**。转义序列在很多编程语言中都有涉及，我们在使用的时候，需要在要转义的字符前加上`\`来表修饰。
#### 2.5.2 特殊字符<a id="特殊字符"></a>
对于一些特殊的祖字符，他们在C++语言中是有含义的，但是我们依然想用这些字符，这时我们可以使用转义符`\`来进行表示，下面的表格是他们的对应含义：
| 字符名称 | ASCII 符号 | C++ 代码 | 十进制 ASCII 码 | 十六进制 ASCII 码 |
| :--- | :--- | :--- | :--- | :--- |
| 换行符 | NL (LF) | `\n` | 10 | 0xA |
| 水平制表符 | HT | `\t` | 9 | 0x9 |
| 垂直制表符 | VT | `\v` | 11 | 0xB |
| 退格 | BS | `\b` | 8 | 0x8 |
| 回车 | CR | `\r` | 13 | 0xD |
| 振铃 | BEL | `\a` | 7 | 0x7 |
| 反斜杠 | \ | `\\` | 92 | 0x5C |
| 问号 | ? | `\?` | 63 | 0x3F |
| 单引号 | ' | `\'` | 39 | 0x27 |
| 双引号 | " | `\"` | 34 | 0x22 |

虽然我们表示时需要转义的字符很多，但是我们在C++11以后需要转义的还剩5个：
- `\n`(换行符)：用于结束当前行
- `\\`(反斜杠)：表示文字路径和处理特殊字符是不可或缺
- `\'`(单引号)：在字符常量中必须使用
- `\"`(双引号)：在字符串中包含双引号时必须使用
- `\t`(水平制表符)：在控制台对齐输出时偶尔会用到。
剩下的进本已经放弃使用/极少使用。这里建议实在是要用或者是看到了一些很老的C++文件，我们直接问问AI吧。
换行符可以替代endl，但是还是要记得endl有`\n`所不具备的功能，清空缓冲区。显示数字时，使用endl比输入“\n”或‘\n’更容易些，但显示字符串时，在字符串末尾添加一个换行符所需的输入量要少些。
对于一些我们常用的、但是不能用简单字符表示的，我们可以用八进制或十六进制的方式表示。例如Ctr+Z的ASCII码为26，对应的八进制编码为032，十六进制编码为0x1a。可以用下面的转义序列来表示该字符：\032或\x1a。
- 八进制格式：`\ooo`，即`\`+八进制数字。
- 十六进制格式：`\x`+十六进制数字，需要注意，我们的十六进制表示要尽可能多的读取后面的十六进制字符。
相较于八进制，我们还是推荐十六进制，因为十六进制不易出错。
对于以上转义，我们在使用的时候为了避免歧义，在必要位置要进行分割，否则会出现一些奇怪的结果。
#### 2.5.3 通用字符名<a id="通用字符名"></a>
C++标准允许实现提供扩展源字符集（在键盘上的符号集）和扩展执行字符集（在程序执行过程中能过显示到显示器上的字符）[^6]。C++有一种表示这种特殊字符的机制，它独立于任何特定的键盘，使用的是通用字符名（universal character name）。这个通用字符名的本意就是让部分地方语言也能够进行计算机编码（比如简体中文/日文/法文等等）。通用字符名可以用`\u`和`\U`打头。后面跟的分别是八进制和十六进制。

#### 2.5 4 wchar_t<a id="wchar_t"></a>
对于一些我们无法用8位一字节的方式表达的文字系统，C++有两种处理方式：一种是编译厂商将char定义为16或着更长的字节，另一种就是我们来实现一个能同时支持一个小型基本字符集和一个较大的扩展字符集。wchar_t（宽字符类型）可以表示扩展字符集。
wchar_t并不是一个固定大小的类型，在不同的厂商所定义的wchar_t有所不同。Windows中将这个定义为了2字节，相当于是`unsigned short`，但是在Linux和Mac系统中就被定义为了4字节。相当于是`int`。
**建议**：现代 C++ 已经引入了更明确的类型（如 char16_t 和 char32_t），它们在所有平台上大小都是固定的。如果你在做跨平台开发，建议优先使用 char16_t 或 char32_t，而不是 wchar_t。

传统的cin/cout是将输入和输出看成char流，无法处理wchar_t，但是在iostream中的最新版本提供了wcin和wcout来处理wchat_t流。对于字符常量和字符串，我们还可以通过加上前缀`L`来指示
```cpp
wchar_t bob = L'P';        // a wide-character constant
wcout << L"tall" << endl;  // outputting a wide-character string
```
这个对于开发者来讲一般是不用涉及的，但是我们要有了解，尤其是在进行国际编程或使用Unicode或ISO 10646时。

#### 2.5.5 char16_t和char32_t<a id="char16_t和char32_t"></a>
C++11新增了类型char16_t和char32_t，其中前者是无符号的，长16位，而后者也是无符号的，但长32位。C++11使用前缀`u`表示char16_t字符常量和字符串常量，并使用前缀`U`表示char32_t常量，前缀u和U分别指出字符字面值的类型为char16_t和char32_t。
**建议**：对于这两个类型，我们由3个实践来借鉴：
1. 存储和网络传输：一律用 char 存 UTF-8。它是目前互联网的事实标准，最省空间，兼容性最好。
2. Windows 界面开发： 必须习惯使用 wchar_t 或 char16_t，否则你的程序无法处理中文路径和复杂的用户名。
3. 算法处理： 如果需要频繁地按字符拆分、计数（比如写一个文本编辑器），转换成 char16_t 或 char32_t 处理会让你少掉很多头发。
如果你只写简单的控制台练习题，char 确实够了；但如果你要写一个正经的、给别人用的软件，Unicode 是你无法绕过的关卡，而 char16_t 就是你手中的武器之一。

### 2.6 无符号类型<a id="无字符类型"></a>
C++ 中的没有像`unsigned`关键字的，都是有符号数。但是无符号数它的优点是表示的范围变大了2倍（因为没有符号位了）要创建他们，只需要用关键字`unsigned`
来修改声明即可。
```cpp
unsigned short change;         // unsigned short type
unsigned int rovert;           // unsigned int type
unsigned quarterback;          // also unsigned int
unsigned long gone;            // unsigned long type
unsigned long long lang_lang;  // unsigned long long type
// 注意，unsigned本身是unsigned int的缩写
```
##### 对于溢出的科普：我们在学C++的时候所讨论的溢出的根本原因在于我们的内存有限制正如前文所述，计算机虽然强大，但是我们不能无限制的存储非常大的数字，我们在这里的类型也是如此，当我们存储了超出该类所该存储的数据范围就会发生数据溢出，这将会导致我们的结果发生非常大的改变，甚至出现一些非常离谱的ERROR，如果真的想试试的话可以用下面的程序试试
```cpp
#include <iostream>
#include <cmath>
using namespace std;
int main(){
    int overflow = pow(114, 514);     //这个数非常大，如果半天没反应可以用Ctrl+C强制结束进程。
    cout << overflow;
    return 0;
}
```
我在VScode上使用的时候是输出了-2147483648（很奇怪不是吗，正正得了负）。所以我们在进行编程的时候我们应非常注意溢出的问题（谁也不想我们的程序最后输出了一个奇奇怪怪的数字）。

### 2.7 bool类型 <a id="bool类型"></a>
在以前，C++和C一样是没有bool值的。C++将**非零值**解释为`true`，将**0**解释为`false`。现在我们可以用预定义的字面值`true`和`false`来表示真假了。
字面值`true`和`false`都可以提升转换成int型，true->1，false->0。另外任何数字或者指针值都可以进行隐式转换为bool值，任何非零值都被转换为true，而零被转换为false，任何非NULL或nullptr的都会转换成true，NULL和nulllptr会被转换成0。

### 2.8 const限定符<a id="const限定符"></a>
如果程序在多个地方使用同一个常量，则需要修改该常量时，只需修改一个符号定义即可。C++有一种比`#define`更好的处理符号常量的方法，这种方法就是使用const关键字来修改变量声明和初始化。格式如下：
`const int Number = 600`
这样我们就规定了一个常量 Number ，当我们以后每次使用 Number 的时候我们都相当于使用其所对应的值。但是，**这样定义的变量以后是不允许重新赋值的,也就是说，我们必须进行初始化名进行赋值[^1]**。这里的关键字 const 称为限定符，它限定了声明的含义。
创建常量的通用格式如下：
`const type name = initial_value`
###### const 变量的命名格式和习惯
- 名称首字母大写
- 整体名称大写
- 以字母 k 打头

如果你之前学习过 C 语言，在 C 语言中我们常用`#define NAME VALUE`来定义常量（这其实是在定义**宏**）。但是 const 比 #define 要好，首先 const 是能够明确指出其类型，其次可以使用C++的作用域规则将定义限制在特定的函数或文件中，第三可以将const用于更复杂的类型（后面会有介绍）。
还有一个就是，在 C++ 中我们可以使用 const 值来声明数组长度。

### 2.9 浮点数<a id="浮点数"></a>
他是 C++ 的第二组基本类型，可以表示小数部分的数字，对于它在计算机中的存储原理，可以学习一下**数字电子技术(Digigtal Fundamental by Thomas L. Floyd)**[^7]
#### 2.9.1 浮点数书写
C++ 中有两种书写浮点数的形式，一种是我们日常的写法：
```cpp
12.34       // floating-point
939001.32   // floating-point
0.00023     // floating-point
8.0         // still floating-point
```
由上例可知，即使小数部分为零，小数点也将会确保数字以浮点数表示。
第二种表示的方法叫做**E 表示法**，像这样：`3.45E6`，这意为 3.45 * 10^6，这里的 6 为指数，3.45 为底数。E 就表示为 10 的……次方。
```cpp
2.52e+8                 // can use E or e, + is optional
8.33E-4                 // exponent can be negative
7E5                     // same as 7.0E+05
-18.32e13               // can have + or - sign in front
1.69e12                 // 2010 Brazilian public debt in reais
5.98E24                 // mass of earth in kilograms
9.11e-31                // mass of an electron in kilograms
```
E 表示法可以表示非常大的数，也可以表示非常小的数。E表示法确保数字以浮点格式存储，即使没有小数点。注意，既可以使用E也可以使用e，指数可以是正数也可以是负数。数字中不能有空格。
#### 2.9.2 浮点类型<a id="浮点类型"></a>
C++ 中有三种浮点类型：float、double 和 long double。他们和整形一样是按照表示的最小范围来分类的。有效位（significant figure）是数字中有意义的位。有效位数不依赖于小数点的位置。
C++ 对于上方三种的要求是：float 至少 32 位，double 至少 48 位，且要大于 float，long double 是要大于等于 double。通常情况下，float 为 32 位，double 为 64 位，long double为 80、96或128 位。我们可以从头文件 cfloat 中找到系统的限制。
对于小数点精度的控制我们在 ostream 文件中有专门的方法，`setf()`方法，下方给出了使用的案例，其中的 `ios_base::fixed` 和 `ios_base::floatfield` 是用来固定小数点位的常量（由iostream文件提供）。
```cpp
// floatnum.cpp -- floating-point types
#include <iostream>
int main()
{
    using namespace std;
    cout.setf(ios_base::fixed, ios_base::floatfield); // fixed-point
    float tub = 10.0 / 3.0;      // good to about 6 places
    double mint = 10.0 / 3.0;    // good to about 15 places
    const float million = 1.0e6;

    cout << "tub = " << tub;
    cout << ", a million tubs = " << million * tub;
    cout << ",\nand ten million tubs = ";
    cout << 10 * million * tub << endl;

    cout << "mint = " << mint << " and a million mints = ";
    cout << million * mint << endl;
    return 0;
}
```
下面是输出
```
tub = 3.333333, a million tubs = 3333333.250000,
and ten million tubs = 33333332.000000
mint = 3.333333 and a million mints = 3333333.333333
```
通常cout会删除结尾的零。但是调用`cout.setf( )`将覆盖这种行为，至少在新的实现（就是在该语句之后的实现）中是这样的。由于 float 的精度问题，我们的第二个输出出现了数值上的小问题，这也体现了计算机对于 float 精度的控制。
对于精度的控制，我们在后面有更详尽的介绍。

#### 2.9.3 浮点常量<a id="浮点常量"></a>
对于浮点常量，在默认情况下我们是储存为 double 类型。如果希望改变类型，float 对应在数字后面加上 `f` 或 `F`后缀。对于 long double 类型，可以使用 `l` 或 `L` 后缀（这里建议使用 `L` 后缀）。
```cpp
1.234f                  // a float constant
2.45E20F                // a float constant
2.345324E28             // a double constant
2.2L                    // a long double constant
```
#### 2.9.4 浮点数的优缺点<a id="浮点数的优缺点"></a>
与整数相比，浮点数有两点好处，一是可以表述整数范围内的值，而是、由于有缩放因子，他们表示的范围要大得多。但是并不是没有缺点，浮点运算的速度是要比整数要慢的。
###### 以上介绍的整型和浮点型统称为算数类型。

### 2.10 C++11 中的auto声明<a id="auto声明"></a>
C++11中新增了可以让编译器根据初始值自行判断变量类型的类型——`auto`。如果使用关键字 auto，但是不指定变量的类型，编译器将会把变量的类型设置成于初始值相同。
```cpp
auto n = 100;         // n is int
auto x = 1.5;         // x is double
auto y = 1.3e12L;     // y is long double
```
显示声明类型的时候，将变量初始化为0不会导致问题，但是用关键字 auto 的时候就会导致问题（因为 0 本身是 int 型的）
对于更加复杂的含义，我们会在后面进一步讨论.

### 2.11 算数运算符<a id="算数运算符"></a>
C++ 由 5 中基本的运算符：
- `+`：对操作数进行加法运算。
- `-`：等同于数学上的减法
- `*`：等同于数学上的乘法
- `/`：等同于数学上的除法（但是要注意类型）
- `%`：等同于数学上的取余（获得余数）

变量和常量都可以用作操作数。**注意`%`的操作数只能是整数**。
#### 2.11.1 运算符的优先级<a id="运算符的优先级"></a>
C++ 的所有运算符都是有各自的优先级的，对于运算上的优先级，我们依然是按照数学上的优先级进行运算。当两个运算符的优先级相同时，C++将看操作数的结合性（associativity）是从左到右，还是从右到左。从左到右的结合性意味着如果两个优先级相同的运算符被同时用于同一个操作数，则首先应用左侧的运算符。从右到左的结合性则首先应用右侧的运算符。
```cpp
int num1 = 120 / 6 * 114514  // 正常从左向右运算
int num2 = 120 * 12 + 4 / 5  // 按照数学上的运算顺序即可
```
#### 2.11.2 除法分支<a id="除法分支"></a>
除法运算符`/`行为取决于两个操作数的类型（其实是涉及了隐式转换），整数和整数就是按整除进行运算，有至少一个浮点数就会转换成浮点数的结果。
实际上，对不同类型进行运算时，C++将把它们全部转换为同一类型。
###### 对于`/`的补充，我们在正常的编程语言中一般是一个运算符对应一种操作，但是这里的`/`其实是由三种运算构成的：int除法，float除法和double除法。这种使用相同符号进行多种操作的叫做运算符重载。像`/`这种是 C++ 内置的实例，我们还可以扩展运算符重载，以便能够用于用于自定义的类。这个我们在后面会有更详尽的介绍。

#### 2.11.3 求模运算<a id="求模运算"></a>
求模运算符返回整数除法的余数。它与整数除法相结合，尤其适用于解决要求将一个量分成不同的整数单元的问题。

#### 2.11.4 类型转换<a id="类型转换"></a>
C++ 有多种类型供用户选择，但也使得计算机的操作更加复杂。为了简化或者避免混合类型运算时的麻烦，C++ 会自动执行很多类型转换。
- 将一种算数运算符的值付给另一种算术类型的变量时，C++ 会进行转换
- 表达式中包含不同的类型时，C++ 会进行转换
- 将参数传递给函数的时候，C++ 会进行值的转换
  
下面是详细的转化解释：
1. 初始化和赋值进行的转换
C++ 允许将一种类型的值赋给另一种类型的变量。这样做时，值将被转换为接收变量的类型。我们将范围大的类型的值赋给范围小的类型的时候，由于精度的减少，C++强制将精度将为被赋值对象的精度，就会导致数据的丢失。但是将范围小的类型的值赋给范围大的值就不会有这种问题。
下面给出一些转换问题：

| 转 换 | 潜在的问题 |
| :--- | :--- |
| 将较大的浮点类型转换为较小的浮点类型，如将double转换为float | 精度（有效数位）降低，值可能超出目标类型的取值范围，在这种情况下，结果将是不确定的 |
| 将浮点类型转换为整型 | 小数部分丢失，原来的值可能超出目标类型的取值范围，在这种情况下，结果将是不确定的 |
| 将较大的整型转换为较小的整型，如将long转换为short | 原来的值可能超出目标类型的取值范围，通常只复制右边的字节 |

还有将 0 赋值给 bool 变量的时候转换为 false，非零值则转换为 true。
我这里给出一个例子：
`int debt = 7.2E12`
这里的 debt 本应该使用double来表示，但是我们的 debt 是 int 型，所以本应该要进行强制转换，但是，这里发生的是**未定义行为**。这里的输出很奇怪，是不可预测的，在不同的硬件处理器下，这里的输出是不同的（笔者编译的时候是输出 INT_MIN，即-2147483648）。因此，我们要避免出现这种情况。

2. 以{}方式初始化时进行的转换（C++11）
C++11 将使用大括号的初始化称为**列表初始化**。因为这种初始化常用于给复杂的数据类型（比如**数组**）提供值列表。 C++11 初始化中存在**窄化转换**规则，这对我们转化会进行强制的类型检查。
这种初始化对于类型转换要求十分严格。**列表初始化是不允许缩窄的，即变量的类型可能无法表示赋给它的值**。换句话说，这里没有所谓的大转小了（数值无法被目的类型有效的表示）。
  ```cpp
  const int code = 66;
  int x = 66;
  char c1 {31325};   // narrowing, not allowed
  char c2 = {66};    // allowed because char can hold 66
  char c3 {code};    // ditto(同上)
  char c4 = {x};     // not allowed, x is not constant
  x = 31325;
  char c5 = x;       // allowed by this form of initialization
  ```
这里的 x 不被允许初始化，是因为 x 是一个变量，编译器在初始化的时候是无法保证 x 一定是不发生窄化的值。**如果使用 {} 初始化，且源类型不是能被证明是安全的常量，则一律禁止隐式窄化。**

3. 表达式中的转换
当表达式中出现了；两种不同的类型的时候，C++ 会执行两种自动转换：首先，一些类型在出现的时候就会发生转换；其次，有些类型用到的时候才会被转换。
- 自动转换
  C++ 将 bool、char、unsigned char、signed char、和 short 值转换成int。这些转换称为**整型提升**。
  ```cpp
  short chickens = 20;        // line 1
  short ducks = 35;           // line 2
  short fowl = chickens + ducks; // line 3
  ```
  在这个程序中，我们将chickens和ducks转换成int，在计算完成后有转换成short型。
  但是对于unsigned short，我们通常会转换成 usnsigned int以避免数据的丢失。
  对于wchar_t的提升和char差不多，是提升成下列能够存储wchar_t取值范围的类型：int、unsigned int、long或 unsigned long。
- 算术表达式的转换
  不同类型进行算术运算的时候会进行一些转换，。当运算设计两种类型的时候，较小的类型会被转换成较大的类型。编译器通过校验表来确定在算术表达式中执行的转换。下面是C++11中的校验表。
  **如果操作数中包含浮点类型**：
  *   (1) 如果有一个操作数的类型是 `long double`，则将另一个操作数转换为 `long double`。
  *   (2) 否则，如果有一个操作数的类型是 `double`，则将另一个操作数转换为 `double`。
  *   (3) 否则，如果有一个操作数的类型是 `float`，则将另一个操作数转换为 `float`。
  **如果操作数均为整型**：
  *   (4) 否则，说明操作数都是整型，执行**整型提升**。
  *   (5) 如果两个操作数都是有符号或都是无符号的，且其中一个操作数的级别比另一个低，则**转换为级别高的类型**。
  *   (6) 如果一个操作数为有符号的，另一个为无符号的，且无符号操作数的级别比有符号操作数高，则将有符号操作数转换为**无符号操作数所属的类型**。
  *   (7) 否则，如果“有符号类型”可表示“无符号类型”的所有可能取值，则将无符号操作数转换为**有符号操作数所属的类型**。
  *   (8) 否则，将两个操作数都转换为**有符号类型的无符号版本**。
核心逻辑：C++ 的转换原则通常是**由小到大、有低精度到高精度**，目的就是为了在运算过程中不丢失信息。
对于上方介绍的类型，对应的级别可以按照需求的内存大小进行初步判断，值得一提：类型bool的级别最低。wchar_t、char16_t和char32_t的级别与其底层类型相同。

4. 传递参数时的转换（这里建议可以先看看[函数](#函数)部分）
对于函数参数的类型转换，他们是由C++函数原型来控制的。当然，我们也可以取消对函数参数传递的控制，但是这么做并不推荐。另外，为保持与传统C语言中大量代码的兼容性，在将参数传递给取消原型对参数传递控制的函数时，**C++将float参数提升为double**。
###### 什么是“有原型”和“无原型”？所谓的“取消对函数参数传递的控制”，指的就是“不使用函数原型”，或着说是不使用不定长的参数列表，即`...`语法
- 有原型（现代C++的标准做法）：
  在调用函数之前知道函数的每一种参数类型。
  ```cpp
  void myFunc(int a, float b); // 明确的函数原型
  ```
- 无原型/取消控制（C语言的遗留做法）：
  在 C 语言中，你可以定义一个参数列表不确定的函数（例如标准库里的 printf）。
  ```cpp
  void myFunc(...); // 使用省略号，取消了对具体参数类型的控制
  ```

5. 强制类型转换
C++ 还运行要通过强制类型转换机制来显示的进行类型转换。强制类型转换的格式有两种。通用格式如下：
```cpp
(typeName) value    // converts value to typeName type
typeName (value)    // converts value to typeName type
```
第一种的格式来自于C语言，第二种格式是纯粹的C++。新格式的想法是，要让强制类型转换就像是函数调用。这样对内置类型的强制类型转换就像是为用户定义的类设计的类型转换。
C++还引进了4个人强制类型转换运算符，对他们的使用要求更加严格。咋这四个运算符中，`static_cast<>`可以用于将值从一种类型转换成另一种类型，例如：
`static_cast<long> (thorn)  // return a type long conversion of thorn`
下面是通用格式：
`static_cast<typeName> (value)  // converts value to typeName type`

## 三、复合类型<a id="复合类型"></a>
### 3.1 数组<a id="数组"></a>
数组（array）是一种数据格式，能够存储多个同类型的值。要创建数组，可使用声明语句。数组声明应指出以下三点：
- 存储在每个元素中的值
- 数组名
- 数组中的元素数

在 C++ 中我们可以按照下面的格式进行数组声明：
`typeName arrayName[arraySize]`

表达式中的arraySize指定元素数目，他必须是整形常数（如10）或者是 const 值，也可以是常量表达式（如8*sizeof(int)），即其中u送有的值在编译时都是已知的。换句话说，arraySize不能够是变量。
数组之所以被称为复合类型，是因为它是使用其他类型来创建的。不能仅仅将某种东西声明为数组，它必须是特定类型的数组。
数组的很多用途都是给予一个事实：可以单独访问数组元素。方法是使用下标或者是索引来对元素进行编号。C++ 数组是从0开始编号。使用带索引的方括号表示法来指定数组元素。**注意左后一个元素的索引比数组长度小1**。因此，数组声明能够使用一个声明创建大量的变量，然后用索引来表示和访问各个元素，
C++的编译器不会检查使用的下标是否有效，因此如果下标出现问题，我们的程序结果就会出现
问题。必须要确保程序只是用有效的下标值。

###### 关于sizeof和数组：sizeof运算符时返回类型或者是数据对象的长度（单位是字节）。但是将sizeof用于数组名，我们会得到整个数组的字节数。但是用在数组元素上就是对应类型的长度。
```cpp
int array[10] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 0};
cout << sizeof(array); // 输出 4 * 10 = 40 
cout << sizeof(array[0])    //输出 4
```

### 3.2 数组初始化<a id="数组初始化"></a>
对于数组的初始化，这里更加推荐`typeName name[arraySize] = {element1, element2, ...};`格式。这种只需提供一个用逗号分隔的值列表（初始化列表），并将它们用花括号括起即可。列表中的空格是可选的。如果没有初始化函数中定义的数组，则其元素值将是不确定的。有的编译器可能会自动给你初始化为全是0（如VS code）。
C++11使用大括号的初始化（列表初始化）作为一种通用的初始化方式，可用于所有的类型。数组以前就可以使用列表初始化，但是C++11中的列表初始化新增了一些功能：
1. 初始化数组时，可省略等号（=）：
    `double earnings[4] {1.2e4, 1.6e4, 1.1e4, 1.7e4};  // ok with C++11`
2. 可不在大括号内包含任何东西，这将把所有元素都设置为零：
    ```cpp
    unsigned int counts[10] = {};   // all elements set to 0
    float balances[100] {};     // all elements set to 0
    ```
3. 列表初始化禁止缩窄转换（这是所有列表初始化的要求）：
    ```cpp
    long plifs[] = {25, 92, 3.0}    // not allowed
    short slifs[4] {'h', 'i', 1122011, '\0'};   // not allowed
    char tlifs[4] {'h', 'i', 112, '\0'};    // allowed
    ```
前两个犯了缩窄转换的问题：精度变小。第三个可以，没有超出类型的范围。
C++ 标准模板库（STL）提供了一种数组替代品——模板类vector，二C++新增了模板类array。这些替代品比内置的复合类型更加复杂、更灵活。

### 3.3 字符串<a id="字符串"></a>
字符串是存储在内存的连续字节中的一系列字符。C++处理字符串
的方式有两种：一种来自传统的C语言，称为C风格字符串。另一种还有基于string类库的方法。
存储在连续字节中的一系列字符意味着可以将字符串存储在char数组中，其中每个字符都位于自己的数组元素中。C-风格字符串具有一种特殊的性质：以空字符（null character）结尾，空字符被写作\0，其ASCII码为0，用来标记字符串的结尾。
```cpp
char dog[8] = {'b', 'e', 'a', 'u', 'x', ' ', 'I', 'I'};        // 不是字符串！
char cat[8] = {'f', 'a', 't', 'e', 's', 's', 'a', '\0'};       // 是字符串！
```
这两个都是char数字，但是只有第二个数组是字符串。空字符对于C风格字符串至关重要。。对于C++中的很多函数，都是遇见`\0`结束的。字符数组和字符串是两个不一样的东西。
对于上方的cat字符串，这种初始化很麻烦，我们有另一种初始化的方法，**用引号引起字符串即可**，这种字符串成为**字符串常量**或**字符串字面值**。
```cpp
char bird[11] = "Mr. Cheeps";   // the \0 is understood
char fish[] = "Bubbles";    //let the compiler count
```
用引号括起的字符串隐式地包括结尾的空字符，因此不用显式地包括它。另外，各种C++输入工具通过键盘输入，将字符串读入到char数组中时，将自动加上结尾的空字符.
当然，要确保数组应该足够的大，能够储存字符串中的所有字符——包括空字符。让编译器计算元素数目更为安全。C++对字符串长度没有限制。
###### 在编写C语言字符串的时候，创建数组的时候不要忘了最后为`\0`流出空间。
**注意，字符串常量（使用双引号）不能与字符常量（使用单引号）互换。**两者有着明显的区别：
```cpp
char ch1 = 'A';  // ok
char ch2 = "A"; // not allowed
```
对于ch2，虽然在表面上我们内容与ch1是一样的，但是我们的ch2其实等价于`char ch2[2] = {'A', '\0'};`

### 3.3.1 拼接字符串常量<a id="拼接字符串常量"></a>
C++允许拼接字符串字面值，即将两个用引号括起的字符串合并为一个。事实上，任何两个由空白（空格、制表符和换行符）分隔的字符串常量都将自动拼接成一个。下面的格式都是可以的。
```cpp
cout << "I'd give my right arm to be" " a great violinist.\n";
cout << "I'd give my right arm to be a great violinist.\n";
cout << "I'd give my right ar"
"m to be a great violinist.\n";
```
注意，拼接时不会在被连接的字符串之间添加空格，第二个字符串的第一个字符将紧跟在第一个字符串的最后一个字符（不考虑\0）后面。第一个字符串中的\0字符将被第二个字符串的第一个字符取代。

### 3.3.2 在数组中使用字符串<a id="在数组中是使用字符串"></a>
要在数组中使用字符串，常用方法有两种：一是将数组初始化为字符串常量，二是将键盘或文件输入读入到数组中。直接看一个例子：
```cpp
// strings.cpp -- storing strings in an array
#include <iostream>
#include <cstring>  // for the strlen() function

int main()
{
    using namespace std;
    const int Size = 15;
    char name1[Size];                // empty array
    char name2[Size] = "C++owboy";  // initialized array
    // NOTE: some implementations may require the static keyword
    // to initialize the array name2

    cout << "Howdy! I'm " << name2;
    cout << "! What's your name?\n";
    cin >> name1;
    cout << "Well, " << name1 << ", your name has ";
    cout << strlen(name1) << " letters and is stored\n";
    cout << "in an array of " << sizeof(name1) << " bytes.\n";
    cout << "Your initial is " << name1[0] << ".\n";
    
    name2[3] = '\0';                // set to null character
    cout << "Here are the first 3 characters of my name: ";
    cout << name2 << endl;
    
    return 0;
}
```
下面是运行结果：
```
Howdy! I'm C++owboy! What's your name?
Basicman     // 这里是我们在键盘上的输入
Well, Basicman, your name has 8 letters and is stored
in an array of 15 bytes.
Your initial is B.
Here are the first 3 characters of my name: C++
```
注意：我们在这里的strlen()函数是C++头文件`<cstring>`中的函数，它接受一个字符串，返回其中的元素个数（不含有`\0`）。这里的sizeof运算符中的结果是15而不是8是因为我们`()`中的是数组名，sizeof(数组名)会返回数组所用的内存长度（char字节数为1），无论你有没有`\0`。至于为什么最后的输出是“C++”是因为cout在处理到\0后就不会继续输出该数组了，后面的内容直接忽略。

### 3.3.3 字符串输入<a id="字符串输入"></a>
```cpp
// instr1.cpp -- reading more than one string
#include <iostream>
int main()
{
    using namespace std;
    const int ArSize = 20;
    char name[ArSize];
    char dessert[ArSize];

    cout << "Enter your name:\n";
    cin >> name;
    cout << "Enter your favorite dessert:\n";
    cin >> dessert;
    cout << "I have some delicious " << dessert;
    cout << " for you, " << name << ".\n";
    return 0;
}
```
下面是我们的运行结果：
```
Enter your name:
Alistair Dreeb  (这里是输入)
Enter your favorite dessert:
I have some delicious Dreeb for you, Alistair.
```
注意到后面的输入出现了问题，我们命名没有输入但是却直接输出了最后的结果。这是因为cin存在着**空白截断**的问题。cin使用空白符来确定字符串的结束位置，因此上方的程序cin只读取了一个单词。读取该单词后，cin将该字符串放到数组中，并自动在结尾添加空字符。因此当我们在出现线面的提示信息之前，我们的数组就已经被处理好了。
上方是字符串中的**空白截断**问题，其实我们还存在着数组长度小于对应的字符串长度的问题。这就需要我们在创立数组的时候进行注意了。

### 3.3.4 字符串读取（整行）<a id="字符串读取"></a>
对于上方的问题，istream文件中的类（如cin）提供类一些面向行的类成员函数：getline()和get()。这两个函数都是读取行输入，值啊都换行符，但是getline()会丢弃最后的换行符，而get()会将换行符保留在输入序列中（即**缓冲区**）。
1. getline()
   getline()读取整行，使用回车键的输入的换行符来确定输入结尾。要调用，可以使用`cin.getline()`。该函数有两个参数。第一个是用来存储行的数组名称。第二个是要读取的字符数。**注意这里的字符数是要加上`\0`的，也就是说实际读取的内容应该改是“字符数-1”。getline( )成员函数在读取指定数目的字符或遇到换行符时停止读取。
   `cin.getline(name, 20)   // 读取19个字符到name中（有\0占位）`
   ###### 如果超出了规定的字符数，当我们输出的时候，也不会输出对应的后续内容，因为已经被自动补充的\0截断了。
   值得补充的是，其实cin.getline()并不是必须是以回车键结尾，我们是可以指定我们想要的结束符的。
   ```cpp
    #include <iostream>
    using namespace std;

    int main() {
        const int ArSize = 50;
        char buffer[ArSize];

        cout << "请输入内容（以分号 ';' 结束输入）：" << endl;
        
        // 这里传入了 ';' 作为第三个参数
        cin.getline(buffer, ArSize, ';'); 

        cout << "你输入的内容是: " << buffer << endl;
        return 0;
    }
   ```
   这个程序中，我们在第二次使用cin.getline()的时候我们是使用";"来作为我们的结束符的。换句话说cin.getline()的完全体应该是有三个参数的：数组名 + 长度 + 结束符。

2. get()
   get()函数有着多种变体。其中一种的工作方式于getline()类似，他们接受的参数相同，解释参数的方式也相同，并且都读到行尾。但是get并不丢弃换行符，而是将其留在输入队列中（缓冲区）。假设我们连续两次调用get()，由于第一次调用后，换行符将留在输入队列中，因此第二次调用时看到的第一个字符便是换行符。因此get()认为已到达行尾，而没有发现任何可读取的内容。如果不借助于帮助，get()将不能跨过该换行符。
   get()还有一种变体，使用不带任何参数的cin.get()可以读取下一个字符（即使是换行符），因此可以使用他来处理换行符。也就是说我们上方遇到的连续使用cin.get()会导致后面的cin.get()出现问题的解决方案就是在两次get()中间添加一个cin.get()来处理换行符。
   还有一种get()的使用方式是将两个类成员函数拼接起来（合并）：
   `cin.get(name, 20).get();    //concatenate member functions`。这样就不用担心下面的cin.get()会出现遇见换行符而停止输入的问题了。
    ###### 补充一下：C++允许函数有多个版本，条件是这些版本的参数列表不同。
    C++ 建议使用get()而不是getline()有两点原因：一是老式的实现是没有getline()的，二是get()可以让输入变得更加仔细。
    如何知道停止读取的原因是由于已经读取了整行，而不是由于数组已填满呢？查看下一个输入字符，如果是换行符，说明已读取了整行；否则，说明该行中还有其他输入。
3. 空行和其他问题：
   当getline()和get()读取空行的时候，C++将会设置**失效位**。这意味着接下来的输入将会被阻断。但是可以使用下面的命令恢复：
   `cin.claer();`
   这个语句的作用是**重置流的错误状态**。
   另一个潜在的问题是，输入字符串可能比分配的空间长。如果输入行包含的字符数比指定的多，则getline()和get()将把余下的字符留在输入队列中，而getline()还会设置失效位，并关闭后面的输入。

### 3.3.5 混合输入字符串和数字 <a id="混合输入字符串和数字"></a>
混合输入数字和面向行的字符串会导致问题。来看一个例子：
```cpp
// numstr.cpp -- following number input with line input
#include <iostream>
int main()
{
    using namespace std;
    cout << "What year was your house built?\n";
    int year;
    cin >> year;
    cout << "What is its street address?\n";
    char address[80];
    cin.getline(address, 80);
    cout << "Year built: " << year << endl;
    cout << "Address: " << address << endl;
    cout << "Done!\n";
    return 0;
}
```
结果如下：
```
What year was your house built?
1966
What is its street address?
Year built: 1966
Address: 
Done!
```
我们发现啊当我们输入1966给address后，后面的输入过程就直接没有了。问题在于，当cin读取年份，将回车键生成的换行符留在了输入队列中。后面的cin.getline( )看到换行符后，将认为是一个空行，并将一个空字符串赋给address数组。我们几种方法来解决这个问题：
1. 使用没有参数的cin.get()和有一个char参数的cin.get()，单独调用。
   ```cpp
   cin >> year;
   cin.get();   // or cin.get(ch);
   ```
2. 也可以使用表达式`cin>>year`返回cin对象，将调用拼接起来：
   ```cpp
   (cin >> year).get();     // or (cin >> year).get(ch);
   ```
这里的两种方法看起来很奇怪，这里的原理是**流提取运算符将内容赋给cin对象后返回的还是cin对象，这将允许我们继续使用cin的成员函数get()来清楚后面的换行操作。这里的更深的原理会在后面提到。

## 3.4 string类 <a id="string类"></a>
C++98 通过添加string类扩展了C++库，现在可以使用string类型的对象来存储字符串。
要使用string类，我们要在程序中包含头文件string。string类位于名称空间std中。所以要么用using编译指令，要么用`std::string`来引用他。string类隐藏了字符串的数组性质，因此我们可以像处理普通字符数组一样处理字符串。下面是一个例子
```cpp
// strtypel.cpp -- using the C++ string class
#include <iostream>
#include <string>       // make string class available

int main()
{
    using namespace std;
    char charr1[20];            // create an empty array
    char charr2[20] = "jaguar"; // create an initialized array
    string str1;                // create an empty string object
    string str2 = "panther";    // create an initialized string

    cout << "Enter a kind of feline: ";
    cin >> charr1;
    cout << "Enter another kind of feline: ";
    cin >> str1;        // use cin for input
    
    cout << "Here are some felines:\n";
    cout << charr1 << " " << charr2 << " "
         << str1 << " " << str2 // use cout for output
         << endl;

    cout << "The third letter in " << charr2 << " is "
         << charr2[2] << endl;
    cout << "The third letter in " << str2 << " is "
         << str2[2] << endl;   // use array notation

    return 0;
}
```
下面是运行情况：
```
Enter a kind of feline: ocelot
Enter another kind of feline: tiger
Here are some felines:
ocelot jaguar tiger panther
The third letter in jaguar is g
The third letter in panther is n
```
string对象使用方式和使用字符数组在很多方面相同：
* **初始化**：可以使用 C-风格字符串（即字符数组或字符串字面值）来初始化 `string` 对象。
* **输入**：可以使用 `cin` 将键盘输入直接存储到 `string` 对象中。
* **输出**：可以使用 `cout` 直接显示 `string` 对象。
* **访问**：可以使用数组表示法（即下标运算符 `[]`）来访问存储在 `string` 对象中的字符。
string类对象和字符数组之间的主要区别是：string对象可以声明为**简单变量**，而**不是数组**。string类的设计让程序能够自动处理string的大小。这使得与使用字符数组相比，使用string对象更加方便，也更加安全。

### 3.4.1 string字符串初始化 <a id="string字符串初始化"></a>
string对象也可以使用列表初始化。
```cpp
string first_date = {"The Bread Bowl"}；
string second_date {"Hank's Fine Eats"};
```

### 3.4.2 赋值、拼接string类字符串 <a id="赋值、拼接string类字符串"></a>
使用string类时，string对象之间可以互相赋值，但是字符数组是不可以的。
```cpp
char charr1[20];               // create an empty array
char charr2[20] = "jaguar";    // create an initialized array
string str1;                   // create an empty string object
string str2 = "panther";       // create an initialized string
charr1 = charr2;               // INVALID, no array assignment
str1 = str2;                   // VALID, object assignment ok
```
对于字符串的拼接，string类简化了字符串的合并操作。可以使用运算符`+`将两个string对象合并起来，还可以使用运算符`+=`将字符串加到string对象的末尾。
```cpp
string str3;
str3 = str1 + str2;     // assign str3 the joined strings.
str1 += str2;   // add str2 to the end of str1
```

### 3.4.3 string类的其他操作
在新增string类之前，我们也可以对字符串进行赋值操作。对于C-风格字符串，我们可以使用C语言库中的函数来玩长任务。头文件`<cstring>`提供了这些函数。比如我们可以使用strcpy()函数将字符串赋值到字符数组中。使用strcat()将字符数组附加到字符数组末尾。
```cpp
strcpy(charr1, charr2);  // copy charr2 to charr1
strcat(charr1, charr2);  // append contents of charr2 to charr1
```
下面的例子是综合应用：
```cpp
// strtype3.cpp -- more string class features
#include <iostream>
#include <string>              // make string class available
#include <cstring>             // C-style string library
int main()
{
    using namespace std;
    char charr1[20];
    char charr2[20] = "jaguar";
    string str1;
    string str2 = "panther";

    // assignment for string objects and character arrays
    str1 = str2;               // copy str2 to str1
    strcpy(charr1, charr2);    // copy charr2 to charr1

    // appending for string objects and character arrays
    str1 += " paste";          // add paste to end of str1
    strcat(charr1, " juice");  // add juice to end of charr1

    // finding the length of a string object and a C-style string
    int len1 = str1.size();    // obtain length of str1
    int len2 = strlen(charr1); // obtain length of charr1

    cout << "The string " << str1 << " contains "
         << len1 << " characters.\n";
    cout << "The string " << charr1 << " contains "
         << len2 << " characters.\n";

    return 0;
}
```
下面是输出：
```cpp
The string panther paste contains 13 characters.
The string jaguar juice contains 12 characters.
```

这里的`str1.size()`原型是`std::string.size()`，我们在C++中的string类中还有一种成员函数来获取字符长度：`std::string.length()`，一般来讲，这两种函数是等价的。第一种：遵循 STL 容器统一接口（如`vector.size()`、`list.size()`）更通用一些。第二种：源于 C 风格字符串的习惯（如`strlen()`），更语义化，适合处理文本。只要不混用，怎么用都行（是不建议混用）。

处理string对象的语法通常比C字符串函数简单：
```cpp
str3 = str1 + str2;
strcpy(str3, str1);
strcat(str3, str2);
```
第一句和后面的是等价的。

另外，使用字符数组时，总是存在目标数组过小，无法存储指定信息的危险，但是string类具有自动调整大小的功能，从而能够避免这种问题发生。C函数库确实提供了与strcat( )和strcpy( )类似的函—strncat( )和strncpy( )，它们接受指出目标数组最大允许长度的第三个参数，因此更为安全，但使用它们进一步增加了编写程序的复杂度。

### 3.4.4 string类的I/O
我们可以使用cin和运算符`>>`来将输入存储到string对象中，使用cout和运算符`<<`来显示string对象。但是string对象每次读取的是一行而不是一个单词。我们来看一个例子：
```cpp
// strtype4.cpp -- line input
#include <iostream>
#include <string>              // make string class available
#include <cstring>             // C-style string library
int main()
{
    using namespace std;
    char charr[20];
    string str;

    cout << "Length of string in charr before input: "
         << strlen(charr) << endl;
    cout << "Length of string in str before input: "
         << str.size() << endl;
    cout << "Enter a line of text:\n";
    cin.getline(charr, 20);      // indicate maximum length
    cout << "You entered: " << charr << endl;
    cout << "Enter another line of text:\n";
    getline(cin, str);           // cin now an argument; no length specifier
    cout << "You entered: " << str << endl;
    cout << "Length of string in charr after input: "
         << strlen(charr) << endl;
    cout << "Length of string in str after input: "
         << str.size() << endl;

    return 0;
}
```
下面是运行结果：
```cpp
Length of string in charr before input: 27
Length of string in str before input: 0
Enter a line of text:
peanut butter
You entered: peanut butter
Enter another line of text:
blueberry jam
You entered: blueberry jam
Length of string in charr after input: 13
Length of string in str after input: 13
```
这里的结果有个问题：为什么`strlen(charr)`的结果要比正常的我们规定的数组长度大？首先，未初始化的数组中的内容是未定义的；其次，函数strlen()从数组中的第一个元素开始计算字节数，直到遇见空字符。但是由于我们的数组中的内容未被定义，所以这个`\0`的位置就是不确定的。在不同的编译器上这里的结果是不同的。
但是，string对象不一样，未被初始化的string对象的长度被自动设置为0。
下面来看两种读取字符串的方法：
```cpp
cin.getline(charr, 20);
getline(cin, str);
```
第一种正是我们前文介绍的cin中的getline()方法。第二种是我们`<string>`中的函数。第一种的句点表示法表明，函数getline()是istream类种的一个类方法。第二种没有句点表示法，这表示getline()不是一个类方法。它将cin作为参数，指出到哪里去查找输入。另外，也没有指出字符串长度的参数，因为string对象将根据字符串的长度自动调整自己的大小。






[^1]: **扩展名** ，也叫**后缀名**，是文件名中最后一个点 `.` 之后那部分的字母或者数字。他是文件的“身份标签”，告诉操作系统**这是一个什么类型的文件**和**怎么打开它**。一个完整的文件名有两部分组成：`文件名 + 扩展名`。其中的 `.` 叫**分隔符**。扩展名有以下三种作用：
    - **关联程序**：系统会查看扩展名并进行对应的操作。
    - **表示格式**：他表示文件内部数据的组织方式。如`.jpg`表示里面存的是像素颜色数据，`.txt`表示里面存的是纯文本字符。
    - **编译器识别**：在编程中，编译器会根据扩展名处理文件。
    下面是一些常见的扩展名：
    1. 源代码文件：
    - `.cpp`：**最标准**的 C++ 源文件
    - `.cc`：用于Unix环境的 C++ 源文件
    - `.c`：C 语言源文件
    1. 头文件：
    - `.h`：通用头文件（C 和 C++ 都能用
    - `.hpp`：C++ 专用头文件
    - 无后缀：标准库头文件
    1. 库文件：
    2. 静态库：链接时将代码直接拷贝到程序中
        - `.lib`：Windows
        - `.a`：Linux
    3. 动态库：程序运行时才会加载。
        - `.dll`：Windows
        - `.so`：Linux
    4. 中间目标文件和执行文件
    - `.obj`：目标文件（Windows）
    - `.o` 目标文件（Linux）
    - `.exe`：可执行文件
[^2]: **接口** 是一组函数或方法的声明。它定义了调用者如何与这个组件沟通（函数名、参数、返回值）。
[^3]: 这里我们对倒数第二点进行详细的说明： C/C++规定：任何以`__`和`_大写字母`开头的名字，保留给**实现**使用。普通程序员不要定义这种名字，避免与编译器或者标准库冲突。
    ```cpp
    int __myVar;  // ❌ 不要用，保留给编译器
    int _Xyz;     // ❌ 不要用
    ```
    这里讲的实现是C/C++语言本身提供的所有内部机制和资源，即`实现(implementation) = 编译器 + 标注库 + 系统提供的运行时支持`。
    - 编译器提供的：
      - 内建函数(bulit-in functions)
      `int x = __builtin_popcount(7); //GCC内建函数`
      - 内部类型、宏、符号(如`__FILE__`,`__LINE__`等)
    - 标准库提供的：
      - STL容器内部变量、函数
      ```cpp
        namespace std {
            int _S_empty_rep_storage; // 内部使用
        }
      ```
      - 内部辅助函数、模板实现
    - 操作系统或运行时库提供的：
      - 运行时的初始化函数、系统调用包装函数等

      总之，“实现”不是指你自己写的库函数，而是 由语言提供者提供的全部内部机制，包括： 
      - 编译器内置
      - 标准库内部
      - 运行时支持

[^4]: 字面量和常量还是有些区别的。 虽然它们都是“常量”（不可改变的值），但在 C++ 中，人们通常把它们分开称呼：
    | 特性 | 字面值 (Literal) | 具名常量 (const Variable) |
    | :--- | :--- | :--- |
    | **是否有名字** | 无 | 有 |
    | **是否有内存地址** | 通常没有（编译器直接编进指令） | 有（它是一个变量，只是不能改） |
    | **可读性** | 差（被称为“魔数” Magic Number） | 好（有意义的名字） |

[^5]: 在头文件iostream文件中也提供了我们的表示其他进制的方法(dec: 10, hex: 16, oct: 8)。下面是一些例子：
    ```cpp
    #include <iostream>
    using namespace std;
    int main()
    {
        using namespace std;
        int chest = 42;
        int waist = 42;
        int inseam = 42;

        cout << "Monsieur cuts a striking figure!" << endl;
        cout << "chest = " << chest << " (decimal for 42)" << endl;
        cout << hex;        // manipulator for changing number base
        cout << "waist = " << waist << " (hexadecimal for 42)" << endl;
        cout << oct;        // manipulator for changing number base
        cout << "inseam = " << inseam << " (octal for 42)" << endl;
        return 0;
    }
    ```
    这是输出结果：
    ```
    Monsieur cuts a striking figure!
    chest = 42 (decimal for 42)
    waist = 2a (hexadecimal for 42)
    inseam = 52 (octal for 42)
    ```
    对于dec，hex和oct，他们不会在终端中输出，仅会改变cout的输出形式，但是这种改变也是一直有效的，直到程序结束，也就是说我们在使用进制转换的时候别忘了在转回我们常用的10进制。

[^6]: Unicode提供了一种表示各种字符集的解决方案—为大量字符和符号提供标准数值编码，并根据类型将它们分组。例如，ASCII码为Unicode的子集，因此在这两种系统中，美国的拉丁字符（如A和Z）的表示相同。然而，Unicode还包含其他拉丁字符，如欧洲语言使用的拉丁字符、来自其他语言（如希腊语、西里尔语、希伯来语、切罗基语、阿拉伯语、泰语和孟加拉语）中的字符以及象形文字（如中国和日本的文字）。到目前为止，Unicode可以表示109000多种符号和90多个手写符号（script），它还在不断发展中。
Unicode给每个字符指定一个编号—码点。Unicode码点通常类似于下面这样：U-222B。其中U表示这是一个Unicode字符，而222B是该字符（积分正弦符号）的十六进制编号。
国际标准化组织（ISO）建立了一个工作组，专门开发ISO 10646—这也是一个对多种语言文本进行编码的标准。ISO 10646小组和Unicode小组从1991年开始合作，以确保他们的标准同步。

[^7]: 对于数字电子技术的电子书可以直接[跳转到这里](https://gitcode.com/Open-source-documentation-tutorial/ed458/blob/main/Thomas%20L%20Floyd%20-%20Digital%20Fundamentals%20A%20Systems%20Approach(2014,%20Pearson).pdf) 