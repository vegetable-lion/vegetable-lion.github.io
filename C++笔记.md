# C++
### 1. C和C++区别
* C++是面向对象的语言，C是面向过程的语言
* C++用运算符>>和<<，取代了printf和scanf
* C++引入new/delete 运算符，取代了C中的malloc /free 库函数
* C++新引入了 **引用** 的概念， **类** 的概念， **函数重载** 的概念

### 2. 定义和声明、描述
* 定义：为变量分配内存空间
* 声明、描述：不涉及分配
  > 外部参照型（extern) 

  > 引用

  > 函数原型

#### 2.1 extern
利用关键字extern，**主要是**在一个文件中引用另一个文件中定义的变量或者函数

一、引用同一个文件中的变量

    #include<stdio.h>
    int func();
    int main()
    {
    func(); //1
    printf("%d",num); //2
    return 0;
    }
    int num = 3;
    int func()
    {
    printf("%d\n",num);
    }
如果按照这个顺序，变量 num在main函数的后边进行声明和初始化的话，那么在main函数中是不能直接引用num这个变量的。

下面的程序就是利用extern关键字，使用在后边定义的变量。

    #include<stdio.h>
    int func();
    int main()
    {
    func(); //1
    extern int num;
    printf("%d",num); //2
    return 0;
    }
    int num = 3;
    int func()
    {
    printf("%d\n",num);
    }

二、引用另一个文件中的变量

如果extern这个关键字就这点功能，那么这个关键字就显得多余了，extern这个关键字的真正的作用是引用不在同一个文件中的变量或者函数。

    main.c

    #include<stdio.h>
    int main()
    {
    extern int num;
    printf("%d",num);
    return 0;
    }

    b.c

    #include<stdio.h>
    intnum = 5;
    voidfunc()
    {
    printf("fun in a.c");
    }
例如，这里b.c中定义了一个变量num，如果main.c中想要引用这个变量，那么可以使用extern这个关键字，注意这里能成功引用的原因是，num这个关键字在b.c中是一个全局变量，也就是说只有当一个变量是一个全局变量时，extern变量才会起作用，下面这样是不行的。

    mian.c

    #include<stdio.h>
    int main()
    {
    extern int num;
    printf("%d",num);
    return 0;
    }

    b.c

    #include<stdio.h>
    void func()
    {
    int num = 5;
    printf("fun in a.c");
    }
另外，extern关键字只需要指明类型和变量名就行了，不能再重新赋值，初始化需要在原文件所在处进行，如果不进行初始化的话，全局变量会被编译器自动初始化为0。像这种写法是不行的。 
> extern int num=4;


但是在声明之后就可以使用变量名进行修改了，像这样：

    #include<stdio.h>
    int main()
    {
    extern int num;
    num=1;
    printf("%d",num);
    return 0;
    }

如果不想这个变量被修改可以使用const关键字进行修饰，写法如下：

> const int num=5;


使用include将另一个文件全部包含进去可以引用另一个文件中的变量，但是这样做的结果就是，被包含的文件中的所有的变量和方法都可以被这个文件使用，这样就变得不安全，如果只是希望一个文件使用另一个文件中的某个变量还是使用extern关键字更好。

三、引用另一个文件中的函数

extern除了引用另一个文件中的变量外，还可以引用另一个文件中的函数，引用方法和引用变量相似。

    mian.c

    #include<stdio.h>
    int main()
    {
    extern void func();
    func();
    return 0;
    }

    b.c

    #include<stdio.h>
    const int num=5;
    void func()
    {
    printf("fun in a.c");
    }
这里main函数中引用了b.c中的函数func。因为所有的函数都是全局的，所以对函数的extern用法和对全局变量的修饰基本相同，需要注意的就是，需要指明返回值的类型和参数。

### 3. 访问和引用
访问：读写或修改存放的数据； 引用：只是个（变量\常量）的别名

### 4. 左值和右值

* 左值其实是对一块内存区域的引用（这个还不是C++中的int a之类的引用），比如上边的a，就对应了一块内存区域(起始地址为a，大小为sizeof(int))。

* 右值对应的玩意其实也在内存里，但是我们忽略这一点，认为它存在于冥冥之中。例如上边那个5，其实它在静态数据段或者程序二级制代码中，但我们不关心这个，认为它无法修改。

* 左值可做右值

**++i是左值，i++是右值**  , <span style="color: rgba(255, 0, 0, 1)">因为++i 返回 i 本身，而 i++ 返回 i 的值。</span>

1.++(a--)这个表达式是非法的，因为前增量操作要求一个可修改的左值，而 "a--" 不是左值（即右值）。

2.早期的c语言教材，for循环语句通常写成：

    for(int i=0;i=10;i++)

而现在多为：

    for(int i=0;i=10;++i) 
两者有区别吗？
a++ 即是返回 a的值，然后变量 a 加 1，返回需要产生一个临时变量类似于

    { int temp = a;
    a=a+1;
    return temp; //返回右值 }
++a 则为：

    { a=a+1;
    return a; //返回左值 }
显然，前增量不需要中间变量，效率更高。

3.易错点
*第一题*

    int m=0;
    for(int i=1;i<=100;i++){
    m=i++;}
输出：99
当i=99时,m=99,然后i加1，为100，然后i++变成101,101超出100，退出。

*第二题*

    int m=0;
    for(int i=0;i<100;i++)
         m=i++;
     cout<<m<<endl;
输出：98
i=98的时候，执行自加，m=98。这个时候，在循环体里面的i已经达到99了，加完后再回到for表达式的自加，i变成100，进入判断i<100,不再执行循环。

### 5. 变量
变量定义需要确定的五要素：
* 存储类型：（生命期、可见性）  
* 数据类型：（存储格式、取值范围）
* 变量名
* 变量的初始值
* 变量的地址（由系统确定）

存储类型 数据类型 变量名 = 初始值

存储类型：

auto类
auto类是指定于在所限定的作用域内部的局部变量的缺省存储类。例如：

    int Count;
    auto int Month;

上述两个变量具有相同的存储类。实际上auto关键词一般都省略。

register
register关键字提示编译器把局部变量或函数的形参尽可能放入CPU的寄存器中，以便快速访问。因此变量的字节长度不应该超过寄存器的长度。不要用取地址符(&)去获得此变量的内存地址。例如：

    register int Miles;

static
static是全局变量的默认存储类。例如：

    static int Count;
    int Road;
    main()
    {
    printf("%d\n", Count);
    printf("%d\n", Road);
    }

在函数之外：
全局变量  ->  存储类型：缺省
静态全局变量  ->  存储类型：static

在函数之内：
静态局部变量  ->  存储类型：static
局部（自动）变量  ->  存储类型：auto或缺省

代码区：
* 存放程序代码
  
数据区：
* 全局数据区、常量池
  
    * 存放全局变量、静态全局变量
    * 静态局部变量、常量
  
* 栈区

    * 存放局部（自动）变量

* 堆区

    * 存放动态变量
