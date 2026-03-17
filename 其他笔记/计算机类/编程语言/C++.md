# <font face="楷体" color="red">C++PrimerPlus笔记</font>

## 目录
### 一、开始学习C++<a id="开始学习C++"> </a>
#### 1.1. 进入C++<a id="进入C++"> </a>
```C++
// myfirst.cpp -- displays a message
#include <iostream>
using namespace std;
int main(){
    cout << "Hello, world." << endl;
    return 0;
}
```
<font face="楷体">
以上是初学者在刚接触C/C++时所能会写的第一个程序(大概率如此)。虽然这很短小，但是却涵盖了C++的一些基本知识。但是，作为初学者，我们一般是照着葫芦画瓢模仿来的这个简短的程序，但是有没有想过，这里面的每句话都是什么意思。现在，我们就要对其进行分块讨论。

这个程序包含了下述元素，我们一句一句向下看：
- 注释：`//`
- 预处理指令：`#include`
- 库：`<iostream>`
- 函数头：`int main()`
- 编译指令：`using namespace`
- 函数体：`{}`
- cin/cout：`cout`
- 结束语句return：`return 0`

我们先看第一个元素`main()`函数：
##### 1.`main()`函数<a id="main()函数"></a>
我们将目光聚集到`main()`函数上，我们对其进行拆分化简，可得下面的结构：
```
int main(){
    statement;  //C++语句
    return 0;
}
```
这几行代码完成了`main()`函数的定义，==取名+描述行为==
该定义有两部分组成：函数名(`int main()`) + 函数体`{}`
函数名对函数与其他程序之间的接口(到函数一章在进行详细阐述)进行阐述。函数体指出函数应该做什么计算机指令。

在C++中，每条完整的指令都被称为**语句**，且所有的语句都以**分号**结束。

最后的那个`return 0`被称为返回语句，他起到结果函数的作用。

###### 语句补充：
语句是要执行的操作，我们在不同的编程语言里有不同的分隔符用于区分语句和语句。在C/C++中，我们使用终止符，也就是所谓的分号，因此，我们万万不可以省略分号。

对于函数`int main()`，我们在这里先仅对`()`进行强调，在C++中，括号为空则表明函数不接受任何参数。与`int main(void)`是等效的(这里看不懂可以等看到函数那节再回来想)。C++与C不同，在C中，我们将空括号表示为对是否接受参数表示沉默。至于那最后的`return 0`语句，在ANSI/ISO C++标准允许我们省略结尾的这个return语句，main()函数默认以`return 0`结尾。
目前为止，我们的程序必须要有一个main函数，因为我们必须要用一个函数来开始我们的程序，在C++程序中，main()函数就担起了这个重担。如果没有main()函数，程序就不能正常运行。（这里指的是一般情况）

##### 2.C++注释<a id="C++注释"></a>
C++的注释是以双斜杠开头的`//`。但是这并不代表仅有这一种注释的方式，C语言的注释方法同样适用。
```cpp
// statement;
/*
    ···
    statements
    ···
*/
```
以上内容中`//`是单行注释，`/**/`是多行注释
###### 提示：注释应用来说明程序，程序越长，注释的含金量就越大。在Microsoft提供的VS和VScode中会内置一种快捷键`Ctrl + /`我们可以进行快速的单行和多行注释

##### 3.预处理指令`#include`和`iostream`文件<a id="预处理和iostream文件"></a>
我们观察到，几乎所有的C++文件的程序前面都会有这种`#include`指令，这种指令叫做预处理指令，我们可以通过这个命令对程序进行所谓的“预处理”——也就是在程序运行前进行的程序操作。至于其后面的`<iostream>`，这是cpp中内置的一个**库**。这里我们先按下不表。
iostream是一个文件，其中的"i"和"o"分别代表了输入和输出。在C++中，我们会用名称空间中的`cin`和`cout`来分别进行输入和输出，但是他们的实现都是在`iostream`文件中的。
像这样的文件，我们称为**包含文件**——他们被包含在其他的文件中，也叫**头文件**——因为他们被包含在文件的起始处。C语言的传统是在头文件的后面使用扩展名"h"，但是在C++中用法发生了改变，对老式的C语言头文件我们保留扩展名。但是C++头文件是没有扩展名的。同时，对于一些C头文件C++进行了再命名。典型的例子就是C语言中的`math.h`变成了C++中的`cmath`。总之，在C++程序中，我们不会在过度的注意头文件的扩展名，但是我们需要使用另一个工具——**名称空间**

##### 4.名称空间：<a id="名称空间"></a>
`using namespace std;`
当我们在使用iostream文件的时候，我们不用iostream.h，而是在后面加上一个名称空间编译指令。
```cpp
#include <iostream>

int main(){
    cout << "Hello, world." << endl;
    return 0;
}
// 未定义标识符 "cout"
// 未定义标识符 "endl"
```
名称空间是用来限定类、函数、变量等C++组件的。由于命名的自由性，我们很容易就会发生命名重复（这个在C++中有其他的含义），就会导致很混乱。因此，我们需要引入名称空间来限定名称的适用范围。要使用名称空间的名称，我们要用`namespace::func_name`的格式来使用，其中`::`被称为**域名解析符**，这个我们在后面再详细的讲解。因此，正常的使用方法应该像下面的做法：
```cpp
#include <iostream>

int main(){
    std::cout << "hello, world." << std::endl;
    return 0;
}
```
但是，我们在使用的时候会觉得总是带着`std::`很麻烦，这个时候我们就可以用`using namespace + 名称空间`来进行转换。这种using编译指令使得std名称空间中的所有名称都可以使用。
###### <font color="red">注意：这个偷懒的做法，在大型项目中不建议使用 </font>

##### 5.cout输出<a id="cout输出"></a>
`std::cout << "Hello, world." << std::endl;"`
这是一个典型的输出语句。由`<<`引起来的是我们要打印的信息。双引号`""`括起来的叫**字符串**，它是由若干个字符组成的。目前，我们仅需要知道如何使用它即可。
###### C++中的流：
这个我们应该在后面的内容进行详细的介绍。我们此简单提一嘴：C++的输入和输出都可以看做是“流”，即程序流进或流出的一系列字符。输出的`<<`是将数据流给cout对象，输入的`>>`是将数据流给cin对象。

##### 6.控制符`endl`<a id="控制符endl"></a>
`endl`是C++中的一个特殊符号，他表示：**重启一行**。在输出流中插入endl会导致屏幕光标移到下一行的开头，像这样的对cout有特殊含义的特殊符号叫做**控制符**。它也在iostream中定义，位于名称空间std中。
##### 7.换行符`\n`<a id="换行符"></a>
C++还延续了C语言的换行符`\n`，其被视为是一种字符，在显示字符串的时候，我们可以用`\n`来代替`endl`从而减少工作量。
注意到，换行符的前面有一个`\`符号，这叫转义符，在很多编程语言中都存在，而`\n`是一种转义序列。这将在后面进行详细说明。
###### endl和\n的差别
这两个在显示字符串的时候都起到了换行的作用。但是，endl还有另一重作用：**刷新缓冲区**（这个将在缓冲区部分进行详细介绍）。但是`\n`只能做到换行。

##### 8.源代码的格式化：<a id="源代码的格式化"></a>
**源代码**指的是程序员用编程语言写出来的，可以被人们阅读的文字。
在C++中，我们使用分号来分割句子，因此，在C++中回车的作用和空格或制表符的作用是相同的。在C++中，通常可以防止回车的地方也能使用空格。
在C/C++中我们不能将空格、回车和制表符（这三个被称为**空白符**）放在元素（比如名称）的中间。回车还不能放在字符串的中间。
下面是一些例子：
```cpp
int ma in() #ERROR -- sapce in name
re
turn 0; #ERROR -- carriage return in word
cout << "Hello,
world"; #ERROR -- carriage return in string
```
<font color="green">在C++11中，新增的raw字符串是可以包含回车的，这个我们也在后面讨论</font>

1. 源代码的标记和空白
一该行代码中不可分割的元素叫做**标记(token)**，通常我们用空白符进行分割。`' '`和`'\t'`和`\n`统称为**空白**。元素间的空白数量我们不用管。
```cpp
return0; #INVALID 
return 0; #VALID
return (0) #VALID
return       (0) #VALID
int main() #VALID
int main () #VALID
```
这里注意一下：`return 0`和`return (0)`是一个东西，核心框架都是`return + expression`而`(0)`也是一个表达式，只不过是加了一个括号，并不改变值。
2. 源代码风格：
<font color="red">一个好的代码风格真的很重要！！！</font>
下面我们介绍一下多数人的优秀风格：
- 每条语句占一行
- 每个函数都有一个开始花括号和一个结束花括号
- 函数中的语句都相对于就花括号进行缩进。
- 与函数名称相关的圆括号周围是没有空白的。

#### 1.2 C++语句<a id="C++语句"></a>
C++程序是一组函数，每个函数又是一组语句。
```cpp
#include <iostream>
using namespace std;
int main(){
    int carrots;    // declare an integer varialbe
    carrots = 5;

    cout << "I have ";
    cout << carrots;
    cout << " carrots.";
    cout << endl;
    carrots = carrots - 1;
    couut << "Crunch, crunch. Now I have " << carrots << " carrots" << endl;
    return 0; 
}
```
##### 1.声明语句和变量：<a id="声明语句和变量"></a>
在C/C++中我们需要明确信息的存储位置和所需的内存空间，这个时候就需要我们的**声明语句**。声明语句的结构：`Typename + Variable-name`。这里的`typename`是类型名，可以是任何一种类型，`variable-name`是变量名/函数名，总之是一种标签。
###### 声明与定义：
程序中的声明语句叫做定义声明语句，简称**定义**。定义意味着他将命令编译器为变量分配内存空间。但是声明还有不分配空间的情况，称为**引用声明**。
声明和定义的关系：定义 = 声明 + 分配内存
|关键词/特征|声明|定义|
|:--:|:--:|:--:|
|extern(无初始化)|[x]|[ ]|
|extern(有初始化)|[ ]|[x]|
|`static`全局变量|[ ]|[x]|
|`struct`标签|[x]|[x]|
|函数原型|[x]|[ ]|
|函数体|[ ]|[x]|
|`typrdef`|[x]|[ ](类型别名)|
|`&`引用|[x]|[ ]|
（这些暂时看不懂没关系，后面也会介绍）
##### 2.赋值语句<a id="赋值语句"></a>
符号`=`叫做赋值运算符，与数学中的`=`不同，他表示**将值绑定到该标签**。我们允许连续赋值，比如下面的例子
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
##### 3.cout补充<a id="cour补充"></a>
如果我们运行上面的例子会发现：`carrots`输出时编程了数字（类型发生了转换）。这就是C++中`cout`的之处。cout可以识别变量，然后将其改成其所绑定的值。最后转化成字符串。cout的智能之处我们会进行进一步说明。
上面的程序还有一个特点：`cout`之间被分割成很多份。但是输出的结果是一样的，是连起来的。这都要归功于我们的`<<`运算符。`cout`是将我们的字符串插入到输出流中，`<<`会将输出的字符串插入到缓冲区中。等到缓冲区刷新才会进行统一的输出。而且下一次输出的光标会跟在上一次输出的后面。
##### 4.使用cin<a id="使用cin"></a>
上面的程序是一种过程式编程（即编写完成后没有改变的过程，只能按部就班的进行输出）。这未免有点太枯燥了，我们想进行参与一下，让他更接近我们日常使用的程序，让用户输入就是一个很好的例子。这时我们就可以使用我们的`cin`来进行输入。
```cpp
#include <iostream>
using namespace std;
int main(){
    int carrots = 0; #initialization
    cin >> carrots;
    cout << "Now, we have " << carrots << "carrots.";
    return 0;
}
```
在这个程序中，我们使用了`cin`来控制我们的输入。`cin`与`cout`一样，都是在`iostream`文件中定义的。`cin`是输入流的一个对象。在输入的时候，其使用`>>`将字符串插入到输入流中。其也是一个智能对象。

#### 1.3 函数<a id="函数"></a>
函数对于C++程序的模块的构建至关重要。我们在这里进行简单的介绍。
##### 1.使用有返回值的函数<a id="使用有返回值的函数"></a>
有返回值的函数将会生成一个值，这个值可以赋给变量火灾气态的表达式中使用。
```cpp
#include <iostream>
#include <camth>
using namespace std;
int main(){
    double x = 0;
    x = sqrt(6.25);
    cout << x;
    return 0;
}
```
上面是一个使用函数返回值的简单程序，`sqrt`函数是C++内置库`cmath`中的一个函数，他的作用就是求一个数的平方根。`double`和`int`一样，是C++的内置类型。表示双精度数。圆括号中的值是发送个函数的信息，这被称为传递给函数。而经过函数的运算后返回的值就叫做返回值。
简而言之：参数是发送个函数的信息，返回值是从寒湿重送回去的值。

C++对函数的使用有着严格的要求。编译器必须知道函数的参数类型和返回值类型。C++体重他们的方式使用**函数原型语句**。

`sqrt`函数原型是像这样的`double sqrt(double)`。函数名之前的`double`是用来指定返回值类型的。`()`内的`double`是用来指定参数类型的。而`sqrt`则是函数标签（也就是他的名字）。

不要混淆函数原型和函数i当以。原型知识描述了函数的接口（使用方式）。但送一则包含了函数的代码。与其他变量一样，我们在首次使用函数之前要提供其原型（用来声明）。

##### 2.其他的一些函数原型<a id="其他的一些函数原型"></a>
函数并不拘泥于一种形式。下面是函数原型的一些例子：
1. 有返回值有参数的函数：
典型例子：
`double sqrt(double)`
2. 有返回值没有参数的函数：
在C++库中有一个与cstdlib相关的库，其中有一个rand()函数，该函数不接受任何参数，可以返回一个随机整数。
其原型为：`int rand(void)`
`void`是一个关键字，他表示函数不接受任何参数，如果省略void，`()`内为空，C++会将其解释为一个不接受任何参数的*隐式声明*（即不显式指出，但是能达到同样的效果。
e.g.: `myGress = rand()`
在C++中函数调用中必须包含括号，即使他没有参数。
###### 一点小补充：
在上面的语句中，`()`被称为**函数调用运算符**，他表示调用函数名所对应的函数。但是我们在函数声明的时候的`()`就是表示参数列表，是为了告诉编译器参数的类型。
3. 无返回值有参数的函数：
函数也可以没有返回值，有返回值的我们叫做**函数**(function)，无返回值的函数我们可以称之为**过程**(procedure)或者**子程序**(subroutine)。C++和C一样将这两种变体都叫做函数。无返回值的函数如下面的例子：
```cpp
void bucks(double number){
    cout << number;
}
```
在这里的`void`就表示这个函数没有返回值，**这里的`void`是不允许省略的！！！**
4. 无返回值无参数的函数；
e.g.:
```cpp
void greeting(){
    cout << "Hello, world."
}
```

##### 3.自定义函数<a id="自定义函数"></a>
C++提供了140多种预定义的函数，但是这并不能满足用户的所有需求。C++允许用户自定义函数，只要符合函数的基本格式并在使用之前提供其原型（函数声明）即可。
1. 函数格式
```
type function_name(argumentlist){
    ···
    statements
    ···
}
```
argumentlist: 参数列表，包含所有的参数（类型）
###### 和C一样，C++是不允许将函数定义嵌套到另一个函数定义中的，每一个函数都是独立的，所有函数的创建都是平等的。
2. 函数头
所谓的函数头就是`type function_name(argumentlist)`部分。他告诉计算机这个函数的返回值、参数列表、以及函数名。
###### 特殊说明：main()函数
###### `int main`,开头的`main`表明函数返回一个整数值，空括号表明`()`中没有参数，对于有返回值的函数，应使用关键字`return`来提供返回值，并结束函数。这就是为什么要在main()结尾使用`return 0;`语句的原因。但是还有一个吧问题，main函数的返回值给了谁？答案是可以将计算机操作系统看成是调用程序。
###### 很多操作系统都可以使用程序的返回值。例如，UNIX外壳脚本和Windows命令行批处理文件都被设计成运行程序，并测试它们的返回值（通常叫做退出值）。通常的约定是，退出值为0则意味着程序运行成功，为非零则意味着存在问题。因此，如果C++程序无法打开文件，可以将它设计为返回一个非零值。然后，便可以设计一个外壳脚本或批处理文件来运行该程序，如果该程序发出指示失败的消息，则采取其他措施
3. 函数体
函数头后面的花括号`{}`及其里面的程序都是函数体。他们主要包含了函数的实现等详细信息。
4. `return`语句
对于所有有返回值的函数，都至少要有一个`return`语句来返回函数的返回值。但这并不代表着cvoid函数不能有`return`语句。
严格来说，`return`语句其实是函数结束的表示，编译器一旦看到了`return`语句，就会立即停止调用，结束这部分程序，有返回值的函数就会返回返回值。无返回值的函数也可以直接使用`return;`来停止函数调用。有返回值的函数的return语句的类型必须与函数原型中的类型相同，否则就会报错
##### 4.关键字<a id="关键字"></a>
关键字是计算机语言中的词汇。本章使用了4个C++关键字：int、void、return和double。由于这些关键字都是C++专用的，因此不能用作他用。也就是说，不能将return用作变量名，也不能把double用作函数名。不过可以把它们用作名称的一部分，如painter（其中包含int）或return_aces。
另外，main不是关键字，由于它不是语言的组成部分。然而，它是一个必不可少的函数的名称。可以把main用作变量名（在一些很神秘的以致于无法在这里介绍的情况中，将main用作变量名会引发错误，由于它在任何情况下都是容易混淆的，因此最好不要这样做）。同样，其他函数名和对象名也都不能是关键字。然而，在程序中将同一个名称（比如cout）用作对象名和变量名会把编译器搞糊涂。也就是说，在不使用cout对象进行输出的函数中，可以将cout用作变量名，但不能在同一个函数中同时将cout用作对象名和变量名。
##### 5.总结<a id="总结"></a>
C++程序由一个或多个被称为函数的模块组成。程序从main()函数（全部小写）开始执行，因此该函数必不可少。函数由函数头和函数体组成。函数头指出函数的返回值（如果有的话）的类型和函数期望通过参数传递给它的信息的类型。函数体由一系列位于花括号中的C++语句组成。
有多种类型的C++语句，包括下述6种。
- 声明语句：定义函数中使用的变量的名称和类型
- 赋值语句：使用赋值运算符（=）给变量赋值。
- 消息语句：将消息发送给对象，激发某种行动。
- 函数调用：执行函数。被调用的函数执行完毕后，程序返回到函数调用语句后面的语句
- 函数原型：声明函数的返回类型、函数接受的参数数量和类型。
- 返回语句：将一个值从被调用的函数那里返回到调用函数中。
C++提供了两个用于处理输入和输出的预定义对象（`cin`和`cout`）这两个是在`iostream`文件中定义的。为`cout`定义的流插入运算符（`<<`）使得将数据插入到输出流成为可能；为`cin`定义的流提取运算符（`>>`）能够从输入流中抽取信息。`cin`和`cout`都是智能对象，能够根据程序上下文自动将信息从一种形式转换为另一种形式。

### 二、处理数据<a id="处理数据"></a>
#### 2.1 简单变量<a id="简单变量"></a>
为了把信息寻出在计算机中，程序必须记录3个基本属性：
- 信息将存储在哪里
- 要存储什么值
- 存储何种类型的信息
```cpp
int braincount;
braincount = 5
```
这些语句告诉程序，它正在存储整数，并使用名称braincount来表示该整数的值（这里为5）。
#### 2.1.1.变量名<a id="变量名"></a>
C++命名要遵寻一下规则:
- 在名称中只能使用字母字符、数字和下划线(_)
- 名称的第一个字符不能是数字
- 区分大小写
- 不能将C++关键字用作名称
- 以两个下划线或下划线和大写字母打头的名称被保留给实现（编译器及其使用的资源）使用。以一个下划线开头的名称被保留给实现，用作全局标识符。
- C++对名称的长度是没有限制的，名称中的所有字符都有意义。但有些平台有长度限制。

这里我们对倒数第二点进行详细的说明：
C/C++规定：任何以`__`和`_大写字母`开头的名字，保留给**实现**使用。普通程序员不要定义这种名字，避免与编译器或者标准库冲突。
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
#### 2.1.2整型<a id="整型"></a>
整型就是我们在数学上所说的整数（没有小数部分）有些语言只提供一种整型（一种类型满足所有要求！），而C++则提供好几种，这样便能够根据程序的具体要求选择最合适的整型。
不同C++整型使用不同的内存量来存储整数。使用的内存量越大，可以表示的整数值范围也越大。另外，有的类型（符号类型）可表示正值和负值，而有的类型（无符号类型）不能表示负值。术语宽度
（width）用于描述存储整数时使用的内存量。使用的内存越多，则越宽。
C++中的整型(按照宽度递增排列)分别是`char, short, int, long, long long(C++11新增)`，这些都有器有符号版本(signed)和无符号版本(unsigned)。其中的`char`最常用来表示字符。
#### 2.1.3.整型short、int、long和long long<a id="整型short、int、long和long long"></a>
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

要知道系统中整数的最大长度，可以在程序中使用C++工具来检查类型的长度。首先，sizeof运算符返回类型或变量的长度，单位为字节。其次，头文件climits（在老式实现中为limits.h）中包含了关于整型限制的信息。具体地说，它定义了表示各种限制的符号名称。例如，INT_MAX为int的最大取值，CHAR_BIT为字节的位数。

下面的程序给出了他们的使用情况：
```cpp
// limits.cpp -- some integer limits
#include <iostream>
#include <climits>    // use limits.h for older systems
int main()
{
    using namespace std;
    int n_int = INT_MAX;    // initialize n_int to max int value
    short n_short = SHRT_MAX;  // symbols defined in climits file
    long n_long = LONG_MAX;
    long long n_llong = LLONG_MAX;

    // sizeof operator yields size of type or of variable
    cout << "int is " << sizeof (int) << " bytes." << endl;
    cout << "short is " << sizeof n_short << " bytes." << endl;
    cout << "long is " << sizeof n_long << " bytes." << endl;
    cout << "long long is " << sizeof n_llong << " bytes." << endl;
    cout << endl;

    cout << "Maximum values:" << endl;
    cout << "int: " << n_int << endl;
    cout << "short: " << n_short << endl;

    cout << "long: " << n_long << endl;
    cout << "long long: " << n_llong << endl;

    cout << "Minimum int value = " << INT_MIN << endl;
    cout << "Bits per byte = " << CHAR_BIT << endl;
    return 0;
}
```
下面是其输出
```
int is 4 bytes.
short is 2 bytes.
long is 4 bytes.
long long is 8 bytes.

Maximum values:
int: 2147483647
short: 32767
long: 2147483647
long long: 9223372036854775807

Minimum int value = -2147483648
Bits per byte = 8
```
下面我们详细讲解一下这里面的语句:
##### 2.1.3.1运算符sizeof和头文件limits.h(或者是climits)<a id="运算符sizeof和头文件climits"></a>
`sizeof`是一个运算符，运算符是内置的语言元素，对一个或多个数据进行运算，并生成一个值。例如，加号运算符+将两个值相加。`sizeof`可以计算类型的所占内存（单位：byte）。对于单纯的类型名，我们要将他放在`()`中，但是如果是变量名，我们可以省略掉`()`
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
###### 写到这里，其实已经接触一段时间C++的我们可能在脑海里有一个问题，为什么char不是用来表示字符吗，他才能表示-128~127，这也能表示数吗，大家不都是用int吗。这个感觉是对的，但是并不是说char没有用武之地。大家使用int最多有一下几个原因：
1. **CPU 的“原生”处理能力 (最核心原因)**
现代 CPU（尤其是 32 位和 64 位架构）在处理`int`类型时是最快的。
- 字对齐（Word Alignment）： CPU 读取内存通常是一次读取 32 位或 64 位。如果你定义一个 int，它正好适配 CPU 的总线宽度。
- 大多数 CPU 的算术指令（加减乘除）是直接针对“寄存器大小”优化的。即便你定义一个 char（8 位）变量进行加法运算，CPU 往往也会先把它提升为 int，做完计算后再截断存回内存。这就使得我们用char反而还增加了运算的任务。
2. **数值溢出的风险**
char 的表示范围非常小（通常是 -128 到 127）。在实际编程中，稍微大一点的计数器或者循环变量，甚至是一次简单的加法，都很容易造成溢出（Overflow）。
3. **代码的可读性和可维护性**
当你在代码中看到 int 时，潜意识里会认为这是一个“正常的数字”。如果你看到 char，大脑会下意识地去想：“这里是不是要处理字符？是不是要处理 ASCII 码？还是这里有极端的内存限制？”使用 int 可以减少沟通成本，避免误解。
真正使用char的情景一般是很极端的：
- **海量数据存储 (内存优化)**
- **通信协议 / 文件格式：**
- **嵌入式开发：**
##### 2.1.3.2 初始化(initialization)<a id="初始化"></a>
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
###### 这里的wren的初始化方式当我们在后面介绍类的时候你就会觉得非常熟悉，这里先按下不表。
如果我们知道变量的初始值应该是什么，对他进行初始化是一个非常好的变成习惯，可以避免忘记给变量赋值的情况发生。
##### 2.1.3.3 C++11的初始化方式<a id="C++11的初始化方式"></a>
除了上面的两个初始化方式，C++还有一种，这种方式用于数组和结构，但在C++98中我们还可以用于单值变量：
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
这样的初始化有一个好处：**更好的防范类型转换错误。**这里我们先按下不表，在这单元的结尾我们统一讨论。

###### 为何需要更多的初始化方法？有充分的理由吗？原因是让新手更容易学习C++，这可能有些奇怪。以前，C++使用不同的方式来初始化不同的类型：初始化类变量的方式不同于初始化常规结构的方式，而初始化常规结构的方式又不同于初始化简单变量的方式；通过使用C++新增的大括号初始化器，初始化常规变量的方式与初始化类变量的方式更像。C++11使得可将大括号初始化器用于任何类型（可以使用等号，也可以不使用），这是一种通用的初始化语法。以后，教材可能介绍使用大括号进行初始化的方式，并出于向后兼容的考虑，顺便提及其他初始化方式。

#### 2.1.4 无符号类型
上面介绍的四种类型都一种不能纯存负值的变体，它的优点是表示的范围变大了2倍（因为没有符号位了）要创建他们，只需要用关键字unsigned
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
#### 2.1.5 选择整型类型
C++提供了大量的整型，通常我们是使用`int`（使用原因可以参照上方2.1.3.1中的最后一部分）。对于其他的C++整型也有一些使用的原因。
##### long和long long的使用
如果知道变量可能表示的整数值大于16位整数的最大可能值，则使
用long。即使系统上int为32位，也应这样做。这样，将程序移植到16位
系统时，就不会突然无法正常工作（这里是因为在Windows系统中int为32位和long相同，但是Linux系统中int为16位。）如果要存储的值超过20亿，可使用long long。
##### short的使用
short比int小我们可以用short来节省内存（这是一个非常好的编程习惯！）通常，仅当有大型整型数组时，才有必要使用short来节省内存。

#### 2.1.6 整型字面值
整型字面值（常量）是显式地书写的常量，如212或1776。与C相同，C++与C相
同，C++能够以三种不同的计数方式来书写整数：基数为10、基数为8（老式UNIX版本）和基数为16（硬件黑客的最爱）。就是我们常说的10进制，8进制，16进制。下面的表格给出了表示方法。

|进制(base)|10|8|16|
|:--:|:--:|:--:|:--:|
|格式|正常表示|第一位用0第二位为1到7|前两位为0x或0X|
|例子| 114514 | 042(等同于34) | 0xF(等同于15)、0XA5(等同于165) |
下面是例子
```cpp
#include <iostream>
int main()
{
    using namespace std;
    int chest = 42;     // decimal integer literal
    int waist = 0x42;   // hexadecimal integer literal
    int inseam = 042;   // octal integer literal

    cout << "Monsieur cuts a striking figure!\n";
    cout << "chest = " << chest << " (42 in decimal)\n";
    cout << "waist = " << waist << " (0x42 in hex)\n";
    cout << "inseam = " << inseam << " (042 in octal)\n";
    return 0;
}
```
输出结果如下：
```
Monsieur cuts a striking figure!
chest = 42 (42 in decimal)
waist = 66 (0x42 in hex)
inseam = 34 (042 in octal)
```
在默认情况下，cout使用十进制格式显示整数（方便我们的日常使用）。虽然我们眼中这些进制有所区别，但是对于计算机来说他们都一样，因为他们都要被以二进制存储。
##### 扩展内容：
在头文件iostream文件中也提供了我们的表示其他进制的方法(dec: 10, hex: 16, oct: 8)。下面是示例演示：
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
结果：
```
Monsieur cuts a striking figure!
chest = 42 (decimal for 42)
waist = 2a (hexadecimal for 42)
inseam = 52 (octal for 42)
```
对于dec，hex和oct，他们不会在终端中输出，仅会改变cout的输出形式，这种改变也是一直有效的，直到程序结束，也就是说我们在使用进制转换的时候别忘了在转回我们常用的10进制。
#### 2.1.7 C++确定常量数字的类型
我们使用声明将整型变量的类型告诉了C++编译器，但是对于像114514这样的常量，除非有理由存储为其他类型（如使用了特殊的后缀来表示特定的类型，或者值太大，不能存储为int），否则C++将整型常量存储为int类型。
先来看看后缀。后缀是放在数字常量后面的字母，用于表示类型。
|后缀| u / U |l / L | ul(顺序任意，大小写任意) | ll / LL | ull等（同ul要求）|
|:--:|:--:|:--:|:--:|:--:|:--:|
|含义|unsinged int|long|unsigned long|long long|unsigned long long|
再来看看长度。在C++中，对十进制整数采用的规则，与十六进制和八进制稍微有些不同。对于不带后缀的十进制整数，将使用下面几种类型中能够存储该数的最小类型来表示：int、long或long long。对于不带后缀的十六进制或八进制整数，将使用下面几种类型中能够存储该数的最小类型来表示：int、unsigned int/long、unsigned long、long long或unsigned long long。这是因为十六进制常用来表示内存地址，而内存地址是没有符号的，因此，usigned int比long更适合用来表示16位的地址。
#### 2.1.8 char类型
char很特殊，大小仅占有 1 byte，他的大小范围是-128到127。虽然char经常用来处理字符，但是我们也可以用它来表示比short更小的整型。对于字符表示，我们有不同的表示方法。目前我们常接触到的ASCII是美国制定定的标准。但是这只是一个方言，并不能很好的实现国际化（例如IBM大型机使用的EBCDIC编码也是一种表示方法）。为了让全世界的文字都能在电脑里显示，人们发明了 Unicode（万国码）。它给世界上的每一个字符都分配了一个唯一的数字编号。但是Unicode需要表示的字符太多，传统的 char 类型（通常只占 1 个字节，即 8 位）装不下这么大的数字。所以，C++ 引入了 `wchar_t`（宽字符类型），它的内存空间更大（通常占 2 字节或更多），专门用来存储这些“国际化”的大数字。
在程序中，我们使用char来表示字符，但是在内存中，我们是以数字的形式进行保存的，比如‘M’，我们查看内存可知是77，但是在输出和输入的时候我们看到或使用的却是M，这是因为智能的cin和cout。输入时，cin将键盘输入的M转换为77；输出时，cout将值77转换为所显示的字符M。在程序中，我们书写字符是应使用单引号`''`而不是双引号`""`（这一点我们在后面有详细说明，双引号代表的是**字符串**，两者有很大区别）。
由于char是一个整数，我们也可以对其进行整数操作。
```cpp
char ch = 'A'
cout << ch + 1;   //输出66
```
这里的输出设计了**自动类型转换**，在C++中，当char类型数据参与算术运算的时候，编译器会自动将char转换成int型。此时的cout检测到后面的表达式是`int`型，所以输出的是66。
#### 2.1.8.1 char字面值
在C++中，书写字符常量的方式有多种。对于常规字符（如字母、标点符号和数字），最简单的方法是将字符用单引号括起。这种表示法代表的是字符的数值编码。这种表示法更加清晰，且不需要知道编码方式，在不同的编译器中都能用。
但是总有一些例外，比如我们的`""`，他们不能简单的用这种表示，因为C++赋予了他们新的含义——用来表示字符串，如果强制用的话会使编译器报错。因此，C++使用了一种特殊的表示方法——**转义序列**。转义序列在很多编程语言中都有涉及，我们在使用的时候，需要在要转义的字符前加上`\`来表修饰。

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

虽然上方的表格给出了很多的需要转义的字符，但是到了现在的C++（C++11及以后）。我们需要转义的目前就剩5个：
- `\n`(换行符)：用于结束当前行
- `\\`(反斜杠)：表示文字路径和处理特殊字符是不可或缺
- `\'`(单引号)：在字符常量中必须使用
- `\"`(双引号)：在字符串中包含双引号时必须使用
- `\t`(水平制表符)：在控制台对齐输出时偶尔会用到。
剩下的进本已经放弃使用/极少使用。这里建议实在是要用或者是看到了一些很老的C++文件，我们直接问问AI吧。

**注意，应该像处理常规字符（如Q）那样处理转义序列（如\n）。也就是说，将它们作为字符常量时，应用单引号括起；将它们放在字符串中时，不要使用单引号。**
换行符可以替代endl，但是还是要记得endl有`\n`所不具备的功能，清空缓冲区。显示数字时，使用endl比输入“\n”或‘\n’更容易些，但显示字符串时，在字符串末尾添加一个换行符所需的输入量要少些。
对于一些我们常用的、但是不能用简单字符表示的，我们可以用八进制或十六进制的方式表示。例如Ctr+Z的ASCII码为26，对应的八进制编码为032，十六进制编码为0x1a。可以用下面的转义序列来表示该字符：\032或\x1a。
- 八进制格式：`\ooo`，即`\`+八进制数字。
- 十六进制格式：`\x`+十六进制数字，需要注意，我们的十六进制表示要尽可能多的读取后面的十六进制字符。
相较于八进制，我们还是推荐十六进制，因为十六进制不易出错。
对于以上转义，我们在使用的时候为了避免歧义，在必要位置要进行分割，否则会出现一些奇怪的结果。
#### 2.1.8.2 通用字符名
C++标准允许实现提供扩展源字符集（在键盘上的符号集）和扩展执行字符集（在程序执行过程中能过显示到显示器上的字符）。C++有一种表示这种特殊字符的机制，它独立于任何特定的键盘，使用的是通用字符名（universal character name）。这个通用字符名的本意就是让部分地方语言也能够进行计算机编码（比如简体中文/日文/法文等等）。通用字符名可以用`\u`和`\U`打头。后面跟的分别是八进制和十六进制。
###### Unicode和ISO 10646
Unicode提供了一种表示各种字符集的解决方案—为大量字符和符号提供标准数值编码，并根据类型将它们分组。例如，ASCII码为Unicode的子集，因此在这两种系统中，美国的拉丁字符（如A和Z）的表示相同。然而，Unicode还包含其他拉丁字符，如欧洲语言使用的拉丁字符、来自其他语言（如希腊语、西里尔语、希伯来语、切罗基语、阿拉伯语、泰语和孟加拉语）中的字符以及象形文字（如中国和日本的文字）。到目前为止，Unicode可以表示109000多种符号和90多个手写符号（script），它还在不断发展中。
Unicode给每个字符指定一个编号—码点。Unicode码点通常类似于下面这样：U-222B。其中U表示这是一个Unicode字符，而222B是该字符（积分正弦符号）的十六进制编号。
国际标准化组织（ISO）建立了一个工作组，专门开发ISO 10646—这也是一个对多种语言文本进行编码的标准。ISO 10646小组和Unicode小组从1991年开始合作，以确保他们的标准同步。

#### 2.1.8.3 signed char 和 unsigned char
与short、int、还有long long不同，char在默认情况下是否有符号由C++实现决定，也就是说，是我们根据用途来进行决定。如果在某个方面必须要使用，可以显式的进行设置。
```cpp
char fodo;                  // may be signed, may be unsigned
unsigned char bar;          // definitely unsigned
signed char snark;          // definitely signed
```
如果将char用作数值类型，则unsigned char和signed char之间的差异将非常重要。unsigned char类型的表示范围通常为0～255，而signed char的表示范围为−128到127。

#### 2.1.8.4 wchar_t
对于一些我们无法用8位一字节的方式表达的文字系统，C++有两种处理方式：一种是编译厂商将char定义为16或着更长的字节，另一种就是我们来实现一个能同时支持一个小型基本字符集和一个较大的扩展字符集。wchar_t（宽字符类型）可以表示扩展字符集。
wchar_t并不是一个固定大小的类型，在不同的厂商所定义的wchar_t有所不同。Windows中将这个定义为了2字节，相当于是`unsigned short`，但是在Linux和Mac系统中就被定义为了4字节。相当于是`int`。
**建议**：现代 C++ 已经引入了更明确的类型（如 char16_t 和 char32_t），它们在所有平台上大小都是固定的。如果你在做跨平台开发，建议优先使用 char16_t 或 char32_t，而不是 wchar_t。

传统的cin/cout是将输入和输出看成char流，无法处理wchar_t，但是在iostream中的最新版本提供了wcin和wcout来处理wchat_t流。对于字符常量和字符串，我们还可以通过加上前缀`L`来指示
```cpp
wchar_t bob = L'P';        // a wide-character constant
wcout << L"tall" << endl;  // outputting a wide-character string
```
这个对于开发者来讲一般是不用涉及的，但是我们要有了解，尤其是在进行国际编程或使用Unicode或ISO 10646时。
#### 2.1.8.5 C++11新增的类型：char16_t和char32_t
C++11新增了类型char16_t和char32_t，其中前者是无符号的，长16位，而后者也是无符号的，但长32位。C++11使用前缀`u`表示char16_t字符常量和字符串常量，并使用前缀`U`表示char32_t常量，前缀u和U分别指出字符字面值的类型为char16_t和char32_t。
**建议**：对于这两个类型，我们由3个实践来借鉴：
1. 存储和网络传输：一律用 char 存 UTF-8。它是目前互联网的事实标准，最省空间，兼容性最好。
2. Windows 界面开发： 必须习惯使用 wchar_t 或 char16_t，否则你的程序无法处理中文路径和复杂的用户名。
3. 算法处理： 如果需要频繁地按字符拆分、计数（比如写一个文本编辑器），转换成 char16_t 或 char32_t 处理会让你少掉很多头发。
如果你只写简单的控制台练习题，char 确实够了；但如果你要写一个正经的、给别人用的软件，Unicode 是你无法绕过的关卡，而 char16_t 就是你手中的武器之一。
#### 2.1.9 bool类型
在以前，C++和C一样是没有bool值的。C++将**非零值**解释为`true`，将**0**解释为`false`。现在我们可以用预定义的字面值`true`和`false`来表示真假了。
字面值`true`和`false`都可以提升转换成int型，true->1，false->0。另外任何数字或者指针值都可以进行隐式转换为bool值，任何非零值都被转换为true，而零被转换为false，任何非NULL或nullptr的都会转换成true，NULL和nulllptr会被转换成0。

#### 2.2 const限定符
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

#### 2.3 浮点数
他是 C++ 的第二组基本类型，可以表示小数部分的数字，对于它在计算机中的存储原理，可以学习一下**数字电子技术(Digigtal Fundamental by Thomas L. Floyd)**[^2]，
##### 2.3.1 书写浮点数
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
##### 2.3.2 浮点类型
C++ 中有三种浮点类型：float、double 和 long double。他们和整形一样是按照表示的最小范围来分类的。有效位（significant figure）是数字中有意义的位。有效位数不依赖于小数点的位置。
C++ 对于上方三种的要求是：float 至少 32 位，double 至少 48 位，且要大于 float，long double 是要大于等于 double。通常情况下，float 为 32 位，double 为 64 位，long double为 80、96或128 位。我们可以从头文件 cfloat 中找到系统的限制。
对于小数点精度的控制我们在 ostream 文件中有专门的方法，setf()方法，下方给出了使用的案例，其中的 ios_base::fixed 和 ios_base::floatfield 是用来固定小数点位的常量（由iostream文件提供）。
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
通常cout会删除结尾的零。但是调用cout.setf( )将覆盖这种行为，至少在新的实现（就是在该语句之后的实现）中是这样的。由于 float 的精度问题，我们的第二个输出出现了数值上的小问题，这也体现了计算机对于 float 精度的控制。
对于精度的控制，我们在后面有更详尽的介绍。
######
##### 2.3.3 浮点常量
对于浮点常量，在默认情况下我们是储存为 double 类型。如果希望改变类型，float 对应在数字后面加上 f 或 F 后缀。对于 long double 类型，可以使用 l 或 L 后缀（这里建议使用 L 后缀）。
```cpp
1.234f                  // a float constant
2.45E20F                // a float constant
2.345324E28             // a double constant
2.2L                    // a long double constant
```
##### 2.3.4 浮点数的优缺点
与整数相比，浮点数有两点好处，一是可以表述整数范围内的值，而是、由于有缩放因子，他们表示的范围要大得多。但是并不是没有缺点，浮点运算的速度是要比整数要慢的。
###### 以上介绍的整型和浮点型统称为算数类型。
### 2.4 C++ 算数运算符
C++ 由 5 中基本的运算符：
- `+`：对操作数进行加法运算。
- `-`：等同于数学上的减法
- `*`：等同于数学上的乘法
- `/`：等同于数学上的除法（但是要注意类型）
- `%`：等同于数学上的取余（获得余数）

变量和常量都可以用作操作数。**注意`%`的操作数只能是整数**。
下面给出一个例子：
```cpp
// arith.cpp -- some C++ arithmetic
#include <iostream>
int main()
{
    using namespace std;
    float hats, heads;

    cout.setf(ios_base::fixed, ios_base::floatfield); // fixed-point
    cout << "Enter a number: ";
    cin >> hats;
    cout << "Enter another number: ";
    cin >> heads;

    cout << "hats = " << hats << "; heads = " << heads << endl;
    cout << "hats + heads = " << hats + heads << endl;
    cout << "hats - heads = " << hats - heads << endl;
    cout << "hats * heads = " << hats * heads << endl;
    cout << "hats / heads = " << hats / heads << endl;
    return 0;
}
```
下面是输出：
```
Enter a number: 50.25  
Enter another number: 11.17  

hats = 50.250000; heads = 11.170000  
hats + heads = 61.419998  
hats - heads = 39.080002  
hats * heads = 561.292480  
hats / heads = 4.498657
```
上方的运算结果是有一点小问题的，对于11.17+61.42=61.419998这个问题，我们不能怪计算机运算能力不行，这不是运算的问题，而是float类型表示有效位数的能力是有限的，由于 float 只能保证6为有效位，将上述结果四舍五入成里为有效位的话，就是我们正常的61.42结果了。
#### 2.4.1 运算符的优先级
C++ 的所有运算符都是有各自的优先级的，对于运算上的优先级，我们依然是按照数学上的优先级进行运算。当两个运算符的优先级相同时，C++将看操作数的结合性（associativity）是从左到右，还是从右到左。从左到右的结合性意味着如果两个优先级相同的运算符被同时用于同一个操作数，则首先应用左侧的运算符。从右到左的结合性则首先应用右侧的运算符。
```cpp
int num1 = 120 / 6 * 114514  // 正常从左向右运算
int num2 = 120 * 12 + 4 / 5  // 按照数学上的运算顺序即可
```
#### 2.4.2 除法分支
除法运算符`/`行为取决于两个操作数的类型（其实是涉及了隐式转换），整数和整数就是按整除进行运算，有至少一个浮点数就会转换成浮点数的结果。
实际上，对不同类型进行运算时，C++将把它们全部转换为同一类型。
###### 对于`/`的补充，我们在正常的编程语言中一般是一个运算符对应一种操作，但是这里的`/`其实是由三种运算构成的：int除法，float除法和double除法。这种使用相同符号进行多种操作的叫做运算符重载。像`/`这种是 C++ 内置的实例，我们还可以扩展运算符重载，以便能够用于用于自定义的类。这个我们在后面会有更详尽的介绍。

#### 2.4.3 求模运算
求模运算符返回整数除法的余数。它与整数除法相结合，尤其适用于解决要求将一个量分成不同的整数单元的问题。

#### 2.4.4 类型转换
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

4. 传递参数时的转换
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

#### 2.4.5 C++11中的auto声明
C++11中新增了可以让编译器根据初始值自行判断变量类型的类型——`auto`。如果使用关键字 auto，但是不指定变量的类型，编译器将会把变量的类型设置成于初始值相同。
```cpp
auto n = 100;         // n is int
auto x = 1.5;         // x is double
auto y = 1.3e12L;     // y is long double
```
显示声明类型的时候，将变量初始化为0不会导致问题，但是用关键字 auto 的时候就会导致问题（因为 0 本身是 int 型的）
对于更加复杂的含义，我们会在后面进一步讨论.

### 三、复合类型
#### 3.1 数组
数组（array）是一种数据格式，能够存储多个同类型的值。要创建数组，可使用声明语句。数组声明应指出以下三点：
- 存储在每个元素中的值
- 数组名
- 数组中的元素数

在 C++ 中我们可以按照下面的格式进行数组声明：
`typeName arrayName[arraySize]`
表达式中的arraySize指定元素数目，他必须是整形常数（如10）或者是 const 值，也可以是常量表达式（如8*sizeof(int)），即其中u送有的值在编译时都是已知的。换句话说，arraySize不能够是变量。
数组之所以被称为复合类型，是因为它是使用其他类型来创建的。不能仅仅将某种东西声明为数组，它必须是特定类型的数组。
数组的很多用途都是给予一个事实：可以单独访问数组元素。方法是使用下标或者是索引来对元素进行编号。C++ 数组是从0开始编号。使用带索引的方括号表示法来指定数组元素。**注意左后一个元素的索引比数组长度小1**。因此，数组声明能够使用一个声明创建大量的变量，然后用索引来表示和访问各个元素，
C++的编译器不会检查使用的下标是否有效，因此如果下标出现问题，我们的程序结果就会出现问题。必须要确保程序只是用有效的下标值。
```cpp
// arrayone.cpp -- small arrays of integers
#include <iostream>

int main()
{
    using namespace std;

    int yams[3];            // creates array with three elements
    yams[0] = 7;            // assign value to first element
    yams[1] = 8;
    yams[2] = 6;

    int yamcosts[3] = {20, 30, 5}; // create, initialize array
    // NOTE: If your C++ compiler or translator can't initialize
    // this array, use static int yamcosts[3] instead of
    // int yamcosts[3]

    cout << "Total yams = ";
    cout << yams[0] + yams[1] + yams[2] << endl;
    cout << "The package with " << yams[1] << " yams costs ";
    cout << yamcosts[1] << " cents per yam.\n";

    int total = yams[0] * yamcosts[0] + yams[1] * yamcosts[1];
    total = total + yams[2] * yamcosts[2];
    cout << "The total yam expense is " << total << " cents.\n";

    cout << "\nSize of yams array = " << sizeof yams;
    cout << " bytes.\n";
    cout << "Size of one element = " << sizeof yams[0];
    cout << " bytes.\n";

    return 0;
}
```
下面是运行结果：
```
Total yams = 21
The package with 8 yams costs 30 cents per yam.
The total yam expense is 410 cents.

Size of yams array = 12 bytes.
Size of one element = 4 bytes.
```
这个程序创建了两个数组：yams和yamcosts。两者使用了不同的初始化方法：逐个初始化和直接初始化，第一的效果和第二种是等效的，所以这里推荐使用第二种。即：`typeName name[arraySize] = {element1, element2, ...};`。这种只需提供一个用逗号分隔的值列表（初始化列表），并将它们用花括号括起即可。列表中的空格是可选的。如果没有初始化函数中定义的数组，则其元素值将是不确定的。有的编译器可能会自动给你初始化为全是0（如VS code）。
###### 关于sizeof和数组：sizeof运算符时返回类型或者是数据对象的长度（单位是字节）。但是将sizeof用于数组名，我们会得到整个数组的字节数。但是用在数组元素上就是对应类型的长度。
```cpp
int array[10] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 0};
cout << sizeof(array); // 输出 4 * 10 = 40 
cout << sizeof(array[0])    //输出 4
```
##### 3.1.1 C++11数组初始化方法
C++11使用大括号的初始化（列表初始化）作为一种通用的初始化方式，可用于所有的类型。数组以前就可以使用列表初始化，但是C++11中的列表初始化新增了一些功能：
首先，初始化数组时，可省略等号（=）：
`double earnings[4] {1.2e4, 1.6e4, 1.1e4, 1.7e4};  // ok with C++11`
其次，可不在大括号内包含任何东西，这将把所有元素都设置为零：
```cpp
unsigned int counts[10] = {};   // all elements set to 0
float balances[100] {};     // all elements set to 0
```
第三，列表初始化禁止缩窄转换（这是所有列表初始化的要求）：
```cpp
long plifs[] = {25, 92, 3.0}    // not allowed
short slifs[4] {'h', 'i', 1122011, '\0'};   // not allowed
char tlifs[4] {'h', 'i', 112, '\0'};    // allowed
```
前两个犯了缩窄转换的问题：精度变小。第三个可以，没有超出类型的范围。
C++ 标准模板库（STL）提供了一种数组替代品——模板类vector，二C++新增了模板类array。这些替代品比内置的复合类型更加复杂、更灵活。

#### 3.2 字符串
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
##### 3.2.1 拼接字符串常量
C++允许拼接字符串字面值，即将两个用引号括起的字符串合并为一个。事实上，任何两个由空白（空格、制表符和换行符）分隔的字符串常量都将自动拼接成一个。下面的格式都是可以的。
```cpp
cout << "I'd give my right arm to be" " a great violinist.\n";
cout << "I'd give my right arm to be a great violinist.\n";
cout << "I'd give my right ar"
"m to be a great violinist.\n";
```
注意，拼接时不会在被连接的字符串之间添加空格，第二个字符串的第一个字符将紧跟在第一个字符串的最后一个字符（不考虑\0）后面。第一个字符串中的\0字符将被第二个字符串的第一个字符取代。

##### 3.2.2 在数组中使用字符串
要在数组中使用字符串，常用方法有两种：一是将数组初始化为字符串常量，而是将键盘或文件输入读入到数组中。下面有个例子：
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
###### 程序说明：我们在这里的strlen()函数是C++库<cstring>中的函数，它接受一个字符串，返回其中的元素个数（不含有`\0`）。这里的sizeof运算符中的结果是15而不是8是因为我们`()`中的是数组名，sizeof(数组名)会返回数组所用的内存长度（char字节数为1），无论你有没有`\0`。至于为什么最后的输出是“C++”是因为cout在处理到\0后就不会继续输出该数组了，后面的内容直接忽略。
由上方的例子可以看到，我们想要用数组来存储我们的字符串，数组长度不能短语strlen(arrayName)+1。

##### 3.2.3 字符串输入
上方的例子有点缺陷，我们来看这个例子：
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
#### 3.2.4 整行读取字符串
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
#### 3.2.5 混合输入字符串而后数字
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

### 3.3 string类
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
#### 3.3.1 C++11字符串初始化
string对象也可以使用列表初始化。
```cpp
string first_date = {"The Bread Bowl"}；
string second_date {"Hank's Fine Eats"};
```
#### 3.3.2 赋值、拼接和附加
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
#### 3.3.3 string类的其他操作
在新增string类之前，我们也可以对字符串进行赋值操作。对于C-风格字符串，我们可以使用C语言库中的函数来玩长任务。头文件<cstring>提供了这些函数。比如我们可以使用strcpy()函数将字符串赋值到字符数组中。使用strcat()将字符数组附加到字符数组末尾。
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
###### 额外说明：这里的`str1.size()`原型是`std::string.size()`，我们在C++中的string类中还有一种成员函数来获取字符长度：`std::string.length()`，一般来讲，这两种函数是等价的。第一种：遵循 STL 容器统一接口（如`vector.size()`、`list.size()`）更通用一些。第二种：源于 C 风格字符串的习惯（如`strlen()`），更语义化，适合处理文本。只要不混用，怎么用都行（是不建议混用）。
处理string对象的语法通常比C字符串函数简单：
```cpp
str3 = str1 + str2;
strcpy(str3, str1);
strcat(str3, str2);
```
第一句和后面的是等价的。

另外，使用字符数组时，总是存在目标数组过小，无法存储指定信息的危险，但是string类具有自动调整大小的功能，从而能够避免这种问题发生。C函数库确实提供了与strcat( )和strcpy( )类似的函—strncat( )和strncpy( )，它们接受指出目标数组最大允许长度的第三个参数，因此更为安全，但使用它们进一步增加了编写程序的复杂度。

#### 3.3.4 string类的I/O
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

#### 3.3.5 其他形式的字符串字面值
之前说过，除了char类型外，还有wchar_t类型，C++11 还新增了char16_和char32_t。可创建这些类型的数组和这些类型的字符串字面值。对于这些类型的字符串字面值，C++分别使用前缀L、u和U表示，下面是一个如何使用这些前缀的例子：
```cpp
wchar_t title[] = L"Chief Astrogrator";   // w_char string
char16_t name[] = u"Felonia Ripova";    // char_16 string
char32_t car[] = U"Humber Super Snipe";    // char_32 string
```
C++11还支持Unicode字符编码UFT-8。C++使用前缀u8来表示这种类型的字符串字面值。
C++11新增的另一种类型式原始（raw）字符串。在原始字符串中，字符表示的就是自己，例如，序列\n不表示换行符，而表示两个常规字符——斜杠和n。
### 3.4 结构（又称结构体）
结构是用户定义的类型，而结构声明定义了这种类型的数据属性。定义了类型后，便可以创建这种类型的变量。创建结构分为两步。首先先定义结构——描述并标记了能够存储到结构中的各种数据类型，然后再创建对应的对象。下面创建结构体的一个例子：
```cpp
struct inflatable{
    char name[20];
    float volume;
    double price;
};
```
程序中的`struct`是关键字，这些代码定义的是一个结构的布局。标识符inflatable是这种数据格式的名称，因此新类型的名称为inflatable。接下来的大括号中包含的是结构存储的数据类型的列表，其中每个列表项都是一条声明语句。列表中的每一项都被称为结构成员。
定义好后我们就可以创建这种类型的变量了。
如果你学过C语言就会发现C++与C语言有点不同：C++允许声明结构变量的时候省略`struct`。在C++中，结构标记的用法与基本类型相同，这种变化强调了**结构声明定义了一种新类型**。
由于结构体是一种新的类，因此我们可以使用**成员运算符**`.`来访问成员。并且我们可以向以往使用内置类型那样使用它们。
#### 3.4.1 在程序中使用结构
先来看一段程序
```cpp
// structur.cpp -- a simple structure
#include <iostream>
struct inflatable  // structure declaration
{
    char name[20];
    float volume;
    double price;
};

int main()
{
    using namespace std;
    inflatable guest =
    {
        "Glorious Gloria",  // name value
        1.88,               // volume value
        29.99               // price value
    };  // guest is a structure variable of type inflatable
    // It's initialized to the indicated values
    inflatable pal =
    {
        "Audacious Arthur",
        3.12,
        32.99
    };  // pal is a second variable of type inflatable
    // NOTE: some implementations require using
    // static inflatable guest =

    cout << "Expand your guest list with " << guest.name;
    cout << " and " << pal.name << "!\n";
    // pal.name is the name member of the pal variable
    cout << "You can have both for $";
    cout << guest.price + pal.price << "!\n";
    return 0;
}
```
下面是结果：
```
Expand your guest list with Glorious Gloria and Audacious Arthur!
You can have both for $62.98!
```
正如上方的程序，我们可以直接对结构进行初始化，只需要**依次**填入对应的值即可，不用必须向上方一样使用多行。值得注意，这里的声明是放在了main函数的外面，对于这种声明，它的作用范围不仅仅局限于main函数，如果存在除了main函数以外的函数，我们也可使用这个结构体，但是如果是在我们main函数当中进行声明，就只有main函数能够创建对应结构的对象了。**C++提倡使用这种外部结构声明，但是不建议使用外部变量**。
另外，我们虽然可以使用类似于数组一样的初始化方式，但是这并不意味着我们可以像使用数组那样使用结构，对于访问结构中的变量，我们还是要使用成员预算符`.`，而不是使用索引`[]`。
#### 3.4.2 C++11结构初始化
与前文所讲的一样，C++11中的结构体也是支持使用**列表初始化**的。当初始化的`{}`中没有包含任何东西，所有的成员都将被设置为0。
同样，**不允许缩窄转换**。
#### 3.4.3 使用string类作为成员
C++是允许使用别的类充当一种类的成员类型的（只要你在使用之前声明过）。这种类型混用的形式我们在后面回说，叫做**组合**。
#### 3.4.4 其他结构属性
C++使用户定义的类型与内置类型尽可能相似。例如，可以将结构作为参数传递给函数，也可以让函数返回一个结构。另外，还可以使用赋值运算符`=`将结构赋给另一个同类型的结构，这样结构中每个员都将被设置为另一个结构中相应成员的值，即使成员是数组。这种赋值被称为**成员赋值**。
###### 注意，这里我们说数组也是可以进行相互赋值的，这里的相互赋值就是将两者的内容拷贝复制，不是公用地址。这点与一些简单的数组之间相互赋值不一样。

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
#### 3.4.5 结构数组
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
#### 3.4.6 结构中的位字段
所谓的**位字段**，就是允许使用以**位（bit）**为单位来指定成员所占用的内存空间，而不是使用传统的**字节（byte）**。字段的类型应为**整型或枚举**，接下来是冒号，冒号后面是一个数字，它指定了使用的位数。可以使用没有名称的字段来提供间距。每个成员都被称为位字段。下面是一个例子：
```cpp
struct torgle_register {
    unsigned int SN : 4;  // SN 占用 4 位（取值范围 0-15）
    unsigned int    : 4;  // 无名位字段，占用 4 位，用来做填充（间距）
    bool goodIn     : 1;  // 占用 1 位（0 或 1，即 false 或 true）
};
```
### 3.5 共用体
共用体（union）是一种数据格式，它能够存储不同的数据类型，但只能同时存储其中的一种类型。也就是说，结构可以同时存储int、long和double，共用体只能存储int、long或double。共用体的句法与结构相似，但含义不同。我们看下面的例子：
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
共用体的是所有成员公用一块地址（也就是说，所有的对象住在一起，你来了我就必须走，即数据丢失）。每一次赋值都会覆盖掉上一次所用的内存。由于我们必须要有足够的空间来存储最大的成员，所以我们共用体的最大长度为**共用体最大成员的长度**。
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

### 3.6 枚举
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
这里的
最后一句是不是有点问题？不是说我们没有为枚举定义算数运算吗，为什么还能与 3 相加？这是因为在这个表达式中，我们将red隐式转换成了0（对应枚举量中的下标）。
```cpp
band = orange + red;    // not valid
```
这里又不行了，因为在算数运算中，我们的orange和red都被转成了int型，运算结果是int型的，int无法自动转换成枚举量，因此报错。
枚举的规则相当严格。实际上，枚举更常被用来定义相关的符号常量，而不是新类型。例如switch（后面介绍）

#### 3.6.1 设置枚举量的值
我们可以使用复数运算符来显式的设置枚举量的值：
`enum bits {one = 1, two = 2, four = 4, eight = 8};`
指定的值必须是整数（并不是必须是int型），也可以进行部分的赋值
`enum bigstep {first, second = 100, thrid};`
这里的first还是0，但是second以后就是从100开始进行依次排序了。
最后我们可以创建多个值相同的枚举量：
`xnum {zero, null = 0, one, numero_uno = 1};`
#### 3.6.2 枚举的取值范围
取值范围的定义如下。首先，要找出上限，需要知道枚举量的最大值。找到大于这个最大值的、最小的2的幂，将它减去1，得到的便是取值范围的上限。例如，前面定义的bigstep的最大值枚举值是101。在2的幂中，比这个数大的最小值为128，因此取值范围的上限为127。要计算下限，需要知道枚举量的最小值。如果它不小于0，则取值范围的下限为0；否则，采用与寻找上限方式相同的方式，但加上负号。例如，如果最小的枚举量为−6，而比它小的、最大的2的幂是−8（加上负号），因此下限为−7。

### 3.7 指针和空间
指针是一个变量，其存储的是值的地址，而不是值本身。要找到常规变量的地址，我们可以使用地址运算符`&`，显示地址时，cout在不同的系统中使用的进制可能会有不同，但是大部分都是十六进制。
下面我们看看指针和C++的关系：
###### 面向对象编程与传统的过程性编程的区别在于，OOP强调的是在运行阶段（而不是编译阶段）进行决策。运行阶段指的是程序正在运行时，编译阶段指的是编译器将程序组合起来时。运行阶段决策提供了灵活性，可以根据当时的情况进行调整。
使用常规变量时，值是指定的量，而地址为派生量。处理存储数据的新策略刚好相反，将地址视为指定的量，而将值视为派生量。**指针用于存储值的地址**，指针名表示的是地址，`*`运算符称为间接值或解引用运算符，用于指针就可以得到指针所存储的地址的对应量。
#### 3.7.1 声明和初始化指针
我们先看一个指针声明的实例：
```cpp
int *p;
```
这个声明表明：`*p` 类型为 int，由于`*`被用于指针，因此，p变量本身必须是指针，可以说：**`p`本身是指针，但是`*p`不是指针，是int型的变量**。
对于`*`运算符两边的空格是可选的，实际上，在C语言中，我们使用`int *ptr`的格式，C++中我们也可以使用`int* ptr`，这两种的效果没有区别，因为C++允许这样做，但是**个人还是推荐前面的格式**。我们来看个例子：
```cpp
int *p1, *p2;   // 我们声明了两个指向int型的指针
int* p1, p2;    // 我们声明了一个int型的指针——p1，一个int型的变量p2.
```
对于每一个指针变量名，我们都要有一个`*`
###### 在C++中，`int*`是一种复合类型，是指向int的指针。
对于其他类型的指针我们也可以这样声明。
虽然不同类型的指针所指对象不一样，但是他们本身的长度是一样的。地址的长度或值既不能指示关于变量的长度或类型的任何信息，也不能指示该地址上有什么内容。
我们可以在声明语句中初始化指针：
```cpp
int number = 666;
int *p_int = &number;
```
这样我们的`p_int`指针就指向了`number`这个int型的变量。

#### 3.7.2 指针的危险
在C++中创建指针的时候，计算机会分配用来存贮地址的内存，但不会分配用来存储指针所指向数据的内存。为数据提供空间是一个独立的步骤。
```cpp
int *p_int;       // 设置一个 int 型的指针 
*p_int = 114514;    // danger! 这里会出问题，这里的114514数据是没有内存地址的，因为他根本就没有被计算机分配内存。这就会导致指针所指向的地址是一个随机值。
```
###### 一定要在对指针应用解除引用运算符（*）之前，将指针初始化为一个确定的、适当的地址。这是关于使用指针的金科玉律。

#### 3.7.3 指针和数字
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

#### 3.7.4 使用new来分配新内存
变量是在编译时分配的有名称的内存，而指针只是为可以通过名称直接访问的内存提供了一个别名。指针真正的用武之地在于，在运行阶段分配未命名的内存以存储值。在这种情况下，只能通过指针来访问内存。在C语言中，我们可以使用`malloc()`库函数来分配内存，在C++中，我们可以使用运算符`new`。
下面是声明语句格式：
`typeName *pointer_name = new typeName(same to former);`
这个语句告诉程序：“我要一块 typeName 类型的内存”。运算符 new 会自动为你分配相应的内存。并返回其地址赋给对应的指针，这样，你的指针就指向了一块新的地址。
上方指针指向的内存没有名称，我们可以说这个指针指向了一个**数据对象**。这里的对象不是我们OOP中的对象，而是一种**东西**。
我们来看一个实例：
```cpp
// 程序清单 4.17  use_new.cpp
// 使用 new 运算符动态分配内存

#include <iostream>

int main()
{
    using namespace std;

    int nights = 1001;                      // 普通变量

    int * pt = new int;                     // 使用 new 为一个 int 分配空间
    *pt = 1001;                             // 在分配的空间中存储值 1001

    cout << "nights 的值 = " << nights
         << " : 地址 " << &nights << endl;

    cout << "int " 
         << "的值 = " << *pt 
         << " : 地址 = " << pt << endl;

    double * pd = new double;               // 为一个 double 分配空间
    *pd = 100000001.0;                      // 存储一个很大的 double 值

    cout << "double "
         << "的值 = " << *pd
         << " : 地址 = " << pd << endl;

    cout << "指针 pd 本身的地址: " << &pd << endl;

    // 比较各种 sizeof 的结果（指针与所指内容的大小区别）

    cout << "pt 的大小 = " << sizeof(pt)
         << " : *pt 的大小 = " << sizeof(*pt) << endl;

    cout << "pd 的大小 = " << sizeof(pd)
         << " : *pd 的大小 = " << sizeof(*pd) << endl;

    // 释放动态分配的内存（注意：原代码中缺少 delete，此处补充提醒）
    // delete pt;
    // delete pd;

    return 0;
}
```
运行结果如下：
```
nights 的值 = 1001 : 地址 0028F7F8
int 的值 = 1001 : 地址 = 000333A98

double 的值 = 1e+008 : 地址 = 0003339BB8
指针 pd 本身的地址: 0028F7FC

pt 的大小 = 4 : *pt 的大小 = 4
pd 的大小 = 4 : *pd 的大小 = 8
```
使用new运算符分配内存是在程序运行的时候进行的。对于指针，需要指出的另一点是，new分配的内存块通常与常规变量声明分配的内存块不同。变量nights和pd的值都存储在被称为栈（stack）的内存区域中，而new从被称为堆（heap）或自由存储区（freestore）的内存区域分配内存。这些我们在后面会说）。

#### 3.7.5 用delete释放内存
当我们使用完对应空间时，我们可以将这片空间归还给计算机。这个时候我们就可以


</font> 

[^1]: 严格来说这里的 const 常量并不一定必须初始化，但是由于我们无法在后面对它进行再赋值，未初始化的 const 常量是无实际意义的（我们并不确定他的值）。
[^2]: 我这里有对应的电子书，有兴趣的可以去看看
