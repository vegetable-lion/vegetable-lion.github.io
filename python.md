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
