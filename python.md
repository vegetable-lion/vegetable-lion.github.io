# python面向对象

## 1.基本理论
### 1.1对象
对象：把所有零散的东西封装成一个整体的整体

python中的体现：python是一门特别彻底的oop语言
        
        一般：基本数据类型（int float）对象类型（string array）
        python：对象类型（int float string array）所有

### 1.2面向对象和面向过程
过程：单纯按照步骤走

对象：分解到对象，再步骤

面向对象是对面向过程的封装，就是把那些过程的步骤，划分到不同代码块，再归给不同对象，根据这个对象及对应行为，抽象出**类**

### 1.3类
类{名称、属性、方法}，但在产生对象后，这些组成才会具体

                    抽象
             ————————————————————> 
         对象                      类
             <————————————————————
                    实例化

定义一个类：``class Money：pass``
创建一个对象：``one=Money()``

### 1.4属性相关

改：

 ``class Money:pass``

 ``one=Money()``

``one.age=18``

``one.age=20``

一开始，开辟一个空间存储18，后来**新**开辟一个空间存储20（此时18的那个空间自动释放了）。通过one这个变量名称找到对象（为根据Money类产生的对象），再找到它的属性age，再通过指针指向它对应的地址的唯一标识，再访问这块内存里面的值18或20。

增：（1）直接对象.属性=值 （2）构造方法

``one.pets = ['bird']
print(one.pets,id(one.pets))``
          
         结果： ['bird'] 1497820303808
``one.pets.append('duck')
print(one.pets,id(one.pets))``
        
         结果：['bird', 'duck'] 1497820303808
不开辟空间（one.pets.append 相当于访问操作，不是修改操作）

删：

``p.age=18``

``del p.age``

删除age属性，删除age发出的指针，此时18就孤零零的，没用！也删除

#### 类属性

python里万物皆对象，类也是对象噢~

``class Money:
    pass``

``Money.count=2``

通过Money变量，找到Money这个类对象，类对象里的count属性，再到关联的值2

上代码= ``class Money:count=2``

查询：一个对象要是访问一个属性，它会优先到自己的属性里找，没有的话就到类里找

``class Money:
    age = 18
    count =1``

``one = Money()``

``one.age=20``

``print(one.age)``=20

``print(one.count)``=1

*内存问题*:属性存储在__dict__的字典中，一般对象可以直接修改__dict__属性，但类对象的__dict__为**只读**，无法修改。

增：

``class Person:``

``__slots__=['a']``
 
 ``pass``
 限制Person对象只能增加'a'属性

 ### 1.4方法相关

 方法：和函数类似，封装了一系列行为动作，被调用之后能执行这些行为动作。区别是调用方式。

 函数调用：
 
 ``def eat():``

``print(1)  print(2)``
    
``eat()``（直接用）


方法调用：

    class Person:
       def eat2(self):
          print(1)
          print(3)
        
    p=Person()
    p.eat2()
（对象.方法）eg.  pd.DataFrame

实例方法、类方法、静态方法：

    class Person:
        def shili(self):
            print("实例方法",self)
            
        @classmethod    
        def lei(cls):
            print("类方法",cls)
        
        @staticmethod
        def jingtai():
            print("静态方法")


    p=Person()

    p.shili() //接收的第一个参数要是实例参数

    p.lei() //接收的第一个参数要是类参数

Person.shili()是错的，因为Person不是实例参数，但是p.lei()可以，它会根据这个实例，把对应的类传递过来。

静态方法不要求

**注** ： 三种方法全部存在类的dict字典里

实例方法：
    
    class Person:
        def eat(self,food,drink):
            print("I'm eatting,",food,drink)
        
    p=Person()
    p.eat("新疆炒米粉,","cola") 

奇怪？eat里有三个参数呀，但是p.eat里面只有两个，因为在p.调用的时候，已经自动把self实例参数给对象p了。

类方法：

    class Person:

        @classmethod    
        def lei(cls,num):
            print("类方法",cls,num)
    

    class A(Person):
        pass

    A.lei(123)
A是衍生类，也可以传导~

**元类**：type
    
     print(int.__class__, str.__class, Person.__class) = type

创建类，除了用class，还能用type噢

    def specific(self):
         print("erha,jingmao")

    xxx = type("dog",(),{"num":100, "specific": specific})

所以，一般在class Person: 时，既给了类一个名字，又建立了变量叫Person，而用type时，类名“dog”和变量名xxx可以不一样。

类的创建流程：

    __metaclass__ = xxx

    class Animal(metaclass = xxx):
        pass

    class Person(Animal):
        pass

    class Dog:
        __metaclass__ = xxx
        pass

类里创建如Dog，类里没有如Person，就去它的父类Animal找，Animal直接在定义时创建，或者直接创建块。

**属性**

_c 为受限制的属性，在另一个文件里无法访问（下两种方法都不行）
  
    from temp import *   /   import temp

但加了 ``__all__=["_c"]``，就能用

    from temp import *   print(_c)  
