## 第一个Java程序

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello world!");
    }
}
------------------------------------------------------------------------------
int[] numbers = new int[3];
numbers[0] = 4;
numbers[1] = 7;
numbers[2] = 10;
System.out.println(numbers[1]);
数组的：Alternate Java
int[] numbers = new int[]{4, 7, 10};
System.out.println(numbers[1]);
------------------------------------------------------------------------------
You can get the length of an array by using .length, for example, the following code would print 3:
int[] numbers = new int[]{4, 7, 10};
System.out.println(numbers.length);
/*也就是说数组的大小并不需要像c那样单独用一个int型变量来记录，直接Arryname.length即可得到
```

**关键语法功能。**我们的第一个程序揭示了Java的几个重要语法特性：

- 所有代码都存在于一个类（class）中。
- 执行的代码在称为的函数（也称为方法）内部`main`。
- 花括号用于表示代码段（例如，类或方法声明）的开头和结尾。
- 语句以分号结尾。
- 变量具有声明的类型，也称为“静态类型”。
- 变量必须在使用前声明。
- 函数必须具有返回类型。如果函数不返回任何内容，则使用void，
- 编译器确保类型一致性。如果类型不一致，则程序将无法编译。

**静态类型。**（我认为）静态类型是Java的最佳功能之一。与没有静态类型的语言相比，它为我们提供了许多重要的优势：

- 在程序运行之前检查类型，使开发人员可以轻松捕获类型错误。
- 如果编写程序并分发编译的版本，则（大多数）可以保证该程序没有任何类型错误。这使您的代码更可靠。
- 每个变量，参数和函数都有一个声明的类型，从而使程序员更易于理解和推理代码。

静态类型的缺点，将在后面讨论。

**编码样式。**编码风格在61B和现实世界中非常重要。应按照教科书和讲座中的说明对代码进行适当的注释。

**命令行编译和执行。** `javac`用于编译程序。`java`用于执行程序。我们必须始终在执行之前进行编译。

————————————————————————————————————————————

# Java核心技术的笔记

## 第三章：Java的基本程序设计结构

### 第一个java程序

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello world!");
    }
}
//注意Java是大小写敏感的
//关键字public称为访问修饰符，这些修饰符用于控制程序的其他部分对这段代码的访问级别
//根据Java语言规范，main方法必须声明为public
//Java应用程序的所有内容都必须放在类中
/*关键字class后隔一个空格紧跟类名，Java对于定义类名的规则挺宽松的，开头必须是字母，后面可以是字母数字的组合，长度基本上没有限制，但是不能使用Java的保留字（关键字）作为类名*/
//注意源代码中的{}，java与C类似，Java中任何方法的代码都以 { 开始而以 } 结束
/*System.out.println("Hello world!"); 在这条语句我们使用System.out对象并调用了它的println（）方法，点号(.)用于调用方法，调用方法的格式为object.method(parameters)
其实就是函数调用
//java与C一样，都使用双引号  ""  来界定字符串
//println()方法在字符串末尾自动添加一个换行符，print()方法则不会
```

**源代码的文件名必须与公共类的名字相同，并用  .Java  作为扩展名，也就是说储存上述代码的文件的名字必须是 ：HelloWorld.java（大小写敏感！！，扩展名java全小写）**

当使用 java ClassName 来运行程序时，Java虚拟机总是从指定类中的main方法的代码开始执行，因此为了代码能执行，类的源文件中必须包含一个main方法

我们暂且不管static void是什么意义，只要记住：每个java程序都必须有一个main方法，其声明格式如下：

```java
public class HelloWorld {
    public static void main(String[] args) {
        program statements;
    }
}
```

### 注释

java支持和C类似的两种注释，即 //  和/*    */型，此外还有第三种可以用来自动生成文档的注释，这种注释以 	/**	开始，以	*/	结束

但是java不能/**/嵌套，反正就是不能像debug C 程序时那样把一部分代码用	/ *   和 * /注释掉

### 数据类型

java的**基本数据类型**和C极其相似，除了布尔类型bool改成boolean，其他的的类型名字都一模一样

与C不同，Java规范中没有“依赖具体实现”的地方，基本数据类型的大小以及有关运算的行为都有明确说明，不像C，int类型的字节数可能是4字节，也可能是8字节，取决于编译器之类的

#### 整型

没有小数部分的数，允许是负数，int最常用，byte和short类型主要用于特定场合

**Java没有无符号整型，unsigned int之类的类型，当然相应的类中有相应的方法可以让我们得以使用无符号类型**

```java
int   4字节
short 2字节
long  8字节
byte  1字节（8位）
//长整型数值有一个后缀L或者l，八进制有前缀0，二进制有前缀0b或者0B，还可以为数字字面量加下换线，如1_00_000,编译器会忽略这些下划线，下划线纯粹为了让人更好读
```

#### 浮点型

基本上没什么情况使用float类型，因为精度不够

**浮点数值不适用与无法接受舍入误差的计算，如果在数值计算中不允许有任何舍入误差，应该使用BigDecimal类**

```java
float  4字节，有效位数6到7位
double 8字节，有效位数15位
//float类型的浮点数有一个后缀F或f，没有后缀的浮点数总是默认为double类型
//三个表示溢出和出错情况的特殊浮点值：正无穷，负无穷，NaN（not a number）
//不能用if（x == Double.NaN）检测x是否是NaN，所有“非数值”的值都认为是不相同的
//有double.isNaN()方法来判断上述
```

#### char类型

原本用于表示单个字符，但是现在某些Unicode字符需要两个char值来描述

char类型字符字面量用单引号（其实就是都和C差不多）

转义序列有一个表，所有的转义序列都可以出现在加引号的字符串或字符字面量中，当然也可以出现在之外，书上有例子

Unicode转义序列会在解析代码之前得到处理

核心技术的作者强烈建议不要使用char类型，最好将字符串作为抽象数据类型处理

#### boolean类型

**两个值：true和false，用于逻辑判断，布尔类型不能与整数类型相互转换**

### 变量与常量

#### 变量声明

变量名必须是以字母开头的字母数字的组合，不过这个范围与一般所想的字母数字稍微有点不同，具体看书吧，对于这个细节我学了也没什么用

大小写敏感,名字长度没什么限制，声明变量的方式与C相同，声明可以放在代码任何地方，这一方面保持使用C的变量时的习惯就行

Java并不区分变量的声明与定义

```java
int a;
int a,b,c;
double d = 1.0
Dog aDog；//type  VariableName；
```

#### 变量初始化

**声明一个变量后，必须对一个变量进行显式初始化，不要使用声明了而未被初始化的变量的值**，过不了编译

```java
int a;
System.out.println(a) //java认为这种语句是错误的，变量未初始化
int b = 1；
（或int c;c = 2;)//这种赋值也可以
System.out.println(b) //正确
```

**从Java 10开始，对于局部变量，如果可以从变量的初始值推断出它的类型，就不再需要声明 类型，只需要使用关键字var而无需指定类型**

```java
var a = 12 //a is an interger;
var greeting = "Hello World"//greeting is a String
//是一个java的特性，作者推荐使用
```

#### 常量

```java
public static Constants{
    public static final double c = 3.0;
    public static void main(String[] args)
    {
        final double a = 1.0;
        double b = 2.0;
    }
}
//关键字final表明变量 a 只能被赋值一次，一旦被赋值之后，就不能再修改变量a的值
//在Java中，如果希望某个常量可以在一个类中使用，称为类常量，可以使用关键字static final设置一个类常量，类常量的定义位于main方法的外部，同一类的其他方法也可以使用这个类常量
//而且，如果一个常量被声明为public，那么 其他类 的方法也可以使用这个常量。如这个c
```

**const 是java的关键字，但目前并没有使用（来定义常量），java中必须使用final定义常量**

#### 枚举类型

第五章详细介绍

```java
enum Size {Small,Medium,Large};
Size s = Size.Small;//可以声明Size类型的变量
//Size类型的变量只能储存这个枚举类型声明中给定的某个值，或者NULL值
```

### 运算符

#### 算术运算符

除法具有和C一样的性质，整数的除法会只保留整数部分，当且仅当分子分母都是整数，才表示整数的除法，否则表示浮点数除法

#### 数学函数与常量

数学类方法都在Math类中。

```java
double x = Math.sqrt(4.0);//x == 2.0,求平方根
double y = Math.pow(2.0,10.0);//y == 1024.0;求幂，方法有两个double参数，返回double值
//Math类还有常用三角函数，指数函数，对数函数，圆周率PI，e（2.71828）常量 E
```

#### 逻辑运算符，关系运算符

**和C基本上一模一样（&&  和||和  ？： 和，同样具有短路运算的特性**

### 字符串

Java类库提供了一个预定义类String，每个用双引号括起来的字符串都是String的一个实例

#### 子串

String类的substring方法可以从一个较大的字符串提取出一个子串

```java
String greeting = "Hello";
String s = greeting.substring(0,3);//substring（a，b） [a,b)位置，子串长度为b-a
//位置从0开始计数，3为不想复制的第一个位置，也就是复制0到3的字符，但是不取3位置上的字符
```

#### 拼接

java允许使用 + 号拼接两个字符串

```java
String a = "abcd";
String e = "efgh";
String p = a + e;//p == "abcdefgh"; +号按给定的次序拼接两个字符串，之间没有空格
int age = 13；
String str = "my age is ";
String st = str + age;
//st == "my age is 13";当将非字符串的值和字符串拼接在一起时，非字符串的值会转换成字符串
```

#### 不可变字符串

String类没有提供修改字符串中单个字符的方法，但是可以利用substring方法提取想要保留的子串，再与希望替换的字符拼接：

```java
//要将 greeting 从Hello变成Help！，不能单独把'l'和'o'替换成'p' 和'!'
String greeting = "Hello";
greeting = greeting.substring(0,3) + "p!";
//greeting == "Help!"
```

#### 检测字符串是否相等

```java
//可以用equals方法检测两个字符串是否相等
string1.equals(string2);
"hello".equals(greeting);
//检测string1和string2是否相等，string1和string2可以是字符串变量，也可以是字符串常量
//如果要检测两个字符串是否相等且忽略大小写，使用equalsIgnoreCase方法
"hello".equalsIgnoreCase("Hello");
```

**一定不要使用 ==来检测两个字符串是否相等，这个字符串只能确定两个字符串是否存放在同一个位置（在肯定相等）**

**只有字符串字面量是共享的，+ 和substring等操作得到的字符串并不共享，所以别用 ==**

#### 空串与Null

空串是长度为0的字符串,null值表示目前没有对象与该变量关联

```java
//检测是否为空串
if(str.length() == 0)或者if(str.equals("");
//检测是否为null
if(str == null);
//检查是否既不是null也不是空串
if(str != null && str.length() != 0)//首先检查是否为null值
```

#### String类的API

```java
boolean empty()
boolean blank()//如果字符串为空或者由空格组成，返回true
boolean startsWith(String prefix);//判断字符串是否是以字符串prefix开头，是则返回true
boolean endsWith(String suffix);;//判断字符串是否是以字符串suffix开头
int length();//返回字符串代码单元个数
String toLowerCase();//返回一个新字符串，将原始字符串的大写字母改为小写字母
String toUpperCase();//返回一个新字符串，将原始字符串小写字母改为大写字母
String repeat(int count);//返回一个字符串，将当前字符串重复count次
```

#### 构建字符串

由较短的字符串构建字符串，使用StringBuilder类

```java
// 用许多小段字符串构建一个字符串，如下步骤
StringBuilder builder = new StringBuilder();//构建一个空的字符串构建器
//每当需要添加一部分内容，调用append方法
builder.append(ch);//添加一个字符
builder.append(str);//添加一个字符串
//在字符串构建完成时调用toString方法，得到一个String对象，其中包含了所要的字符串
String completedString = builder.toString();
```

### 输入与输出

#### 读取输入

```java
//控制台输入
Scanner in = new Scanner(System.in);//构造一个与System.in标准输入流关联的Scanner对象
//然后就可以使用Scanner类的方法来读取输入了,如nextline()读取一行输入
System.out.println("What is your name?");
String name = in.nextline();
//只想读取一个“单词”（空格分割）用next()方法，读取一个整数nextInt()方法，浮点数nextDuble()
int a = in.nextInt(); double b = in.nextDouble();
//java.util.Scanner API
boolen hasNext();//检测输入中是否还有其他单词，还有类似的hasNextInt()，hasNextDouble()
Scanner(InputStream in);//用给定输入流创建一个Scanner对象
```

还介绍了读取密码，不用Scanner类,用Console类

#### 格式化输出

```java
//java沿用了C中的printf()方法，用法和C一样
System.out.printf()
```

这一部分细节，需要的时候再看

#### 文件输入与输出

留白，暂时用不到

### 控制流程

分支循环之类的，语法和C不能说十分相似，简直就是一模一样

唯一有些不同的是break关键字和C的用法不一样，break关键字多了可以跳出至标签处的用法，和goto有一些相似

switch case语句不用也罢，没什么用

### 大数

当基本的整数和浮点数的精度不够用，使用java.math包中的两个类：BigInteger和BigDecimal

```java
//使用静态valueOf方法将普通数值转换为大数
BigInteger a = BigInteger.valueOf(100);
//对于更大的数，可以使用一个带字符串参数的构造器
BigInteger reallyBig = new BigInteger("99999999999999999999999999999999999");
//不能使用普通的+-*/算术运算符处理大数类,只能用add和multiply和divide和subtract方法
BigInteger c = a.add(b);//使用add()方法求a + b
BigInteger d = c.multiply(b.add(BigInteger.valueOf(2)))//d = c * (b + 2)
大数的比较也不能用 == < >,都有相应的方法
```

### 数组

数组储存相同类型值的序列

#### 声明数组，访问数组

```java
//声明数组和C有一些小区别了，C中声明一个int型数组只需要 int a[100];就建立了一个100单元的int数组
int[] a = new int[100];//or var a = new int[100] 声明了一个100单元的int数组
int[] a;//只声明了变量
int[] b = new int[n]; //可变数组，长度为n，和C差不多，且数组一旦创建就不能再改变长度，也类似C
int[] c = {1,2,3,4,5};//这种情况下不仅不需要指定数组长度，连new都不需要
new int {2,3,4,5,};//甚至可以声明一个连名字都没有的匿名数组
GiveItaName = new int[] {2,3,4,5,};//可以使用这种语法重新初始化一个数组而无需创建新变量
//java支持长度为零的数组 int [] zero = {}或者int zero = new int[0]//为零
//访问数组元素和C一样，下标也都从0开始
//一个数字数组，所有元素都初始化为0，boolen数组则全为false。对象数组则全为null，表示没有存对象
a.length == 100;//要得到数组长度，只要arrayName.length
```

#### for each循环

```java
//专门用来依次处理数组中的每个元素。而不必考虑下标问题，其实就是遍历数组专用的简单循环
for(variable : collection)statement;//collection为数组的名字
//定义一个变量暂存数组的每个元素，并用这个变量取执行statement的操作
for(int i : c)
    System.out.printf("%d ",i);//打印数组c中的每一个数字
//variable将会遍历数组的每个元素而不是下标
//打印数组每一个元素的更简单的方法：Arrays类的toString()方法
System.out.println(Arrays.toString(C));//打印C的内容
```

#### 数组拷贝

```java
int[] a = {1,3,5,7};
int[] b = a;//此时a和b引用同一个数组
b[0] = 0;//此时a[0] == 0，修改的是同一个数组
//如果想要得到a的副本，用Arrays的copyOf()方法
int[] Copis_Of_a = Arrays.copyOf(a,a.length);
//两个参数，前一个是要拷贝的数组，后一个是新数组的长度,第二个参数可以比要拷贝的数组的长度长
int[] Copis_Of_a = Arrays.copyOf(a,a.length * 2);//这个特性常用来增加数组长度
//如果长度比a.length小，只拷贝前面能拷贝的部分
//Java中的[]运算符被预定义为会完成越界检查
//新数组长出来的部分会被初始化为零或者false或者null
```

#### 命令行参数

看看书就好

#### java.util.Arrays API

#### ![1611774704454](D:\Pictures Of Markdown\Java笔记\1611774704454.jpg)![1611774704462](D:\Pictures Of Markdown\Java笔记\1611774704462.jpg)

#### 多维数组

多维数组使用多个下标访问数组元素

##### 二维数组

```java
int[][] a = new int[3][4];
//也可以直接初始化而不用new
int[][] b = {{1,2,3},{4,5,6},{7,8,9},{10,11,12}};
//一层for each循环不能自动处理二维数组的每一个元素，它会循环处理每一行，这些行本身是一维数组，所以要处理二维数组每一个元素要嵌套两层for each，外层处理每一行，内层对每一行处理每一个
for(int[] i : a){//注意这里不是int i，而是int[] i
    for(int j : i);
    statements;
}
```

##### 不规则数组

不想看了暂时，跳过去吧

## 第四章 对象与类

### 面向对象程序设计概述

#### 类

类是构造对象的蓝图或模板，由类构造对象的过程称为创建类的实例，用Java编写的代码都位于某个类里面

封装：从形式上看就是将数据和行为组合在一个包中，并对对象的使用者隐藏具体实现方法

实例字段:对象中的数据，操作数据的过程称为方法

状态：作为一个类的实例，特定对象有特定的实例字段值，这些值的集合就是这个对象的状态

可以通过扩展其他类来构建新类，新类具有被扩展的类的全部属性和方法，只需要提供适用于新类的新方法和数据字段就可以，这称为继承

#### 对象

清楚对象的三个特性：

1.对象的行为——可以对对象完成哪些操作，或者可以对对象应用哪些方法

2.对象的状态——当调用方法时，对象会如何响应

3.对象的标识——如何区分具有相同行为与状态的不同对象

对象状态的改变必须且只能通过调用方法实现，否则就是封装性被破坏

对象的状态并不能完全描述一个对象，每个对象都有唯一的标识，即使状态行为完全相同，也是不同的对象，例如两个货物属性完全相同的订单，也是不同的订单

#### 类之间的关系

依赖：没什么解释的，字面意思，要尽量减少类与类之间的依赖关系

聚合：感觉没什么用的关系

继承：不想说什么

### 使用预定义类

#### 对象与对象变量

要想使用对象，首先必须构造对象，并指定其初始状态，然后对对象使用方法

```java
//使用构造函数（构造器）构造一个对象（实例），构造器是一个用来构造并初始化对象的方法
//构造器的名字与类名相同，Scanner类的构造器就是Scanner()
//以Date（日期）类为例，要想构造一个Date类的对象，要在构造器前加new操作符
new Date();//构造了一个Date对象并被初始化为当前日期时间
System.out.println(new Date);//可以把这个对象传递给一个方法
String s =  new Date().toString();//可以对新构造的对象应用一个方法
Date d = new Date();//将新构造的对象存放在一个对象变量中，这样这个对象就可以多次使用
Date D;//这只定义了一个对象变量，可以用来引用一个Date类的对象，但D不是一个对象，它还没有引用任何对象，此时还不能在这个变量上使用Date类的方法
String S = D.toString();//编译都过不了，D没有引用任何对象
D = d；String S = D.toString();//先引用了new Date(),这样可以
//对象变量并没有实际包含一个对象，它只是引用了一个对象，类似C的指针
//在java中，任何对象变量都是对储存在另外一个地方的对象的引用，new 操作符的返回值也是一个引用
Date deadline = new Date();//new的返回值给了deadline，返回值类似C的指针，new构造了一个对象并把对象的地址传给了deadline（并将引用储存在deadline）
//可以显式地将对象变量的设置为null，表示这个对象变量没有引用任何对象
```

#### Java类库中的LocalDate类

```java
//不要使用new构造符和构造器来构造这个类的对象，使用静态工厂方法
LocalDate.now();
```

#### 更改器方法与访问器方法

更改器方法会更改调用这个方法的对象，而访问器方法只会访问调用这个方法的对象，而不会更改这个对象，一个对象在调用访问器方法的前后其没有发生更改

更改器方法会更改调用这个方法的对象并将其存在对象变量中，而访问器方法则会生成一个新的对象，这个对象是调用这个方法的对象经过方法的操作之后的版本，并将这个新的对象存在对象变量中

### 用户自定义类

```java
//最简单的类定义形式
class ClassName{
    field1;
    field2;
    construnctor;
    method;
}
```

```java
//一个源文件中只能有一个公共类，可以有任意数目的非公共类，文件名字和公共类名字相同
import java.time.LocalDate;
//不要把Employee类放在公共类里面，注意检查公共类最外层的花括号有没有把Employee类囊括
public class EmployeeTest {
    public static void main(String[] args) {
        Employee[] staff = new Employee[3];
        staff[0] = new Employee("carl Cracker", 75000, 1987, 12, 15);//构造器必须结合new运算符使用
        staff[1] = new Employee("Harry Hacker", 50000, 1989, 10, 1);
        staff[2] = new Employee("Tony Tester", 40000, 1990, 3, 15);
        for (Employee e : staff) {
            e.raiseSalary(5);
        }
        for (Employee e : staff) {
            System.out.println("name = " + e.getName() + ",salary = " + e.getSalary() + ",HireDay = " + e.getHireDay());
        }
    }
}//报错不能在静态环境下引用非静态变量，多半是这个花括号把Employee类括进去了
    class Employee {
        private String name;//private关键字确保只有Employee类内的方法可以访问这些数据，其他类的方法不可以
        private double salary;
        private LocalDate hireDay;
//注：可以用public关键字标记实例字段，但这将会导致程序中任何类的任何方法都可以对这些实例字段进行访问和修改，破坏了封装
//所以类内的实例字段标记为private
        public Employee(String n, double s, int year, int month, int day) {
            name = n;
            salary = s;
            hireDay = LocalDate.of(year, month, day);
        }//构造器，构造器的名字和类名相同，在构造Employee类对象时，构造器会将实例字段初始化
        //不能用一个已经构造好的对象调用构造器以重新构造这个对象
//public关键字意味着任何类的任何方法都可以调用这些方法
//每个类可以有一个以上构造器，构造器可以有0个或多个参数，构造器没有返回值     
        public String getName() {
            return name;
        }

        public double getSalary() {
            return salary;
        }

        public LocalDate getHireDay() {
            return hireDay;
        }

        public void raiseSalary(double byPercent) {
            double raise = salary * byPercent / 100;
            salary += raise;
        }
    }
////////////////////////////不！要！写！出！这！样！的！构！造！器！：
        public Employee(String n, double s, int year, int month, int day) {
            String name = n;
            double salary = s;
            LocalDate hireDay = LocalDate.of(year, month, day);
        }
//在构造器内部定义了与实例字段同名的局部变量，这将导致实例字段无法被初始化为传入的值，传入的值被赋给了与实例字段同名的局部变量
```

#### 使用var关键字声明局部变量

如果可以从局部变量的初始值推导出其类型，那么就可以用var关键字声明局部变量，**var关键字只能用于方法中的局部变量，参数和字段的类型必须声明**

#### 使用null引用

```java
//如果对null值使用一个方法，会产生 NullPointerException异常
LocalDate birthday = null;
String s = birthday.toString();//NullPointerException
```

#### 隐式参数与显式参数

```java
//object为隐式参数，parameters为显式参数
object.method(parameters)；
```

#### 基于类的访问权限

方法可以访问调用这个方法的对象的私有数据，**一个方法可以访问所属类的的所有对象的私有数据，不仅仅是调用这个方法的，只要是这个类的对象，这个方法都可以访问它们的私有数据**

#### 私有方法

实现私有化方法只需要将方法的public关键字改为private，如果一个方法是私有的，那么其他类就不能使用，可以将其删去，如果一个方法是公共的，由于不能确定别处是否调用了这个方法，就不能轻易删去

#### final实例字段

可以将类的实例字段定义为final，这样的字段必须在构造对象是就初始化，并且以后不能再修改

将对象变量定义为final表示这个对象变量的值被初始化后就不能再被修改，这个对象变量不能再引用另外一个同类的对象

### 静态字段与静态方法

```java
//statci的含义是属于这个类而不属于任何类对象的变量或函数
```

#### 静态字段

```java
//来讨论一下main方法中的static修饰符，术语静态只是沿用了C++的叫法，无实际意义，有的面向对象语言称之为类字段
class example{
    private static int ID = 1;//ID为静态字段，它不属于任何对象，属于这个类，即使没有对象ID也存在，ID为所有这个类对象共享
    private String name;//name不是静态字段
}
//如果将一个字段定义为static，那么每个类只有一个这样的字段
//对于非静态字段，每个对象都有属于自己的副本
//对于上述example类，每一个example类的对象都有属于自己的name字段，但所有的对象都将共享同一个ID == 1
//静态字段不代表这个字段的值一旦初始化就不可以改变，ID是static的但不是final的，可以让ID ++
```

#### 静态常量

```java
public class Math{
    public static final PI = 3.14159....;//PI是一个静态常量，可以直接通过Math.PI来访问
}
//如果省略关键字static，PI就变成了Math类的实例字段，需要通过一个Math类的对象才能访问PI，并且每一个对象都有一个PI的副本
```

#### 静态方法

```java
//在Math类的pow方法就是静态方法,静态方法是不在对象上执行的方法，使用pow计算幂时，并没有使用任何Math对象
//静态方法不能访问非静态字段，因为它不在对象上执行操作，但是静态方法可以访问静态字段
//调用静态方法一般使用类名而不是类的一个对象，但是使用对象也可以调用静态方法
//以下情况可以使用静态方法：
//1.方法不需要访问对象状态，因为它所需要的所有参数都从显式参数提供
//2.方法只需要访问类的静态字段
```

#### 工厂方法

```java
//留个坑
```

#### main方法

```java
//main方法不对任何对象进行操作，static
//每一个类可以有一个main方法，这是一个进行单元测试的技巧，只需要测试某一个类即可
```

### 方法参数

```java
//按值调用：方法接收的是调用者提供的值
//按引用调用：方法接收的是调用者提供的变量地址
//方法可以修改按引用调用的变量的值，不修改按值调用的变量的值
//java总是采用按值调用，方法得到的是所有参数值的一个副本，方法不能修改传递给它的任何参数变量的内容
//对于java的两种数据类型，基本类型和对象引用类型，后者相当于C的指针
//对于一个参数是对象引用的方法，因为方法得到的值是对象引用的副本，这个副本和原来的引用都引用的是内存同一个位置的对象，所以可以修改
//总而言之，方法不能修改基本数据类型的参数，可以改变对象的状态，方法不能让一个对象变量参数引用一个新的对象
```

### 对象构造

java提供了多种编写构造器的机制

#### 重载

```java
//如果多个方法有相同的名字，不同的参数，就出现了重载
String s = new StringBuilder();//这是StringBuilder构造器
String st = new StringBuilder("another")//这也是StringBuilder构造器
//编译器用各个方法首部中的参数类型与特定的方法调用中所使用的值类型进行匹配，找到正确的方法，这个过程称为重载解析
//java允许重载任何方法，不仅仅是构造器,因此，要完整描述一个方法，必须指明其方法名以及参数类型，即其签名，返回类型不属于签名
```

#### 默认字段初始化

如果在构造器中没有对字段进行初始化，那么字段会被默认的初始化为0，false，null，这是局部变量与字段的重要区别，局部变量一定要初始化，但在类中字段没有被初始化，会被初始化为默认值

#### 无参数的构造器

```java
//由无参数构造器创建对象时，对象的状态会设置为适当的默认值
//如对employee类的构造器重载：有一个无参数构造器
        public Employee(String n, double s, int year, int month, int day) {
            name = n;
            salary = s;
            hireDay = LocalDate.of(year, month, day);
        }
        public Employee() {
            name = " ";
            salary = 0;
            hireDay = LocalDate.now();
        }//这个是无参数构造器，各个字段被设置为一定的默认值
//如果编写一个类是没有编写构造器，那么默认给你提供一个无参数构造器，这个构造器将所有的实例字段设置为0，null，false等默认值
//如果类中提供了至少一个有参数的构造器，但是没有显式提供无参数构造器，那么构造对象时必须提供参数
//对于没有写无参数构造器的employee类，以下构造对象是不合法的：
Employee e = new Employee();//编译过不了
//仅当类内没有其他构造器的时候才会得到一个默认的无参数构造器
//如果写了一个有参数的构造器，但仍希望可以通过无参数构造器创建对象，那么需要加入无参数构造器的代码
//如果希望无参数构造器设置所有字段为默认值，只需要提供以下代码：
public ClassName(){
    
}
```

#### 显式字段初始化

```java
//可以在类定义中，执行构造器之前为任何字段赋值，如果一个类的所有构造器都希望把某个特定字段赋为某个值，这个语法省事
public class example{
    private int PI = 3.14;
    private int s；
    constructor1；
    constructor2；
}
//进行显式字段初始化时初始值不一定要是常量，譬如利用方法调用来初始化某个字段
public class example{
    private Date time = LocalDate.now();//将字段初始化为当前时间
}
```

#### 参数名

```java
//构造器中的this关键字，解决局部变量遮蔽类中同名的字段
public Employee(String name,double Salary){
    this.name = name;
    this.Salary = Salary;
}
```

#### 调用另一个构造器

#### 初始化块

```java
//除了在构造器中设置值和在声明中赋值以外的第三种初始化数据字段的方法

```

### 包

Java允许使用包将类组织在一个集合中，借助包可以方便的组织自己的代码

#### 包名

使用包的主要原因是确保类名的唯一性，如果有两个类具有相同的类名，把这两个类放在不同的包中就不会产生冲突

**从编译器的角度看，嵌套的包之间没有任何关系，每一个包都是独立的包**

#### 类的导入

```java
//一个类可以使用所属包中的所有类，以及其他包中的public class
//访问另一个包中公共类的两种方法
java.time.LcoalDate today = java.time.LocalDate.now()//使用完全限定名，包名.类名
//使用import语句：引用包中各个类的简洁方式，import语句应位于代码顶部（但位于package语句后面）
import java.time.*;//可以导入一个包(java.util)的所有类
import java.time.LocalDate;//或者导入包中一个特定的类，然后就可以直接 LocalDate today = LocalDate.now()
//只能用星号(*)导入一个包，不能import java.*.*导入以java为前缀的所有包
//如果导入的两个包之间有某两个类产生类名冲突，那就用完全限定名来解决冲突问题
```

#### 在包中增加类

```java
//要想将类放入包中，就必须将包的名字放在代码最顶部，即放在定义这个包中各个类的代码之前
package com.horsramnn.corejava;//如果没有这句package语句，源文件中所有类都属于无名包
public class Employee{
    .......
}
//将源文件放到与完整包名匹配的子目录中，如上述包的所有源文件就要放在com\horsramnn\corejava中，编译器将.class文件也放这里
```

#### 包访问

```java
//标记为public的部分可以由任意类使用
//标记为private的部分只能由定义它们的类使用/
//如果没有标记则可以被同一个包中的所有方法使用
```

#### 类路径，设置类路径，留个坑

### JAR文件 留个坑



### 文档注释留个坑

### 类设计技巧

```java
//保证数据的私有性，不要破坏封装性
//一定要对数据进行初始化，java不会为你初始化局部变量，会给实例字段默认初始化，但最好显式的初始化实例字段
//不要使用过多的基本类型，这个想法是指要用其他的类来替换相关的基本类型，这样会使类易于理解和修改
class example{
    private String street;
    private String city;
    private String state;
    private int zip;
}//使用一个Address类会比用这么多String更好，而且这么多实例字段还无法处理国外地址，而用一个Address类就方便很多
//不是所有的字段都需要单独的字段访问器和字段更改器，因为有的字段它初始化后就不需要修改
//分解有过多职责的类，将一个过于复杂的类分解成多个更简单的类
//类名和方法名要体现其职责
//一些命名传统：类名应该是名词，访问器方法用小写get开头（getSalary()），更改器方法用小写set开头（setSalary())
//优先使用不可变的类——没有方法可以更改类的对象
//更改对象的问题在于，如果发生多线程并发更改，结果不可预料
```

## 第五章：继承

### 类、超类、子类

#### 定义子类

```java
//使用关键字extends表示继承
public class Manager extends Employee//关键字extends表明新类派生于已存在的类
{
    (added methods and fields);//Employee称为父类或超类，Manager称为子类，子类的功能是多于父集的
    private double bonus;
    public void setBonue(double bonus)
    {
        this.bonus = bonus;
    }
}
//可以构建一个manager类对象，可以使用Employee类的所有方法以及setBonus方法，但Employee类的对象不能使用setBonus方法
Manager boss = new....
boss.getSalary() ；boss.getName();boss.raiseSalary(100);//可行
//通过扩展超类定义子类的时候，只用指出子类与超类的不同之处，因此设计类的时候要将更一般的方法放在超类中，特殊的放在子类中
```

#### 覆盖方法

```java
public class Manager extends Employee{
    (added methods and fields);
    private double bonus;
    public void setBonue(double bonus)
    {
        this.bonus = bonus;
    }
}
//如果需要得到manager的salary，实际上是要得到salary+bonus的和，但是getSalary()只能返回salary，无法返回和
//所以引入另外一个getSalary方法，它调用超类的getSalary()方法并返回salary和bonus之和
public class Manager extends Employee{
    (added methods and fields);
    private double bonus;
    public void setBonue(double bonus)
    {
        this.bonus = bonus;
    }
    public double getSalary()
    {
       var baseSalary =  super.getSalary();//super关键指示编译器调用超类的getSalary()方法，而不是调用子类的方法
       return bonus + baseSalary；
    }
}//如果没有super关键字编译器将会无限次调用Manager的getSalary()方法
//继承是绝对不会删除方法或字段的，只会增加字段或方法，或覆盖超类的方法
```

#### 子类构造器

```java
//提供一个构造器
public Manager(String name,double salary,int year,int month,int day)
{
    super(name,salary,year,month,day);//调用超类具有相应参数列表的构造器
    bonus = 0
}
//由于manager类不能访问employee类的私有字段，所以必须通过employee类的构造器来初始化这些私有字段
//对超类构造器的调用必须是子类构造器的第一条语句
//如果子类的构造器没有显式调用超类构造器，将自动调用超类的无参数构造器，如果超类没有无参数构造器，将会报错
```

#### 多态

```java
//程序中出现超类对象的任何地方都可以用子类对象替换，例如可以用子类对象给超类对象变量赋值
Employee e = new Manager(......);//对象变量是多态的，一个超类对象变量可以引用其任何一个子类的对象
//反之则不行，不能将一个超类对象的引用赋给一个子类对象变量
//可以将一个子类的数组转换成超类数组
```

