# C#语言学习

## 类

### 什么是类

类是现实世界事物的模型

类是现实世界事物进行抽象所得到的结果

### 类与对象的关系

对象是实例是类经过实例化后得到的内存中的实体NEW

使用new操作符创建类的实例

### 类的三大成员

属性

存储数据 组合起来可以表达类或者对象当前的状态

方法 表示类或实例能够做什么

事件 C#特有的 类或者对象通知其他类或对象的机制

### 类的静态成员与实例成员

静态成员（static）：他是类的成员

实例成员（非静态）：他是对象的成员

绑定指编译器把一个成员与类或对象关联起来

## C#的五大数据类型

类（class）

结构体（struct）

枚举（enum）

接口

委托

### C#类型的派生谱类  

引用类型：类 接口 委托

值类型：结构体 枚举

## class和struct的区别：

1、class是引用类型，struct是值类型；
2、class可以继承类、接口和被继承，struct只能继承接口，不能被继承；
3、class有默认的无参构造函数，有析构函数，struct没有默认的无参构造函数，且只能声明有参的构造函数，没有析构函数；
4、class可以使用abstract和sealed，有protected修饰符，struct不可以用abstract和sealed，没有protected修饰符；
5、class必须使用new初始化，结构可以不用new初始化；
6、class实例由垃圾回收机制来保证内存的回收处理，而struct变量使用完后立即自动解除内存分配；
7、从职能观点来看，class表现为行为，而struct常用于存储数据；
8、作为参数传递时，class变量以按址方式传递，而struct变量是以按值方式传递的。

## 变量

表面上变量的用途是存储数据

实际上，变量表示了存储位置，并且每一个变量都有一个类型，以决定什么样的值能够存入变量

变量名表示变量的值在内存中的存储位置（例如x=100x实际上是一个指向存储位置的指针（大内存空间可以存储小内存空间 例如short s; int x = s））

变量有七种：静态变量（成员变量，字段），数组元素，值参数，引用参数，输出形参，局部变量

狭义的变量指的是局部变量，因为其他种类的变量都有自己的约定名称

局部变量会在栈（stack）上分配内存

## 装箱拆箱

先将栈上的内容copy在找到堆上的一块空间存储

然后将地址存储到obj内存空间上 引用的堆上的实例

## 方法

方法的前身就是C/C++中的函数

### 为什么需要方法和函数

1.隐藏复杂逻辑 

2.把打算法分解为小算法

3.复用

## get和set用法

```
public class person1
{
    public string name;
}
 
 
public class person2
{
    public string Name{set;get;}
}
```

第一个类型的name属性未封装，其name属性直接通过public关键字暴露给系统中的其他类了。而第二个类型的name属性通过get和set关键字进行了封装，get和set分别对应的是可读和可写。由于其除了可读可写外，没有其它的条件限制操作，所以相当于如下代码：

```
// 字段
private string name;
 
// 属性
public string Name
{
    get { return name; } //相当于通过Name来返回name的值
　  set { name = value; } //通过Name来设置name的值
}
```

get和set的作用，除了控制读写之外还有逻辑、条件判断的作用。例如当我给Name赋值的时候想要先进行一些逻辑判断，就可以这样：

```
class Test
{
    private int age;
    public int Age
    {
        get { return age; }
        set { if (value < 0 || value > 200)
            {
                Console.WriteLine("无效的赋值，年龄必须在0——200之间");
            }
            else
            {
                age = value;
            }
            
        }
    }
}
```

一般简化写法

```
class Test
    {
        public int Age
        { get; set; }
    }
```

## 委托

### 什么是委托

委托是函数指针的升级版 

变量是以某个地址为起点的一段内存中所存储的值

函数是以某个地址为起点的一段内存中所存储一组机器语言指令

#### 直接调用与间接调用

直接调用：通过函数名来调用函数，CPU通过函数名直接获得函数所在地址并开始执行返回

间接调用：通过函数指针来调用函数，CPU通估计读取函数指针存储的值获得函数所在地址并开始执行返回

### 委托的声明

委托是一种类，类是数据类型所以委托也是一种数据类型

它的声明方式与一般类不同，主要是为了照顾可读性

注意声明的位置避免写错位置结果声明成嵌套类型

委托与所封装的方法必须类型兼容

```
delegate double calc(double x, double y);
		 double add(double x, double y) {return x + y}
```

返回值的数据类型一致

参数列表在个数和数据类型上一致（参数名不需要一致）

### 委托的使用

实例：把方法当做参数传给另一个方法

正确使用1：模板方法 借用指定的外部方法产生结果（委托有返回值）

正确使用2：回调方法 调用指定的外部方法（委托无返回值）

缺点：

1.这是一种方法级别的紧耦合，现实工作中要慎之又慎

2.使可读性降低，debug的难度增加

3.把委托回调，异步调用和多线程纠缠在一起，会让代码变得难以阅读和维护

4.委托使用不当有可能造成内存泄漏和程序性能下降

### 委托的高级使用

#### 多播委托

每个委托都只包含一个方法调用，调用委托的次数与调用方法的次数相同。如果要调用多个方法，就需要多次显示调用这个委托。当然委托也可以包含多个方法，这种委托成为多播委托。

##### 注：

1、多播委托包含一个以上方法的引用且必须是同类型的

2、多播委托包含的方法必须返回void，否则会抛出run-time exception，并且不能带参数（但可以带引用参数）

```
using System;

namespace ConsoleApp
{
    public delegate int MyDel(int name);
    class Program
    {
        static int Add1(int a)
        {
            var b = 10 + a;
            Console.WriteLine(b);
            return b;
        }
        static int Add2(int a)
        {
            var b = 10 - a;
            Console.WriteLine(b);
            return b;
        }
        static void Main(string[] args)
        {
            var add = new MyDel(Add1);
            add += new MyDel(Add2);
            Console.WriteLine(add(10));
            Console.ReadKey();
        }
    }
}
```

在使用委托时，+=将会被重载，它的作用变成了 在不影响已赋值委托变量的前提下，为已赋值的委托变量再次添加一个引用委托方法
 也就是说，每使用一次 +=运算符，委托变量就会被加入一个委托方法，比如原来 委托变量被赋值了，使用一次+=运算符后，委托变量也就同时作为两个 委托方法的委托了 ，而且，它们运行的先后顺序与添加的先后顺序一致。

#### 隐式异步调用

##### 同步与异步

同步：你做完了我在你的基础上接着做

异步：同时进行

##### 同步调用与异步调用的对比

每一个运行的程序是一个进程

每个进程可以有一个或者多个线程

同步调用时在用一个线程内

异步调用的底层机理是多线程

串行=同步=单线程

并行=异步=多线程

##### 应在适时地使用接口取代一些对委托的使用

## 事件

事件的订阅者，事件消息的接受者，时间的响应者，事件的处理者，被事件所通知的对象

事件信息，事件消息，事件数据，事件参数

使对象或类具备通知能力的成员

使用：用于对象或类间的动作协调和信息传递

原理：事件模型中的2个5

发生到响应5个部分

发生到响应5个动作：1.我有一个事件->2.有一个或者多个人关心我的这个事件 ->3.我的这个事件发生了 ->所有关心这个事件的人都被一次通知到 ->5.被通知到的人根据拿到的事件信息对事件进行响应

### 事件的应用

实例：派生与扩展

事件模型的五个组成部分

1.事件的拥有者

2.事件的成员

3.事件的响应者

4.事件处理器-本质是一个回调方法

5.事件订阅-把事件处理器与事件关联在一起，本质上是一种以委托类型为基础的约定

### 注：

事件处理器是方法成员

挂接时间处理器时，可以使用委托实例，也可以直接使用方法名

事件处理器对时间的订阅不是随意的，匹配与否事件时所使用的委托类型来检测

事件可以同步调用也可以异步调用

```
using System;
using System.Timers;

namespace Event
{
    class Program
    {
        static void Main(string[] args)
        {
            Timer timer = new Timer();
            timer.Interval = 1000; //Timer是一个定时器，它可以按照指定的时间间隔或者指定的时间执行一个事件。
            Boy boy = new Boy();
            Girl girl = new Girl();
            timer.Elapsed += boy.Action; //事件响应者+=事件处理器 c#中的Timer.Elapsed 事件，达到设置的间隔时间将设置该事件的事件处理程序，启动计时器。
            timer.Elapsed += girl.Action;
            timer.Start();
            Console.ReadLine();
        }
    }

    class Boy
    {
        internal void Action(object sender, ElapsedEventArgs e) //类型必须是前object后ElapsedEventArgs
        {
            Console.WriteLine("Jump!");
        }
    }
    class Girl
    {
        internal void Action(object sender, ElapsedEventArgs e)
        {
            Console.WriteLine("Sing!");
        }
    }
}
//结果会每1s反复输出Jump和Sing
```

### 事件的声明

有了委托字段/属性为什么还需要事件

为了程序的逻辑更加严谨更加安全防止借刀杀人

#### 事件的本质是委托字段的一个包装器

这个包装器对委托字段的访问起到限制作用

封装的一个重要功能就是隐藏

事件对外界隐藏了委托实例的大部分功能，仅暴露添加/移除事件处理器的功能

添加移除事件处理器时可以直接使用方法名，这是委托实例所不具备的功能

#### 用于声明事件的委托类型的命名约定

用于声明Foo事件的委托，一般命名为FooEventHandler

FooEventHandler委托的参数一般有2个

第一个是object类型，实际上就是事件的拥有者

第二个是EventArgs类的派生类，事件的参数

我们可以把委托的参数列表看做是事件发生后发送给事件响应者的事件消息

触发Foo事件的方法一般命名为OnFoo，即因何而起，事出有因

访问级别为protected不能为public不然可以借刀杀人

### 事件与委托的关系

事件不是以特殊方式声明的委托只是声明时候看起来像

事件声明时候使用了委托类型，简化声明造成了事件看起来像一个委托字段（实例），而event关键字更像是一个修饰符

订阅事件的时候+=操作符后面可以使一个委托实例，这与委托实例的赋值方法语法相同，这也让事件看上去像是一个委托字段

事件的本质是加装在委托字段上的一个“蒙板”，是一个起掩蔽作用的包装器，这个用于阻挡非法操作的蒙板不是委托字段本身

### 为什么要使用委托类型来声明事件

站在source的角度看是为了表明source能对外传递哪些消息

站在subscriber的角度来看他是一个约定为了约束能够使用什么样签名方法；来处理（响应）事件

委托类型的实例将用于存储（引用）事件处理器

### 对比事件与属性

属性不是字段：很多时候属性是字段的包装器，这个包装器用来保护字段你不被滥用

事件不是委托字段：他是委托字段的包装器，这个包装器不用来保护委托字段不被滥用

包装器永远都不可能是被包装的东西

## Lambda

1.匿名函数

2.Inline方法

```

```



## 类的继承

类成员的横向扩展（成员越来越多）

类成员的纵向扩展（行为改变，版本增高）

类成员的隐藏（不常用）

重写与隐藏的发生条件：函数成员，可见，签名一致

## 多态

基于重写机制（virtual->override）

函数成员的具体行为由对象决定

```
using System;

namespace Lambda
{
    class Program
    {
        static void Main(string[] args)
        {
            Func<int, int, int> func = new Func<int, int, int>((int a, int b) => { return a + b; });
            int res = func(100, 200);
            Console.WriteLine(res);
            func = new Func<int, int, int>((int x, int y) => { return y * y; });
            res = func(3, 4);
            Console.WriteLine(res);
        }
    }
}
//300 12
using System;

namespace Lambda
{
    class Program
    {
        static void Main(string[] args)
        {
            Func<int, int, int> func = (int a, int b) => { return a + b; };
            int res = func(100, 200);
            Console.WriteLine(res);
            func = (int x, int y) => { return y * y; };
            res = func(3, 4);
            Console.WriteLine(res);
        }
    }
}

using System;

namespace Lambda
{
    class Program
    {
        static void Main(string[] args)
        {
            DoSomeCalc((a, b) => { return a * b; }, 100, 200); //根据100 200确定为int型
        }
        static void DoSomeCalc<T>(Func<T,T,T>func, T x, T y)
        {
            T res = func(x, y);
            Console.WriteLine(res);
        }
    }
}
```

## 什么是接口和抽象类

1.接口和类都是软件工程的产物

2.具体类->抽象类->接口：越来越抽象，内部实现的东西越来越少

3.抽象类是为完全实现逻辑的类（可以有字段和非public成员，它们代表了具体逻辑）

4.抽象类为复用而生：专门作为基类来使用也具有解耦功能

5.封装确定的，开放不确定的，推迟到合适的子类中去实现

6.接口是完全未实现逻辑的类（纯虚类；只有函数成员；成员都是public）

7.接口是为解耦而生：高内聚，低耦合，方便单元测试

8.借口是一个协约，早已为工业生产所熟知（有分工必有写作，有写作必有协约）

9.它们都是不能实例化，只能用来声明变量，引用具体类的实例

```
using System;

namespace Lambda
{
    class Program
    {
        static void Main(string[] args)
        {
            Vehicle v = new RaceCar();
            v.Run();
        }
        interface VehicleBase
        {
            void Stop();
            void Fill();
            void Run();
        }

        abstract class Vehicle:VehicleBase
        {
            public void Stop()
            {
                Console.WriteLine("Stopped!");
            }
            public void Fill()
            {
                Console.WriteLine("Pay and fill...");
            }
            abstract public void Run();
        }
        class Car : Vehicle
        {
            public override void Run()
            {
                Console.WriteLine("Car is running...");
            }
        }

        class Truck: Vehicle
        {
            public override void Run()
            {
                Console.WriteLine("Truck is running...");
            }
        }

        class RaceCar : Vehicle
        {
            public override void Run()
            {
                Console.WriteLine("RaceCar is running...");
            }
        }
    }
}
```

## 接口与单元测试

1.接口的产生自下向上（重构），自顶向下（设计）

2.C#中接口的实现（隐式，显示，多接口）

3.语言对面向对象设计的内建支持：依赖翻转，接口隔离，开/闭原则......

## 反射与依赖注入

反射：以不变应万变（更松的耦合）

反射与接口的结合

反射与特性的结合

依赖注入：此id非彼id，但是没有彼id就没有此id

 


