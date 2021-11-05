# Data Structure Lists

## projet1A

## IntList

```java
//最基本的
public class IntList {
    public int first;
    public IntList rest;        
//构造器与字段
    public IntList(int f, IntList r) {
        first = f;
        rest = r;
    }
//递归求链表的长度size
    public int size() {
    if (rest == null) {
        return 1;
    }//边界情况
 return 1 + this.rest.size();//该节点加上剩下节点的size
   }
//迭代求size	
    public int iterativeSize() {
    IntList p = this;
    int totalSize = 0;
    while (p != null) {
        totalSize += 1;
        p = p.rest;
    }
    return totalSize;
    }
}
//缺一个使用递归得到第 i 个数字的方法get(int i);

```

## SLList: single-linked list

```java
//相对于IntList有一定的提升，IntList是裸露的数据结构
//会有指针的表现
//SLList则包裹住了指针，不至于在使用数据结构时裸露出指针来
//第一步修改：由IntList变成IntNode类
public class IntNode {
    public int item;
    public IntNode next;
    public IntNode(int i, IntNode n) {
        item = i;
        next = n;
    }
}
//第二步修改：
//单IntNodes类难用，再建一个SLList类
//这个才是用户将会直接交互的类
public class SLList {
    public IntNode first;//头指针，不是每个node都有的
    public SLList(int x) {
        first = new IntNode(x, null);
    }//构造器，构造第一个对象，也即第一个节点
    //对比IntList类，建立只有一个节点的list更容易
    //这个构造器隐藏了null指针的细节，参数只剩一个x
    /** Adds an item to the front of the list. */
    public void addFirst(int x) {
        first = new IntNode(x, first);
//first指向新的第一个节点，新节点的next指向老的第一个节点
    }
    /** Retrieves the front item from the list. */
    public int getFirst() {
    return first.item;
    }
}
/*比较：SLList L = new SLList(15);
L.addFirst(10);
L.addFirst(5);
int x = L.getFirst();
和：IntList L = new IntList(15, null);
L = new IntList(10, L);
L = new IntList(5, L);
int x = L.first;**/
//SLList比IntList好用一些，指针什么的都封装起来了，更符合IntList的使用习惯，只出现Int，不出现reference

//第三步提升，public VS private
//因为我们可以直接绕过SLlist类，对裸露的IntList操作
//譬如先建立起一个IntList，然后知道first
//然后Intnode p = first，p = p.next；
//用p可以直接操控任意一个Intnode
//实例：
SLList L = new SLList(15);
L.addFirst(10);
L.first.next.next = L.first.next;
//所以为了避免外界访问到first从而绕过SLList类，使用private
//第四步，内嵌IntNode类，代码更有条理，节省一丢丢内存
//最终版本的SLList
public class SLList {
    public class IntNode {
        public int item;
        public IntNode next;
        public IntNode(int i, IntNode n) {
                item = i;
                next = n;
            }//可以把IntNode声明为static，这样IntNode内的方法无法访问外周方法或是字段
//if you don't use any instance members of the outer class, make the nested class static.
       }
       private IntNode first; 
       public SLList(int x) {
           first = new IntNode(x, null);
       }
//addLast()方法，比较简单，递归实现不会
    /** Adds an item to the end of the list. */
public void addLast(int x) {
    IntNode p = first;

    /* Advance p to the end of the list. */
    while (p.next != null) {
        p = p.next;
    }
    p.next = new IntNode(x, null);
}
//size()方法，递归实现，迭代实现和addLast()差不多
    /** Returns the size of the list starting at IntNode p. */
//实际的实现是有一个helper方法，与底层的裸露数据结构交互
//因此下述方法声明为private
private static int size(IntNode p) {
    if (p.next == null) {
        return 1;
    }

    return 1 + size(p.next);
}
//size()方法本体，方法的重载
    public int size() {
    return size(first);
}
//又一个改进，size()方法替换为一个size变量，直接记录节点个数
public class SLList {
    ... /* IntNode declaration omitted. */
    private IntNode first;
    private int size;//这个叫缓存，caching

    public SLList(int x) {
        first = new IntNode(x, null);
        size = 1;
    }

    public void addFirst(int x) {
        first = new IntNode(x, first);
        size += 1;
    }

    public int size() {
        return size;
    }
}
//创建一个空list，最自然的想法
    public SLList() {
    first = null;
    size = 0;
}
    //但这样导致我们在addLast()上会有一个if排除空表的情况
    public void addLast(int x) {
    size += 1;

    if (first == null) {
        first = new IntNode(x, null);
        return;
    }

    IntNode p = first;
    while (p.next != null) {
        p = p.next;
    }

    p.next = new IntNode(x, null);
}
//所以添加一个哨兵节点，这样就不会有if
    public void addLast(int x) {
    size += 1;
    IntNode p = sentinel;
    while (p.next != null) {
        p = p.next;
    }

    p.next = new IntNode(x, null);
}
//最终版本的SLList
    public class SLList {
    public class IntNode {
        public int item;
        public IntNode next;
        public IntNode(int i, IntNode n) {
                item = i;
                next = n;
            }
       }
       private IntNode sentinel;
       private int size = 0;
       public SLList(int x) {
           sentinel = new IntNode(x, null);
           ;//这个x可以随便是什么
       }
public void addFirst(int x){
     sentinel.next = new Intnode(x,sentinel.next);
    size = size + 1;
        }
        public void addLast(int x){
            Intnode p = sentinel;
            while(p != null){
                p = p.next;
            }
            p = new Intnode(x,null);
            size = size + 1;
        }
     //最终用户只需要关注自己想加什么数，而不需要考虑指针问题，但是底层实现的时候肯定是要的，这样实现就是为了在最终使用的时候只需要调用方法考虑加什么数
        public int size(){
            //迭代的size，
            int totalSize = 0;
            Intnode p = sentinel；
                while(p != null){
                    p = p.next;
                    totalSize ++;
                }
            return totalSize
        }
        public int size(){
            //caching size
            return size;
        }
        public int size(){
            //递归的size(),且没有采用helper method的形式
            if(sentinel.next == null){
                return 1;
            }
            else
                return 1 + size(sentinel.next);
        }//不管写不写成helper method的形式，最后在调用者那里看来表现形式是一样的
```

## DLList：double-linked list

```java
//考虑SLList的addLast()
        public void addLast(int x){
            Intnode p = sentinel;
            while(p != null){
                p = p.next;
            }
            p = new Intnode(x,null);
            size = size + 1;
        }
//问题在于它可能会很慢(线性时间),当list很长的时候，时间很长
//一个解决方法是添加一个末尾指针last
public class SLList {
    private IntNode sentinel;
    private IntNode last;
    private int size;    

    public void addLast(int x) {
        last.next = new IntNode(x, null);
        last = last.next;
        size += 1;
    }
    ...
}
//有了last指针后，addLast()和getLast()都会变快
//但removeLast()不会
//removeLast()要使last指向倒数第二个节点，而我们不知道他在哪里
//所以这不是最终的解决办法
//最自然的解决办法之一，添加一个previous指针，doubly-linked
//IntNode新定义如下
public class SLList {
    private IntNode sentinel;
    private IntNode last;
    private int size;    
    public class IntNode {
        public IntNode prev;
        public int item;
        public IntNode next;
    }
}
//但这样会有一个问题，last可能会指向哨兵节点，在add和remove的时候就可能要一个if判断last有没有指向哨兵节点
//所以进一步改进，可以在末尾也添加一个哨兵节点
//在projet1A试验一下循环链表和双向链表
//Generic DLLists；泛型List
//刚才实现的list只能储存整数，而不能储存其他的
//但我们又不想每次都重新实现一次，希望可以找个模板
//因此引入Generic DLLists，语法如下
public class DLList<BleepBlorp> {
    private IntNode sentinel;
    private int size;

    public class IntNode {
        public IntNode prev;
        public BleepBlorp item;
        public IntNode next;
        ...
    }
    ...
}
DLList<String> d2 = new DLList<>("hello");
d2.addLast("world");
//基本类型要用其引用类型替代
//在实现数据结构的.java文件中，仅在类名之后的文件顶部仅指定一次通用类型名。
//在其他使用您的数据结构的.java文件中，在声明期间指定特定的所需类型，并在实例化期间使用空的菱形运算符。
//如果您需要实例化一个基本类型的数据结构，使用Integer，Double，Character，Boolean，Long，Short，Byte，或Float代替它们的原始等价物
DLList<Integer> d1 = new DLList<Integer>(5);
```

## AList:Array-based list

```java
//System.arraycopy takes five parameters:
//The array to use as a source 
//Where to start in the source array 
//The array to use as a destination 
//Where to start in the destination array
//How many items to copy
//java只会在运行时检查下标是否越界，所以下列代码编译时不会出错
//只有在运行时会crash
int[] x = {9, 10, 11, 12, 13};
int[] y = new int[2];
int i = 0;
while (i < x.length) {
    y[i] = x[i];
    i += 1;
}
//使用数组来实现一个list，借助于[]的访问
//addLast() size() addFirst() get()等都很简单
//get()也没有DLList的查找第i个元素的线性时间的增长限制
//但是AList一个比较大的限制就是，大小是固定的不能扩展
//如何向已满的ALListaddLast()?
//建造一个大一个单元的数组，然后arraycopy然后再插入
//并让原指针指向新数组
//但最好的不是插入一个加一个，这样如果插入1000次那么耗时会很久
//更好的是倍数扩大
public void insertBack(int x) {
    if (size == items.length) {
           resize(size * RFACTOR);
    }
    items[size] = x;
    size += 1;
}
private void resize(Int x){
    Type[] copy = (Type[]) new Object[x];
    array.copy(copy,items.....);
    items = copy;
}
//防止空间过于浪费，引入使用率R = size / items.length
//R <= 0.25时减半数组长度
```

## Inheritance, Implements

### 引例

```java
//假如我们写了一个类 WordList 用来处理List的字符
//求最长字符串的函数
public static String longest(SLList<String> list) {
    int maxDex = 0;
    for (int i = 0; i < list.size(); i += 1) {
        String longestString = list.get(maxDex);
        String thisString = list.get(i);
        if (thisString.length() > longestString.length()) {
            maxDex = i;
        }
    }
    return list.get(maxDex);
}
//如果我们要把这个函数也应用于DLList呢？
//最笨的办法就是在这个类里copy一遍代码，然后把参数改一下
SLList<String> list ==>DLList<String> list
    //也即overload一下这个method
//这样做有几个问题
//代码的维护很麻烦，如果要修复一个bug那么要修复很多处代码
//代码太多重复的地方了，很不
//如果我们新增一个list类，那么又要去修改wordlist类的代码，再对longest方法进行对应的重载，对于每一个不同类的list，都要有一个属于它的longest方法在Wordlist类中
//所以我们引入了interface inheritance，接口继承
//superclass提供interface，subclass在自己的类实现中去实现每一个接口
```

### 上位词，下位词，接口继承

```java
//Step 1: Define a type for the general list hypernym -- we will choose the name List61B
public interface List61B<Item> {
    public void addFirst(Item x);
    public void add Last(Item y);
    public Item getFirst();
    public Item getLast();
    public Item removeLast();
    public Item get(int i);
    public void insert(Item x, int position);
    public int size();
}
//使用关键词interface，而不是class，创造了SLList和DLList的上位概念List61B，名字是随便取的
//只记录了所有的list的what to do，但没有how to do
//Step 2: Specify that SLList and AList are hyponyms of that type.
public class AList<Item> implements List61B<Item>
//使用关键字implements，表明ALlist 和DLList是List61B的下位，也即ALList和DLList会实现interface List61B中的所有运算
//而在subclassALList和DLList中具体实现interface的每一个方法how to do的过程叫做override，重写
@Override//这个tag可以避免拼写错误
public void addFirst(Item x) {
    insert(x, 0);
}
//接口继承指的是子类继承父类的所有方法之类的，在interface中没有具体实现的代码，只有父类的所有方法的签名
//具体的实现代码是subclass来做的，61BList提供方法，DLList和ALList提供具体实现
//这种继承也可以是多代的，如果一个subclass的superclass还有superclass，那么subclass将会继承从祖宗往下的所有接口
```

This inheritance is also **multi-generationa**l. This means if we have a long lineage of superclass/subclass relationships like in **Figure 4.1.1**, AList not only inherits the methods from List61B but also every other class above it all the way to the highest superclass AKA AList inherits from Collection.![subclass](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/subclass.png)

```java
//the code below runs
public static void main(String[] args) {
    List61B<String> someList2 = new SLList<String>();
    List61B<String> someList1 = new DLList<String>();
    someList.addFirst("elk");
}
```

### Implementation Inheritance

```java
// 在interface inheritance的继承上可以实现继承
//我们可以在interface list61b中告诉subclass的对象how to do
//we can write methods in List61B that already have their implementation filled out. These methods identify how hypernyms of List61B should behave.
//为了在superclass中实现how to do，需要在签名最前头使用关键字default
//Then everything that implements the List61B class can use the method below!
default public void print() {
    for (int i = 0; i < size(); i += 1) {
        System.out.print(get(i) + " ");
    }
    System.out.println();
}//每一个subclass的一个对象都可以调用这个方法
//但我们仍然可以在subclass中override已经在superclass中给出how to do的方法
@Override//在SLList中override
public void print() {
    for (Node p = sentinel.next; p != null; p = p.next) {
        System.out.print(p.item + " ");
    }
}//Now, whenever we call print() on an SLList, it will call this method instead of the one in List61B.
```

### Interface Inheritance vs Implementation Inheritance

- Interface inheritance (what): Simply tells what the subclasses should be able to do.
  - EX) all lists should be able to print themselves, how they do it is up to them.
- Implementation inheritance (how): Tells the subclasses how they should behave.
  - EX) Lists should print themselves exactly this way: by getting each element in order and then printing them.



## Extends

### 引例

如果我们要写一个新的类RotatingSLList，这个类和SLList有一样的功能，但不同的是它还有一个额外的方法rotateRight()用于把list最末端的元素放至最前端

一个很简单的方法就是把SLList类的代码复制粘贴然后再手动添上rotateRight的代码

但我们这样没有很好地利用java继承的特性

继承可以让子类再利用已定义类的代码

### extends

  

```java
//We can set up this inheritance relationship in the class header, using the extends keyword like so:
public class RotatingSLList<Item> extends SLList<Item>
{
    public void rotateRight() {
        Item x = removeLast();
        addFirst(x);
    }
}
//RotatingSLList shares an "is-a" relationship SLList. 
//通过使用extends关键字，子类继承了父类以下成员：
//All instance and static variables
//All methods
//All nested classes
//需要注意的是构造器没有被继承，并且private成员无法直接被子类访问到
//
```

### VengefulSLList

```java
//建立一个记录被removeLast()的items的list
public class VengefulSLList<Item> extends SLList<Item> {
    SLList<Item> deletedItems;

    public void printLostItems() {
        deletedItems.print();
    }
}
//VengefulSLList内的removeLast()方法应该做和SLList中的removeLast()一样的事，只不过在删除最后一个元素的同时要把这个元素添加到deletedItems中去
//为了再利用SLList中的代码，我们可以在VengefulSLList中重写removeLast()方法以使其符合我们的需求
public class VengefulSLList<Item> extends SLList<Item> {
    SLList<Item> deletedItems;

    public VengefulSLList() {
        deletedItems = new SLList<Item>();
    }

    @Override
    public Item removeLast() {
        Item x = super.removeLast();
//使用super来调用定义在父类SLList中的removeLast()
        deletedItems.addLast(x);
        return x;
    }
//super在调用removeLast时删除的是谁的最后一个元素？？

```

### Constructors Are Not Inherited

子类继承了父类的实例变量和静态变量，方法，内嵌类，**但是没有继承父类的构造器**

While constructors are not inherited, Java requires that all constructors **must start with a call to one of its superclass's constructors**.

```java
//java要求子类的构造器都必须最先调用父类的某个构造器
//以下是更深的解释
//假如我们有两个类
public class Human {...}
public class TA extends Human {...}
//TA extends Human是很符合逻辑的，因为每一个TA都是一个人
//如果我们运行下列代码：
TA Christine = new TA();
//首先，一个human必须先被创建，然后这个human再被给予TA的属性和行为
//Then first, a Human must be created. Then, that Human can be given the qualities of a TA. It doesn't make sense for a TA to be constructed without first creating a Human first.
//所以我们可以显式的在子类构造器中先调用父类某个构造器
//使用super关键字
public VengefulSLList() {
    super();
    deletedItems = new SLList<Item>();
}
//如果我们没有显式super(),java默认调用父类中的无参数构造器
//在上面这个构造器中，是否加入spuer()都没有区别
//但如果我们在VengefulSLList中写另外一个构造器
//隐式的调用父类构造器可能就不是我们想要的
//if we were to define another constructor in VengefulSLList, Java's implicit call may not be what we intend to call.
public VengefulSLList(Item x) {
    super(x);
    deletedItems = new SLList<Item>();
}
```

### The Object Class

Every class in Java is a descendant of the Object class, or `extends` the Object class. Even classes that do not have an explicit `extends` in their class still *implicitly* extend the Object class.

For example,

- VengefulSLList `extends` SLList explicitly in its class declaration
- SLList `extends` Object implicitly

This means that since SLList inherits all members of Object, VengefulSLList inherits all members of SLList *and* Object, transitively. So, what is to be inherited from Object?

As seen in the [documentation for the Object class](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html), the Object class provides operations that every Object should be able to do - like `.equals(Object obj)`, `.hashCode()`, and `toString()`.

### How Inheritance Breaks Encapsulation

假如我们有以下方法：

```java
public void bark() {
    System.out.println("bark");
}

public void barkMany(int N) {
    for (int i = 0; i < N; i += 1) {
        bark();
    }
}
```

以及以下方法：

```java
public void bark() {
    barkMany(1);
}

public void barkMany(int N) {
    for (int i = 0; i < N; i += 1) {
        System.out.println("bark");
    }
}
```

从使用者的视角来看，这两种实现是完全相同的

但是，如果我们创建一个Dog的子类verboseDog，并且重写barkMany()方法

```java
@Override
public void barkMany(int N) {
    System.out.println("As a dog, I say: ");
    for (int i = 0; i < N; i += 1) {
        bark();
    }
}
//Given a VerboseDog vd, what would vd.barkMany(3) output, given the first implementation above? The second implementation?
//a: As a dog, I say: bark bark bark
//b: bark bark bark
//c: Something else
```

### Type Checking and Casting

 For each line of code below, decide the following:

- Does that line cause a compilation error?
- Which method uses dynamic selection?

![dynamic_selection](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751030.png)

```java
VengefulSLList<Integer> vsl = new VengefulSLList<Integer>(9);
SLList<Integer> sl = vsl;
//能够编译，因为VengefulSLList is a SLList，所以s1可以被赋予vs1的值
sl.addLast(50);
//VengefulSLList没有重写这个方法，所以调用SLList中的方法
sl.removeLast();
//removeLast()被重写了，所以我们看一眼s1的动态类型
//s1的动态类型是VengefulSLList，所以调用VengefulSLList中的removeLast方法
sl.printLostItems();
//这句话造成一个编译时错误
//记住编译器是基于静态类型来确定某行代码是否是合法的
//s1的静态类型是SLList
//但SLList类中没有定义printLostItems()方法
//所以就算s1的动态类型是VengefulSLList
//代码也无法编译
VengefulSLList<Integer> vsl2 = sl;
//与上行代码类似的原因，s1的静态类型是SLList
//vsl2的静态类型是VSlist，所以编译器不允许这样的赋值
//In general, the compiler only allows method calls and assignments based on compile-time types.
```

### Expressions

**Like variables as seen above, expressions using the `new` keyword also have compile-time types.**

```java
SLList<Integer> sl = new VengefulSLList<Integer>();
//可以赋值，VengefulSLList is a SLList
VengefulSLList<Integer> vsl = new SLList<Integer>();
//编译错误，静态类型错误
//方法调用也有静态类型，等于其声明的返回类型
public static Dog maxDog(Dog d1, Dog d2) { ... }
Poodle frank = new Poodle("Frank", 5);
Poodle frankJr = new Poodle("Frank Jr.", 15);
Dog largerDog = maxDog(frank, frankJr);
Poodle largerPoodle = maxDog(frank, frankJr); //does not compile! RHS has compile-time type Dog
```

### Casting

为了解决上述由于编译器只看静态类型而导致的赋值和调用的编译错误 引入casting

```java
//类型转换
Poodle largerPoodle = (Poodle) maxDog(frank, frankJr); 
// compiles! Right hand side has compile-time type Poodle after casting
//casting本质上是告诉编译器不要去做静态类型检查
//让编译器信任你并按你想让它去做的方式做
//所以有可能导致下面的问题
Poodle frank = new Poodle("Frank", 5);
Malamute frankSr = new Malamute("Frank Sr.", 100);
Poodle largerPoodle = (Poodle) maxDog(frank, frankSr); // runtime exception!
//函数返回的是更大的那只狗，也就是Malamute Dog，而我们试图把malamute转换成poodle来看待
//这两个类没有继承关系，不符合类型转换的定义
//出现ClassCastException
```

### Higher Order Functions高阶函数

一个高阶函数是一个将其他函数作为数据的函数

类似高中数学中常见的函数符号嵌套迭代 F(F(x))

```java
//借助接口继承来实现高阶函数
//先写一个interface
public interface IntUnaryFunction {
    int apply(int x);
}
//然后写一个implements上述interface的类，并写明how to do
public class TenX implements IntUnaryFunction {
    /* Returns ten times the argument. */
    public int apply(int x) {
        return 10 * x;
    }
}
public class HOFDemo{
    public static int do_twice(IntUnaryFunction f, int x) {
        return f.apply(f.apply(x));
    }
    public static void main(String [] args) {
        System.out.println(do_twice(new TenX(), 2));
    }
}


```

## 10Subtype Polymorphism（多态）

### 引例

假如我们想写一个方法用来比较大小，这个方法要适用于所有的对象，不管我们用什么可比较的对象调用这个方法，都可以返回正确的结果

我们可以使用HOF，参数表中有一个比较的函数和其他需要的函数

类似这样的

```python
def print_larger(x, y, compare, stringify):
    if compare(x, y):
        return stringify(x)
    return stringify(y)
```

或者是

我们想要写一个找出一系列同类型对象的最大值的方法，同样的这个方法也适用于所有对象

其实要解决这个问题，最显然的方法是每一个类写一个找出max的方法

但这和我们的想法就完全相反了，我们正是不想这样才试图写一个万能的方法

我们借助interface来实现

```java
//我们把比较的方法叫compareTo，接口叫做OurCompareable
public interface OurComparable {
    public int compareTo(Object o);
    //注意方法的参数这里是Object o
}
//定义compareTo的行为如下：
//返回-1,如果this更小，返回0如果相等，返回1如果this更大
//我们利用最先的Dog类来implement OurCompareable
public class Dog implements OurComparable {
    private String name;
    private int size;

    public Dog(String n, int s) {
        name = n;
        size = s;
    }

    public void bark() {
        System.out.println(name + " says: bark");
    }

    public int compareTo(Object o) {
        Dog uddaDog = (Dog) o;
        //注意上一行这里我们cast了o
        //因为不能直接用O调用size()
        if (this.size < uddaDog.size) {
            return -1;
        } else if (this.size == uddaDog.size) {
            return 0;
        }
        return 1;
    }
}
//现在我们可以写一个max()方法了
//但这个方法并不是万能的，它只能适用于implements了OurCompareable接口的类，接受这些类的对象来max
//因为我们可以保证这些类里都有一个CompareTo()方法
public static OurComparable max(OurComparable[] items) {
    int maxDex = 0;
    for (int i = 0; i < items.length; i += 1) {
        int cmp = items[i].compareTo(items[maxDex]);
        if (cmp > 0) {
            maxDex = i;
        }
    }
    return items[maxDex];
}
//可以compareTo()简化一下，不过不简化也行
//this更大返回正数，反之负数，再则0
public int compareTo(Object o) {
    Dog uddaDog = (Dog) o;
    return this.size - uddaDog.size;
}
//这样我们的max()方法就只需要写一次而可以适用于OurCompareable的任意一种对象
//java已经有实现这个功能的类Compareable
//compareable类使用泛型，避免了从object到其他类的cast
public class Dog implements Comparable<Dog> {
    ...
    public int compareTo(Dog uddaDog) {
        return this.size - uddaDog.size;
    }
}
```

## Comparator

这是另外一个interface，Comparable interface 使得一只狗可以把自己和另外一只狗比较，但如果我们要按狗名字的字典序来对狗进行排序呢？

Java's way of doing this is by using `Comparator`'s. Since a comparator is an object, the way we'll use `Comparator` is by writing a nested class inside Dog that implements the `Comparator` interface

```java
public interface Comparator<T> {
    int compare(T o1, T o2);
}//Comparator interface内的接口

import java.util.Comparator;

public class Dog implements Comparable<Dog> {
    ...
    public int compareTo(Dog uddaDog) {
        return this.size - uddaDog.size;
    }

    private static class NameComparator implements Comparator<Dog> {
        public int compare(Dog a, Dog b) {
            return a.name.compareTo(b.name);
        }
    }
//内嵌一个NameCompare implements Comparator<Dog> 类
    //字典序比较我们直接使用String类的方法
    public static Comparator<Dog> getNameComparator() {
        return new NameComparator();
    }
}
```

## Lists,Sets,ArraySet

使用java内置的List和Set数据结构，同时自己试着建立ArraySet

### 使用java内置的List

```java
//java提供的是List interface，所以不能去实例化一个List
//We must instatiate one of its implementations.
import java.util.List;
import java.util.ArrayList;

public class Example {
    public static void main(String[] args) {
        List<Integer> L = new ArrayList<>();
        //右边不能写成new List()哦
        L.add(5);
        L.add(10);
        System.out.println(L);
    }
}
```

### Sets

集合，和高中的集合概念一样，元素具有互异性，也没有顺序

```java
//java有Set interface以及一系列implements Set的不同的Class
//例如HashSet，ArraySet
//如果不想使用全名，记得import
import java.util.Set;//example
import java.util.HashSet;
Set<String> s = new HashSet<>();
s.add("Tokyo");
s.add("Lagos");
System.out.println(S.contains("Tokyo")); // true
```

### ArraySet

我们意图实现我们自己的集合，ArraySet

```java
//我们实现的ArraySet具有以下方法：
//add(value): 添加一个集合中没有的值
//contains(value): 确定某个值是否存在于集合中
//size(): 集合大小

public class ArraySet<T> {
    private T[] items;
    private int size;
 // the next item to be added will be at position size
    
    public ArraySet() {
        size = 0;
        items = (T[]) new Object[100];
    }

    /* Returns true if this map contains a mapping for the specified key.
     */
    public boolean contains(T x) {
        for(int i = 0;i < size ;i ++) {
            if(items[i].equals(x)) {
                //判断不要写成items[i] == x!!!!!!!!!!!
                return true;
            }
        }
        return false;
    }

    /* Associates the specified value with the specified key in this map. 
       Throws an IllegalArgumentException if the key is null. */
    public void add(T x) {
        if(contains(x)){
         return;   
        }
        else{
            items[size] = x;
            size ++;
        }
    }

    /* Returns the number of key-value mappings in this map. */
    public int size() {
        return size;
    }

    public static void main(String[] args) {
        ArraySet<String> s = new ArraySet<>();
        s.add(null);
        s.add("horse");
        s.add("fish");
        s.add("house");
        s.add("fish");        
        System.out.println(s.contains("horse"));        
        System.out.println(s.size());       
    }

    /* Also to do:
    1. Make ArraySet implement the Iterable<T> interface.
    2. Implement a toString method.
    3. Implement an equals() method.
    */
}
```

### Throwing Exceptions

上述实现的ArraySet有一个小小的bug,如果我们add一个null，会有NullPointerException

问题出在contains()方法，当我们试图加入null时，先检查是否存在null，在items[i].equals(x)，如果有null.equals(x),就会出错==》NullPointerException

```java
//异常使控制流终止，我们可以选择抛出异常，在java中异常是对象，我们使用如下格式抛出异常
throw new ExceptionObject(parameter1, ...)
//当用户试图加入null时我们抛出一个异常
// We'll throw an IllegalArgumentException which takes in one parameter (a String message).
/* Associates the specified value with the specified key in this map.
   Throws an IllegalArgumentException if the key is null. */
public void add(T x) {
    if (x == null) {
        throw new IllegalArgumentException("can't add null");
    }
    if (contains(x)) {
        return;
    }
    items[size] = x;
    size += 1;
}
//我们也可以干脆禁止add(null)
//也可以改变contains()单独处理items[i] == null的情况
```

### Iteration

```java
//We can use a clean enhanced for loop with Java's HashSet
//对于java内置的HashSet我们可以使用增强for循环
Set<String> s = new HashSet<>();
s.add("Tokyo");
s.add("Lagos");
for (String city : s) {
    System.out.println(city);
}
//但是我们手动实现的ArraySet是不支持增强型for循环的
//接下来就是提供ArraySet对增强型for循环的支持
//关键在于一个叫iterator的对象
public Iterator<E> iterator();
//使用这个对象去遍历所有的元素
List<Integer> friends = new ArrayList<Integer>();
...
Iterator<Integer> seer = friends.iterator();

while (seer.hasNext()) {
System.out.println(seer.next());
}
```

#### Implementing Iterators

这一部分是怎么实现增强for循环

```java
List<Integer> friends = new ArrayList<Integer>();
Iterator<Integer> seer = friends.iterator();

while(seer.hasNext()) {
    System.out.println(seer.next());
}
//为了实现增强型for循环，我们需要让编译器知道以下：
//List interface内有iterator方法，用于返回一个迭代器
//iterator interface内有hasNext()和next()方法
//实现第一个要求：
//List interface extends Iterable interface
public interface Iterable<T> {
    Iterator<T> iterator();
}
public interface List<T> extends Iterable<T>{
    ...
}
//实现第二个要求：
public interface Iterator<T> {
    boolean hasNext();
    T next();
}
```

```java
//为ArraySet实现增强型for循环：
private class ArraySetIterator implements Iterator<T> {
    private int wizPos;

    public ArraySetIterator() {
        wizPos = 0;
    }

    public boolean hasNext() {
        return wizPos < size;
    }

    public T next() {
        T returnItem = items[wizPos];
        wizPos += 1;
        return returnItem;
    }
}
```

以下是完整代码

```java
import java.util.Iterator;

public class ArraySet<T> implements Iterable<T> {
    private T[] items;
    private int size; // the next item to be added will be at position size

    public ArraySet() {
        items = (T[]) new Object[100];
        size = 0;
    }

    /* Returns true if this map contains a mapping for the specified key.
     */
    public boolean contains(T x) {
        for (int i = 0; i < size; i += 1) {
            if (items[i].equals(x)) {
                return true;
            }
        }
        return false;
    }

    /* Associates the specified value with the specified key in this map.
       Throws an IllegalArgumentException if the key is null. */
    public void add(T x) {
        if (x == null) {
            throw new IllegalArgumentException("can't add null");
        }
        if (contains(x)) {
            return;
        }
        items[size] = x;
        size += 1;
    }

    /* Returns the number of key-value mappings in this map. */
    public int size() {
        return size;
    }

    /** returns an iterator (a.k.a. seer) into ME */
    public Iterator<T> iterator() {
        return new ArraySetIterator();
    }
//ArraySet类要有一个iterator方法，返回一个iterator对象
//iterator对象要有hasNext和next方法
    private class ArraySetIterator implements Iterator<T> {
        private int wizPos;

        public ArraySetIterator() {
            wizPos = 0;
        }

        public boolean hasNext() {
            return wizPos < size;
        }

        public T next() {
            T returnItem = items[wizPos];
            wizPos += 1;
            return returnItem;
        }
    }

    public static void main(String[] args) {
        ArraySet<Integer> aset = new ArraySet<>();
        aset.add(5);
        aset.add(23);
        aset.add(42);

        //iteration
        for (int i : aset) {
            System.out.println(i);
        }
    }
//要使用这种形式，必须要声明ArraySet implements Iterable接口
}
//总结一下就是：
//ArraySet类要有一个Iterator方法，返回一个Iterator对象
//这个对象要有hasNext()方法和next()方法
//并且要声明ArraySet implements Iterable 接口，可以让编译器知道我们满足了要求一和二
```

### Object Methods

所有的类都有一个超类叫Object，有如下方法

- `String toString()`
- `boolean equals(Object obj)`
- `Class <?> getClass()`
- `int hashCode()`
- `protected Objectclone()`
- `protected void finalize()`
- `void notify()`
- `void notifyAll()`
- `void wait()`
- `void wait(long timeout)`
- `void wait(long timeout, int nanos)`

我们只给我们的ArraySet实现前两个

#### toString()

这个方法将ArraySet中的元素以字符形式打印出来

```java
//当我们使用System.out.println()的时候，它实际上先调用了toString(),然后把得到的字符串打印出来
//譬如dog类 有一行代码System.out.println(dog)，实际上是：
String s = dog.toString();
System.out.println(s);
//Object类中的默认toString方法打印的是对象的十六进制地址
//所以我们要在自己的类中重写toString方法
@Override
public String toString() {
    String returnString = "{";
    for (int i = 0; i < size; i += 1) {
        returnString += keys[i];
        returnString += ", ";
    }
    returnString += "}";
    return returnString;
}
//这种方法看上去很高效优雅，但是实际上效率不高
//因为用字符串拼接returnString += keys[i];的时候
//并不是在原有的字符基础上往后拼接
//而是创建一个完全的新的字符，也就是先创建一个原有字符的副本然后再拼接
//为了改进这个问题，java有一个StringBuilder类
Solution
public String toString() {
        StringBuilder returnSB = new StringBuilder("{");
        for (int i = 0; i < size - 1; i += 1) {
            returnSB.append(items[i].toString());
            returnSB.append(", ");
        }
        returnSB.append(items[size - 1]);
        returnSB.append("}");
        return returnSB.toString();
    }
```

#### equals()

equals()和 == 在java中是不一样的，==检查的是等号两边的东西值是否相等，对于基本类型，就是检查值是否相等,如果两侧都是对象变量，检查的实际上是两侧的地址是否相等，如果要确定两个对象是否相等（这种相等又有两种情况，一种是两个实际上是同一个对象，还有一种是两个独立的相同的对象），就要用equals()

```java
public boolean equals(Object other) {
        if (this == other) {
            return true;
        }//地址相等
        if (other == null) {
            return false;
        }
        if (other.getClass() != this.getClass()) {
            return false;
        }//检查类名是否相等
        ArraySet<T> o = (ArraySet<T>) other;
        if (o.size() != this.size()) {
            return false;
        }
        for (T item : this) {
            if (!o.contains(item)) {
                return false;
            }
        }
        return true;
    }
//写equals()时必须要满足的规则：
//反身性：x.equals(x) 为真
//对称性，x.equals(y)为真当且仅当y.equals(x)为真
//传递性，x.equals(y)为真且y.equals(z)为真，则x.equals(z)也为真
```

## Lecture 13 - Asymptotics 1

### ADTs

抽象数据结构是由其运算（行为）而不是其实现定义的高级类型

### 运行时间分析

在比较算法的时间复杂度时，我们进行一些简化

1.只考虑最坏的情况 

2.将注意力集中在某一种运算上，选择有代表性的运算操作作为模型

3.忽略最终结果表达式的常数项和低阶项，只保留最高阶项

4.忽略代数符号N前面的乘法系数，也即忽略最高阶项的系数

### 总结

给一段代码，我们可以将其的运行时间函数表达为一个函数R(N)

N通常是输入的大小

我们通常只关心R(N)的增长趋势，而不关系精确的R(N)的精确表达式

## Lecture 14 Disjointed Sets 并查集

### 简介

如果两个集合没有公共元素，则称这两个集合为不相交集，并查集用于跟踪固定数量的元素，这些元素被划分为多个不相交的集合

```java
/*并查集有两个运算**/
/1.connect(x,y),连接x和y，或者说把x和y所在的两个集合合并
 2.isConnected(x,y),判断x和y是否连接**/

 public interface DisjointSets {
    /** connects two items P and Q */
    void connect(int p, int q);

    /** checks to see if two items are connected */
    boolean isConnected(int p, int q); 
}
```

除了学习如何实现一个数据结构以外，我们将看到数据结构的实现是如何进化的，接下来将讨论并查集在变得令人满意的过程中的四次迭代：*Quick Find → Quick Union → Weighted Quick Union (WQU) → WQU with Path Compression*.

我们将看到对于如何实现一个数据结构的决策是怎样影响代码的复杂度和运行时间的复杂度

### 1.第一代Quick Find

#### ListOfSets

通过直觉我们可能会用链表去实现并查集，实现一个集合的链表（每一个节点的元素是一个集合）:List<Set<Integer>>

对于N = 6个元素，并且一开始没有任何元素相连，则这个集合的链表是[{0}, {1}, {2}, {3}, {4}, {5}, {6}]

如果要进行一次   connect(5,6)  ,那么必须要遍历整个链表才可以找到5，遍历第二次链表才能找到6，也即对N个元素，connect()的时间复杂度会是O(N),并且实现的代码也很复杂

isConnected()和构造器都是O(N)的时间复杂度

#### Quick FInd

另外一种使用整数型数组的实现方法：1.数组的下标代表了我们的元素  2.某一个下标所对应的**数组元素**的值代表了所在集合的编号

比如{0, 1, 2, 4}, {3, 5}, {6}这三个集合构成的并查集就可以表示为：

![9.2.1](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751084.png)

数组的下标是并查集的元素，而 id[i]  的值是下标所属的集合，当然这个集合编号是多少并不重要，只要保证在同一个集合的值共享同样的id的值

接下来考虑Connect()运算：connect(2,3)，在调用connect()之前，id[2] = 4, id[5] = 5, 在connect(2,3)以后，**所有和2有一样id的数（以及所有和3有一样id的元素，即所有id为4和id为5的数，都要被赋予相同的新id**，暂时都赋予id为5:

![9.2.2](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751118.png)

这个实现起来不难

而对于	isConnected(x  ,  y) 	,实现很简单，只需要检查id[x] == id[y]是否成立，这个函数只需要常数时间

我们称这个实现为Quick Find就是因为检查元素是否相连只需要常数时间

isConnected(x,y)为常数时间复杂度，而connect(x,y)和构造器都是O(N)

#### Quick FInd代码

·

```java
   public class QuickFindDS implements DisjointSets {

    private int[] id;

    /* Θ(N) */
       /*构造的是一开始N个数为N个集合的并查集*/
    public QuickFindDS(int N){
        id = new int[N];
        for (int i = 0; i < N; i++){
            id[i] = i;
        }
    }

    /* need to iterate through the array => Θ(N) */
    public void connect(int p, int q){
        int pid = id[p];
        int qid = id[q];
        /*将拥有pid的元素的id改为qid，使两个集合合并*/
        for (int i = 0; i < id.length; i++){
            if (id[i] == pid){
                id[i] = qid;
            }
        }
    }

    /* Θ(1) */
    public boolean isConnected(int p, int q){
        return (id[p] == id[q]);
    }
}
```

### 2.第二代Quick Union

我们优先考虑让	connect(x,y)	运算变快（也是为什么第二代叫quick union，因为connect快）

仍然使用数组来表示我们的并查集以及各个集合，下标仍然是并查集元素，但是我们不用id了，我们给每一个下标的数组元素赋予它的父元素的下标值，如果某一个下标没有父元素了，我们给对应数组元素一个负数，标记为根节点，这使得我们可以将集合表现为树的形式

对于{0, 1, 2, 4}, {3, 5}, {6}来说，id[0] = -1，0为根节点，id[1] = 0，0 是1的父节点

![9.3.1](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751155.png)

**注意我们使用数组来实现它，但我们把它看成一棵树**

#### connect()

对于Quick Union，我们定义一个helper method名为 find(int item)，这个方法返回item所在的树的root，对于上图的树，find(4) == 0，find(1) == 0，find(5) == 3

为了connect(x,y)两个元素x和y，我们先找到x和y的根节点，也即找到其各自所属的树的根节点，然后将其中一棵树变成另外一棵树的子树

例如：connect(5,2):

1.find(5) --> 3（5的根节点是3，parent[3] == -1）

2.find(2) -->0（2的根节点是0,parent[0] == -1）

3.将find(5)对应的节点的值设为find(2),也即：将parent[3] == -1 变为 parent[3] == 0，注意此时将两棵树，也即两个集合合并了

在最好的情况下，如果x和y恰好是两个根节点，那么只需要做第3步，常数时间 (Hence the name QuickUnion)

![9.3.2](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751190.png)

#### isConnected(x,y)

如果x和y是同一个集合的元素，那么x和y会在同一棵树上，因此x和y的根节点相同，所以只需要检查 find(x) == find(y)

#### 运行表现分析

Quick Union存在一个很大的潜在问题：树可能变的很长很长，在这种情况下要想找到某个节点的根节点就变的代价很大

![9.3.3](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751226.png)

在上图最坏的情况下我们需要从树的最底下往上找根节点，这需要O(N)的时间，也即find()的时间复杂度为O(N)，由于connect()和isConnected()都需要调用find，所以这两个方法的时间复杂度也为O(N)

#### 代码及总结

似乎看起来Quick Union的connect()和Quick Find的时间复杂度没什么区别，但是quick union的是在树很长的情况下才会出现O(N)的时间复杂度，所以我们下一代只需要解决	”如何保证树不变的很长“	的问题，而在树比较平衡的时候，connect()和isconnected()的表现都很优异

```java
public class QuickUnionDS implements DisjointSets {
    private int[] parent;

    public QuickUnionDS(int num) {
        parent = new int[num];
        for (int i = 0; i < num; i++) {
            parent[i] = -1;
        }
    }

    private int find(int p) {
        while (parent[p] >= 0) {
            p = parent[p];
        }
        return p;
    }

    @Override
    public void connect(int p, int q) {
        int i = find(p);
        int j= find(q);
        parent[i] = j;
    }

    @Override
    public boolean isConnected(int p, int q) {
        return find(p) == find(q);
    }
}
```

### 3.第三代Weighted Quick Union (WQU)

提升Quick Union的关键之处在于，每次调用find()的时候，我们都需要走到树的根节点去，所以树越短，速度越快

所以引入新的规则：**无论何时调用connec(x,y)，我们把较小的树的根连接到较大的那一颗树的根节点上去**

遵循上述规则会使得树的最高高度不超过Log(N)，N为树中的元素个数

**举个例子来说明：将下图T1和T2联合在一起**

![9.4.1](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751250.png)

**有两种选项：**第一种将T1连在T2上，第二种将T2连在T1的根上

![9.4.2](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751283.png)

第二种连法显然是更加优越的，因为其高度只有2，根据规则我们也会选择第二种，因为T2是一棵较小的树

划分树的大小的标准：以一棵树中元素的个数为参照物，个数多的树更大，所以在连接两棵树时，我们需要先知道两棵树的size(or weight)，我们可以将这个信息存储在根节点的那个原来放-1的地方，也即parent[root_index] = - (size of the tree) ，为什么不是依据高度而是依据size(weight)？因为代码更加复杂而得到的效果是一样的

#### 代码及总结

connect()和isConnected() 都是	O(logN)	的时间复杂度，构造器则是O(N) 

```java
//代码是lab6，待完成
```

### 4.第四代Weighted Quick Union with Path Compression

第三代其实已经很好了，但是我们可以做到更好

不放这么考虑：当我们调用find(x)，我们必须从x一路往上走到root节点，在这条路上我们会经过一些节点，而这些经过的节点的根节点和x是一样的，所以我们其实可以将这些经过的节点也直接与根节点相连，而这样可以大大缩小我们的树的高度

而connect(x,y)和isConnected(x,y)都会调用find()，因此在足够多次数的调用了connect()和isConnected()之后，基本上所有的节点都会直接与它们各自的根节点相连，这样下来，connect()和isConnected()的平均运行时间几乎趋近于常数

More specifically, for M operations on N elements, WQU with Path Compression is in O(N + M (lg* N)). lg* is the [iterated logarithm](https://en.wikipedia.org/wiki/Iterated_logarithm) which is less than 5 for any real-world input

#### 总结及代码

N: number of elements in Disjoint Set

| **Implementation**         | **isConnected** | **connect** |
| -------------------------- | --------------- | ----------- |
| Quick Find                 | Θ(N)            | Θ(1)        |
| Quick Union                | O(N)            | O(N)        |
| Weighted Quick Union (WQU) | O(log N)        | O(log N)    |
| WQU with Path Compression  | O(α(N))*        | O(α(N))*    |

*behaves as constant in long term.

```java
//lab6

```

## Lecture 15 - Asymptotics 2

### example1

```java
int N = A.length;
for (int i = 0; i < N; i += 1)
   for (int j = i + 1; j < N; j += 1)
      if (A[i] == A[j])
         return true;
return false;
//C=1+2+3+...+(N−3)+(N−2)+(N−1)=N(N−1)/2,θ(N^2)
```

### example2

### 3Binary Search

复杂度为Θ(⌊log2(*N*)⌋)=Θ(log*N*)

### 4Merge Sort，归并排序

最后一个例子是归并排序，个人认为是最有必要详细写的

#### Selection Sort

首先引入选择排序，这是归并排序的基石

选择排序通过以下两步工作：

1.找到未排序序列中最小的那一个，然后将它移到最前面去（和最前面的元素交换一下位置），然后固定这个最前面的位置的元素不变

2.继续在剩下的未排序且未固定的序列中使用1中的方法

选择排序的复杂度是  Θ(N^2)

#### arbitrary units of time

引入另外一个概念：任意单位时间

While the exact time something will take will depend on the machine, on the particular operations, etc., we can get a general sense of time through our arbitrary units (AU).

If we run an N=6 selection sort, and the runtime is order N^2*N*2, it will take ~36 AU to run. If N=64, it'll take ~2048 AU to run. Now we don't know if that's 2048 nanoseconds, or seconds, or years, but we can get a relative sense of the time needed for each size of N.

#### merging

现在再来讨论关于归并：

假如我们有两个已经排好序的数组（假设是升序），我们想把这两个数组合并成一个排好序的数组，我们可以先把一个数组拼接到另外一个数组的后面，然后再对整个大数组重新进行排序，但这样做没有充分利用两个数组都是分别已经排好序的条件，所以应该如何利用？

我们可以利用两个数组都是已经排好序的特点来更快的合并两个数组，每个序列各自最小的元素一定在每个序列的开头，我们比较它们

```java
//我觉得我写半天文字写不明白，还是写代码吧
int[] a = {1,3,5,7,9,};
int[] b = {2,4,6,8,10,};
int[] c = new int[10];
int i = 0,j = 0,p = 0;
/**比较过程，一直比较到某一个数组为空为止（某一个数组的所有元素都被放进去的c里面）**/
while((i < 5) &&(j < 5) ) {
    if(a[i] < b[j]){
        c[p] = a[i];
        p ++;
        i ++
    }
    else {
        c[p] = b[j];
        p ++;
        j ++
    }
}
/*再把非空数组剩下的没有放进去的数按顺序原封不动的放后面去**/
if(i == 5){
    c[p] = b[j];
}
else if( j == 5) {
    c[p] = a[i];
}
System.out.println(Arrays.toString(c));
    }
}
```

**桌面有一个归并的实例PDF**

[ArrayMerging]: https://docs.qq.com/pdf/DZktlWHNSdU1VYmdj	"ArrayMerging"



##### 归并的运行时间分析

我们可以使用对新列表的“写入”操作次数作为我们的成本模型，并对操作进行计数。由于我们只需将每个列表的每个元素写入一次，因此运行时间为Θ ( *N* ).

### merging sort

我们考虑对于N = 64 的序列数组进行选择排序，那么需要大约2048的单位时间（N^2），如果我们把N = 64的数组分为两个 N1 = 32 和 N2= 32的两个序列数组，先对两个数组分别进行选择排序，然后再利用归并来将已排好序的两个N = 32 的数组合并为一个有序数组，这样只需要 2 * 32 ^2  +  64 = 2112，约1088 的单位时间,比直接选择排序快很多，先分成两半再选择排序再归并，操作下来的时间是 N + 2 * (N / 2) ^2，最终还是一个N平方的时间复杂度，但是如果再继续将两个32单位的数组都分别分成两半，然后对每一个小数组选择排序，最后再利用归并把小数组合并呢？这样会进一步优化时间，大约是 64 +  4  * （16 * 16 ）=  ，约为640单位时间  

如果我们一直把数组分成两半，分了再分？最终我们会得到只有一个单元的数组，在这种情况下我们甚至不需要再进行选择排序了，因为一个数本身就是排好序的

##### **This is the essence of merge sort:**

- **If the list is size 1, return. Otherwise:**
- **Mergesort the left half**
- **Mergesort the right half**
- **Merge the results**

#### 归并排序的运行时间分析

以N= 64的为例：

每一次归并都是归并64个元素，所以每一次归并所用时间都是N，则总时间为 kN，k为归并的次数，而每一层归并都是把上层的每个数组数组一分为二，直到一个数组只有一个元素，所以 k = log_2 (64) = 6,即k = logN，所以总时间为NlogN，Θ(*N**l**o**g**N*).

NlogN的性能还是比较优越的：**N^2 对比NlogN 是天壤之别。从NlogN到N很好，但并不是一个根本性的变化**

> ![timetable](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751317.png)

## Lecture 16 - ADTs, Sets, Maps, BSTs

### ADTs

抽象数据结构由其可进行的运算定义，而不是由其实现定义

一些常用的ADT：

- 堆栈(Stacks)：支持后进先出元素检索的结构
  - `push(int x)`: 将 x 放在栈顶
  - `int pop()`: 取栈顶元素
- 列表(Lists：一组有序的元素 
  - `add(int i)`: 添加一个元素
  - `int get(int i)`: 获取索引 i 处的元素
- j集合(Sets)：一组无序的唯一元素（无重复）
  - `add(int i)`: 添加一个元素
  - `contains(int i)`: 返回一个布尔值，表示集合是否包含该值
- Maps：一组键/值对
  - `put(K key, V value)`: 将键值对放入映射中
  - `V get(K key)`: 获取key对应的值

后面三个ADT是一个更大的名为collections的接口的子接口

### Binary Search Trees，二叉查找树

#### 二叉树的由来思想，个人简述

或许是最重要的数据结构之一

链表挺好的，但是要查找某一个值可能需要很长的时间( O(N) )，就算链表是有序的还是如此，因为最坏的情况是要查找的值在链表的最后面，这样需要线性的时间。

而对于一个有序的数组来说，我们可以使用二分查找来更快的查找值，LogN的时间

在二分查找时我们可以利用数组已经是排好序的属性，通过要查找的值和中间的值的比较来去掉一般范围，比中值大则去掉左半边，比中值小则去掉右半边，这样反复缩小范围直到我们查找到这个值，或者最终确定这个值不在数组内

但是我们如何对链表使用二分查找呢？我们首先要遍历一半的链表，找到中间值，而这个过程就已经是O(N)的了，不像数组找到中间值只需要常数时间。对于这个问题我们可以做一个优化，那就是我们增加一个对正中间节点的引用，这样第一层找中间值就是常数时间，但是在我们去掉一半的链表以后呢，在剩下的那一半里我们还是要遍历(剩下的链表的）一半来找到新的中间值，但是这样还是O(N)的，所以我们做同样的优化，增加一个记录这个新的中间值的引用，第二层也优化为常数时间了，那么对于第三层呢？第四层呢？我们也做这样的优化，最终我们再进行一些优化得到的就是一棵二叉树了

#### 树的属性（是树，不是二叉树）

树由：1.节点 2.连接节点的线，也就是指针，组成，其中任意两个节点之间只有一条路径相通

在某些树中我们取一个节点作为根节点，这个节点没有父节点，叶子指的是树中没有子节点的节点

**以下为不合法的树：粉色的都是由于有多条路径所以不合法**

![Invalid Trees](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751352.png)

#### 二叉树的属性（不是二叉查找树）

**二叉树**由：

1.节点 

2.连接节点的线，也就是指针，组成，其中任意两个节点之间只有一条路径相通

3.每个节点只有0或1或2个子节点，所以上图的第三个是树但不是二叉树

#### 二叉查找树的属性

**二叉查找树**由：

1.节点 

2.连接节点的线，也就是指针，组成，其中任意两个节点之间只有一条路径相通

3.每个节点只有0或1或2个子节点

4.对于某一个节点X，X的左子树的**任意一个节点**的值都要小于X的值，X的右子树的**任意一个节点**的值都要大于X的值

```java
//以下为会用到的BST类，节点的定义
/*一个节点由：1.一个关键字key 2.指向左子节点和右子节点的引用**/
private class BST<Key> {
    private Key key;//大小写不一样，一个Kye类型的值kye
    private BST left;//指向左节点的引用,或者说指向左子树？
    private BST right;//指向右节点的引用，或者说指向右子树？

    public BST(Key key, BST left, BST Right) {
        this.key = key;
        this.left = left;
        this.right = right;
    }

    public BST(Key key) {
        this.key = key;
    }
}
```

#### 二叉查找树的运算

##### 查找值

为了查找某个值，我们利用二分查找，基于二叉查找树的属性，还算不难

一棵二叉查找树的某个节点的左子树任意值恒小于该节点，右子树则恒大于该节点的值。我们可以从根节点开始，然后将要查找的值和节点值比较，要查找的值更大就往右子树查找，否则往左子树查找，我们递归的重复这个过程直到找到这个值，或者直到达到某一片叶子，这种情况下我们要查找的值不在这棵树里

```java
//递归的写法
//返回值是以sk所在的节点为
static BST find(BST T, Key sk) {
   if (T == null)
      return null;
   if (sk.equals(T.key))
      return T;
   else if (sk ≺ T.key)
      return find(T.left, sk);
   else
      return find(T.right, sk);
}
```

如果我们的树是比较平衡的，相对枝叶茂盛，分布比较对称，那么时间复杂度是logN，因为树的高度是logN

##### 插入值

我们**永远**是往叶子节点插入值

先查找要插入的值是否已经存在于二叉树中，如果要插入的值在树中已经存在，那么直接return，如果在查找完成发现要插入的值事先在树中不存在，此时我们其实已经找到了合适的叶子节点，只需要按BTS的规则往叶子节点的左边或者右边加一个节点就行了

```java
//返回值是插入了值后的二叉查找树
static BST insert(BST T, Key ik) {
  if (T == null)
    return new BST(ik);
  if (ik ≺ T.key)
    T.left = insert(T.left, ik);
  else if (ik ≻ T.key)
    T.right = insert(T.right, ik);
  return T;
}
```

##### 删除值

删除树的某个节点相对麻烦，因为我们要保证我们每次删除节点后树仍然是一棵二叉树

###### 被删除的节点没有子节点

如果我们想删除的节点没有子节点，我们只需要将其父节点对要删除的节点的引用删除，这个节点就会被内存回收器回收掉

###### 被删除的节点有一个子节点

由于二叉查找树的特性，被删除的节点的子节点遵循一定大于（如果被删除的节点是右节点）或者一定小于（被删除的节点 是左节点）被删除节点的父节点的规律，所以我们可以直接让子节点代替掉要删除的节点的位置，要做这样的操作只需要将父节点的引用由指向要删除的节点指向  要删除的节点的子节点  即可，要删除的节点会被回收掉

###### 被删除的节点有两个子节点

当被删除的节点有两个子节点，我们不能简单的将其中一个子节点作为新的根节点来代替被删除的节点，这有可能不符合BST的规则

所以我们找一个新的节点来代替被删除的节点，这个节点必须满足：1.比左子树任意节点大	2.比右子树任意节点小

在下面的例子中我们删去dog节点来演示如何寻找到合适的替代节点:

![[ADTs, Sets, Maps, BSTs, Video 6] - BST Deletion.mp4_20210902_133830.620](D:\PotPlayer\Capture\[ADTs, Sets, Maps, BSTs, Video 6] - BST Deletion.mp4_20210902_133830.620.jpg)

在这个情况下cat和elf都是合适的选项，因为cat在dog的左子树，所以cat一定比右子树的任意一个节点小，又因为cat在左子树的所有节点里是最大的一个节点，所以我们把cat放上去替代dog以后，cat比剩下的左子树的任意节点都要大，从而保证了cat作为根节点不破坏BST的规则，elf作为右子树最小的节点也是类似的逻辑

总结一下就是：选取左子树最大的节点，或者是右子树最小的节点，先把被删除的节点的值写入原先根节点，然后再把选取的节点删除，也即，先在dog节点所在位置用cat或者是elf覆盖dog的值，然后把cat或者elf节点删除，**而删除我们选取的替代节点，由于我们选取的替代节点的特性（很简单，因为如果替代节点有两个子节点，那么左子节点比它更小，右子节点比它更大，它既不可能是左子树最大，也不可能是右子树最小），一定是情况1或者情况2的删除，换句话说我们选取的替代节点要么是只有一个子节点(elf)，要么没有子节点(cat)**要找到那些替代的节点，我们只需要找到左子树里最靠右的节点，以及右子树里最靠左的节点

#### BSTs as Sets and Maps

我们可以使用二叉查找树来实现一个集合(Set)，性能会比之前用数组实现的ArraySet更好，因为在ArraySet中我们调用contains()来确定某个元素是否存在是我们需要花费O(N)的运行时间，因为我们需要遍历数组,但是如果使用树来实现Set的话，在理想条件下(树枝叶茂盛)，我们的时间复杂度只有logN

我们同样可以通过将树的节点中的值由单个的值变为(key,value)的键值对来实现map,通过比较每一个元素的键值来确定把它放在树的哪一个位置

#### 总结

抽象数据类型 (ADT) 是根据操作而不是实现来定义的。

几个有用的 ADT：

- 不相交的集合、映射、集合、列表。
- Java 提供 Map、Set、List 接口以及几种实现。

我们已经看到了两种实现 Set（或 Map）的方法：

- ArraySet: Θ(*N*) operations in the worst case.
- BST: Θ(log*N*) operations if tree is balanced.

BST 实现：

- 搜索和插入很简单（但插入有点棘手）。

- 删除更具挑战性。典型的方法是“Hibbard 删除”。

## Lecture 17 - B Trees （Balanced Search Trees)

#### Binary Tree Height

最差情况的树的运行时间和最好情况的树的运行时间差别很大，最好的情况O(logN)，最坏的情况O(N)，运行时间的取决于树的结构，如果树是细长的，那么树和链表几乎是一样的，运行时间是线性的，如果树是下图左边那样茂密平衡的，那就是logN

![[B Trees, Video 1] - Tree Height, Big O vs Worst Case.mp4_20210902_154443.836](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751378.jpg)

#### BST性能

##### 先引入一些术语：

1.深度：在某个节点和根节点之间连接线的数量

2.高度：最深的叶子的深度，或者说深度的最大值

3.平均深度：所有节点的深度的和 除以 节点数

树的高度确定了最坏的情况下的运行时间，因为最坏的情况就是我们要找的节点在树的最底下，平均深度则确定了平均情况下的运行时间

##### BST插入顺序

插入节点的顺序决定了树的高度，譬如，给1-7七个整数要插入一棵二叉查找树，最坏的情况是按照自然数顺序插入，这种情况下树是长长的一条链表，最好的情况是按4，2，6，1，3，5，7的顺序插入，会得到一棵平衡的二叉树

一个特定的插入顺序有助于我们得到一棵茂密且平衡的树，但是我们甚至不需要那么刻意去按照某个顺序插入，即使是按照随机顺序插入节点，最终我们得到的树也还算平衡。懒得证明，实际上也不会证明，只需要知道以随机顺序插入的时候树的高度和深度都是 Θ(logN)的就行

但是我们无法做到总是按照随机顺序插入，比如我们的数据是实时流出的，我们就不得不按照数据出现的先后顺序来插入节点

所以接下来介绍一种永远能保持平衡的树

#### B - Trees

BST的问题在于我们总是往叶子节点插入新的节点，这导致了树的高度增长，以下面这棵平衡的树为例，我们插入17,18,19，按照一般的二叉查找树的规则去插入，则树变得不平衡了，右下角凸出一块，平衡结构被破坏

![B tree1](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751420.png)

为了解决这个问题，引出一个新的想法：我们避免再给叶子节点添加子节点，同时让一个节点可以同时保存多个值，在插入值的时候直接往现有的叶子节点里插入值，这样的话树的高度就不会增长

按照这个想法我们把17 18都插入到16所在的叶子里，变成下图

![](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751445.jpg)

但是这样存在一个潜在的问题，那就是，如果我们按照上述想法插入了很多个数，像下图那样，这就类似于我们插入了一个长度为N数组在叶子节点，如果我们要找的树是最后一个，那么时间复杂度就退化回了O(N),譬如下图我们要找24，我们要先走到最底下然后遍历数组

![B trees 2](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751482.png)

**这种情况下我们退化为了一个链表，时间变成了线性的**

解决方法：我们限制每一个节点能够存的值的个数，比如说3，当我们往一个已经有了3个值的节点里插新的值，我们把节点对半分，把中间靠左的节点提高一级放到父节点上去，如下图把17放到15所在节点，并把{16,18，19}拆分成了两个节点{16}和{17,18}。注意现在15节点有了三个子节点，并且每个节点要么小于15，要么介于15和17之间，要么大于17，这样我们就可以继续利用二分查找

这种树可以叫B树也可以叫2-3树或者是2-3-4树，2-3指的是可以有2个或3个子节点，这种情况下L = 2,	2-3-4同理，L = 3

![[B Trees, Video 3] - B Tree Basic Insertion.mp4_20210902_200359.609](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751506.jpg)

#### B树的插值过程

以一棵2-3-4树为例：

1.带着要插入的节点从根节点往下走，根据经过的节点的值和要插入的值的大小决定是往左走还是往右走，直到找到合适的叶子节点

2.在插入了新的值后，如果新节点有4个值，那么将中间靠左的值往上放一级，然后相应的重新整理一下子节点

3.如果上一步的操作致使父节点也有4个值，对父节点重复2的操作

4.重复这个过程直到父节点可以放下或者是到达根节点

2-3树类似，只不过临界点变成了插入值后节点有3个值

#### B 树的规律总结

在最开头我们提到，插入值的顺序对于二叉查找树来说是很重要的，那么对于B树来说，同样的规律适用吗？

答：B树的高度确实依赖于插入值的顺序，插入值的顺序不同树的高度可能不同，但不管怎么样插入，B树都会是茂密平衡的

B树还有以下两点规律：这两点规律决定了B树一定是平衡茂盛的

**1.所有的叶子节点到根节点的距离都必须相同	**

**2.一个有k个值的非叶子节点必须有k + 1 个子节点**

#### 总结

二叉查找树的最坏情况是O(N)，最好情况是O(logN)

大O表达式不等于是最坏情况，除非O表达式恰好是时间的上确界

B树是二叉查找树的改进版本，它避免了线性时间的最坏情况

- 节点可以包含1到L个值，L为限制
- contains(x)像一般的二叉查找树里的一样工作
- add()通过向叶子结点添加值工作，如果节点太满就分裂节点
- 这样导致了一棵平衡的树，运行时间为O(logN )
- 还没有讨论删除怎么工作
- 只讨论了当L <= 3的时候怎么分裂节点
- B树更加复杂，但是也可以更有效的应对任何插入顺序

### Lecture 18 - Red Black Trees 红黑树

虽然B树有着很好的平衡，但是B树实现起来非常困难，我们需要追踪不同的节点并且要实现节点的分割非常复杂，引入另外一种实现平衡的树的方法

对任意二叉查找树来说，有许多种方法来构建它使得它保持二叉树的属性，之前我们讨论了插入顺序是如何影响树的结构的，下图是插入1,2,3时不同顺序带来的不同二叉搜索树结构，但是除了通过不同的插入顺序来得到不同的二叉树结构，我们还可以通过旋转的操作从一种二叉树变换到另外一种，也就是说我们可以通过一系列的旋转操作使得树由不平衡变得平衡

![红黑树1](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751553.png)

#### Tree Rotation

​	对于树的旋转的正式定义是：

2.rotateRight(G): Let x be the left child of G. Make G the new right child of x.，设x是G的左子节点，让G变为x的右子节点

1.rotateLeft(G): 设x是G节点的右子节点，让G节点成为x节点的左子节点

在下图情况下x是P，也就是让G通过旋转变成P的左子节点（这个过程中还调整了K的指向，让P由3个子节点变成两个）

![红黑树2，旋转](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751579.png)

**我们可以把这个旋转的过程看作是G和P节点先融合，然后G(连带着K）再往下走一级变成P的左子节点，注意旋转并不改变树的意义**

并且这个例子的旋转使得树的高度增加了1

![1. Red Black Trees, Video 1  Intro, Rotation.mp4_20210903_143904.935](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751616.jpg)





**再做一个逆向的，rotateRight(P)，和上面的刚好是逆运算，这种情况下树的高度减一，我们同样对k进行了调整**

![1. Red Black Trees, Video 1  Intro, Rotation.mp4_20210903_144601.635](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751653.jpg)

以下是一个通过旋转优化树的结构的demo

![Rotate Demo](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751690.png)

#### Red-Black Trees

在这一章我们将范围限定于2-3树

在之前提到我们很喜欢2-3树因为2-3树永远是保持平衡的，但是我们也讨厌2-3树因为它实现起来非常麻烦。

红黑树则是取了二叉树和B树的优点，它使用二叉树实现，但是结构上又是和2-3树相同

##### Enter the Red-Black Tree

我们将从一棵2-3树开始，思考对一棵2-3树做什么样的修改，从而将其转变为一棵BST

分情况讨论：

1.对于一棵只有2-节点，也即节点只有两个子节点的B树来说，这已经是一棵二叉树了，无需做什么修改

2.节点有3个子节点的情况：

一种办法是我们可以创造一个胶水节点，这个节点不保存任何东西，只是拿来表明胶水节点的两个子节点实际上是同一个节点保存的两个值，不过这样非常不优雅，因为浪费了内存空间并且代码很难写，而且还有多余的link，虽然我没有写过，但是Josh这样说那肯定就是很难写

![3. Red Black Trees, Video 3  Red Black Tree Definition.mp4_20210903_154358.560](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751747.jpg)

所以我们选择胶水链接而不是胶水节点，详细一点说就是，对于一个本身存储了两个值，有三个子节点的节点来说，我们将存储的两个值的节点分为两个小节点，中间用胶水链接(glue links)连接在一起表明这是同一个节点保存的两个值，而这个胶水链接我们使用红色的线来表示，实际的链接我们使用黑色的线来表示。这样做避免了wasted link

![3. Red Black Trees, Video 3  Red Black Tree Definition.mp4_20210903_154919.944](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751779.jpg)

在修改的过程中我们选择的是让d变成f的子节点，换句话说就是让有两个值的节点，小的那个值变成大的那个值的子节点，从而glue links总是靠左边的(红色的线总是在左侧），在CS61B中我们始终选择让小的值做大的值的子节点，这种选择得到的二叉树叫**left-leaning red-black trees (LLRB)**

对于任意一种结构的2-3树来说，存在唯一一种LLRB与之对应，LLRB与2-3树是一一对应的

![3. Red Black Trees, Video 3  Red Black Tree Definition.mp4_20210903_160216.220](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751814.jpg)

##### Properties of LLRB's

对待LLRB就像对待普通的二叉查找树一样，想要查找某个值，把红色链接和黑色链接无差别对待

Here are the properties of LLRB's:

- 1-1 correspondence with 2-3 trees.

  这个显然

- No node has 2 red links.（下图最左边）

  因为如果有两个red links 连接在同一个节点上，那么这个节点对应的2-3树一个节点存了三个值(最多一个节点存两个)

- There are **no** **red** **right-links**.

  有红色的链接靠右那就和定义完全相反了

- Every path from root to leaf has same number of black links (because 2-3 trees have same number of links to every leaf).

  这个的意思就是说B树的最底下的叶子结点一定在同一平面上，一定是等高的，如果黑链接数目不一样，还原以后叶子结点不等高

  （从左往右第二个）

- Height is no more than 2x height of corresponding 2-3 tree.

  数学问题，不多解释，上确界就是2x，**由于2-3树的高度是logN，所以对应的红黑树的高度也是logN**

![6A717DDD3614F40500B615221D3C7F66](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751844.png)

还剩最后一个问题就是，我们怎样得到一棵红黑树？

一种方法是先实现一棵2-3树然后再把2-3树转换为一棵红黑树，但这与我们最开始的目的完全相反，我们一开始就是想避免2-3树的复杂实现

所以我们实现一个LLRB的	insert(x)	函数，它像普通的二叉查找树一样插入，然后通过旋转来得到对应的2-3树

##### Inserting into LLRB

在实现insert()的过程中我们有几个问题需要考虑：

1.插入值的时候是用红色link还是黑色link：红色

2.如果我们插入的时候按照二叉查找树的原则，被插入的元素变成了二叉树的右节点怎么办，换句话说要解决red links靠右的问题 ？

当我们发现插入后red links leaning right，我们使用旋转来保证red links leaning left，如下图插入S后red links靠右，旋转E解决

![5. Red Black Trees, Video 5  Red Black Tree Insertion.mp4_20210903_195027.495](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751880.jpg)

然后引入一个概念：**临时4-节点**，也即我们可以允许某个节点在插入时临时保存三个值，如下图，先后插入E和Z后，S节点有两个red link，这是不符合红黑树要求的，但我们允许它临时存在，接下来就是解决它的方法

![5. Red Black Trees, Video 5  Red Black Tree Insertion.mp4_20210903_200341.120](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751909.jpg)

如下图，先后插入S和E，导致S与两个red links 相连，形成一个4-node，并且是错误的4-node表现形式，因为出现了连续的left links，所以首先要解决这个形式上错误的4-node

![5. Red Black Trees, Video 5  Red Black Tree Insertion.mp4_20210903_200541.293](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751943.jpg)

经过一次旋转以后，我们得到了形式上比较像World2-3 的2-3树对应的非法红黑树(含有临时4-node)

![-](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751991.jpg)

但是我们还是没有解决这个非法临时4-node的问题，要解决这个问题我们先画出插入E后的2-3对应的红黑树（在此之前当然要先在下面的2-3树里把存了3个值的节点split一下），然后会发现画出来的红黑树和目前已有的红黑树只是在S的三根links上颜色刚好相反，所以我们只需要写一个翻转节点上links颜色的函数	flip()	来翻转，翻转其实是为了模拟split的过程，如下图的例子

**翻转颜色并不改变任何性能或形状上的东西，只是为了保持红黑树和2-3树的一一对应**

![5. Red Black Trees, Video 5  Red Black Tree Insertion.mp4_20210903_203838.699](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751018.jpg)

还有一个小小的问题就是，通过旋转或者flip修正一个错误可能会引出新的错误，不过问题不大，按照上面的依次处理就行了

## Lecture 19 - Hash Table

### hashing

目前为止我们为了高效地在数据结构中搜寻某个元素是否存在，我们讨论了二叉查找树，然后是2-3树，然后是红黑树

但是对于这些数据结构，都存在两个限制：1.它们要求我们存入的项目都要是可以比较的，通过比较我们确定项目放在树的哪个位置，但是对于一些对象来说，它可能没法比较大小 	2.他们的复杂度是log(N),虽然这已经很好了，但是也许我们可以进一步优化

#### A first attempt: `DataIndexedIntegerSet`

我们首先考虑如下方法：

目前呢，我们只试着优化第二个问题，也就是把复杂度从θ(logN)优化为θ(1)，我们先不考虑问题一中的可比较性问题

实际上我们将会只考虑存储和查找整数

我们的想法如下：建立一个boolean类型的ArrayList，大小为两百万，然后所有的值都默认初始化为false

add(x):插入值的方法，我们将下标为x的数组元素的值由false变为true，花费常数时间

contains(x):查询值x是否存在，我们只需要查看数组下标为x处的数组元素是true还是false，同样是θ(1)

```java
public class DataIndexedIntegerSet {
    private boolean[] present;

    public DataIndexedIntegerSet() {
        present = new boolean[2000000000];
    }

    public void add(int x) {
        present[i] = true;
    }

    public boolean contains(int x) {
        return present[i];
    }
    //这说明其实可以避免比较并且优化时间为常数的
```

但是这么做存在一些潜在的问题：

1.内存空间极度浪费，假设一个Boolean只要一字节，那一个ArrayList需要2GB空间，更何况使用者可能只插入几十个值

2.只能插入int，假如想插入String，甚至是对象呢？

#### Solving the word-insertion problem

接下来我们来解决插入字符串的问题

我们可以给每个字符串一个编号，比如"cat"是1，"dog"是2，之类的，这样如果有人想add("cat")，我们就可以把present[1] 设置为true，如果有人想确定"cat"是否存在，我们可以通过检查present[1] 的值来确定，但是如果想插入的字符串事先没有编号？我们不可能人为给所有的字符串都一个编号，比如想add("example")，这时我们该修改哪个位置的元素呢？

所以我们需要找到一种方法，使得我们可以通过这种方法得到任意字符串的编号，并且这种编号是一一对应的

##### Strategy 1: Use the first letter.

我们使用字符串的首字母，但是这样显然不行，因为以同一个字母开头的字符串太多了，dog-》4，drum -》4，如果先后add("dog")，add("drum")，显然会造成冲突，我们还不知道怎么处理冲突，所以接下来就是要避免冲突

**当然，还有的字符串根本就不是以字母开头的，这个稍后也会处理**

##### Strategy 2: Avoiding collisions

我们先考虑十进制数字系统是如何工作的

对于任意一个4位数abcd，都可以**唯一写成**写成 a*⋅10^3+*b*⋅10^2+*c*⋅10^1+*d*⋅10^0 ，比如说5149，它可以写成5⋅10^3+1⋅10^2+4⋅10^1+9⋅10^0，必须要以10为底，比如以2为底就不行，会造成同一个十进制数可以写成不同的二进制和形式

类似的，我们可以给26个字母每个字母一个数字，然后将字符串写成上面类似的形式

- "cat" = "c" 26^2 *+ 'a'* 26^1 + 't' 26^0= 3_26^2 + 1_26^1 + 20_26^0 =2074.

**这种表示为每个包含小写字母的英语单词提供一个唯一的整数，就像使用基数 10 为每个数字提供一个唯一的表示一样。我们保证不会发生冲突。**

**实际上任意选择不小于26的数为幂的底数都可以保证不发生冲突**

```java
public class DataIndexedEnglishWordSet {
    private boolean[] present;

    public DataIndexedEnglishWordSet() {
        present = new boolean[2000000000];
    }

    public void add(String s) {
        present[englishToInt(s)] = true;
    }

    public boolean contains(int i) {
        resent present[englishToInt(s)];
    }
    public static int letterNum(String s, int i) {
    /** Converts ith character of String to a letter number.
    * e.g. 'a' -> 1, 'b' -> 2, 'z' -> 26 */
    int ithChar = s.charAt(i)
    if ((ithChar < 'a') || (ithChar > 'z')) {
        throw new IllegalArgumentException();
    }

    return ithChar - 'a' + 1;
}

public static int englishToInt(String s) {
    int intRep = 0;
    for (int i = 0; i < s.length(); i += 1) {
        intRep = intRep * 26;
        intRep += letterNum(s, i);
    }

    return intRep;
}
}
```

所以现在我们还有哪些问题？

回想我们的目的，一是优化运行时间为常数，这一点我们已经为int和特定的string优化了，二是容许插入无法比较的项目，在这一点上我们比较接近了，虽然字符串和数字都是可以比较的，但是我们在过程中并没有比较它们，但是我们实际上还是没有试过去插入不可比较的项目

除此之外，我们目前还只能将优化应用于小写字母组成的字符串以及数，接下来我们要解决插入任意字符串对象的问题，包括空格等等

最后一点就是，我们对内存的浪费还是没有解决

### Inserting `String`s beyond single english words

我们利用字符编码的ASCII系统，这个编码系统为**英文文本的**每个字符都分配了一个编码，就相当于此前的字母表，在之前我们使用26作为给小写字母字符串分配编码的底数，现在我们使用126作为给任意字符串分配编码的底数，代码和上面的类似，只不过把26换成了126

```java
public static int asciiToInt(String s) {
    int intRep = 0;
    for (int i = 0; i < s.length(); i += 1) {           
        intRep = intRep * 126;
        intRep = intRep + s.charAt(i);
    }
    return intRep;
}
```

![Hashing, Video 3 - ASCII and Data Indexed String Sets.mp4_20210904_153755.958](D:\PotPlayer\Capture\[ADTs, Sets, Maps, BSTs, Video 6] - BST Deletion.mp4_20210902_133830.620.jpg)

但是如果我们也想支持中文？给任意一个中文字符串也一个独一无二的编码，那就要使用Unicode编码系统，对于中文来说底数变成了40959![Hashing，example _chinesw](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751063.png)

从例子来看，要存储一个三个汉字组成的字符串，需要的数组大小要在3亿亿左右..显然不太现实

所以接下来该怎么办？

### Handling Integer Overflow and Hash Codes

#### Overflow issues

java最大的整数是2,147,483,647，最小的整数是 -2,147,483,64**8**.，所以如果我们给最大的整数加一，得到的将会是最小的整数

这就会导致我们的编码出现一些问题，就算仅仅考虑ASCII字符也是如此，比如omens对应的编码是28,196,917,171，由于这是一个溢出了的数，所以最终编码函数返回的是-1,867,853,901，同样的，因为溢出，melt banana	和	subterresetrial anticosmetic这两个字符串会有同样的编码

对java来说，整数的个数为4,294,967,296，而我们可以造出的字符串的个数是远远多余这么多的，毕竟连整数字符串都可以从"一"到"1万亿"，超出整数的个数的部分一定会有编码冲突（抽屉原理）

**所以这样看来我们最终还是无法完全避免冲突问题，所以我们只能去解决冲突**

注意：我们的问题不是解决溢出，这个是解决不了的，我们的目标是解决冲突

### Hash Codes

在计算机科学中，取一个对象然后把它转换成一个整数叫做计算它的哈希码，我们已经看过了如何给字符串计算哈希值。对于其他对象来说，以下两件事我们会做其一

1.java的每个对象都有一个默认的.hashcode()方法，java计算hashcode的方法是先找到对象在内存中的地址，然后对地址做一些计算，最终得到一个独一无二的hashcode

2.有时候我们也会写自己的hashcode()方法，比如给一只dog，我们可能使用它的各种实例变量的集合来得到一个hashcode

#### Properties of HashCodes

1.hashcode必须是一个整数值

2.如果我们对同一个对象计算哈希值两次，两次的结果必须是一样的

3.如果	a.equals(b)	为真，a和b的哈希值必须是一样的

**到现在这个阶段我们已经可以往我们的数据结构中插入任意类型的对象了**

仍然未解决的问题一是内存空间的浪费，二是我们还没有处理哈希值冲突

### Handling Collisions

接下来我们要对我们一开始用于存储Boolean值的数组做一些改变，我们不再存储true or false了，我们将拥有相同的哈希值的项目都存在一个"篮子"里，然后我们把"篮子"存在一开始存Boolean值的数组里，而这个篮子，我们选择用linked list，实际上任意可以被遍历的collection都是可以作为篮子的（创建一个linked list的数组就行）

最开始的时候数组是空的，假设我们插入一个新的项目，它的哈希值是H，

1.如果在下标H处，什么东西都没有，我们就在下标H对应的地方创建一个新的linked list，然后把项目放入Linked List

2.如果在下标H处已经有了一个linked list，那么就直接把新的项目放入linked list

**注意：这个数据结构不允许副本的出现，所以在插入新项目之前我们必须先检查这个项目是否已经存在于hash table中，如果事先存在那啥都不要做，如果事先不存在，那么我们就直接把新项目插入到对应链表的末端**

#### 工作流总结

add()：1.先通过.hashcode()得到要插入的项目的哈希值2.如果对应的下标处是空的，建立一个新的链表，然后把项目放进去3.如果对应下标处已经有链表，遍历链表确定要插入的值是否已经存在，不存在则插入至链表末端（就是在链表末端新增一个节点）

contains():1.求得哈希值 (下标）2.如果下标对应的数组是空的，返回假 3.否则遍历链表来确定要查找的项是否存在

### 复杂度分析

记Q为所有链表长度的最大值，则add()和contains()的复杂度都是θ(Q)，因为不管是插入一个值还是查找一个值是否存在，我们都需要遍历某一条链表，最坏的情况下我们要遍历的链表就是最长的那一条链表

目前为止我们解决了哈希值冲突的问题，还没有解决内存空间浪费的问题

**而对于时间复杂度，在最最最坏的情况下，我们插入的N个项目的哈希值全部是一样的（比如哈希值函数无脑return 0，所有的哈希值总是0），这时候N个项目全在一条链表上，这时候运行时间复杂度为θ(N)**

### Solving space

接下来解决内存空间的问题

回到我们一开始的目的，我们为什么要几百万个数组单元来存储Boolean的值？因为我们最开始希望一个单元只用来说明一个项目的存在与否，把可能会造成哈希值冲突的项目分开来存放，从而避免冲突

但是我们实际上发现哈希值冲突是无法避免的，**因为我们想插入项目可以有无限多个，而java受限于计算机硬件，可以使用的数字只有有限多个**

所以我们最后决定把哈希值相同的项目放入一个链表里面，所以我们其实不需要那么大的数组，我们只需要长一点的链表

如果我们只有100个数组空间，意味着最多一百条链表，但是哈希值又很大怎么办？把哈希值对100取余得到下标就好，这样的话链表会更长，因为余数相同的哈希值可能有很多个，**所以时间复杂度稍微变大了点**

![Hashing, Video 5 Separate Chains and Hash Tables.mp4_20210904_221951.958](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751098.jpg)

### Our Final Data Structure: `HashTable`

经过一系列改进之后，我们现在得到的数据结构就是hashtable

- 输入由散列函数 ( `hashcode`) 转换为整数。然后，使用模数运算符将它们转换为有效索引。然后，将它们添加到该索引处（使用 LinkedLists 处理冲突）。
- `contains` 通过找出有效索引并在相应的 LinkedList 中查找项目，以类似的方式工作。

#### Dealing with Runtime

最后剩下要处理的就是运行时间的问题了，我们优化一下

假如我们有100个项目，数组大小为5，最好的情况下是5条长度为20的链表，最坏的情况是一条长度为100的链表

从两个方面来优化：1.使哈希值分布均匀 2.动态调整hashtable的大小

##### 动态调整hashtable

设M为数组大小，N为项目的数目，负载因子 Q = N/M（同样也是我们可以达到的最理想情况），我们现在希望做的就是保持Q低于某个较小的常数，比如说1.5，当Q超过1.5时，我们就扩充数组的大小，具体操作如下：

1.创建一个大小为2M的数组（创建一个大小为2M的hashtable）

2.遍历原先的hashtable，把所有的项目放入新的hashtable，**注意这个过程中原先在一条链表上的项目可能最终不在同一条链表上了**

**在同一条链表上的项目哈希值不一样，只是哈希值对M的余数（下标）一样，所以在新的hashtable上原先在同一条链表的项目可能之后不在同一条链表，从而链表变短了，这也是我们的目的——让链表变短**

下面这个例子就是链表变短了，正是上面叙述的情况（苹果和另外两个不再处于同一链表了，当然，相反的情况有可能发生，由哈希值和M共同决定）

![Hashing, Video 6  Hash Table Performance and Resizing.mp4_20210904_224201.787](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751131.jpg)

**假设项目是均匀分布的，那么运行时间就是θ(N/M)，而N / M被限定在一个常数附近，所以θ(N/M)也就是θ(1)**

下图就是N / M都是同一个常数，左边由于分布不均，导致时间复杂度拉胯

**Note also that resizing takesΘ(*N*) time**. Why? Because we need to add *N* items to the hashtable, and each add takes Θ(1) time.

**对哈希值为负数的取模可能得到的结果需要做一些处理，java是由于%的原因**

![Hashing, Video 6  Hash Table Performance and Resizing.mp4_20210904_225738.976](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751162.jpg)



所以最后要解决的问题就是让项目均匀分布了，而要解决这个，我们就要有好的哈希值

#### Assuming that items are evenly distributed?

Items will distribute evenly if we have good hash codes (i.e. hashcodes which give fairly random values for different items.) Doing this in general is.. well... hard.

Some general good rules of thumb:

- Use a 'base' strategy similar to the one we developed earlier.
- Use a 'base' that's a small prime.
  - Base 126 isn't actually very good, because using base 126 means that any string that ends in the same last 32 characters has the same hashcode.
  - This happens because of overflow.
  - Using prime numbers helps avoid overflow issues (i.e., collisions due to overflow).
  - Why a small prime? Because it's easier to compute.

### java工业代码中的hashtable

![Hashing, Video 7  Hash Tables in Java.mp4_20210904_230520.194](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751209.jpg)

**对哈希值为负数的处理：**

![Hashing, Video 7  Hash Tables in Java.mp4_20210904_230546.949](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751249.jpg)

![Hashing, Video 7  Hash Tables in Java.mp4_20210904_230557.206](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751275.jpg)

![Hashing, Video 7  Hash Tables in Java.mp4_20210904_230603.105](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751310.jpg)

## Lecture 20 - Priorit Queues and Heaps

二叉树使我们可以用logN的时间去查找某个元素，因为每一次查找我们都可以去掉一半的元素

但如果我们关心的是如何最快的找到最小的或者是最大的元素而不是快速的查找呢？

这就引出了一个新的抽象数据结构：优先队列。优先队列可以看做是有一个袋子，你可以往袋子里装东西，你也可以从袋子里拿掉东西，但是需要注意的是你只能和袋子里最小的东西交互

```java
/** (Min) Priority Queue: Allowing tracking and removal of 
  * the smallest item in a priority queue. */
public interface MinPQ<Item> {
    /** Adds the item to the priority queue. */
    public void add(Item x);
    /** Returns the smallest item in the priority queue. */
    public Item getSmallest();
    /** Removes the smallest item from the priority queue. */
    public Item removeSmallest();
    /** Returns the size of the priority queue. */
    public int size();
}
```

### Using Priority

这个数据结构有什么使用场景？我们在什么时候会需要它？

比如我们要监听公民间的文本信息，并且想追踪最不和谐的对话，每天你得写一份含有M条最不和谐信息的的报告（使用HarmoniousnessComparator来比较）

我们使用如下方法：将所有我们一整天收集到的信息放入一条链表里，然后对链表排序，并返回前M条信息

```java
public List<String> unharmoniousTexts(Sniffer sniffer, int M) {
    ArrayList<String>allMessages = new ArrayList<String>();
    for (Timer timer = new Timer(); timer.hours() < 24; ) {
        allMessages.add(sniffer.getNextMessage());
    }

    Comparator<String> cmptr = new HarmoniousnessComparator();
    Collections.sort(allMessages, cmptr, Collections.reverseOrder());

    return allMessages.sublist(0, M);
```

潜在的问题？这个方法需要θ(N)的空间，但是我们实际上只需要使用θ(M)的空间

### Priority Queue Implementation

我们使用优先队列解决同样的问题，同时使得内存利用更加高效。代码可能稍微复杂一点，但是并不是总是这样

可能用来实现优先队列的数据结构有：

1.已排好序的数组：1.add() ： O(N) 2. getSmallest(): θ(1) 3:removeSmallest()：θ(N)

2.平衡的二叉树：全是log(N)

3.hashTable:1.add():θ(1)2.getSmallest() : θ(N) 3.removeSmallest():θ(N)

### Heap Structure 堆

我们目前已知的数据结构对于优先队列来说运行时间最好的是二叉树，对它的结构和规则做一些改变，可以进一步优化时间和效率

我们定义满足如下条件的二叉最小堆为完备的和遵循最小堆性质的：

1.每个节点都小于等于它的子节点（最小堆）2.缺少的项目只在树的底部，所有节点都尽可能的靠左

![heap-13.2.1](https://raw.githubusercontent.com/CorneliaStreet1/PictureBed/master/202111051751337.png)

上图的左边两个绿色的都是合法的，右侧红色的非法

现在让我们考虑这种结构如何适用于我们在前一章中描述的抽象数据类型。我们将通过分析我们想要的操作来做到这一点。

### Heap Operations

优先队列中我们关心的数据结构是add() ，getSmallest()以及removeSmallest()。我们接下来在理论上考虑如何在给定的堆模式下实现这些方法

- add()：临时添加到堆的末端，沿着结构寻找适合的位置放置，如果子节点更小还要交换节点
- getSmallest()：返回结构的根节点，显然，由结构的定义保证的
- removeSmallest()：

