# C programming

## Output
单字符：
```
putchar('char')

cout << 'char';
cout << '\'';
``` 

多字符：
```
printf("str");

cout << "str";

```
格式化输出：
```c
"%d", int
"1.6f%", float   //'1'表示至少1 Byte

cout << "str" << int << endl
```

```c
//formatting output
%[flags][width][.prec][hiL]type 
flags(标志):   
/*   
 -     //左对齐  
 +     //强制输出加号  
 0     //0填充  
 space //强制在结果前加一个空格(负数则输出负号)
 #     //保证浮点数定会输出一个小数点
*/
width: 
/*          
            数字 //最小字符数
            *    //下一个参数是字符数
            .数字  //小数点后位数
            .*       //下一个参数是小数点后位数


         */

hiL(修饰符):
/*
和类型说明符一起使用，表示延伸的变量类型
h  //unsigned short int
hh //char
ll  // long long
L  //long double
z  //sizeof()专用类型
*/
type:  
/*  
    %f  //float 6位小数
    %g  //float   
    %n  //统计%n之前出现过多少个字符并赋给一个地址(&num)   
    %a或%A  //十六进制浮点 
*/ 
printf()   //返回输出的字符数，可以作为程序错误检验的依据
```

## Comments：

```
//  行
/**/ 块
```

## 变量(variable type)：
```
· int        --4 Bytes(常量同)
· float      --8 Bytes
· bool       --1 Bit
· double     --   
· char       --1 Byte
· 
```

## evaluation：
```
var = var2;  
var = value; //此表达式返回变量值
var = ++ var3;
```

## initialization 
```
int b = 0;
```

## type conversion 
```
int --> char  ----丢掉高位字节
char --> int  ----先赋低位字节，如果低位字节的最高位是0，前面都补0；是1，则补1
10000000 --> -128
01111111 --> 127
-n == ~n+1 --->'~'取反符号，即每个bit 0 -> 1, 1 -> 0.




bool isMale = 100;     ---- isMale == 1
hexadecimal  --
octonary
binary
```

## input
```
cin >> var >> var2 >> var3;  ---非链式赋值而是多个赋值

```

```c
//formatting input
%[flag]type
flag: /*     *   //跳过这个type
            数字 //最大字符数
            hh  // char
            h  // short
            l  // long or double
            ll  // long long
            L   //long double

        */
type:/*
    %d   //十进制整数
    %i   //按照类型判断进制的整数类型
    %a, e, f, g    //浮点数
    %[^]    //过于复杂，暂不赘述

*/
scanf()  //返回输入的item数

```
## macro宏(完全的文本替换)
```c
__宏名字__   //预定义的宏，代表一些特殊的值，例如文件名，行数，系统时间
#define mm     // (mm) 是宏的名字，这表明mm是一个空的宏(一般用于条件判断)，一般一行写完，用\可以写多行

#define max(a, b) (a > b ? a : b)   //带参数的宏，若参数前加个‘#’ 后面紧跟的参数(表达式)会被变成字符串
// 定义宏的时候可以引用别的宏
#define SAVE(exp) exp##Temp = exp     // expTemp = exp    //## 连接符，连接符两端可以调用参数值

#ifndef 宏名字      /* 若宏没被定义过则执行ifndef与endif之间的内容*/
#define 宏名字      //防止文件重引用，一般用文件名来命名macro
#endif

#if 
#elif 
```
```c
#include <filename>  //在标准库里找filename

#include "filename"  //在当前目录找filename

```
```c
//pifalls
#define MAX(a, b) (a > b ? a : b)
MAX(x++, y++)  //这样用容易出问题，别传表达式
```

## 声明
```c
static  //用于声明局部作用域，可以对函数，本地变量，全局变量使用，让其仅能在相应局部作用域使用
volatile  //用于声明变量是随时可变的(线程数据共享，寄存器)
extern //变量或者函数的定义在别的文件中。提示编译器遇到此变量或函数时，在其它模块中寻找其定义
```
## identifier

命名规范，可读性好，不能用保留字

## variable

变量都有标识符和类型
匿名变量不能作左值
```c
static int i; //i是静态本地变量，特殊的全局变量(static表示局部作用域)，只能初始化一次，每次进入函数后值是上一次离开函数后的值，作用域是从定义之处开始，到文件结尾处结束即使在这个变量声明前的语句无法访问
extern int i； //声明i是全项目共享的全局变量，让编译器明白i在项目某处，让它去找值
```

## const
通常大写，常量更好辨识

## literal(常量值)

1. int
```
十进制 123
八进制 0173 --零开头则是八进制
十六进制 0x7B == 0X7B --0x(X)开头是十六进制
```
    ·representing negative values
        1. 思路: 用高位整数映射负数(补码)   ---待证明
        2. 对称性，正数的补码还是它本身
        3. 例子(ten's complement)：Negative(i) = 10^k - i,where k is the number of digits.  ---binary negative(i) = 2^k - i
        4. binary 最高位为1的为负数
        5. 反码(取反后的bits序列)+1即为补码
        6. 计算机算数时要么得到结果，要么溢出(Overflow)
            ·Overflow is ...
            .正正得负，负负得正(相加运算)，则溢出
            .永远别太相信计算机计算的结果(怕溢出)
    .bit(位)  1 or 0
    .byte(字节)    8bits
        .c types maybe int8_t, uint8_t, char
    .representing fraction part
        1. 移位操作 << >>  等价于乘(除)base
        2. 少判断两个表面看起来相等的数是否相等
            ·(float)0.15 != (float)0.45 / 3    
        3. 小数点
            ·定点表示法  --用固定的byte表示小数部分，别的byte表示整数,泾渭分明
            ·浮点表示法  --科学计数法 sign * mantissa * 2^exp用exp来决定小数点要移动的位(单独拿出一位来作为sign)        
        



2. float
`12.3 == 1.23e1`
3. char
```
'\123'  --- 表示该字符ASCII码是 八进制的“123”
'\xFF'  --- 表示该字符ASCII码是 十六进制的“FF”
'\b'    --- backspace
'\r'    --- carriage return 
'\?'    --- question mark

```

4. string
```
printf("hello,"
"world");   ---是可以的

printf("Hello, \
world");  ---也是可以的(world前面若有空格也会作为string的一部分)


```
5. long int
`123456789L                      `
6. unsigned long int
`123456789ul`
7. enum
```
enum bool { false, true };
    enum bool m = false;   ----m == 0
定义一个类型bool,花括号里的名字若不初始化的话默认从0开始，不赋值的话后面的值总为前面值+1
使用时声明如上;
枚举的值只能是整数。

```

## Data Types and Sizes
```
short int -- 16 bits
long int -- 8 bytes
long double --- 16 bytes
unsigned numbers --无符号数
sizeof() --返回字节数

```

## Arithmetic Operators
```
+-*/ ---不同输出类型运算方法不同(典型的例子：整除)
%    ---整数求模
```

```
优先级：左结合(+-*/%)
(unary)+ == (unary)- > (binary)* == / == % > (binary)+ > (binary)-
(unary)! > && > ||    ----非 > 与 > 或
```

```
赋值操作符:返回左操作数，遵循右结合
(x = 1) = 2   ---- 等价于x = 2
x = y = z = 100;   ----x, y, z都是100 
```

优先级见C programming language  P<sub>42</sub>






## Type Conversions
操作符两旁数据类型不同时会触发类型转换
```
赋值时会类型转换 ---   int d = 1.1;     ----d == 1
运算符遵循由“窄”到“宽”原则 使用更“宽”的运算符，并转换数据类型
warning:规则上的错误不会触发类型转换(例如: 3.5%2  a[1.5])
```

```
char -> int 时，值不变
作为参数传递时也会触发类型转换
显式转换:(double)n ----n的值不受影响(中间过程存在匿名变量)
```

## Increment and decrement
```

++ i ------令i = i + 1，并返回i + 1, 可作左值相当于i
i ++ ------令i = i + 1，并返回i的值(中间过程存在匿名变量)，不可作左值
arr[i++]
*p++
```

## Bitwise operator

`据说比四则运算快`

& | ^ << >> ~

把整数看成bit组成的序列

按位与：& 每一位进行与运算(1与1等于1，其他都为0), (按位与1可以消除前面的所有位)用于取某些位的一段

按位或：可用于把两个数拼起来

按位异或：0^0 and 1^1  == 0, 否则 == 1      x ^ y ^ y == x
`a^b = c 则 a^c = b 且 b^c = a`

按位取反：'~'  0->1   1->0  -x = ~(x - 1)

左移右移：   1.   << 整个序列往左移动，多的位补0 
            2.   >> ·有符号补符号位
                    ·没符号补0
            3. 等价于乘除2

```c  
struct bitpass{
    int a : 2;
}        //位段 整个结构体公用n个int，： 
         //冒号用于分配这个变量用几个位
```


 
## Assignment Operators
```
= - * / % << >> & ^ |     ----都有增强运算符
x &= x - 1                ----去掉x最后一位1 
```



## Conditional Expression
 condition  ? exp1 : exp2 
 若condition为真执行exp1并将结果作为返回值，为假执行exp2并将结果作为返回值
 
## Boolean expression
Morgan's Law

Short-circuit operation

尽量减少逻辑运算符两边出现改变程序的表达式

可以用^来代替相等判断
        
## switch statement
```c
switch(expression){
    case const-expression: statements;(break;)
    case const-expression: statements;(break;)
    default:statement; 
}

```
## C++ formatting output
```c++
#include<iostream>
#include<iomanip>
setw(int)  //制造打印空间，之后打的字符太少则补空格，默认右对齐
cout << left ; //此语句后默认左对齐
setprecision(int) //设置有效数字，打印前int个数字(包括整数部分)
cout << fixed; //次语句后setprecision的有效数字从小数部分开始计
```
## for-loop
```c
for ( ; i ; i ++ )
```

for-loop里的i++在循环一次后会执行，不受continue影响

引入flag变量，结合if语句可以辨认出所需的量，再count++


## function
>a good c program consists of many small functions instead of a few big ones<br/>
><del>By ME and, ME</del><br/>
```matlab
和c的风格不同，maybe
```
<h3>函数调用过程</h3>

1. 分配callee中定义变量的空间(函数返回的变量，本地变量，匿名变量(可重用))(连续空间)
2. 传参：把实参赋给形参
3. 运行callee
4. 返回值(匿名变量储存)
5. 释放匿名变量
6. caller继续运行
7. 常量属于全局变量
8. 函数内调用的函数调用函数时，前两个函数都在运行
### local variable
函数调用时才给 automatic variable 分配内存空间
<pre>   
    1. 节省空间
    2. 有利于空间重用
</pre>

### return type

`gcc *.c      //编译一大把c文件 `

### declaration

<b>函数内部不能定义别的函数，但是可以声明哈哈哈。</b>

### external variables

also global variables;<br/>
functions share data among functions<br/>
functions communicate with others 

函数内部要声明 extern int x ;否则会创建一个新的本地变量覆盖掉x；<br/>
不同文件共享一个全局变量的时候要声明 extern <br/>

### static external variables and functions

static global variables or functions 这个变量(函数)只能在本文件访问(封装)<br/>
static internal variable 目的是防止x被外部访问且让x可以存在下去(方便创造每次调用返回值不同的函数)
```c
void next()
{
    static int next;    //会自动初始化,global variable 也是
    return ++ next;
}
```
### register variable
寄存器是放在CPU里面的变量，操作可以更快，常被访问的变量可以放寄存器<br/>
只能用于局部变量，个数有限制，类型有限制，但声明其实是无害的<br/>
寄存器变量是没有地址的

### block
block 就是 block

### initialization
外部变量和静态变量自动初始化为 0 <br/>
内部变量不自动初始化是因为当时计算机速度太慢<br/>
外部变量初始化要是一个常量

### function matching(c++)
c++允许函数重名,用函数签名(名字和参数表类型)来区分<br/>
函数重载<br/>
近似匹配，完全匹配<br/>

### default argument(c++)
定义函数的时候可以给默认值,默认参数必须是最后几个，中间不能有正常参数

### Recursion / recursive function call(递归)
递归调用后，编译器会开辟出新的内存空间分配给新的函数<br/>
在另一个函数里改变局部变量并不影响之前函数相应变量的值<br/>

递归函数:<br/>
    基础步<br/>
    递归链条<br/>

优点：好写(好写吗？)<br/>
缺点：效率低

#### Dynamic programming
改进递归<br/>
查表法(记录前一次的答案)





## structure
```c
struct Student {
    int id;
    int grades;
}student; //创建一个实例student

student={0, 0}; //直接赋值
```


## Array
```c
int array[10] = {0}; //全初始化为0
int  array[] = {1, 1}; // 两个元素的数组
static int array[]; //函数内部的独有数组
//除非用结构，不然不能直接赋数组
//初始化和赋值不同
//运行的时候数组的长度被舍弃了
```

### 编译过程
`//c++ 尽量把可做的事情放在编译时完成,为了效率，编译后的程序和源码已经有很大区别(舍弃了很多信息)`

### 地址
`&a[i][j] = a + i * N  * sizeof(a[0][0]) + j * sizeof(a[0][0]) //N是数组总列数`
<pre>
一个整数，内存编号来的(一般十六进制)
32位计算机内存地址用32位表示
对数组名取址和数组名一样都是第一元素的地址，但若对指针(作为参数的数组名)取址则不同
数组名作为函数参数会被翻译成第一元素的地址
数组越界访问"数据"会报错，但不访问内存内容却不会(仅打印地址)
sizeof(Array) 编译时替换成数组占用内存大小(byte)
习惯上，传非字符数组参数时，会传数组长度，其实字符串也会
</pre>

### 字符数组(字符串)
<pre>
C语言规定，字符串以 '\0' 结尾
字符数组是特殊的，初始化可以直接用字符串常量(自动以0结尾)，但不可以直接赋值
scanf("%s")会自动给数组添'\0'，会忽略字符前的空格及类空格(非可见字符)，从第一个可见字符读起，读到空格或类空格结束
标准函数打印字符串时，内部的不可见字符会用相应方式打出来
善用strlen(字符串长度不包括'\0')， strcpy(字符串复制)， strcmp(char* a, char* b)(一个个比较字典序，2 > 1, b > a,第一个相同比下一个，不同为止，或比完为止)
</pre>

### 多维数组

```c
&a[i][j] = a + i * N  /* * sizeof(a[0][0]) */+ j /* * sizeof(a[0][0])*/ //N是数组总列数
```
多维数组是上一维数组的数组(逻辑上)，物理上是行主导连续排列，
所以不能简单靠相乘算出元素的地址。

多维数组作为参数时，第一维不用写明，但第二维及以后的维数必须要写明

多维数组的名字是第一个元素的地址，第一个元素可以是一个数组


## Std Input and Output

### redirection 

freopen了解下

stdin,stdout,stderr 都是抽象的文件指针

sprintf,sscanf了解下

### Pipe mechanism




































