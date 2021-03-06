## C语言概述

- C并不存在赋值语句这一语句，赋值的代码应该称为是赋值表达式，是一种表达式语句


## 数据和C

- 对于一些算术运算，**浮点数损失的精度更多**，计算机的浮点数不能表示区间内的所有值**(实际上计算机中表示的数是离散的)**，浮点数通常只是实际值的近似值。


- 0x前缀和0X前缀表示十六进制值，0前缀表示八进制值


- 十六进制转换说明`%x`，八进制`%o`，显示各进制的前缀，转换说明符使用`%#x` ， `%#X`  ，`%#o`


- 要把一个数字当long 或long long 类型或 unsigned long 对待，可在数字后面加 后缀L（l） ， LL（ll）， LLU，ULL 


```c
//a example written in 2021.9.20
int a = 20;
long b = 20(LL);
long c = 20(ll);
//这个细小的知识其实一般没啥用
```

## switch case

- 如果没有break；语句则程序会从匹配的case开始执行到switch语句的末尾


- break；语句使程序离开switch语句，跳转至switch语句后面的下一条语句

- ```c
  switch（expression1）//表达式1的值应该是一个整数值（含char型），case相同
  {	case expression_0:
   		SomeExpression1;
      case expression_1：
          SomeExpression2;
  }
  ```

```c
//一般都直接用if-else替代掉了，switch -case 狗都不用
//example - 1 2021.9.20
int condition；//以整型为
    switch(condition) {
        case 1：
                expression_1;
            	break;
        case 2:
            expression_2;
            break;
        default:
            default_expression;
            
    }
```



## 循环

- for循环省略第二个表达式，即结束循环的条件被省略，会视为条件一直为真，一直循环
  - `while`这样写不是更好看吗可恶，狗都不用`for`这样写。

```c
for( ; ; ;) {
    expressions;
}
//等效于
while(true) {
    expressions;
}
```

- for循环的第一个条件不一定要是给变量赋初值，可以使用`printf()`
  - 不过一年后我觉得这样写的人都应该去浸猪笼

```c
//也可以是别的函数表达式
for( ; ; ;)
//while也有类似的
    while( ( char c = getchar() )!= EOF)
```

- for是入口条件循环，可能一次也不执行


### continue

- 对于do-while和while循环，执行continue；后的下一步是对测试表达式求值，即判断是否符合循环继续的条件


- 对于for（表达式1；表达式2；表达式3）循环，执行continue；后的下一步是执行更新表达式（表达式3），再对测试表达式求值（表达式2）


### 逗号运算

- 啊我觉得这个如果老师上课没讲就忽略吧，只是一些语法糖，在那个内存很宝贵的远古年代用来节省一些内存的。
- 代码是写给人看的。机器比人便宜。

`表达式1 ， 表达式2`   

- 逗号运算符保证了逗号左侧的表达式先做运算，副作用比右侧的表达式2先发生，整个逗号连接的表达式的值是逗号右侧的值，即整个表达式的值是表达式2的值

```c
//说实话我觉得这些奇奇怪怪的小特性什么用都没有，对代码的可读性毫无益处，除了会在远古代码看见，其他时候可能就是拿来装逼用了
//example：懒得写了，这种烂特性狗都不用
int a = 12;
int b = 5;
int c = (a + b) , b;//c的值为5，与逗号右侧的表达式值相同
```



## 函数

**函数头的形式参数也是局部变量，而在函数原型中可以省略变量名，因为在函数原型中使用变量名实际上并没有实际创建变量，函数原型中的变量名是假名，不必与函数头中的完全一致**

- ANSIC之前的函数头的括号中没有变量的类型，将类型置于后续，但是一种被淘汰的写法


```c
//淘汰了就别管了 我当初看书为什么要记这个东西，奇奇怪怪
void  a(a，b)
char a; int b;
```



### 黑盒

- **爱看不看吧，复习的时候可以不看，只是一种思维。**

被调函数使用的实际参数可以是常量，变量甚至是表达式，但最终都会被具体求值并拷贝给被调函数相应的形式参数，由于被调函数使用的实际参数的值是从主调函数拷贝而来，所以无论被调函数对拷贝数据进行什么操作，都不会影响主调函数中的原始数据，当然，可以通过指针来修改主调函数中的值

从黑盒的视角看，主调函数中发生了什么被调函数是不知道的，被调函数只知道主调函数传给了它几个值，值是多少，反之，主调函数也不知道被调函数中发生了什么，主调函数只知道它传给了被调函数几个值，然后被调函数给了主调函数一些响应，或者一些值

### 返回值

**函数的类型指的是其返回值的类型**

- 函数的返回值可以是任意表达式的值


- 如可以用`return (m > n ) ? n :  m;`来简化求两数中较小值的函数


```c
//涉及到c语言中的唯一一个三目运算符
//也是降低代码可读性的辣鸡特性，不怎么用
return  （m > n ) ? n :  m;
//等效于
if(m > n) {
    return n;
}
else {
    return m;
}
```

- 如果函数的返回值的类型与函数声明中函数的返回值类型不一样，**编译器按函数声明中的返回值类型为准**，**返回的值将会被强制转换为声明的返回类型**。

- ```c
  int fun(void);//函数原型，函数声明。
  //函数的具体实现。
  float fun(void){
      int m = 12.05;
          return m; 
  } 
  //最终返回的是12.05强制转换为int的12。
  ```

- 写于2021平安夜：**当然，到现在这个现代编译器，这样的写法编译器是会报错的，而不是选择容忍这种错误。哎不过都是初学C的笔记了 见谅一下。**

- ![image-20211224233230581](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/image-20211224233230581.png)

- <img src="https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/image-20211224233206161.png" alt="image-20211224233206161"  />





- 使用return语句的另一个作用是，终止函数并把控制返回给主调函数的下一条语句，因此在被调函数的return 语句后面的代码永远不会被执行

很有意思的作者的例子：  教授将永远也不知道写这个函数的学生对他的看法

```c
int ismin（int m， int n）
{
return （n < m )? n : m ;//执行到这一句之后这个函数就结束了
printf("Professor Fleppard is like totally a fopdoodle\n")
}
```

- **如果没有事先声明函数的返回值类型，编译器会假定函数的返回值类型是int** ，然而C99标准不再支持int 类型函数的假定设置
  - 也就是这种代码也是该浸猪笼的可恶。怎么可以不声明函数的返回类型啊啊啊啊，你坏事做尽可恶。

## 旧式函数原型（别看了啊啊啊啊）

```c
//我当时到底在想什么，这种过时的东西也记了，可能是觉得自己以后有机会看远古大能写的远古代码把
```

如int min() //求两数较小值；以上声明并未告诉编译器函数的参数个数和类型，因此如果调用min（）时，编译器无法察觉参数个数的不对（只给一个数）或者是参数类型不匹配（给个浮点数）

主调函数根据实际参数的值来决定传递的类型，而被调函数则根据形式参数的类型来读取值，这两个过程并没有互相协调

## 数组

- 二维数组一要知道列数，作为函数参数时列数（第二位）一定要写，函数头中的数组的列数是给编译器用的，在函数体中不能用到，因此在函数头中需要传入数组的行数和列数


- 如果一个char型数组末尾包含一个 '\0' ，该数组中的内容就构成一个字符串 


## C99指定初始化器

- 可以在数组初始化时在花括号中用[]指定初始化某个元素


- 在声明一个数组时如果没有给出数组大小，则编译器会把数组大小设置的刚好装下


- a[] = {1 , 2 ,  [9] =  5 ,6 ,7},最终a[]中有十二个元素


```c
//为啥是12个捏
int a[] = {1,2,3 [7] = 7,8,9,10};//最终a[7] == 7,a[8] == 8，a[8] == 9，a[3]到a[6] 都没有被赋予初始值
```



### VLA 可变长数组

C11放弃了这一特性，变为可选选项，声明VLA时无法对数组进行初始化

###  二维数组

初始化的时候可以按行按列列成矩阵的形式，每一行用花括号括起，最外层一对花括号，也可只要最外层的花括号

但是这两种情况下如果初始化的值个数不够，被初始化为0的元素的位置的情况就有所不同

如果某一行的值的个数超过数组每行元素个数，只会影响该行初始化，其他行的初始化不会被影响

### 数组与指针

- 指针加一指的是增加一个储存单元，而不是增加一个字节，这对于数组来说就是指向了下一个单元。
- 这也是为什么必须声明指针所指对象的类型的原因之一。只知道地址不够，计算机还需要知道储存一个对象需要多少字节。

## （别看了没用）ctype.h头文件(ChararterType,也即ctype)

专门处理字符的函数，可用于判断字符是否是字母 ，数字，空白符，大写小写字母，或者是输入大写字母返回一个小写字母（不改变原输入的大（小）写字母参数的值，返回值是小（大）写字母，要改变原参数的值，将函数返回	值赋给原参数即可）

## 逻辑运算符和逻辑表达式

！ 运算符是单目运算符，含义为非

逻辑表达式的求值顺序是从左往右，一旦发现有使整个表达式为假的因素，则立即停止求其余表达式的值

### 条件运算符 ： ?:

- 唯一一个三元运算符


`条件表达式的通用形式：表达式1？表达式2：表达式3；`

`如果表达式1为真，执行表达式2，否则执行表达式3`

​                                  

```c
max = （A> B) ?   A  ：B；  
等价于: 
if（表达式1为真）//如果A更大
    max = A ;
//否则max = B
else
max = B;
```



## 指针

### 不要解引用未初始化的指针！！！！

```c
int *p;
*p = 5;
//这样是不行的，指针p未被初始化，此时p的值也即地址是一个随机值，
//我们不知道5将会被储存在何处，这样做可能不会出什么错，也可能无意擦除了数据或代码
```

**创建一个指针时系统只分配了储存指针本身的内存，并未分配指针指向的内存，创建了指针型变量一定要先初始化了，给它赋了某一个变量的地址，再使用它**

- 指针是什么？从根本上看，指针是一个值为内存地址的变量


- 指针的值是它所指对象的地址，地址的表示方式依赖于计算机内部的硬件，通常计算机都是按字节编址，意思是内存中的每一个字节都按顺序编号。采用这种编址方式，对于一个较大的储存对象，（如8字节的double型的某一个变量），通常是该对象第一个字节的地址


- 指针加一，指针的值递增它所指类型的大小（字节为单位)


声明指针变量时必须指定指针所指变量的类型，因为不同的变量类型占用不同的储存空间（字节的不同），**一些指针操作要求知道操作对象的大小，例如给数组名加一，指向的是下一个单元，要求的下一个单元的地址就必须知道单个单元的大小是多少字节，而指向不同类型的指针的单元大小不一样。程序必须知道储存在指定地址上的数据类型**

**以一个int 型的标量变量为例，int  num =  4 ，* p；p = &num；假定一个int型变量是四个字节，由于地址的编值是以字节为单位，p的值实际上是num所在的内存单元 的首地址，只有知道num在内存中占了四个字节，才能正确的从首地址开始往后取总共四个字节，才能读取num正确的值**

### 指针操作

- 赋值：将地址赋给指针型变量，注意地址应该和指针所指类型兼容


- 解引用： * 运算符，没啥好说的


- 取址：指针型变量与其他类型变量一样，也具有自己本身的地址和自己本身的值（本身的值即地址）


- 指针与整数相加：可以使用 + 运算符把指针与整数相加（**指针 + 数 或数 + 指针都可以**），整数会自动乘以指针所指类型的大小，再和初始值相加。如果相加的结果造成数组越界，计算结果则是未定义的。**但C保证正好超过数组最末尾那一个字节的地址的第一个位置的指针有效**


- 指针递增：递增指向数组元素的指针可以使该指针移动至下一个元素的位置，但指针变量的值（也即地址）不是加一（譬如，加四），指针变量本身的地址是不变的，指针指向的地址是变了的


- 指针减去一个整数：这种情况下指针必须是第一个运算对象，必须用指针的值 -  整数（减法不满足交换律呗），其余性质与指针与整数相加的性质相同


- 递减指针：和地址指针一样


- 指针求差：可以计算两个指针的差值，通常是对于指向同一数组的两个不同元素的两个指针进行求差，差的结果是所指两个元素之间的距离，单位是指针所指的类型为单位，比如两个int，两个char，只要两个指针指向同一数组（**或其中一个指针指向数组最末尾的一个字节的下一个字节的地址**），C都会保证相减的运算有效。指向不同数组的指针求差可能得出一个值，也可能运行时错误


**C只能保证指向数组内任意元素的任意指针和指向数组后面第一个位置的指针有效的，可以被解引用的**

- 指针比较：可以用关系运算符比较比较两个指针的值，前提是两个指针都指向相同类型的对象（也就是不要求指向同一个数组）


## (没讲的话别看了)保护数组中的数据

### 对形式参数使用const

如果函数的意图不是修改数组中的数据内容，那么在函数原型和函数定义（函数头）中声明形式参数时应使用const，例如求数组各项总和的函数int sum（const  int a[] ,int n);  const告诉编译器该函数不能通过a去修改该指针指向的数组中的数据内容

使用const并不是要求数组是常量，而是要求函数在使用这个数组时将数组当做不可更改的常量来对待

如果编写的函数不需要修改数组的数据，最好对形式参数使用const

### const的其它内容

#### const数组

const int days[12] =  {十二个月的天数，懒得打}；使用const限定后，数组中元素的值将不能被修改

#### 指向const的指针

int rates[5] = {1 , 2 , 3 , 4  , 5};const int* p  = rates ;  声明指针p时将p指向的int类型声明为const，这表明不可以通过指针p来修改它所指向的值（但是可以用rates来修改），但是不可以通过指针修改指向的值不代表指针不可以指向其他同类型的变量，还是可以让p指向别处的，譬如p = &rates[3];可以的

指向const的指针常用于函数形参中，表明函数不会通过指针来改变数据

void  a（const double array[]  , int n）；，a将无法用array来修改主调函数的数据

关于指针赋值和const的规则：

**指向const的指针可以被赋值为const或非const的数据的地址，但是普通指针只能被赋值为非const数据的地址，因为前者本来就是不打算修改数据值的，而后者是普通指针，通过普通指针是可以改变数据的，如果const数据的地址也可以被赋值，则通过指针就可以改变const数据的值了**因此要想让函数去处理const的数组，那么必须对函数的形参使用const

C标准规定，使用非const的标识符取处理const的数据导致的结果是未定义的

#### const指针

可以声明并初始化一个指针，使它只能指向固定的位置，不能指向别处，但可以修改它所指向的值

int rates[5] = {1 , 2 , 3 , 4  , 5}; int *  const  p  = rates ;

p只能指向rates[0],不能指向别处，但是可以通过p修改rates[0] 的值， 如* p =  0；将rates[0]修改为0

#### const两次，使指针既不能用于修改它所指向的值，也不能指向别处

int rates[5] = {1 , 2 , 3 , 4  , 5};const int*  const  p  = rates ;

### 乱七八糟的，看翁凯写的(不用看了，你想看也行 但是我没润色)

- 数组名字即是数组的地址，也是指针，也是a[0]的地址，即 &a[] == a == &a[0]


- 指针类型的变量用于保存地址，不存在`int*` 类型， `*`表明声明的变量是一个指针，`int   *  p， q;`，`p`为指向`int`类型的指针，`*p`是`int`类型，`q`为普通的`int`型变量 ，`int  *p， * q;`才是`p，q`都为指针，指针变量中保存的值是其他具有实际值的变量的地址，当然，指针变量的名字还是叫p ，q，如果要访问p中所存地址所对应的那个变量，* 如给i赋值，则:  * p =  6 ,* p 直接看做是  i  就行，也把它当做int 


```c
/************这一大堆看不懂没关系，我也看不懂，都是一些很简单的东西****************/
//指针作为函数参数： 
    void f(int  *p);//调用时交给f（）一个地址，int i;
//调用时：
int i = 0;
f(&i);//在函数内可以通过指针访问函数作用域外的变量,访问的方式是通过 * p 运算来访问 i从而对i进行读写
void  f(int * a,int * b)，调用函数  f(&i, &j),
在写函数体时，访问i 和 j，还是用 *  a和*  b
 int  * const  p = &i 指针一旦指向某一个变量后就不能再用来指向另一个变量
const int * p = &i：不能通过 * p来改变   i  的值，让p再指向其他变量 （p = &j）可以  给 i 赋值（i = 26） 可以
const在 * 后面，指针不可再指向其他变量，const在 * 前面，不能通过指针修改变量
const  int a[] = {1,2,3,4},表明数组中每一个单元都是const ，值不能被修改，只能通过初始化赋值
char型变量的指针也是char型
指针加一，会让指针指向下一个单元，减一则指向上一个单元
int  num[]
int * p = num；
则 *（p+1） = num[1]，如果指针指向的不是一片连续的空间则这种运算是没有意义的
指针相减，得到的差是（地址的差）/sizeof（变量类型）,
int   a[]
int * p = a；
int * q = &a[6];
q  -  p =  6 
指针也可以做强制类型转换，还有一种void * 型的指针，int * p =  & i；  void * q = （void*） p；
```



### 动态内存分配

`#include<stdlib.h>`

`malloc(n * sizeof(变量类型))`,**返回的是void *  需要自己强制转换为自己需要的类型，以字节为单位**

`int * p =  (int * )malloc(400 * sizeof(int))`，相当于申请了400个单位的int型数组，当数组用就行。类似`p[0];p[1];`   

**用完记得free(p);**

- free 只能free申请来的初始的空间的首地址，不能free变化后的首地址


## 字符串，输入输出，文件，重定向

- sizeof()以字节为单位给出大小


- strlen（）( %zd 为转换说明 ) 给出字符串的长度，不会计入字符串结尾的  '\0'


- `#define NAME value`  **预处理宏结尾不需要分号!!**

### printf（）

printf（）函数使用转换说明时，转换说明对应的数据可以是常量，比如%c对应一个常量字符 ‘a’，其他类似，printf（）使用的是值

printf（）的返回值是打印的字符的个数，如果输出有错误就返回一个负值，字符的个数包含所有字符数，包含空格和不可见的字符如换行符\n

### scanf（）

如果使用%d转换说明，scanf一个个读取字符，跳过所有空白，直至遇见第一个非空白字符才开始的读取并保存，直到遇到第一个非数字字符，scanf读取到后认为到了数字末尾，将读取到的该非数字字符放回缓冲区

因此如果读取到的第一个非空白字符不是数字或者正负号，那scanf将会停止在此处

scanf（）允许将普通字符放在格式字符串中，除空格字符以外的普通字符必须与输入字符串严格匹配

scanf（ "%d，%d",&a,&g）; 用户必须输入一个整数 一个逗号 一个整数

格式字符串前面的空白意味着跳过下一个输入项之前的所有空白

scanf（）返回成功读取的项数

字段宽度：scanf("%7s", ch);告诉scanf最多只能读7个字符进去，可以少于7，读到7个就停止

scanf（）读取字符串会自动添加  '\0'

scanf（）的  *  修饰符：scanf（"% * d    % * d   %d", &b）; 跳过前两个数字，读入第三个数字并存入b

scanf（）与gets（）一样不安全，可能导致缓冲溢出

**scanf()如果没有成功读取，会将其放回输入队列中，这时要对放回去的错误的输入进行处理**

**缓冲输入**：**完全缓冲 I/O** ：当缓冲区被填满时才刷新缓冲区（将内容发送至目的地） ,通常出现在文件输入中、

**行缓冲 I/O** ：**出现换行符时刷新缓冲区** ；键盘输入通常是行缓冲输入

**无缓冲输入**也不是被淘汰了，在游戏这种即时交互的程序上还是有用的

**回显输入**：用户输入的字符直接显示在屏幕上，无回显输入则不将用户输入的字符显示在屏幕上

### 文件，流和键盘输入

C程序处理的是流而不是直接处理文件，流是一个实际输入或输出映射的理想化数据流，打开文件的过程就是将流与文件相关联，读写都通过流来完成

C把输入和输出设备视为储存设备上的普通文件，键盘和显示设备被视为每个C程序自动打开的文件，因此可以使用处理文件的方式来处理键盘输入

**EOF**：  getchar()  和  scanf() **在检测到文件结尾时返回**的特殊值，这俩函数并不是只返回EOF

一般定义EOF值为 -1，将getchar()  的返回值（返回值实际上是int ，因此ch也要是int型变量，ch是int不会影响putchar（ch） 打印正确的字符）和 EOF做比较就可以确定是否达到文件结尾，因此有：while（（ch = getchar（） ） != EOF)，这样就可以以单个字符为单位循环读入字符

**EOF的使用：**在利用上述循环条件时，键盘输入必须设法输入EOF，一般是ctrl + z输入EOF,使用EOF的上述循环条件时，每一次输入字符并按下enter时缓冲区才刷新并将内容送入程序，但程序不会在输出之后就直接结束，而是继续等待下一次输入并按下回车，直到输入Ctrl + Z(win)并回车 程序才结束

**重定向**（命令行概念）：运算符   >   和    <，看做漏斗，尖的地方指的就是接受内容的一方，开口指的一方就是投入内容的一方，组合重定向,命令与顺序无关，重定向输入文件和输出文件不能相同，否则输入会导致输入文件长度变为0，重定向运算符不能读取多个文件的输入，也不能将输出定向至多个文件，重定向运算符只能连接一个程序和数据文件

**scanf():**scanf的返回值是成功读取的项的个数，可以用于排除用户输入和期望的输入不匹配的情况

**输入流和数字：**输如由字符组成，但是scanf可以把输入转换成整数值或浮点数值，取决于转换说明，使用转换说明限制了可接受输入的字符类型，而**getchar（） 和 使用%c的scanf（）接受所有字符**

## 字符串和字符串函数

puts（）函数只显示字符串，并且自动在字符串的末尾加上换行符

### 在程序中定义字符串

### 1.字符串字面量（字符串常量）

双引号括起来的内容称为字符串常量，双引号中的字符串和编译器自动在结尾添加的  ’\0‘  都作为字符串储存在内存中

从ANSIC起，如果字符字面量之间没有间隔或者空白符分隔，C会 将其视为串联起来的字符串字面量，要在字符串内使用双引号，用转义字符  \ "

字符串字面量属于静态存储类别，如果在函数中使用字符串常量，该字符串常量只会被储存一次，在整个程序的生命周期内存在，**用双引号括起来的内容被视为指向该字符串储存位置的指针，类似于把数组名作为指向该数组位置的指针**

对于同一双引号括起来的字符串，使用不同的转换说明可以打印出不同的效果，%s 字符串，%p，打印储存该字符串的地址，同样可以用 * 来对双引号括起来的内容做解引用运算

printf("%s,    %p    ,    %c","we"  ,    "we"   , * "we"),输出：we，we在内存的地址，地址指向的值的首字母w

### 2.字符串数组和初始化

定义字符串数组时必须让编译器知道需要多少空间，一种是用足够的空间来储存字符串，注意要保证有空间来容纳字符串结尾的  '\0'  ，所有未被使用的数组元素都会被自动初始化为空字符 '\0'，  

const char m1[40]  =  "Asdfghjkl  qwertyuiop"; const表明不会更改这个字符，字符串结尾后续剩余单元全为 '\0'，可以不用const来限定，这样就可以更改字符串的内容

或者可以让编译器来计算数组大小，但只适用于用字符串初始化数组时，如果创建一个数组等待稍后再填充而不初始化，就必须事先在声明时给出数组的大小

**还可以用指针表示法创建字符串**

const char  *p = "asdfghjkl";  这种声明和 const char m2[]  =  "asdfghjkl";   **几乎相同**，这两种形式并不完全相同

### 3.数组和指针创建字符串的区别

使用数组来创建字符串，数组中的字符串是一个副本，程序在运行时为数组分配空间，再将处于静态存储区的字符串字面量拷贝过来

使用指针表示法创建字符串，指针的值是字符串在静态存储区的地址，指针最初指向首字符，但可以改变指针的值使它指向第二个第三个字符，但不能通过指针修改字符串的内容

如果使用普通的指针来指向字符串，那么可以用指针修改字符串，但是这样可能导致程序出现问题，这种行为是未定义的，可能导致出错，因此最好在把指针初始化为字符串字面量时使用const限定符

然而使用非const数组初始化为字符串字面量就不会有这种问题，因为数组得到的是原始字符串的一个副本

### 4.字符串数组

创建字符串数组的方式有两种：指向字符串的指针数组和char类型的二维数组

#### 指向字符串的指针数组：

const char * p1[] ={"string1"  , "string2" ,.....}

指向字符串的指针数组中的每一个元素都是一个指针（而不是字符串或者字符），每一个指针指向不同的字符串，这些字符串不需要是储存在连续的内存中，不同的字符串可以处于静态内存中非连续的位置，且与二维数组不同的是，二维数组每一个字符串都需使用相同个数的个单元（这就必然有多余的内存浪费），而指针不需要，需要多少单元就使用多少单元，把指针数组的字符串组合成二维数组，这个数组的列数是非对齐的，二维数组的列数是对齐的

#### 二维char型数组：

char string[][40] []  [40] = {"string1"  , "string2", "string3" ,....}

二维char型数组就不是新鲜东西了，数组中的字符串是处于静态存储区的字符串的一个副本，每一个元素是一个字符，每一行是一行不大于40个字符的字符串

但是内存比较容易浪费，40个一行的单元未必全被利用，未被初始化的默认初始化为 '\0'

####  相同之处

二者的相同之处在于，使用一个下标时都代表一个字符串，使用两个下标时都代表单个的字符

p[0]  就是第一行字符串，p [0] [0] 就是第一行的第一个字符，二维数组不要多讲

#### 如果要使用数组来表示字符串，最好使用指针数组来指向字符串，效率高于二维字符数组，但是缺点在于指针是指向const的，无法通过指针来修改字符串字面量，因此如果想要改变字符串，还是需要使用数组

### 字符串输入(要先为字符串分配了内存空间，数组或者malloc)

#### gets（）函数

gets（）函数读取整行输入，直至遇到换行符，然后丢弃换行符，储存其余字符，并在这些字符的末尾添加一个 '\0'使其成为一个C字符串

经常与puts()配合使用，puts（）显示字符串并自动在末尾添加换行符

gets（）是不安全的，它的唯一参数就是一个数组或者说指针，它无法检查分配给字符串的内存空间（或数组）是否装得下输入的字符串，如果输入的字符串过长，会导致缓冲区溢出（多余的字符超出了指定的目标内存空间），可能会擦除数据，也可能没事

gets（）返回值是指针

C11标准委员会以及从标准中废除了gets（）函数，不过编译器还是兼容gets（），有一说一还是挺好用的

#### gets（）的替代品

##### fgets（）和fputs（）

fgets（）第一个参数和gets相同，第二个参数用来限制读入的字符数，该函数专门设计用来处理文件输入

第二个参数用于限制可读入的字符数，如果参数值为n，则**最多读入n - 1个字符为止**（这n-1个字符包括换行符\n的，譬如字符个数远小于n，就会把换行符也储存下来，由于至少要留一个单元写入空字符 \0,所以最多读入n-1个字符），**或者读到遇到的第一个换行符为止**

如果fgets（）读到一个换行符，会把它储存在字符串中而不是丢弃它，与gets（）不同

fgets（）的第三个参数指明要读入的文件，如果从键盘输入，则以stdin为参数，该标识符定义在stdio.h中

由于fgets在读取到换行符\n后会将其储存在字符串中（因此字符串结尾最后两个单元是\n '\0')

所以与之配对使用的fputs（）在打印字符时不会在字符末尾自动添加一个换行符

这与gets（）和puts（）类似，前者丢弃读到的换行符，后者打印的时候就自动添加一个换行符

fputs（）函数的第二个参数指明它要写入的文件，如果是显示在屏幕上，则以stdout为参数

fputs（）返回指向char 的指针，如果一切顺利，返回的地址与传入的第一个参数相同，如果读到文件结尾就返回空指针，gets（）在读到文件结尾也返回空指针

#### gets_s()函数

C11新增，与fgets（）类似，用第二个参数限制读入的字符数，第一个参数是指针

不需要第三个参数，gets_s（）只从标准输入中读取数据

**gets_s（）读到换行符，会丢弃换行符而不是储存换行符，也是读到第一个换行符为止**

如果gets_s()在读到最大字符数都没有读取到换行符（换句话说就是字符串太长了超过限制了）,会执行以下操作：先把目标数组中的首字符设置为空字符，读取并丢弃随后的输入直至读到换行符或者文件结尾，然后返回空指针。接着调用依赖实现的 ”处理函数“ ，可能中止或退出程序

反正还是没fgets（）好用

有时候要丢弃过多的输出部分的字符，这是为了防止多出的字符串留在缓冲区中成为下一次读取的输入，导致输入与键盘输入不同步

#### puts（）

puts（）遇见空字符时就停止输出

puts（）从给定的地址指向的字符开始打印后续的内容直至遇见空字符，如果没有遇见空字符就会一直往后读取打印下去直至遇见某个地方的换行符

———————————————————————————————————————————

## 结构

### 枚举

enum  name { 名字0，名字1,名字2， }，也是结构的一种，可以跟上enum作为变量类型，作为函数参数是也要像int之类的一样带上去

enum   months{...........};

enum months    month =  ..;

实际上内部enum就是int，以整数的方式来进行计算和输入（%d）输出(%d)的

声明枚举量的时候可以指定值 如{red = 1 ，yellow ，blue = 4 ，white}，这时red 的值为1而不是0.yellow的值不是1而是1后面的2，blue后面的white不是3而是5

给enum name 型的变量赋名字列表中不存在的值也是可以的（比如按上面1245的值，赋0就是不存在的值），但是没啥意义

### **结构类型**

是一个复合的数据类型，在一个结构里会有很多种数据类型，甚至可以在一个结构里有另一种结构类型的数据

结构中声明同一类型变量可放一行

struct name(可以没有name的 但是建议有，会方便很多)

struct name{

int xxx;   char xxx;

};(注意结尾有分号)        声明结构型变量：struct name      变量名；

如 struct point  p，q；

```c
//example 2021.9.20
struct people {
    int age,height,weight;
    char[100] name;
    char id;//瞎写的
};
struct people taylor;//声明一个struct people类型的变量
```

声明结构的另外一种形式是 

struct{

int  xxxxx；

char  xxxxx；

}p，q；（没有结构的名字，在结构后面直接声明该种结构类型的变量p和q），多个结构时使用不太方便，个人觉得还是第一种更像声明普通的int 、char类型的变量语法差不多 更习惯点

```c
//example
struct {
    int a;
    char b;
} A ,B ;
```

甚至可以将两种声明的方式结合：

struct name{

int xxx;   char xxx;

}p，q;，声明一个结构的同时声明这种结构的两个变量p，q

```c
//example 2021.9.20
struct people {
    int age,height,weight;
    char[100] name;
    char id;//瞎写的
}taylor;
```

结构型变量的作用域的性质与普通的变量类型的性质相同，也存在本地变量和全局变量之分，通常把结构放于全局区，类似对自定义的函数的做法

结构的初始化与数组的初始化类似，既可以按照结构内变量的顺序直接在{1,2,3}内依次罗列值，也可以通过点号访问结构中特定的变量并赋值stuct date   today(结构型变量名) = {.month = 12,   .year =  2020 },结构中未被初始化的变量则会被初始化为 0 ；

```c
//example 2021.9.21
struct date {
    int day;
    int month;
    int year;
};
struct date today = {20,9,2021};
//初始化today的方式也可等效于:
today.day = 20;
today.month = 9;
today.year = 2021;//要访问某一个结构类型的变量的成员变量，变量名加.运算
```

struct  day {        

int  month；访问month： today.month = 12；

int day；

int year；

}today；

要访问整个结构，直接用结构类型变量的名字（标识符），对于整个结构，可以赋值（struct date p ={2020 , 12 , 1 }）（并且数组不能做的那种赋值，整个结构的赋值，数组不能整体的赋值，即不能    a[10]   =   b[10] ，而结构可以 ，并且赋值是成员对应赋值的），取地址，也可以整个传递给函数作为参数

结构变量的地址并不就是结构变量的名字，这一点与数组不同，要取地址也要先声明结构型的指针 

struct date today；

struct date * p = & today；

### 结构与函数

int fun（struct date   p），整个结构可以作为参数传入函数，这时候是在函数内新建一个结构变量并复制调用者的结构的值，也可以返回一个结构

要通过函数来修改主调函数中的结构的值，可以建立一个返回类型为结构类型的函数，在函数新建一个结构并修改，再将结构返回给调用者 或者可以通过指针来访问结构变量进而访问结构中的成员

单个成员传入函数与普通变量相同，函数参数表不需要做特殊修改

void fun(int x);调用fun：fun(today.month)

### 结构与指针

struct date today；（省略声明结构的过程了，直接声明结构变量了）struct date * p  = & today；（取结构变量的地址）

要访问结构变量today中的成员month day year：

可以用指针加点号的方法（点号的运算优先级更高）：

(* p) .month = 12 ;或者直接用另外一种 ： p -> month = 12

用    ->   表示指针所指的结构变量中的成员

p -> month 整体可以当做是一个普通变量类型的变量来使用，可以在scanf（）内用&来取地址赋值，也可以在printf（"&d",p -> month）内用p -> month

### 结构与数组

可以声明一个结构，再声明一个结构类型的数组

struct date {

int  year；int month；int day；

}；

//下方声明含有2个struct date 类型变量的数组，初始化时也要一组结构结构地初始化，有点类似二维数组的初始化

struct date  days[2]  = {

{2000 ，11,21}，{2020,12,4}

}；结尾的分号别漏了

访问每一个数组单元和普通数组一样

先访问数组，再访问该数组的结构里的成员：days[1].year   days[0].day

### 结构中的结构

结构中的元素可以是其他事先以定义的结构，访问结构中的结构还是通过点号，访问结构中的结构的成员，先访问结构中的结构，再访问内层结构的成员，struct date {

int year；int day；

}；

struct dates{

struct date  date1；struct date  date2；

}dates1；

访问date 1：dates1.date1 ；

访问date1 的year： dates1.date1.year；

可以通过指针来访问：struct dates * p  = &dates1;

那么要访问date1的year：如下方式是等价的

dates1.date1.year；

（*p）.date1.year；（注意括号，.的运算优先级更高）

(p -> date1).year;这个括号可以没有，二者优先级相同

## 联合

可以用typedef来简化声明结构类型变量时的字数

在定义结构类型时同时利用typedef给这个结构一个单词的名字

typedef   struct date{

int day；int year；int month；

}     （Date左边是有空格分割的）Date；

最后一个单词为名字（用空格分割），typedef和最后一个单词之间的为你要重新定义名字的内容

要声明struct date型变量只需 Date date1；，等效于struct date date1；

有了typedef以后甚至可以不需要struct后面的结构的名字，

直接typedef  struct {

}   Date；   Date   date1；（声明变量date1）

不需要此前那个struct后的date，因为Date足够了

## 神奇错误

在交换两个变量a b的值的时候

要写成int y = a；

b = a；a = y；

**不要写成 int  y； a = y；这样是吧y的值赋给a，而不是把a的值赋给y；**

**在自定义的函数中一定不能出现与函数参数表中同名的变量名字！！！！！！！！！！！！！！！**