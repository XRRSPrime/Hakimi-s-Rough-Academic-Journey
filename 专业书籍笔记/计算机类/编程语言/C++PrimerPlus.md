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
**赋值语句的值是最后左操作数的值**：
```cpp
cout << (a = 1) + 2;    // 输出 3 （1 + 2）
```

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
这样我们就规定了一个常量 Number ，当我们以后每次使用 Number 的时候我们都相当于使用其所对应的值。但是，**这样定义的变量以后是不允许重新赋值的,也就是说，我们必须进行初始化名进行赋值**[^1]。这里的关键字 const 称为限定符，它限定了声明的含义。
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

##### 2.9.3 浮点常量<a id="浮点常量"></a>
对于浮点常量，在默认情况下我们是储存为 double 类型。如果希望改变类型，float 对应在数字后面加上 `f` 或 `F`后缀。对于 long double 类型，可以使用 `l` 或 `L` 后缀（这里建议使用 `L` 后缀）。
```cpp
1.234f                  // a float constant
2.45E20F                // a float constant
2.345324E28             // a double constant
2.2L                    // a long double constant
```
##### 2.9.4 浮点数的优缺点<a id="浮点数的优缺点"></a>
与整数相比，浮点数有两点好处，一是可以表述整数范围内的值，而是、由于有缩放因子，他们表示的范围要大得多。但是并不是没有缺点，浮点运算的速度是要比整数要慢的。
###### 以上介绍的整型和浮点型统称为算数类型。

#### 2.10 C++11 中的auto声明<a id="auto声明"></a>
C++11中新增了可以让编译器根据初始值自行判断变量类型的类型——`auto`。如果使用关键字 auto，但是不指定变量的类型，编译器将会把变量的类型设置成于初始值相同。
```cpp
auto n = 100;         // n is int
auto x = 1.5;         // x is double
auto y = 1.3e12L;     // y is long double
```
显示声明类型的时候，将变量初始化为0不会导致问题，但是用关键字 auto 的时候就会导致问题（因为 0 本身是 int 型的）
对于更加复杂的含义，我们会在后面进一步讨论.

#### 2.11 算数运算符<a id="算数运算符"></a>
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
#### 3.1 数组<a id="数组"></a>
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

#### 3.2 数组初始化<a id="数组初始化"></a>
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

#### 3.3 字符串<a id="字符串"></a>
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

#### 3.3.1 拼接字符串常量<a id="拼接字符串常量"></a>
C++允许拼接字符串字面值，即将两个用引号括起的字符串合并为一个。事实上，任何两个由空白（空格、制表符和换行符）分隔的字符串常量都将自动拼接成一个。下面的格式都是可以的。
```cpp
cout << "I'd give my right arm to be" " a great violinist.\n";
cout << "I'd give my right arm to be a great violinist.\n";
cout << "I'd give my right ar"
"m to be a great violinist.\n";
```
注意，拼接时不会在被连接的字符串之间添加空格，第二个字符串的第一个字符将紧跟在第一个字符串的最后一个字符（不考虑\0）后面。第一个字符串中的\0字符将被第二个字符串的第一个字符取代。

#### 3.3.2 在数组中使用字符串<a id="在数组中是使用字符串"></a>
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

#### 3.3.3 字符串输入<a id="字符串输入"></a>
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

#### 3.3.4 字符串读取（整行）<a id="字符串读取"></a>
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

#### 3.3.5 混合输入字符串和数字 <a id="混合输入字符串和数字"></a>
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

### 3.4 string类 <a id="string类"></a>
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

#### 3.4.1 string字符串初始化 <a id="string字符串初始化"></a>
string对象也可以使用列表初始化。
```cpp
string first_date = {"The Bread Bowl"}；
string second_date {"Hank's Fine Eats"};
```

#### 3.4.2 赋值、拼接string类字符串 <a id="赋值、拼接string类字符串"></a>
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

#### 3.4.3 string类的其他操作 <a id="string类的其他操作"></a>
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

#### 3.4.4 string类的I/O <a id="string类的I/O"></a>
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

#### 3.4.5 其他形式的字符串字面值<a id="其他类型的字符串字面值"></a>
这里的其他类型指的是wchar_t, char16_t, char32_t。对于这些类型的字符串字面值，C++分别使用前缀L、u和U表示，下面是一个如何使用这些前缀的例子：
```cpp
wchar_t title[] = L"Chief Astrogrator";   // w_char string
char16_t name[] = u"Felonia Ripova";    // char_16 string
char32_t car[] = U"Humber Super Snipe";    // char_32 string
```
C++11还支持Unicode字符编码UFT-8。C++使用前缀u8来表示这种类型的字符串字面值。
C++11新增的另一种类型式原始（raw）字符串。在原始字符串中，字符表示的就是自己，例如，序列\n不表示换行符，而表示两个常规字符——斜杠和n。

### 3.5 结构（又称结构体）<a id="结构"></a>
结构是用户定义的类型，而结构声明定义了这种类型的数据属性。定义了类型后，便可以创建这种类型的变量。创建结构分为两步。首先先定义结构——描述并标记了能够存储到结构中的各种数据类型，然后再创建对应的对象。下面创建结构体的一个例子：
```cpp
struct inflatable{
    char name[20];
    float volume;
    double price;
};
```
程序中的`struct`是关键字，这些代码定义的是一个结构的布局。标识符inflatable是这种数据格式的名称，因此新类型的名称为inflatable。接下来的大括号中包含的是结构存储的数据类型的列表，其中每个列表项都是一条声明语句。列表中的每一项都被称为**结构成员**。
定义好后我们就可以创建这种类型的变量了。
如果你学过C语言就会发现C++与C语言有点不同：C++允许声明结构变量的时候省略`struct`。在C++中，结构标记的用法与基本类型相同，这种变化强调了**结构声明定义了一种新类型**。
由于结构体是一种新的类，因此我们可以使用**成员运算符**`.`来访问成员。并且我们可以像以往使用内置类型那样使用它们。
我们可以直接对结构进行初始化，只需要**依次**填入对应的值即可，不用必须使用多行。
结构的声明尽量是要在进行外部声明。**C++提倡使用外部结构声明，但是不建议使用外部变量**。

#### 3.5.1 结构初始化 <a id="结构初始化"></a>
C++11中的结构体也是支持使用**列表初始化**的。当初始化的`{}`中没有包含任何东西，所有的成员都将被设置为0。同样，**不允许缩窄转换**。

#### 3.5.2 使用string类作为成员 <a id="使用string类作为成员"></a>
C++是允许使用别的类充当一种类的成员类型的（只要你在使用之前声明过）。这种类型混用的形式我们在后面回说，叫做**组合**。

#### 3.5.3 结构属性 <a id="结构属性"></a>
C++使用户定义的类型与内置类型尽可能相似。例如，可以将结构作为参数传递给函数，也可以让函数返回一个结构。另外，还可以使用赋值运算符`=`将结构赋给另一个同类型的结构，这样结构中每个员都将被设置为另一个结构中相应成员的值，即使成员是数组[^8]。这种赋值被称为**成员赋值**。

C++中的结构是支持在声明的同时进行对象创建和初始化的：
```cpp
struct perks
{
    int key_number;
    char car[12];
} mr_smith, ms_jones;    // two perks variables
```
在声明结构的同时创建了两个该结构的对象`mr_smith`和`ms_jones`。
```cpp
struct perks
{
    int key_number;
    char car[12];
} mr_glitz =
{
    7,              // value for mr_glitz.key_number member
    "Packard"       // value for mr_glitz.car member
};
```
声明结构体的同时创建了对象并进行了初始化。
虽然C++支持上述的做法，但是将结构定义和变量声明分开是个非常好的习惯。除了上述的两种做法，我们还可以创建一种没有名称的结构和对应对象，但是这种方法只能创建一个对象（因为没有名称，后续无法使用这种结构继续创建对象，也就是说是一次性的）。
除了上面说的这些，C++还允许结构中出现成员函数（在结构中定义的函数，可以当作该结构的成员来使用，这在C语言中是不被允许的）。这将在后面说**类**的时候进一步区分。

#### 3.5.4 结构数组 <a id="结构数组"></a>
由于结构可以像内置类型一样使用，那么也是可以创建数组的。方法和创建基本类型数组完全相同。
`structName name[size]`
这个时候创建的是数组，不是结构对象，所以我们使用直接使用成员运算符是会报错的（但是我们对数组中的元素使用是不会有问题的，因为这个时候是该结构的对象）。
由于数组中的每个元素都是结构，所以我们可以使用结构初始化的方式来提供他的值，就像这样：
```cpp
inflatable guests[2] =          // initializing an array of structs
{
    {"Bambi", 0.5, 21.99},      // first structure in array
    {"Godzilla", 2000, 565.99}  // next structure in array
};
```

#### 3.5.5 结构中的位字段 <a id="结构中的位字段"></a>
所谓的**位字段**，就是允许使用以**位（bit）**为单位来指定成员所占用的内存空间，而不是使用传统的**字节（byte）**。字段的类型应为**整型或枚举**，接下来是冒号，冒号后面是一个数字，它指定了使用的位数。可以使用没有名称的字段来提供间距。每个成员都被称为位字段。下面是一个例子：
```cpp
struct torgle_register {
    unsigned int SN : 4;  // SN 占用 4 位（取值范围 0-15）
    unsigned int    : 4;  // 无名位字段，占用 4 位，用来做填充（间距）
    bool goodIn     : 1;  // 占用 1 位（0 或 1，即 false 或 true）
};
```

#### 3.5.6 使用new创建动态结构 <a id="使用new创建动态结构"></a>
在运行时创建数组优于在编译时创建数组，对于结构也是如此。需要在程序运行时为结构分配所需的空间，这也可以使用new运算符来完成。通过使用new，可以创建动态结构。当然这里介绍的也适用于类。
使用new创建动态结构分为两步：**创建结构和访问成员**。我们在定义好一个结构后，我们就可以像之前分配动态变量一样创建相应的结构：
```cpp
structName *ptr = new structName;   // 创建好了一个动态结构
```
但是访问成员有点复杂，不能将成员运算符句点用于结构名，因为这种结构没有名称，只是知道它的地址。C++专门为这种情况提供了一个运算符：箭头成员运算符`−>`。该运算符由连字符和大于号组成，可用于指向结构的指针，就像点运算符可用于结构名一样。
###### 如果结构标识符是结构名，则使用句点运算符；如果标识符是指向结构的指针，则使用箭头运算符。
另一种访问结构成员的方法是：使用`(*ptr).contributorName`，由于ptr是一个指针，我们可以解引用，然后再访问成员名。

### 3.6 共用体<a ud="共用体"></a>
**共用体**（union）是一种数据格式，它能够存储不同的数据类型，但只能同时存储其中的一种类型。也就是说，结构可以同时存储int、long和double，共用体只能存储int、long或double。共用体的句法与结构相似，但含义不同。我们看下面的例子：
```cpp
union one4all
{
    int int_val;
    long long_val;
    double double_val;
};

one4all pail;
pail.int_val = 15;            // 存储一个 int 类型的值
cout << pail.int_val;
pail.double_val = 1.38;       // 存储一个 double 类型的值，原先的 int 值将丢失
cout << pail.double_val;
```
共用体的是所有成员公用一块地址（也就是说，所有的对象住在一起，你来了我就必须走，即数据丢失）。每一次赋值都会覆盖掉上一次所用的内存。由于我们必须要有足够的空间来存储最大的成员，所以我们**共用体的最大长度为共用体最大成员的长度**。
共用体的用途之一是，当数据项使用两种或更多种格式（但不会同时使用）时，可节省空间。与结构一样，共用体也可以没有名称**匿名共用体**，这个时候他们的成员将成为位于相同地址的变量。
```cpp
struct widget
{
    char brand[20];
    int type;
    union                // anonymous union
    {
        long id_num;     // type 1 widgets
        char id_char[20]; // other widgets
    };
};
...
widget prize;
...
if (prize.type == 1)
    cin >> prize.id_num;
else
    cin >> prize.id_char;
```
由于共用体是匿名的，因此id_num和id_char被视为prize的两个成员，它们的地址相同，所以不需要中间标识符id_val。

### 3.7 枚举<a id="枚举"></a>
C++中的enum工具提供了另一种创建符号常量的方式，则种方式可以替代 const。还允许新类型，但必须按严格的限制进行。看下面的语句：
```cpp
enum spectrum {red, orange, yellow, green, blue, violet, indigo, ultraviolet};
```
上面的语句完成两项工作：
- 让`sqectyum`成为新的类型名称：spectrum称为**枚举**。
- 将red, orange, yellow等作为符号常量，对应整数值0~7。这些量叫做**枚举量**。

在默认情况下，将整数值赋给枚举量，第一个枚举量值为0，第二个枚举量为1。
因为枚举名算是一种类型名，所以我们也可以用来声明变量：
`spectrum band;`
虽然枚举可以算是一种类型，但是在不进行强制类型转换的情况下，只能将枚举时使用的枚举量赋给这种枚举的变量。
```cpp
band = blue;    // vaild
band = 2000;    // invaild
```
对于枚举，只定义了赋值运算符。具体地说，没有为枚举定义算术运算。
枚举量时整型，可以转为int型，但是int型不能自动转换成枚举类型：
```cpp
int color = blue;   // valid
band = 3;   // incalid, ing not converted to spectrum
color = 3 + red;    // valid, red converted to int
```
枚举的规则相当严格。实际上，枚举更常被用来定义相关的符号常量，而不是新类型。
#### 3.7.1 设置枚举量的值 <a id="设置枚举量的值"></a>
我们可以使用复数运算符来显式的设置枚举量的值：
`enum bits {one = 1, two = 2, four = 4, eight = 8};`
指定的值必须是整数（并不是必须是int型），也可以进行部分的赋值
`enum bigstep {first, second = 100, thrid};`
这里的first还是0，但是second以后就是从100开始进行依次排序了。
最后我们可以创建多个值相同的枚举量：
`xnum {zero, null = 0, one, numero_uno = 1};`
#### 3.7.2 枚举的取值范围 <a id="枚举量的取值范围"></a>
取值范围的定义如下。首先，要找出上限，需要知道枚举量的最大值。找到大于这个最大值的、最小的2的幂，将它减去1，得到的便是取值范围的上限。例如，前面定义的bigstep的最大值枚举值是101。在2的幂中，比这个数大的最小值为128，因此取值范围的上限为127。要计算下限，需要知道枚举量的最小值。如果它不小于0，则取值范围的下限为0；否则，采用与寻找上限方式相同的方式，但加上负号。例如，如果最小的枚举量为−6，而比它小的、最大的2的幂是−8（加上负号），因此下限为−7。

### 3.8 指针 <a id="指针"></a>
指针[^9]是一个变量，其存储的是值的地址，而不是值本身。要找到常规变量的地址，我们可以使用地址运算符`&`，显示地址时，cout在不同的系统中使用的进制可能会有不同，但是大部分都是十六进制。

#### 3.8.1 声明和初始化指针 <a id="声明和初始化指针"></a>
```cpp
int *p;
```
这个声明表明：`*p` 类型为 int，由于`*`被用于指针，因此，p变量本身必须是指针，可以说：**`p`本身是指针，但是`*p`不是指针，是int型的变量**。
对于`*`运算符两边的空格是可选的，实际上，在C语言中，我们使用`int *ptr`的格式，C++中我们也可以使用`int* ptr`，这两种的效果没有区别，因为C++允许这样做，但是**还是推荐前面的格式**。我们来看个例子：
```cpp
int *p1, *p2;   // 我们声明了两个指向int型的指针
int* p1, p2;    // 我们声明了一个int型的指针——p1，一个int型的变量p2.
```
对于每一个指针变量名，我们都要有一个`*`[^10]，对于其他类型的指针我们也可以这样声明。
虽然不同类型的指针所指对象不一样，但是他们本身的长度是一样的。地址的长度或值既不能指示关于变量的长度或类型的任何信息，也不能指示该地址上有什么内容。
我们可以在声明语句中初始化指针：
```cpp
int number = 666;
int *p_int = &number;
```
这样我们的`p_int`指针就指向了`number`这个int型的变量。

#### 3.8.2 指针的危险 <a id="指针的危险"></a>
在C++中创建指针的时候，计算机会分配用来存贮地址的内存，但不会分配用来存储指针所指向数据的内存。为数据提供空间是一个独立的步骤。[^11]
```cpp
int *p_int;       // 设置一个 int 型的指针 
*p_int = 114514;    // danger! 这里会出问题，这里的114514数据是没有内存地址的，因为他根本就没有被计算机分配内存。这就会导致指针所指向的地址是一个随机值。
```

#### 3.8.3 指针和数字 <a id="指针和数字"></a>
指针不是整形，但是计算机总是将地址当作整数处理。指针描述的是位置，将两个地址相乘是没有意义的（但是相加可能会有一点点意义吧——笔者并不是很了解）。因此，我们不能简单的将整数赋给指针。
```cpp
int *pt;
pt = 0xB800000000;  // 类型不符
```
这里的语句在C语言中是被允许的，但是在C++中，我们对类型的要求是更加严格的，所以我们要用到强制类型转化换：
```cpp
int *pt;
pt = (int *) 0xB80000000;   // 类型匹配
```

#### 3.8.4 使用new来分配新内存 <a id="使用new来分配内存"></a>
变量是在编译时分配的有名称的内存，而指针只是为可以通过名称直接访问的内存提供了一个别名。指针真正的用武之地在于，在运行阶段分配未命名的内存以存储值。在这种情况下，只能通过指针来访问内存。在C语言中，我们可以使用`malloc()`库函数来分配内存，在C++中，我们可以使用运算符`new`。
下面是声明语句格式：
`typeName *pointer_name = new typeName(same to former);`
这个语句告诉程序：“我要一块 typeName 类型的内存”。运算符 new 会自动为你分配相应的内存。并返回其地址赋给对应的指针，这样，你的指针就指向了一块新的地址。
上方指针指向的内存没有名称，我们可以说这个指针指向了一个**数据对象**。这里的对象不是我们OOP中的对象，而是一种**东西**。
使用new运算符分配内存是在程序运行的时候进行的。对于指针，需要指出的另一点是，new分配的内存块通常与常规变量声明分配的内存块不同。变量nights和pd的值都存储在被称为**栈**（stack）的内存区域中，而new从被称为**堆**（heap）或自由存储区（freestore）的内存区域分配内存。这些我们在后面会说）。

#### 3.8.5 用delete释放内存 <a id="用delete释放内存"></a>
当我们使用完对应空间时，我们可以将这片空间归还给计算机。这个时候我们就可以使用运算符`delete`了。使用delete时[^12]，后面要加上指向内存块的指针（这些内存块最初是用new分配的）。
只能用delete来释放使用new分配的内存。然而，对空指针（NULL或者是nullptr）使用delete是安全的.
事实上`delete`运算符删除的并不是我们的指针，而是指针所指向的内存地址，所以：**当我们使用两个指针指向同一块节点的时候，不要轻易使用`delete`，因为有可能导致错误删除**。

#### 3.8.6 使用new来创建动态数组 <a id="使用new来创建动态数组"></a>
如果我们使用简单的变量，`new`运算符的用处不是很大，但是，我们在使用大型数据的时候（如数组、字符串和结构），应使用new。如果通过声明来创建数组，则在程序被编译时将为它分配内存空间。不管程序最终是否使用数组，数组都在那里，它占用了内存。在编译时给数组分配内存被称为**静态联编**。但使用new时，如果在运行阶段需要数组，则创建它；如果不需要，则不创建。还可以在程序运行时选择数组的长度。这被称为**动态联编**。
1. 使用 new 来创建动态数组
   在C++中，我们创建动态数组很容易，我们只需要告诉 new 数组的元素类型和我们想要的数量即可。
   ```cpp
   int *ptr = new int [10]; // 创建一个10个int型数据的数组
   ```
   new 运算符会返回首元素的地址。这样我们就创建好了一个动态数组。当我们不用的时候我们就可以使用delete进行释放内存。
   ```cpp
   delete [] ptr;   // 释放内存
   ```
   如果使用new时带方括号，则使用delete时也应带方括号。
   总之，使用new和delete有一下的规则：
   - 不要使用delete释放不是new分配的内存
   - 不要使用delete释放同一个内存块两次
   - 如果使用new[]为数组分配内存，用delete[]释放
   - 使用new[]为实体（元素）分配内存，同delete释放。
   - 空指针( NULL 和 nullptr)对于delete是安全的
    
   还有就是对于new分配的内存，程序是不公用的，倒是我们不能使用sizeof运算符来确定动态分配的数组包含的字节数。
   下面是通用格式：
   ```cpp
   type_name *pointer_name = new type_name [num_elements];
   ```
2. 使用动态数组
   当我们想要访问数组的时候，我们可以将**指针名当数组名使用**。也就是说，我们在分配内存创建数组以后，这个指针不仅指向数组中的第一个元素，还可以代表着这个数组。数组和指针基本等价是C和C++的优点之一。但是这并不代表着两者就是一个东西。
   虽然我们可以使用指针名代表对应的数组，但是**指针和数组名有一些本质区别**：**指针是变量，但是数组名不是**，我们可以改变指针，但是不能修改数组名。

#### 3.8.7 指针、数组和指针算数 <a id="指针、数组和指针算数"></a>
**指针和数组基本等价的原因在于指针算数**。将整数变量加1后，其值将增加1；但将指针变量加1后，增加的量等于它指向的类型的字节数。于此同时，数组名在C++中被解释为地址。
```cpp
int arr1[10] = {0};
double arr2[3] = {114.514, 0.01, 350.234};
int *arr_ptr1 = arr1;     // ok;
int *arr_ptr2 = &arr2[0];   // ok;
```
上面的操作中，效果都是一样的，我们将指针指向了第一个元素。且都是合法的。
我们来看看数组表达式`arr2[0]`，C++编译器将该表达式看作是`*(stacks + 1)`这意味着先计算数组第2个元素的地址，然后找到存储在那里的值。两者是等价的。即`arrayname[index] == *(arrayname + index)`。如果是指针我们也支持这样的操作：`pointername[index] == *(pointername + index)`。
我们再看看sizeof运算符，对着数组用，我们返回的是数组的长度，而对指针应用则是指针的长度，即使指针指向了一个数组。在使用sizeof时，C++不会讲数组名解释为地址。

#### 3.8.8 指针和字符串 <a id="指针和字符串"></a>
数组和指针的特殊关系可以扩展到C-风格字符串。当我们创立一个C风格字符串后，使用cout进行输出时，我们传递的时首地址，无论是指针还是数组名。**这意味着对于数组中的字符串、用引号括起的字符串常量以及指针所描述的字符串，处理的方式是一样的，都将传递它们的地址。**这种传递地址的习惯，我们是比较提倡的，因为我们不用将所有的内弄进行传递，不仅减少了运行时间，我们还减少了消耗的内存。
对于字符串常量：有些编译器将字符串字面值视为只读常量，如果试图修改它们，将导致运行阶段错误。在C++中，字符串字面值都将被视为常量。有些编译器只使用字符串字面值的一个副本来表示程序中所有的该字面值
**C++不能保证字符串字面值被唯一地存储。**如果在程序中多次使用了字符串字面值“wren”，则编译器将可能存储该字符串的多个副本，也可能只存储一个副本。但是无论如何，由 const 声明的指向常量的指针是不被允许修改其中的内容的（因为本身我们就不可以修改常量的内容）。
我们来看一个非常重要的例子：
```cpp
char *ptr1;
char *ptr2 = new char [20];
// cin >> ptr1;    // error
cin >> ptr2;    // ok, 但是不要超过19个字符
cout << ptr1;   // 不建议，因为不同的编译器会导致不同的结果，可能会输出一段垃圾信息，也可能会导致崩溃。
cout << ptr2;   // 输出ptr2指向的字符串
int *ptr3;
char str = "pig";
cout << str << ' ' << (int *)str << endl;
cout << ptr << ' ' << (int *)ptr << ' ' << *ptr << endl;   
```
最后两个输出很有意思，第一段输出都是字符串内容，第二个是地址，第二行的第三个输出的是字符串的第一个字符。[^13]
这里直接输入ptr1是不可以的，因为他没有分配内存（是没分配指向的内存，cin和cout对于字符串的处理很相似，都是处理所指向的内容）。如果我们像ptr2输入，我们就可以正常输入。因为ptr2指向了分配好的内存。
对于指针代表的字符串之间的赋值，我们要用`<cstring>`库中`strcpy()`函数，他有两个参数：第一个是要复制的指针和数组名，第二个是样本。然后返回拷贝成功的首地址。如果分配的内存长度够用，但是我们不想用这么多，我们还可以使用`strncpy()`，与上方的函数差不多，但是有第三个参数来指定复制几个字符。

#### 3.8.9 多级指针 <a id="多级指针"></a>
指针具有很高的灵活性，指针不仅可以指向一些我们自己创建的类或者是系统内置的类，还可以做到**指向另一个指针**。这种指向指针的指针就叫做**多级指针**。
```cpp
int **point_pointer;
```
这里我们声明了一个二级指针，它可以指向`int*`类型的指针。变量名前面的`*`有几个就代表了是几级指针：
```cpp
int ***pointer3;    // 三级指针
```
多级指针的重要用途就是[创建和使用多维数组](#动态二维数组)。
使用多级指针可以创建多维的数组，但是与之相应的，我们也需要使用多次delete来对内存进行释放（详情见[动态二维数组](#动态二维数组)）

#### 3.8.10 指针数组 <a id="指针数组"></a>
由于指针本身也是一种类型，所以也会有对应的数组：
```cpp
typeName *ptrArray[Size];   // 创建了一个size大小的type类型的指针数组。
```
同普通指针一样，我们是也是将一堆指针放在了一片连续的空间中。冰洁都是相同类型。使用时像使用普通数组一样访问其成员即可。但是成员毕竟还是指针，对成员操作还是要像操作指针一样。
###### 这里其实就是多维指针的雏形。

#### 3.8.11 数组的替代品 <a id="数组的替代品"></a>
1. 模板类vector：<a id="vector"></a>
    模板类vector类似于string类，也是一种动态数组。您可以在运行阶段设置vector对象的长度，可在末尾附加新数据，还可在中间插入新数据。基本上，它是使用new创建动态数组的替代品。实际上，vector类确实使用new和delete来管理内存，但这种工作是自动完成的。
    首先，要使用vector对象，必须包含头文件vector。其次，vector包含在名称空间std中，因此我们可使用using编译指令、using声明或std::vector。第三，模板使用不同的语法来指出它存储的数据类型。第四，vector类使用不同的语法来指定元素数。
    ```cpp
    #include <iostream>
    #include <vector>
    using namespace std;
    ···
    vector<int> vi;     // 创建一个规模为0的数组
    int n;
    cin >> n;
    vector<double> vd(n);   // 创建一个n大小的数组
    ```
    其中，vi是一个vector<int>对象，vd是一个vector<double>对象。由于vector对象在我们插入或添加值时自动调整长度，因此可以将vi的初始长度设置为零。但要调整长度，需要使用vector包中的各种方法。
    下面是标准格式：
    ```cpp
    vector<typeName> vt(n_elem);    // 建一个n_elem大小的数组
    ```
2. 模板类array：<a id="array"></a>
    vector虽然比数组强大且方便，但是效率较低。鉴于此，C++11新增了模板类array，也位于名称空间std中。array数组的长度是固定的，也使用栈来存储，因此效率跟数组相同，但是更加方便和安全。要创建array对象，需要包含头文件array。array对象的创建语法与vector稍有不同：
    ```cpp
    #include <array>
    ···
    using namespace std;
    array<int, 5> ai;   // 创建一个含有5个int型的数组
    array<double, 4> ad = {1.2, 2.1, 3.43, 4.3};    // 像普通数组一样初始化
    ```

    下面是通用格式：

    ```cpp
    array<typeName, n_elem> arr; 
    ```
    和vector不同的是，n_elem不能是变量
3. 数组、vector、array之间的对比 <a id="数组、vector、array之间的对比"></a>
   1. 无论是数组、vector还是array对象，我们都可以使用标准的数组表示法来访问各个元素。
   2. array 和数组存贮在相同的内存区域（栈）中，而vector储存在另一个区域中（堆）。
   3. 一个array对象可以赋给另一个array对象，但是数组必须一个一个复制
   4. 对于非法索引的处理：数组是直接强行使用非法索引（但是你并不知道会获得什么，vector和array也是允许使用非法索引的，但是他们有一个更好的保障方法：使用`at()`成员函数。使用`at()`后，我们会检查这个索引是否合法，不合法程序就会中断，同时运行时间也会更长（因为使用了函数进行检查）。
   ###### 进一步的说明在后面的模板类中，可以直接[跳转](#模板类vector)

#### 3.9 存储方式 <a id="存储方式"></a>
C++有三种分配内存的方式：自动、静态和动态存储（或者叫**堆**）。这三种在存在时间长短的方面各不相同[^14]。
###### C++11中还新增了第四种类型——线程存储。
1. 自动存储
   在函数内部定义的常规变量使用自动存储空间，被称为**自动变量**。它们在所属的函数被调用时自动产生，在该函数结束时消亡。
   事实上，自动变量是一种**局部变量**。作用域是包含他的代码块。代码块是被包含在花括号中的一段代码。
   自动变量通常存储在**栈**中。这意味着执行代码块时，其中的变量将依次加入到栈中，而在离开代码块时，将按相反的顺序释放这些变量，这被称为**后进先出**。
   ###### 什么是局部变量？事实上局部变量都是自动变量，被存储在栈上，但是**静态局部变量**不是。他术语静态存储中的变量。
2. 静态存储
   **静态存储是整个程序执行期间都存在的存储方式**。使变量成为静态的方式有两种：一种是在函数外面定义它；另一种是在声明变量时使用关键字`static`：`static int number = 100;`
   自动存储和静态存储的关键在于：这些方法严格地限制了变量的寿命。变量可能存在于程序的整个生命周期（静态变量），也可能只是在特定函数被执行时存在（自动变量）。
3. 动态存储
   new和delete运算符提供了一种比自动变量和静态变量更灵活的方法。它们管理了一个内存池，这在C++中被称为自由存储空间（freestore）或**堆（heap）**。
   这个空间是独立于静态变量和自动变量内存空间的。数据的生命周期不完全受程序或函数的生存时间控制。然而，**内存管理也更复杂了**。在栈中，自动添加和删除机制使得占用的内存总是连续的，但new和delete的相互影响可能导致占用的自由存储区不连续，这使得跟踪新分配内存的位置更困难。

## 四、循环和关系表达式 <a id="循环和关系表达式"></a>
### 4.1 for循环 <a id="for循环"></a>
#### 4.1.1 for循环组成<a id="for循环组成"></a>
for循环组成步骤有下面几个：
1. 设置初始值
2. 执行测试
3. 执行循环操作
4. 更新用于测试的值

初始化、测试和更新操作构成了控制部分，这些操作由括号括起。其中每部分都是一个表达式，彼此由分号隔开。控制部分后面的语句叫作循环体，只要测试表达式为true，循环体就会被执行。通用格式如下：
```cpp
for(initialization; test-expression; update-expression)
    body
```
C++时将整个for看作一个语句。

循环只执行一次初始化，可以设置起始值，然后用对应的变量计算循环周期。

测试表达式决定循环体是否执行。通常这个表达式为关系表达式（将两个值进行比较）。

for循环是入口条件（entry-condition）循环。这意味着在每轮循环之前，都将计算测试表达式的值，当测试表达式为false时，将不会执行循环体。这种在循环之前进行检查的方式可避免程序遇到麻烦。
update-expression（更新表达式）在每轮循环结束时执行，此时循环体已经执行完毕。通常，它用来对跟踪循环轮次的变量的值进行增减。然而，它可以是任何有效的C++表达式，还可以是其他控制表达式。

for语句看这样一个函数调用，但是for在C++中是一个关键字，并且不允许将函数命名为for。

#### 4.1.2 for循环变量初始化 <a id="for循环变量初始化"></a>
在以前，for语句中是不能进行初始化的，也就是说，像`for(int i = 0; i < 2; i++)`这样的语句在以前是非法的。以前是可以同构一种新的表达式来是其合法的：
```cpp
for(for-init-statement condition; expression)
    statement;
```
这种语句中`for-init-statement`被视为一条语句，它既可以是表达式语句也可以是一种声明语句。这种表达式有一种好处：这种生命的变量作用域只用for语句中：
```cpp
for(int i = 0; i < 2; i++){
    cout << i << ' ' << endl;
}
cout << i;  // ERROR!
```

#### 4.1.3 循环步长 <a id="循环步长"></a>
for循环中的i变化程度就是**步长**。它可以是任何整数值，格式如下：
`i = i + by`
其中的 by 是我们的步长。
当然最常见的还是**自增和自减**：
```cpp
for (int i = 0; i < 2; i++);
for (int i = 0; i < 2; ++i);
for (int i = 0; i > -2; i--);
for (int i = 0; i > -2; --i);
```

#### 4.1.4 遍历字符串 <a id="遍历字符串"></a>
由于for循环有着便利的作用，我们可以使用for语句来访问字符串中的每一个元素，就像下面的例子：
```cpp
#include <iostream>
#include <string>

int main()
{
    using namespace std;
    cout << "Enter a word: ";
    string word;
    cin >> word;

    // display letters in reverse order
    for (int i = word.size() - 1; i >= 0; i--)
        cout << word[i];
    
    cout << "\nBye.\n";
    return 0;
}
```
下面是结果：
```
Enter a word: animal
lamina
Bye.
```
遍历数组也是一样的方法，字符串就是一种特殊的数组。

#### 4.1.5 自增 / 自减运算符 <a id="自增 / 自减运算符"></a>
这两个运算符是将一个变量进行加1和减1的操作，但是他们有两个版本：**前缀和后缀版**。前缀（prefix）版本位于操作数前面，如++x；后缀（postfix）版本位于操作数后面，如x++。两者的效果一样，但是影响的时间不同，返回值也不同。
```cpp
int a = 1;
cout << a++;    // 输出1，此时a变成了2.
cout << ++a;    // 输出3，此时a变成了3
cout << a;  // 输出3
```
前缀变体`++a`是先进行自增，在将增完后的值返回，后缀变体`a++`则是将变量值先返回，再进行自增。总的来说`++a`返回`a+1`，`a++`返回`a`。自减运算符同理。
这种运算符虽然方便，但是我们不能在一条语句中使用多次。这样会导致我们的行为未定义。
```cpp
x = 2 * (x++) * (3 - ++x);  // 未定义行为
```
###### 对于自增 / 自减运算符我们还有对应的副作用：
C++程序中，我们是存在**顺序点**的。在这里，进入下一步之前将确保对所有的副作用都进行了评估。在C++中，语句中的分号就是一个顺序点，这意味着程序处理下一条语句之前，赋值运算符、递增运算符和递减运算符执行的所有修改都必须完成。
我们有这样的程序：
```cpp
for(int i = 0; i < 2; i++){
    cout << i;
}
```
在这个语句中，括号内有三个顺序点。分别对应着三个表达式的末尾。这就保证了三个语句在运行时会严格按照我们的顺序点对效果进行评估。
这里的输出是`01`，为什么不是`12`?这还与我们的语句的执行顺序有关。for语句中的执行顺序是：
1. 先初始化循环变量（在第一轮）
2. 再进行条件判断
3. 执行循环体
4. 进行变量更新（调整循环变量）

像for语句这样的语句还可以叫做**完整表达式**，**因为他们并不在另一个更大的表达式中充当子表达式**。像这个例子：
```cpp
y = (4 + x++) + (6 + ++x);
```
像这样的表达式，`x++`和`++x`都不是完整表达式，括号中没有顺序点，我们无法对其进行评估，也就无法对x的值进行确定。

前缀和后缀版本虽然执行的操作效果相同，但是只是对内置类型看不出来。当我们在进行自定义类型的时候，前缀和后缀的差别就显现出来了。
**前缀的效率要比后缀效率高**。

#### 4.1.6 组合赋值运算符 <a id="组合赋值运算符"></a>
C++语法中有能够合并简单运算和赋值的运算符，他们的功能都等价于先进行运算在进行赋值（虽然这个运算符本身也是这个效果。
```
// 一种方式：
number1 operatorAndEqual number2;

// 等价方式
number1 = number1 operator number2;
```
下边是对应组合赋值运算符和其含义：
| 操作符 | 作用 (L为左操作数，R为右操作数) |
| :--- | :--- |
| += | 将 L+R 赋给 L |
| -= | 将 L-R 赋给 L |
| *= | 将 L*R 赋给 L |
| /= | 将 L/R 赋给 L |
| %= | 将 L%R 赋给 L |

### 4.3 表达式和语句 <a id="表达式和语句"></a>
#### 4.3.1 表达式 <a id="表达式"></a>
C++成为非常具有表现力的语言。任何值或任何有效的值和运算符的组合都是表达式。无论是算数、赋值还是任何能得出有效值的表达式。
```cpp
maids = (cooks = 4) + 3;
x = y = z = 0;
```
上面是两个很好的例子，第一个虽然在C++中是允许的（因为赋值表达式返回结果4），但是不提倡，第二个表达式时**链式赋值**，可以进行连续赋值，顺序是从左向右。
**任何表达式带上`;`都可以成为一个语句，但是不一定有编程含义，但是反过来就不一定正确。**就像下面两个例子：
```cpp
int number;
int fx = for(int i = 0; i < 2; i++) cout << "I love you.";
```
这两个语句第一个去掉分号不是一个表达式，因为没有值，第二个是非法的，因为for语句不是表达式，我们不能将他赋给fx。

#### 4.3.2 复合语句（语句块）<a id="复合语句"></a>
复合语句（代码块）由一对花括号和它们包含的语句组成，被视为一条语句。下面有一对对比例子：
```cpp
// 正常使用代码块的：
for (int i = 0; i < 10; i++){
    cout << "I Love China.";
    cout << "Now, we remained " << 10 - i << "times.";
}
cout << "Over";

// 没有花括号的：
for (int i = 0; i < 10; i++)
    cout << "I Love China.";
    cout << "Now, we remained " << 10 - i << "times.";
cout << "Over";
```
第一个程序会正常执行。但是第二程序就会报错。编译器会返回“不知道 i 是什么变量的错误”（当然也可能会输出在外界定义的 i 的值。用花括号将程序括起来会将`{}`中的内容视为一个语句。但是如果省略了花括号，只有缩进，编译器将忽略缩进，这时**只有第一条语句位于循环中。**
对于代码块中的**变量生命周期**[^15]。

复合语句还有一种有趣的特性。如果在语句块中定义一个新的变量，则仅当程序执行该语句块中的语句时，该变量才存在。执行完该语句块后，变量将被释放。在外部语句块中定义的变量在内部语句块中也是被定义了的。
如果在一个语句块中声明一个变量，而外部语句块中也有一个这种名称的变量，在声明位置到内部语句块结束的范围之内，新变量将隐藏旧变量；然后就变量再次可见。（也就是说，内部的变量会掩盖外部的变量，这里可以看Framework图）。

#### 4.3.3 逗号运算符 <a id="逗号运算符"></a>
语句块允许把两条或更多条语句放到按C++句法只能放一条语句的地方。逗号运算符对表达式完成同样的任务，允许将两个表达式放到C++句法只允许放一个表达式的地方。给个例子：
```cpp
for (int i = 0, j = 0; i < 2, j < 2; i++, j++){
    ···
}
int i = 0, j = 0;
i++, j++;
cout << i << j;
```
到目前为止，逗号运算符最常见的用途是将两个或更多的表达式放到一个for循环表达式中。不过C++还为这个运算符提供了另外两个特性。首先，它确保先计算第一个表达式，然后计算第二个表达式（换句话说，逗号运算符是一个顺序点）。如下所示的表达式是安全的：
```cpp
i = 20, j = 2 * i;
```
其次，C++规定，逗号表达式的值是第二部分的值。上述表达式的值为 40，因为 j = 2 * i 的值为40。
在所有运算符中，**逗号运算符的优先级是最低的**。下面是一个典型例子：
```cpp
cats = 17, 240;
```
这里是被解释成：`(cats = 17), 240`。这里的 240 就不起作用，最后的
表达式效果就是`cats = 17;`。
但是，如果表达式是这样的：`cats = (17, 240);`，这样的结果就是`cats = 240`。由于逗号表达式是最后一个表达式的值。

#### 4.3.4 关系表达式 <a id="关系表达式"></a>
C++提供了6种关系运算符来对数字进行比较。由于字符用其ASCII码表示，因此也可以将这些运算符用于字符。不能将它们用于C-风格字符串，但可用于string类对象。对于所有的关系表达式，如果比较结果为真，则其值将为true，否则为false，因此可将其用作循环测试表达式。（老式实现认为结果为true的关系表达式的值为1，而结果为false的关系表达式为0。）
| 操作符 | 含义 |
| :--- | :--- |
| < | 小于 |
| <= | 小于或等于 |
| == | 等于 |
| > | 大于 |
| >= | 大于或等于 |
| != | 不等于 |

**关系运算符的优先级比算术运算符低**。例：
```cpp
// 原始表达式
x + 3 > y - 2              // Expression 1

// 实际对应的逻辑（算术运算优先）
(x + 3) > (y - 2)          // Expression 2

// 错误的理解（关系运算不优先）
x + (3 > y) - 2            // Expression 3
```
**注意事项**：对于`==`和`=`两个运算符，他们很相似，但是一定不能混用。对于将`==`用于进行判断的循环，两者混用会导致严重的后果——陷入死循环。发现这种错误的困难之处在于，代码在语法上是正确的，因此编译器不会将其视为错误。

#### 4.3.5 C-风格字符串的比较 <a id="字符数组的比较"></a>
对于这样的语句（其中的 word 是字符数组）：
```cpp
word == "mate";
```
这样的比较是不会得出我们想要的结果的，因为**数组名是数组的首地址**，这样的比较其实是在比较两个字符串的首地址是否是同一地址。显然不能得出我们想要的结果。，这种情况下就需要我们进行一个一个字符的比对，或者是使用头文件 `<cstring>` 中的 `strcmp()` 函数。
`strcmp()` 函数接受两个字符串地址作为参数。这意味着参数可以是指针、字符串常量或字符数组名。如果两个字符串相同，该函数将返回零；如果第一个字符串按字母顺序排在第二个字符串之前，则 `strcmp()` 将返回一个负数值；如果第一个字符串按字母顺序排在第二个字符串之后，则 `strcmp()` 将返回一个正数值。这里的字符比较是根据字符的系统编码来进行比较的。使用ASCII码时，所有大写字母的编码都比小写字母小，所以按排列顺序，大写字母将位于小写字母之前。

C-风格字符串是通过结尾的空值字符定义的，而不是由其所在数组的长度定义的。这意味着两个字符串即使被存储在长度不同的数组中，也可能是相同的：
```cpp
char big[80] = "Daffy";     // 5 letters plus \0
char little[6] = "Daffy";       // 5 letters plus \0
```

虽然不能用关系运算符来比较字符串，但却可以用它们来比较字符，因为字符实际上是整型。
```cpp
cout << ('1' == '2');     // 输出 0
```

总结：
1. **`strcmp()` 的基本功能**
    * 用于测试 C-风格字符串（以 `\0` 结尾的字符数组）是否相等或其排列顺序。

2. **判断字符串相等**
    * 当 `str1` 和 `str2` **完全相等**时，表达式返回 `true`：
    ```cpp
    strcmp(str1, str2) == 0
    ```

3. **判断字符串不相等**
    * 当 `str1` 和 `str2` **不相等**时，以下两个表达式均为 `true`：
    ```cpp
    strcmp(str1, str2) != 0
    strcmp(str1, str2)      // 在 C/C++ 中，非 0 即为 true
    ```

4. **判断字典序（排列顺序）**
    * 如果 `str1` 在 `str2` 的**前面**：
    ```cpp
    strcmp(str1, str2) < 0
    ```
    * 如果 `str1` 在 `str2` 的**后面**：
    ```cpp
    strcmp(str1, str2) > 0
    ```

5.  `strcmp()` 函数非常灵活，可以分别扮演 `==`、`!=`、`<` 和 `>` 运算符的角色。

#### 4.3.6 string类字符串的比较 <a id="string类字符串的比较"></a>
如果使用string类字符串而不是C-风格字符串，比较起来将简单些，我们能够使用关系运算符进行比较。这之所以可行，是因为类函数重载（重新定义）了这些运算符。
string之间的比较规则和strcmp()的规则差不多。但是符号更加接近我们正常的使用：
1. **相等判断 (`==` 与 `!=`)**
    * `==`：如果两个字符串长度相同且每个对应位置的字符都相同，返回 `true`。
    * `!=`：只要长度不同或有任何一个字符不同，返回 `true`。
    ```cpp
    string s1 = "hello";
    string s2 = "hello";
    if (s1 == s2) { /* 结果为 true */ }
    ```

2. **顺序判断 (`>`、`<`、`>=`、`<=`)**
    * `<`：如果 `s1` 在字典中排在 `s2` 之前，返回 `true`。
    * `>`：如果 `s1` 在字典中排在 `s2` 之后，返回 `true`。
    ```cpp
    string a = "apple";
    string b = "banana";
    if (a < b) { /* 结果为 true，因为 'a' < 'b' */ }
    ```

### 4.4 while循环 <a id="while循环"></a>
while循环是没有初始化和更新部分的for循环，它只有测试条件和循环体：
```
while (test-condition)
    body
```

首先，程序计算圆括号内的测试条件（test-condition）表达式。如果该表达式为true，则执行循环体中的语句。与for循环一样，循环体也由一条语句或两个花括号定义的语句块组成。执行完循环体后，程序返回测试条件，对它进行重新评估。如果该条件为非零，则再次执行循环体。测试和执行将一直进行下去，直到测试条件为false为止。
显然，如果希望循环最终能够结束，**循环体中的代码必须完成某种影响测试条件表达式的操作**。例如，循环可以将测试条件中使用的变量加1或从键盘输入读取一个新值。和for循环一样，while循环也是一种入口条件循环。因此，如果测试条件一开始便为false，则程序将不会执行循环体。

#### 4.4.1 for和while <a id="for和人while"></a>
在C++中for和while的本质是相同的：
```cpp
for (init-expression; test-expression; update-expression)
{
    statement(s)
}
// 等价于下面的语句
init-expression;
while (test-expression)
{
    statement(s)
    update-expression;
}
// 同理还可以像下面一样等价。
for (; test-expression;)
    body
// 等价于
while (test-expression)
    body
```
for循环需要3个表达式（从技术的角度说，它需要1条后面跟两个表提示：错误的标点符号达式的语句），不过它们可以是空表达式（语句），只有两个分号是必需的。另外，省略for循环中的测试表达式时，测试结果将为true，因此下面的循环将一直运行下去：
```cpp
for ( ; ; )
    body
```
由于for循环和while循环几乎是等效的，因此究竟使用哪一个只是风格上的问题。它们之间存在三个差别:
1. 在for循环中省略了测试条件时，将认为条件为true；
2. 在for循环中，可使用初始化语句声明一个局部变量，但在while循环中不能这样做
3. 如果循环体中包括continue语句，情况将稍有不同.
通常，程序员使用for循环来为循环计数，因为for循环格式允许将所有相关的信息—初始值、终止值和更新计数器的方法—放在同一个地方。在无法预先知道循环将执行的次数时，程序员常使用while循环。[^16]

for循环和while循环都由用括号括起的表达式和后面的循环体（包含一条语句）组成。这条语句可以是语句块，其中包含多条语句。**语句块是由花括号，而不是由缩进定义的**。
当我们只有分号没有花括号进行括起时：**分号就会结束语句**。同理，如果在`while (test-exprssion)`后面紧跟这个一个分号，这个分号就会直接将这个while语句和后面的内容分开。容易造成死循环。

#### 4.4.2 延时循环 <a id="延时循环"></a>
ANSI C和C++苦衷有一个函数可以帮助我们进行延时循环（为了方便我们进行读取计算结果等）。函数名为`clock()`，他会返回层序开始执行后所用的系统时间。但是有两个问题：首先，`clock()`返回时间的单位不一定是秒；其次，该函数的返回类型在某些系统上可能是long，在另一些系统上可能是unsigned long或其他类型。
但是头文件`<ctime>`解决了这个问题：首先，它定义了一个符号常量`CLOCKS_PER_SEC`，该常量等于每秒钟包含的系统时间单位数。因此，将系统时间除以这个值，可以得到秒数。或者将秒数乘以`CLOCK_PER_SEC`，可以得到以系统时间单位为单位的时间。其次，ctime将`clock_t`作为`clock()`返回类型的别名[^17]。这意味着可以将变量声明为`clock_t`类型，编译器将把它转换为long、unsigned int或适合系统的其他类型。
下面给出一个例子：
```cpp
#include <iostream>
#include <ctime>
using namespace std;
int main(){
    cout << "Enter thew delay time, in senconds.";
    float secs;
    cin >> secs;
    clock_t delay = secs * CLOCK_PRE_SEC;

    cout << "starting\a\n:";
    clock_t start = clock();
    while (clock() - start <delay);
    cout << "done \a\n";
    return 0;
}
```

### 4.5 do while循环 <a id="do while循环"></a>
do while循环不同于另外两种循环，它是出口条件（exit condition）循环[^18]。这种循环将首先执行循环体，然后再判定测试表达式，决定是否应
继续执行循环。下面是对应的语法：
```
do 
    body
while (test-expreesion)
```
这里的循环体是一条语句或用括号括起的语句块。
通常，入口条件循环比出口条件循环好，因为入口条件循环在循环开始之前对条件进行检查。
给出个对比例子：
```cpp
int I = 0;
for(; ;){
    I++;
    // do something ···
    if (30 <= I) break;
}
// 等价于
int I = 0；
do {
    I++;
    // do something;
} while (30 > I);
```

### 4.6 基于范围的for循环 <a id="基于范围的for循环"></a>
C++11 新增了一种循环：基于范围的for循环。这简化了一种常见的循环任务：对数组（或容器类，如vector和array）的每个元素执行相同的操作。
这里先给出格式：
```cpp
for (typeName variable : container/range)
    body
```
基于范围的for循环有很多用途：
1. 对数组或容器类中的元素进行相同的操作：
    ```cpp
    double prices[5] = {4.99, 10.99, 6.87, 7.99, 8.49};
    for (double x : prices)
        cout << x << std::endl;
    ```
2. 修改容器中的元素：
   ```cpp
   for (double &x : prices)
        x = x * 0.8;    // 打八折     
   ```
   这里的`&`表示引用，在[这里](#引用)有介绍。
3. 这里的容器没有规定必须是变量或者是常量，我们甚至可以使用初始化列表：
   ```cpp
    for (int x : {3, 5, 2, 8, 6})
        cout << x << " ";
    cout << '\n';
   ```

### 4.7 循环和文本输入 <a id="循环和文本输入"></a>
while循环常用的场景是逐字符的读取文本（用户输入）。
#### 4.7.1 cin输入 <a id="cin输入"></a>
为了让程序知道什么时候停止读取，我们可以设置**哨兵字符**作为停止标记。下面有一个简单的例子：
```cpp
#include <iostream>
int main()
{
    using namespace std;
    char ch;
    int count = 0;          // use basic input
    cout << "Enter characters; enter # to quit:\n";
    cin >> ch;              // get a character
    while (ch != '#')       // test the character
    {
        cout << ch;          // echo the character
        ++count;            // count the character
        cin >> ch;          // get the next character
    }
    cout << endl << count << " characters read\n";
    return 0;
}
```
运行情况：
```
Enter characters; enter # to quit:
see ken run#really fast
seekenrun
9 characters read
```
上面的程序有一个问题：我们无法记录空格和换行符，因为cin会忽略到这两个字符，同时发送给cin的输入被缓冲。这意味着只有在用户按下回车键后，他输入的内容才会被发送给程序。这就是在运行该程序时，可以在#后面输入字符的原因。按下回车键后，整个字符序列将被发送给程序，但程序在遇到#字符后将结束对输入的处理。

#### 4.7.2 使用cin.get(char)输入 <a id="cin.get()输入"></a>
当我们想要进行一个一个字符读取的时候，会使用cin.get()来获取输入。这是cin所属的istream类中的成员函数。成员函数cin.get(ch)读取输入中的下一个字符（即使它是空格），并将其赋给变量ch。这样我们就能一个一个计数我们想要的字符了。

#### 4.7.3 cin.get()补充 <a id="cin.get()补充"></a>
cin.get()一种版本接受两个参数：数组名 + 输入规模。另一种是不接受参数：只能读取一个字符。但是在这里我们有使用了一个新的格式：cin.get(char)。这种一个函数可以实现多个功能的情况叫做**函数重载**，可以直接[跳转](#函数重载)进行了解。
不接受任何参数的cin.get()成员函数返回输入中的下一个字符。我们可以这样使用：
```cpp
ch = cin.get();
```
该函数的工作方式与C语言中的`getchar()`相似，将字符编码作为int值返回；而`cin.get(ch)`返回一个对象，而不是读取的字符。同样，可以使用`cout.put()`函数来显示字符。
```cpp
cout.put(ch);
```
该函数的工作方式类似C语言中的`putchar()`，只不过其参数类型为char，而不是int[^21]。






#### 4.7.4 文件尾条件 <a id="文件尾条件"></a>
对于结束输入的哨兵字符的使用，可能会与我们正常的字符使用相冲突。毕竟像`#`在文件中也会有固定的含义，当我们进行文件读取的时候这个问题就会被放大。当我们的输入来自于文件的时候[^19]，我们可以使用**检测文件尾(EOF)**。
检测到EOF后，cin将两位（eofbit和failbit）[^20]都设置为1。可以通过
成员函数`eof()`来查看`eofbit`是否被设置；如果检测到EOF，则`cin.eof()`将返回bool值true，否则返回false。同样，如果`eofbit`或`failbit`被设置为1，则`fail()`成员函数返回true，否则返回false。注意，`eof()`和`fail()`方法报告最近读取的结果；也就是说，它们在事后报告，而不是预先报告。因此应将`cin.eof()`或`cin.fail()`测试放在读取后。
1. EOF状态的清除
   cin设置EOF后，cin将不读取输入，再次调用cin也不管用。这个时候我们就可以使用`cin.clear()`方法来清除EOF标记，但是在某些系统中这个是无效的。
2. 常见字符输入做法：
   直接给出一个例子：
   ```cpp
   cin.get(ch);
   while (cin.fail() == false) {
    ···
    cin.get(ch);
   }
   ```
   这里`cin.get(ch)`返回的其实是一个cin对象，但是C++中的istream类提供了一个将istream类转换成bool值的函数，这样我们就可以直接将cin用于条件判断了：
   ```cpp
    while (cin);    // while input is successful
   ```
   这样上方的程序就可以简化为：
   ```cpp
   while (cin.get(ch)){
    ···
   }
   ```

为了正确使用`cin.get()`，我们要知道如何让处理EOF。`cin.get()`返回的EOF是一种符号常量，该常量是在头文件iostream中定义的。EOF值必须不同于任何有效的字符值，以便程序不会将EOF与常规字符混淆。通常，**EOF被定义为值−1**。但是EOF不表示输入中的字符，而是指出没有字符。

对于有些系统，char类型是没有符号的，因此char变量可能为 EOF。我们在测试 EOF 的时候就必须使用 int 变量，但是这是如果我们要显示对应的 char 变量就必须将 int 变量转成 char 类型。

总结一下`cin.get(ch)`和`cin.get()`：
| 属性 | cin.get(ch) | ch = cin.get( ) |
| :--- | :--- | :--- |
| **传递输入字符的方式** | 赋给参数 ch | 将函数返回值赋给 ch |
| **用于字符输入时函数的返回值** | istream 对象（执行 bool 转换后为 true） | int 类型的字符编码 |
| **到达 EOF 时函数的返回值** | istream 对象（执行 bool 转换后为 false） | EOF |

对于这两种方式的使用，使用字符参数的版本更符合对象方式，因为其返回值是istream对象。这意味着可以将它们拼接起来：
```cpp
cin.get(ch1).get(ch2)
```
这是可行的，因为 cin.get(ch) 会返回一个 istream 对象。然后就可以通过这个对象调用 get(ch)。

### 4.8 嵌套循环和二维数组 <a id="嵌套循环和二维数组"></a>
前面第三大节说的是一维数组，这里记录的是二维数组。一维数组本身可以看成是一行数据，二维数组看起来更像一个表格————既有数据行也有数据列。
C++并没有提供二维数组类型，但是我们可以通过使用指针和嵌套循环来创建一个二维数组。

#### 4.8.1 静态二维数组 <a id="静态二维数组"></a>
创建二维数组的方式是将数组中的每一个元素看成是一个数组。就像下面的例子：
```cpp
int doubleDimensionArray [4][5];
```
这样我们就声明了一个 4 * 5 的二维数组————有四行，每行有5个 int 类型的值。
下面是通用格式：
```cpp
typeName marixName [row][col] = {{···}, {···}, ···};    // initialization
```

#### 4.8.2 动态二维数组 <a id="动态二维数组"></a>
和创建静态二维数组的思路一样，我们将一个数组的每一元素看成是另一个数组，但是这两层都是动态的数组。
```cpp
int row = 2, col = 3;
int **arr = new int*[rows];
for (int i = 0; i < row;i i++){
    arr[i] = new int [col];
}

for (int i = 0; i < row; i++){
    delete[] arr[i];
}
delete[] arr;
```
这里是使用 new 和二级指针来创建二维数组。上面的程序既包含了二维数组的创建，也包含了对应空间的释放。
1. 使用new来创建二维数组：
   二维数组本质上是一个指针数组，就像前面说的，数组名和指针其实在一些方面是等价的，我们可以使用指针来创建数组，元素对应的数组由于要看成数组，所以我们要使用指针来充当对应的数组元素。从而达到多维的目的。
   ```cpp
   typeName **matrixName = new typeName* [rows];    // 先开一个指针数组的空间
   for (int index = 0; index < rows; index++){
        matrix[index] = new typeName [cols];    // 在让指针数组中的每一个元素指向对应的空间。
   }
   ```
   我们先创建一个指针数组，这个数组由二级指针来表示（因为数组中的元素是指针，所以要用指向指针的指针）。然后我们再用for循环来对每一个元素分配空间，这时就是正经创建数组了。
2. 使用delete来释放内存：
   我们在创建动态二维数组的时候我们一共进行了两次大的分配内存的方式————一是为指针数组开辟内存，二是让指针数组中的元素指向new出来的空间。所以我们释放内存也要进行两次————**先释放元素，在释放指针数组**。
   ```cpp
   for (int index = 0; index < rows; index++){
        delete[] matrixName[index];
   }
   delete[] matrixName;
   ```
   如果先释放了对应的指针数组，就是释放了数组中的指针，**指针指向的内容就会丢失，发生内存泄露**。

以上方式是使用传统的 new 和 二级指针来创建二维数组的方法，我们还可以使用 vector 来创建二维数组（更推荐）。
```cpp
vector<vector<typeName>> matrix(rows, vector<typeName>(cols, initial_value));
```
或者是：
```cpp
vector<vector<typeName>> matrix = {
    {elements},
    {elements},
    {elements},
    ···
}
```
我们甚至可以逐行进行添加（即行数和列数是不固定的）
```cpp
vector<vector<typeName>> matrix;
// 添加一行
vector<typeName> row = {···};
matrix.push_back(row);

// 在循环中添加
for (int i = 0; i < 3; i++) {
    vector<typeName> temp;
    for (int j = 0; j < 4; j++) {
        temp.push_back(i+j);
    }
    matrix.push_back(temp);
}
```

#### 4.8.3 初始化二维数组 <a id="初始化二维数组"></a>
创建二维数组时，可以初始化其所有元素。这项技术建立在一维数组初始化技术的基础之上：提供由逗号分隔的用花括号括起的值列表：
```cpp
int maxtemps[4][5] =   // 2-D array
{
    {96, 100, 87, 101, 105},   // values for maxtemps[0]
    {96, 98, 91, 107, 104},    // values for maxtemps[1]
    {97, 101, 93, 108, 107},   // values for maxtemps[2]
    {98, 103, 95, 109, 108}    // values for maxtemps[3]
};
```
当然，静态数组我们可以进行初始化，我们的动态数组也可以。
```cpp
int **matrix = new int* [rows];
for (int i = 0; i < rows; i++){
    matrix[i] = new int [cols] {0};
}
```
这时我们就创建了一个元素全为0的二维数组。

#### 4.8.4 嵌套循环和使用二维数组 <a id="嵌套循环和使用二维数组"></a>
for循环和while循环都是可以嵌套的：
```cpp
// for 的嵌套
for (int i = 0; i < 10; i++){
    for (int j = 0; j < 10; j++){
        cout << "Now it's (" << i << "," << j << ")." << endl;
    }
}

// while 的嵌套
int i = 0, j = 0
while (i < 10){
    while (j < 10){
        cout << "Now it's (" << i << "," << j << ")." << endl;
        j++;
    }
    i++;
    j = 0;
}
```
上面的两个部分是等效的。都使用了循环嵌套，循环嵌套是遍历对应数组的一个很好的方法，但是需要注意的是，这种遍历的时间复杂度通常是$O(n^2)$
下面给出一个循环嵌套和遍历二维数组的例子。
```cpp
char matrix[5][30] = {
    "YaoYaoSiWuYaoSi",
    "YaoJiuYaoJiuBaYaoLing",
    "Hakimi",
    "GuGuGaGa",
    "What's dog doing"
}
const char* stringName[5] = {
    "BaGeYaLu",
    "BiBiLaBu",
    "Hello~",
    "Man",
    "OUT"
}
for (int i = 0; i < 5; i++){
    int j = 0;
    while (matrix[i][j]){
        cout.put(matrix[i][j]);
        j++;
    }
    cout << endl;
}
```
上方的字符数组换成 string 类也是可以的。
```cpp
const string cities[Cities] =    // array of 5 strings
{
    "Gribble City",
    "Gribbletown",
    "New Gribble",
    "San Gribble",
    "Gribble Vista"
};
```

## 五、分支语句和逻辑运算符 <a id="分支语句和逻辑运算符"></a>
### 5.1 if语句 <a id="if语句"></a>
当C++程序必须决定是否执行某个操作时，通常使用if语句来实现选择。if有两种格式：`if`和`if else`。如果测试条件为true，则if语句将引导程序执行语句或语句块；如果条件是false，程序将跳过这条语句或语句块。因此，if语句让程序能够决定是否应执行特定的语句。

if的格式和 while 很相似：
```
if (test-expression)
    statement
```
如果test-condition（测试条件）为true，则程序将执行statement（语句），后者既可以是一条语句，也可以是语句块。如果测试条件为false，则程序将跳过语句

和循环测试条件一样，if测试条件也将被强制转换为bool值，因此0将被转换为false，非零为true。整个if语句被视为一条语句。通常情况下，测试条件都是关系表达式。

### 5.2 if else语句 <a id="if else语句"></a>
if语句让程序决定是否执行特定的语句或语句块，而if else语句则让程序决定执行两条语句或语句块中的哪一条，这种语句对于选择其中一种操作很有用。下面是对应的格式：
```
if (test-expression)
    statement1
else
    statement2
```
如果测试条件为true或非零，则程序将执行statement1，跳过statement2；如果测试条件为false或0，则程序将跳过statement1，执行statement2。每条语句都既可以是一条语句，也可以是用大括号括起的语句块，从语法上看，整个if else结构被视为一条语句。

if else中的两种操作都必须是一条语句。如果需要多条语句，需要用大括号将它们括起来，组成一个块语句。
```cpp
// 有问题的语句
if (ch == 'Z')
    zorro++;        // if ends here
    cout << "Another Zorro candidate\n";
else                // wrong
    dull++;
    cout << "Not a Zorro candidate\n";

// 正确的语句
if (ch == 'Z')
{                   // if true block
    zorro++;
    cout << "Another Zorro candidate\n";
}
else
{                   // if false block
    dull++;
    cout << "Not a Zorro candidate\n";
}
```
C++是自由格式语言，因此只要使用大括号将语句括起，对大括号的位置**没有任何限制**。

### 5.3 if-else if-else 结构 <a id="if-else if-else结构"></a>
if 和 else if语句只能分别处理一个语句块、两个语句块。当我们想进行多个语句的判断时，就要使用`if-else if-else`结构。
实际上，这种结构只是一个if else被包含在另一个if else中。判断顺序是`if->else(if->else(···))`，这样就会一层一层的向下判断。

### 5.3 逻辑表达式 <a id="逻辑表达式"></a>
为了进行条件判断，C++ 提供了三种逻辑运算符：OR(`||`), AND(`&&`), NOT(`!`)。
#### 5.3.1 OR运算符 <a id="OR运算符"></a>
OR运算符(`||`)表示**或运算**。即在一个表达式中，只要有一个是真值，那么整个表达式的值就是 true，否则就是 false。
C++规定，||运算符是个**顺序点**（sequence point）。也是说，先修改左侧的值，再对右侧的值进行判定（C++11的说法是，运算符左边的子表达式先于右边的子表达式）。也就是说我们的表达式是从左向右进行的。
下面是工作原理：
| | expr1 || expr2 的值 | |
| :--- | :--- | :--- |
| | expr1 == true | expr1 == false |
| expr2 == true | true | true |
| expr2 == false | true | false |

**注意**：对于这个`||`运算，我们有一个叫**短路求值**的东西，即**当我们在左侧已经能判断出整个表达式的值，我们便不再执行后面的语句了**：
```cpp
cout << 1 > 0 || 1 / 0;
```
这里的程序会正常运行，因为左面的判断已经决定了表达式的值为 true。所以就不会执行右侧的表达式，也就不会出现`ZeroDivisionError`（除零错误）。

#### 5.3.2 AND运算符 <a id="AND运算符"></a>
AND 与 OR 类似，但是他为**与运算**。即**所有的表达式都为真，这个表达式才为真**。
下面是运行原理：
| | expr1 && expr2 的值 | |
| :--- | :--- | :--- |
| | **expr1 == true** | **expr1 == false** |
| **expr2 == true** | true | false |
| **expr2 == false** | false | false |

对于 && 运算符，有一个要注意的点：如何表示 min <= number <= max，这种连环的表达式。对于这种情况，C++指出必须使用两个大小比较在进行 && 运算。
```cpp
int number = 15;
cout << (number >= 11 && number <= 19);     // 等价于数学上的 11 <= number <= 19
```
如果我们使用了像数学一样表示的方法，我们的结果就是：
```cpp
int number = 100;
cout << (17 <= number <= 35);     // 输出为 1 ，因为这个时候C++编译器会认为表达式为(17 <= 100) <= 35。前一个表达式的值为 1，而 1 <= 35。所以，使出 1（真值）。
```
与 OR 运算符一样，我们的 AND 运算符也有**短路求值**，当我们左面的表达式中有一个式子为假，这个表达式的值就为 0.

#### 5.3.3 NOT运算符 <a id="NOT运算符"></a>
`!`运算符将它后面的表达式的真值取反。也是说，如果`expression`为`true`，则`!expression`是`false`；如果`expression`为`false`，则!expression是true。更准确地说，如果expression为true或非零，则!expression为false。

#### 5.3.4 运算符注意事项 <a id="运算符注意事项"></a>
1. C++中 OR 和 AND 运算符优先级**低于**关系运算符。
    ```cpp
    x > 5 && x < 10;    // 解释为 (x > 5) && (x < 10);
    ```
2. `!`运算符的优先级高于所有的关系运算符和算数运算符，所以当我们要对表达式取反就要用括号。
   ```cpp
   !(x > 5)    // 判断是否 x 大于 5，然后再求反
   !x > 5   // 0 / 1 是否比 5 大
   ```
3. AND 运算符比 OR 运算符优先级更高
   ```cpp
   int year = 1900;
   cout << (year % 4 == 0 && year % 100 != 0 || year % 400 == 0);   // 判断是否为闰年
   ```
   上述程序被解释为`(year % 4 == 0 && year % 100 != 0) || (year % 400 == 0)`。
4. 短路求值：
   C++确保程序从左向右进行计算逻辑表达式，并在知道答案后立刻停止。
5. 其他表示方式
    | 运算符 | 另一种表示方式 |
    | :--- | :--- |
    | && | and |
    | || | or |
    | ! | not |
    这些都是 C++ 的保留字。



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

[^8]: 注意，这里我们说数组也是可以进行相互赋值的，这里的相互赋值就是将两者的内容拷贝复制，不是公用地址。这点与一些简单的数组之间相互赋值不一样。

[^9]: 面向对象编程与传统的过程性编程的区别在于，OOP强调的是在运行阶段（而不是编译阶段）进行决策。运行阶段指的是程序正在运行时，编译阶段指的是编译器将程序组合起来时。运行阶段决策提供了灵活性，可以根据当时的情况进行调整。
使用常规变量时，值是指定的量，而地址为派生量。处理存储数据的新策略刚好相反，将地址视为指定的量，而将值视为派生量。**指针用于存储值的地址**，指针名表示的是地址，`*`运算符称为间接值或解引用运算符，用于指针就可以得到指针所存储的地址的对应量。

[^10]: 在C++中，`int*`是一种复合类型，是指向int的指针。
[^11]: 一定要在对指针应用解除引用运算符（*）之前，将指针初始化为一个确定的、适当的地址。这是关于使用指针的金科玉律。

[^12]: **不要尝试释放已经释放的内存块**，C++标准指出，这样做的结果将是不确定的，这意味着什么情况都可能发生。**另外，不能使用delete来释放声明变量所获得的内存：
    ```cpp
    int *ps = new int;
    delete ps;  // ok
    delete ps;  // not ok
    int jugs = 5;   // ok
    int *pi = &jugs;    // ok
    delete pi;  // not allowed, memory not allocated by new
    ```
[^13]: C++对于`char *`类型做出了**重载**，当我们正常使用cout输出char *类型的指针的时候，我们结果是输出字符串，但是当变成别的类型（像 int * ），这个时候我们得到的是指针的所指向的地址。

[^14]: ###### 栈、堆和内存泄漏：
    由于栈“用完即走”的特性，我们不用担心他的内存不会释放的问题，但是指针和指针指向的堆中的内存就不好办了，当代码块中的指针随着代码块运行结束被释放，其所指向的内存如果没有新的指针指向，就会成为一个无名的且无法被使用的但是还存在的一块内存，这叫**内存泄露**，这是很危险的，会占用大量的内存导致程序崩溃。

[^15]: **变量的生命周期（Lifetime / Scope & Duration）**是指一个变量从“出生”到“死亡”的整个过程。简单来说，就是从**计算机为这个变量分配内存空间**开始，到**这块内存被释放/回收**为止的时间跨度。
   变量生命周期分为三段：**分配，使用，回收**：
    1. **分配**：当你定义变量（如 int a = 10;）时，程序在内存里给它找了个“房间”。
    2. **使用**：在程序运行期间，你可以读写这个房间里的数据。
    3. **回收**：当变量不再需要时，它占用的“房间”会被退租，交给系统重新分配给别人。
    
    下面是各种常见变量的生命周期：
    1. **局部变量（自动变量）**
        * **寿命**：最短。
        * **规律**：通常定义在函数或代码块 `{ }` 内部。
        * **生存期**：进入代码块时出生，**离开代码块时立即死亡**。
        * **比喻**：像“快闪店”，只在特定活动期间存在。

    2. **全局变量 / 静态变量**
       * **寿命**：最长。
       * **规律**：定义在函数外部，或者使用了 `static` 关键字。
       * **生存期**：**从程序启动到程序结束**一直存在。
       * **比喻**：像“百年老店”，只要公司（程序）没倒闭，它就一直在。

    3. **堆变量（动态分配）**
       * **寿命**：由程序员手动控制。
       * **规律**：使用 `new` 或 `malloc` 创建。
       * **生存期**：从你创建它开始，直到你手动删除它（或被垃圾回收机制 GC 处理）。
       * **比喻**：像“长期租房”，租多久取决于你什么时候退房。

[^16]: 在设计循环时，有几条指导原则：
      *   **指定循环终止的条件。**
      *   **在首次测试之前初始化条件。**
      *   **在条件被再次测试之前更新条件。**
      `for` 循环的一个优点是，其结构提供了一个可实现上述 3 条指导原则的地方，因此有助于程序员记住应该这样做。但这些指导原则也适用于 `while` 循环。

[^17]: 类型别名：
    C++为类型建立别名的方式有两种：
    - 一种是使用预处理器：
    `#define BYTE char // preprocessor replaces BYTE with char`
    这样，预处理器将在编译程序时用char替换所有的BYTE，从而使BYTE成为char的别名。
    - 第二种方法是使用C++（和C）的关键字typedef来创建别名。例如，要将byte作为char的别名，可以这样做：
    `typedef char byte // make byte an alias for char`

    下面是通用格式：
    `typedef typeName aliasName;`
    我们也可以声明指针：
    `typedef char* byte_pointer;    // pointer to char type`
    也可以使用`#define`，但是在声明一系列变量的时候这个方法就不再适用:
    ```cpp
    #define FLOAT_POINTER float*
    FLOAT_POINTER pa, pb;
    ```
    由于define是直接将对应部分预处理替换成我们定义的部分所以上面的程序就变成了：
    ```cpp
    float *pa, pb;      // pa a pointer to float, pb just a float
    ```
    但是 typedef 不会发生这种情况，因为他是真的给这个变量气力一个别名。但是，**typedef不会创建新类型**，而只是为已有的类型建立一个新名称。如果将word作为int的别名，则cout将把word类型的值视为int类型。

[^18]: 出口条件循环（Exit-controlled loop）是指先执行循环体，再判断条件的循环结构。也称为后测试循环（Post-test loop）。
    - 特点：循环体**至少会执行一次**，然后才检查循环条件是否成立。
    - 如果条件成立（或不成立，取决于写法），就继续循环；否则退出循环。
    - 因为判断条件放在循环体的出口（最后），所以叫出口条件循环。

[^19]: 读取文件中的信息似乎同cin和键盘输入没什么关系，但其实存在两个相关的地方:
    1. 很多操作系统（包括Unix、Linux和Windows命令提示符模式）都支持重定向，允许用文件替换键盘输入:
    ```
    exe < fileName
    ```
    这样就告诉操作系统，我们的程序拒绝键盘输入，直接从文件中读取文本。其中的`<`是Unix和Windows命令提示符的重定向运算符。
    2. 很多操作系统都允许通过键盘来模拟文件尾条件。在Unix中，可以在行首按下`Ctrl+D`来实现；在Windows命令提示符模式下，可以在任意位置按`Ctrl+Z`和`Enter`。有些C++实现支持类似的行为，即使底层操作系统并不支持。用于PC的Microsoft Visual C++、Borland C++ 5.5和GNU C++ 都能够识别行首的Ctrl + Z，但用户必须随后按下回车键。总之，很多PC编程环境都将Ctrl+Z视为模拟的EOF，但具体细节各不相同。

[^20]: 在C++的iostream库中，`eofbit`和`failbit`是用来描述**流**当前状态的**标志位**。
    1. eofbit(End-Of-File bit)
    - 含义：eof 是 End Of File 的缩写。当程序尝试读取数据，但已经到达了文件的末尾（或者输入流的末尾）时，就会设置这个标志。
    - 触发时间：
        - 读取文件：使用 `<` 重定向读完一个文件的最后一个字符，但是还想继续读下去。
        - 键盘输入：使用`Ctrl + Z`然后回车。
    - 检测方法：使用 cin.eof()，如果是返回了 true，说明已经读完了。
    2. failbit(Failure bit) 
    - 含义：表示一个输入操作没能成功完成，通常是因为**数据格式不匹配**。
    - 触发时间：
        - 类型不匹配：比如 `int num; cin >> num;` 结果在键盘上敲了一个字母 `A`。`cin` 发现没法把 `A` 转换成整数，读取就失败了，此时会设置 `failbit`。
        - 文件打不开：试图读取一个不存在的文件。
        - 到达 EOF 的同时，读取到了文件的末尾，但是还是没有读取到我们想要的数据，这个时候`eofbit`和`failbit`都会被设置。
    - 检测方法：使用 `cin.fail()`

[^21]: 最初，put( )成员只有一个原型—put(char)。可以传递一个int参数给它，该参数将**被强制转换为char**。C++标准还要求只有一个原型。然而，有些C++实现都提供了3个原型：`put(char)`、`put(signed char)`和`put(unsigned char)`。在这些实现中，给`put()`传递一个int参数将导致错误消息，因为转换int的方式不止一种。使用显式强制类型转换的原型（如cin.put(char(ch))）可使用int参数。
