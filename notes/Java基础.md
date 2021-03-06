[**首页**](https://github.com/qdw497874677/myNotes/blob/master/首页检索.md)

## 设计模式六原则

- 开闭原则：对扩展开放，对修改关闭。拓展时不能修改原有代码。
- 里氏代换原则：子类可以扩展父类的功能,但不能改变父类原有的功能。
- 依赖倒转原则：针对接口编程，依赖于抽象而不依赖与实体
- 接口隔离原则：使用多个隔离的接口，比使用单个接口更好，抽象更细致。
- 最少知道原则：一个实体尽量少与其他实体之间发生相互作用（不需要知道具体是谁，我自己做好就行）
- 合成复用原则：尽量使用合成/聚合的方式，而不是使用继承。

## Java是编译型还是解释型

Java代码需要编译，才能去运行，像是编译型。

Java编译后的代码不能直接运行，操作系统不能直接理解，需要运行在JVM上，像是解释型。

个人认为是解释型的，因为就算是编译后的.class文件，是字节码，操作系统还是不识别的，需要在JVM上解释后才能执行，所以才会是跨平台的。

## JRE和JDK

- JRE：java运行时环境。主要包含两个部分，jvm的标准实现和java的一些基本类库。相较于jvm多出来一部分java类库。
- JDK：java开发工具包。集成了jre和一些工具，如
  - javac.exe：表一java源文件
  - java.exe：启动java虚拟机运行java程序
  - jar.exe：把java应用打包成一个文件

## 面向对象

万物皆对象。对象有具体的实例化。任何方法或者属性都要写在对象（类）里面。

三大特征：

- 封装：把类的方法和属性用访问控制符修饰，只通过适当的方式暴露给外面。
  - 便于权限控制，保护属性。提高了代码的可维护可拓展性。
- 继承：一个新类可以从现有的类中派生。允许和鼓励类的重用。
- 多态：**同一个行为具有多个不同表现形式或形态的能力**。允许**基类的指针或者引用指向派生类对象**，在具体访**问方法时实现方法的动态绑定**。可以把不同的子类都看作父类，来屏蔽掉子类对象之间的差异，写出通用的代码。
  - 三个定义
    - 父类定义子类结构，父类不能调用子类独有的方法。子类方法覆盖（隐藏）了父类，调用会使用子类的方法。
    - 接口定义实体类结构
    - 抽象类定义实体类
  - 两个方法
    - 重载：同一个类中，定义了多个方法，具有相同的方法名和不同的参数列表
    - 重写：final类型的方法和static类型的方法是不能被重写的。
      - 一个前提：前提是在继承体系结构下。
      - 三个相同：子类中定义的方法与父类中的方法，有相同的名字、参数列表、返回值类型。
      - 两个规定：子类方法的**范围限定不能比父类的范围小**，不能抛出更多的异常。

好处：

- 易维护
- 易复用
- 易拓展

## 覆写、重写和重载

- 覆写指实现了抽象方法
- 重写指子类对父类允许访问的方法的实现过程进行重新编写。
- 重载指同一个类中有相同的名字不同参数的方法来执行。
  - 方法名相同，参数类型不同或者个数不同

## 继承（待更新！！！）

子类拥有父类所有的属性和方法（包括私有的）。但是父类中私有的方法和属性，子类无法使用，只能通过父类的非私有去简介访问。只能使用私有之外的。

## Java怎么实现多态

多态的实现是通过对访问的方法进行动态绑定来实现的。而动态绑定的实现主要依赖于方法表。当某个方法被调用时，JVM检查对应的运行时常量池，找到方法的符号引用，并查找调用类的方法表来确定方法的直接引用，然后才进行真正的调用。

方法表是动态调用的核心。存储于方法区中的类型信息，包含有该类型所定义的所有方法及指向这些方法代码的指针。这些具体的方法代码可能是被重写的方法，也可能是继承基类的方法。

## 接口和抽象类

接口：

- 可以有变量和方法。变量隐式的指定为public static final变量。方法隐式地指定为public abstract方法。如果一个非抽象类实现接口就要实现所有方法。如果是抽象类实现接口就可以不实现接口中的抽象方法。
- JDK8之后，接口也可以定义静态方法，可以直接用接口名调用。
- 一个类可以实现多个接口
- 从使用者角度看问题。接口是对行为的抽象。强调合约，双方无法犯错，是一种行为规范。

抽象类：

- 可以有成员变量，普通成员方法，可以有静态代码块和静态方法。
- 有抽象方法，有了抽象方法这个类就成为了抽象类。抽象方法必须为public或者protected修饰（private修饰后不能被继承，子类就不能实现方法）。抽象方法子类必须实现。如果子类没有实现父类的抽象方法，那必须将子类也定义为抽象方法。
- 抽象方法不能创建对象，
- 不可以多重继承，**接口可以**（只要继承的两个方法没有一样的方法就可以（方法名和参数））
- 抽象是对类的抽象，是一种模板式设计。

两者的区别

- 抽象类可以提供一些共有的实现，接口可以表示具有哪些功能
- 抽象类是对一种事物的抽象，包括属性和行为。接口是强调合约，双方无法犯错。
- 抽象类是一种模板式设计，接口是一种行为规范。
- 抽象类修改添加删除方法，子类不受影响。接口进行变更，实现接口的类都要改变。



### 抽象类可以实现接口吗

抽象类可以实现接口，可以不重写全部方法。没有重写实现的方法，需要让子实体类实现。

### 抽象类可以继承实体类吗

父类如果没有声明构造器，jvm是会生成一个默认的无参构造。其实上面的意思就是继承要满足一般继承的规则。**和实体类的继承一样，也要求父类可继承，并且拥有子类可访问到的构造器。**

### 多继承

- 类为什么不能多继承
  - 当类



## 基本数据类型

![img](Java基础.assets/clipboard-1596854735023.png)

- 逻辑型
  - boolean：1bit
- 整型
  - byte：8bit-1字节-最大128
  - short：16bit-2字节
  - int：32bit-4字节-最大大概能表示二十亿
  - long：64bit-8字节
- 浮点型
  - float：32bit-4字节
  - double：64bit-8字节
- 字符型
  - char：16bit-2字节



## （待更新！！！）浮点数

浮点数组成：符号位 | 基数m | 指数e

- 基数m部分：使用二进制数来表示此浮点数的实际值。
- 指数e部分：占用8bit(1个字节)的二进制数，可表示数值范围为0-255。但是指数可正可负，所以，IEEE规定，此处算出的次方必须减去127才是真正的指数。所以，float类型的指数可从-126到128。



## 访问控制符

![img](Java基础.assets/clipboard.png)

抽象类方法

接口：方法可以用public（不同包-子父类-同包-同类），protected（子父类-同包-同类），defailt（同包-同类）修饰。不能用private修饰方法。属性只能定义常量。



## （待更新！！！）内部类

优点：

- 内部类和外部类可以方便的访问彼此的私有域。（包括私有方法和私有属性）
- 内部类时另外的一种封装，对外部的其他类隐藏。
- 内部类可以突破单继承的局限。（每一个内部类都可以继承一个类）

分类

- 成员内部类：内部不允许存在静态方法或者变量。
- 静态内部类：使用static修饰内部类，就是静态内部类。
- 方法内部类：方法内部类不允许使用访问权限修饰符。对外部类完全隐藏，除了创建这个类的方法可以访问它以外，其他地方都不行。方法内部类如果想使用方法的形参，这个参数必须是final的。（JDK8形参变为隐式final声明）
- 匿名内部类：没有类名所以没有构造方法，必须继承一个抽象类或者实现一个接口。其他的类似于方法内部类。

内部类与外部类的关系

- 对于非静态的内部类，内部类的创建依赖外部类的实例对象，在没有外部类实例之前是无法创建内部类的。
- 内部类可以直接访问外部类的元素，包括私有域。外部类会在内部类创建之前创建，创建内部类时会将外部类的对象引用。
- 外部类可以通过内部类的引用间接访问内部类元素。想要访问内部类属性，必须先创建内部类对象。



## 异常

### 异常的结构

![image-20200516193617250](Java基础.assets/image-20200516193617250.png)

- Throwable
  - Error：错误。无法被程序处理。不应该试图去处理它引起的异常状态。（JVM承担）
    - VirtualMachineError：虚拟机错误
    - OutOfMempryError：内存溢出
    - ThreadDeath：线程死锁
  - Exception：异常。能被程序本身处理，可以catch，尽量在程序中处理这些异常。
    - Checked Exception：受检异常。Java编译器负责，可检测出，如果不处理，就不能编译通过。
      - IOException：IO异常。
      - SQLException：SQL异常
      - ClassNotFoundException：找不到类定义的异常
        - Class.forName()找不到指定的类
        - ClassLoader中的loadClass()找不到指定类
    - RuntimeException：非受检异常（运行时异常）。Java编译器不会检查这类异常。即使没有用trycathc捕获，也没有throws抛出，也会编译通过。为了就是找出错误从而修改程序。
      - NullPointerException：空指针异常
      - ArrayIndexOutOfBoundsException：数组下标越界异常
      - ArithmeticException：算数异常
      - ClassCastException：类型转换异常



- 可查异常：除了Exception中的RuntimeException及RuntimeException的子类以外，其他的Exception类及其子类(例如：IOException和ClassNotFoundException)都属于可查异常。
- 不可查异常：包括运行时异常（RuntimeException与其子类）和错误（Error）。

### 异常处理机制

- 抛出异常
- 捕获异常

### 异常堆栈信息

~~~
Exception in thread "main" java.lang.NullPointerException
	at com.bsx.test.TestException.m3(TestException.java:22)
	at com.bsx.test.TestException.m2(TestException.java:17)
	at com.bsx.test.TestException.m1(TestException.java:13)
	at com.bsx.test.TestException.main(TestException.java:9)
~~~

对于单个堆栈块。先出现的是最底层的抛出异常的方法。从被调用者开始报错，然后是调用者，依次报错。

### 断言

执行断言语句时，java计算断言assertion的值，如果是false，抛出AssertionError异常。该类有一个无参构造方法和七个重载的单参数构造方法，参数类型为int，long，float，double，boolean，char和Object。

不应该使用断言代替异常处理，异常处理用于在程序运行期间处理非常环境，断言是要确保程序的正确性。异常处理针对程序的健壮性，而断言设计程序的正确性。与异常处理类似，断言不能代替正常的检验，只是检测内部的一致性和有效性。断言在运行是检验，可以在程序启动时打开或关闭。

## （！！！待完成）JDK1.8新特性

- Lambda表达式

- 函数式接口：只有定义一个方法的接口。为了更方便的使用lambda表达式。得到的是一个函数式接口的实现类。

- 方法引用和构造器引用：lambda体中的内容有方法已经实现了，那么可以使用“方法引用”。直接把方法作为lambda体中的那个方法，最终得到的还是一个lambda，是一个函数式接口的实现类。

- Stream API：可进行流操作，将原先数据库能执行的操作提供到编码方式实现

  - | 去重：distinct  | distinct()      |
    | --------------- | --------------- |
    | 排序：order by  | sorted()        |
    | 分页：limit     | limit()、skip() |
    | 条件：where     | filter()        |
    | 分组： group by | groupingBy()    |

  - 创建stream

  - 中间操作（过滤、map）

  - 终止操作

- 接口中的默认方法和静态方法

  - ~~~java
    public interface Interface {
        default  String getName(){
            return "zhangsan";
        }
    
        static String getName2(){
            return "zhangsan";
        }
    }
    ~~~

    

- 新时间日期API

- Optional<T>：用于代替null的一个工具类

  - 它是box类型，保持对另一个对象的引用。
  - 是不可变的，不可序列化的
  - 没有公共构造函数
  - 只能是present 或absent
  - 通过of(), ofNullable(), empty() 静态方法创建。

- 

## 数组的存储

new一个数组之后，在堆内存中开辟连续的空间，返回这个空间的首地址给引用类型变量

## static关键字的作用

static关键字有四种使用场景

- 修饰**成员变量和成员方法**：被static修饰的成员属于类，可以通过类名调用。静态方法只能访问和调用静态的。在方法区（元空间）中。
- 修饰代码块：静态代码块，在类被初次加载时执行静态代码块（在构造方法前），只有执行一次。对于定义在他后面的静态变量可以赋值（不能对非静态的变量赋值），但是不能访问。
  - 和非静态代码块（构造代码块）的相同与不同：
    - 相同点：都是**在构造方法之前执行**，在类中都可以定义多个，定义多个时按照顺序执行，一般在代码块中对一些static变量进行赋值。
    - 不同点：静态代码块在非静态代码块之前执行。静态代码块只在第一次初始化执行一次（特例：Class.forName("ClassDemo")创建Class对象的时候也会执行。）；非静态代码块每初始化一次就执行一次。非静态代码块可在普通方法中定义；静态代码块不行。
- 静态内部类：静态内部类和非静态内部类的最大区别：**非静态内部类在编译完成后会隐含保存一个引用，引用指向创建它的外部类**，但是静态内部类没有。所以：
  - 静态内部类的创建不需要依赖外部类创建。
  - 静态内部类不能使用外部类的非静态的成员变量和方法。
- 静态导包：可以导入某个类中的指定静态资源，使用时不需要使用类名。

### 非静态代码块和构造方法的区别

非静态代码块是给所有对象进行统一初始化，而构造方法是给对应的对象初始化，因为构造方法可以有过个。

## final、finally、finalize

final有四种使用场景。

- 修饰类：这个类**无法被继承**、类中的所有成员方法都默认是final修饰的。
- 修饰方法：该**方法不能被重写**。private方法都隐式地指定为final。
- 修饰方法参数：编写方法时，在参数前面加final修饰，表示整个方法中，**不会改变参数的值**。
- 修饰变量：表示常量，**只能被赋值一次**，不能再改变。如果是基本数据类型，值是不能改变的。如果是引用类型的变量，变量指向对象后，不能让其再指向另外一个对象了。引用的对象的值是可以改变的，但引用类型的变量值是不能改变的。
  - 赋值可以在声明的时候赋值，或者在类的构造方法中赋值（包括非静态代码块）。staticc final修饰的变量会直接在类加载过程的准备阶段赋值（不是零值）。

finally是异常处理的一部分，无论是否捕获或处理异常，finally 块里的语句都会被执行。当在 try 块或 catch 块中遇到 return 语句时，finally 语句块将在方法返回之前被执行。下面四种情况finally块不会被执行：

- finally块中第一行发生了异常。
- 前面执行的代码中用了System.exit(int)退出了程序。
- 程序所在的线程死亡。
- 关闭CPU。

finalize()是Object类中的成员方法。被垃圾收集的时候会先执行这个方法，只执行一次，如果执行了，下次如果被回收，就会直接回收了。

## 为什么数组快

数组为随机访问。为什么支持随机访问：

- 数组占用内存的连续空间
- 数组元素大小一样

还有一个快的原因是：因为连续内存，所以在cpu读取时，把数据块取到缓存中，这样多次访问数据效率会高（现在缓存中找）。

## （待更新！！！）父类子类

### 变量

子类同名的变量会隐藏父类中的



### 构造器

父类的构造器不能被子类继承。

- 子类的构造器中有一个**隐式的super()来调用父类中的无参数构造器**。
- 如果子类调用了父类的有参构造器，就不会隐式调用无餐的了。

## Java类中的代码调用顺序

先类再实例，先父再子

子类和父类第一次调用

new Son()；

1. 父类静态代码块
2. 子类静态代码块
3. 父类非静态代码块
4. 父类构造方法
5. 子类非静态代码块
6. 子类构造方法

new Son();

1. 父类非静态代码块
2. 父类构造方法
3. 子类非静态代码块
4. 子类构造方法

new Fathar();

1. 父类非静态代码块
2. 父类构造方法

## Object类

一共有11个方法

- getClass()

> 是一个native方法，是final修饰的，不允许重写。
>
> 返回当前实例的Class对象。

- hashCode()

> 返回对象的哈希码。哈希码为了确定对象在哈希表中的索引位置。

- equals()

> 用于判断两个对象是否相等。默认是比较两个对象是否拥有相同的地址。

- clone()

> 用于克隆一个对象，被克隆的对象需要实现Cloneable接口，并且重写clone()。
>
> 浅拷贝：对基本数据类型进行值传递，对引用数据类型进行引用传递的拷贝。**只拷贝值。**
>
> 深拷贝：对基本数据类型进行值传递，对引用数据类型，创建一个新的对象，复制其内容，即在浅拷贝的基础上对所有引用的其他对象也进行了拷贝，为深拷贝。**如果是引用类型也拷贝引用的对象。**

- toString()

> 默认返回类名和哈希码。

- notify()

> native方法，被final修饰。
>
> 唤醒这个对象锁上的等待队列中的一个等待状态的线程。最终唤醒的线程是随机的。

- notifyAll()

> native方法，被final修饰。
>
> 唤醒这个对象锁上的等待队列中所有等待状态的线程。

- wait()

> 一个线程调用wait()方法后，当前调用线程**将进入等待状态**或者调用带超时时间地**进入超时等待状态**，进入对象锁的等待队列中，同时**释放持有的锁**，直到被唤醒，即其他线程调用此对象锁的notify()或notifyAll()。wait()只能在同步代码中调用，也就是说必须先获得锁，调用后会释放锁，让出CPU。
>
> 与sleep()的区别
>
> - sleep()是Thread类中的静态方法；wait()是Object类中的方法。
> - sleep()导致程序暂停执行指定的时间，让出cpu，它会保持监控状态，时间到了自动恢复运行，过程中不会释放锁。调用wait()方法的时候，线程会放弃对象锁，进入等待状态，只有针对此对象调用notify()方法后本线程才进入准备状态。
> - sleep()可以在任何地方使用；wait()只能在同步方法或者同步块中用。
> - 它们都可以被interrupted方法中断。
> - ![image-20200516222435082](Java基础.assets/image-20200516222435082.png)

- finalize()

> 垃圾回收器回收一个无用的对象时，会调用这个方法。可以重写这个方法来做一些清除工作。



## 序列化

Java对象在JVM中创建和销毁。如果想持久保存就需要通过序列化来实现，将内存中的对象转化成数据，需要对象时在去做反序列化。

### Java原生序列化

实现Serializable接口。序列化也会保留对象类的元数据（成员变量，继承信息等）。这个接口只是为了标识。使用transient修饰的变量不会被序列化。内部没有用transient修饰的引用变量的类也需要实现Serializable接口。

如果想把父类的值也保存，则也需要让父类实现Serializable接口，否则负责只会默认根据他的构造器创建。

通过ObjectOutputStream将对象序列化成二进制流。

~~~java
class Father{
    int val;
    public Father(int a){
        val = a;
    }

    @Override
    public String toString() {
        return super.toString()+" "+val;
    }
}
class Demo2 implements Serializable{
    int val;
    public Demo2(int a){
        val = a;
    }
}
public class Demo extends Father implements Serializable {
    int val;
    Demo2 o;
    public Demo(int a){
        super(a);
        val = a;
    }
    @Override
    public String toString() {
        return super.toString()+" "+val+" "+o.toString()+" "+o.val+" 父类："+super.toString();
    }
    public static void main(String[] args) {
        Demo demo = new Demo(1);
        demo.o = new Demo2(2);
        System.out.println(demo.toString());
        try {
            // 序列化
            ByteArrayOutputStream baos = new ByteArrayOutputStream();
            ObjectOutputStream oos = new ObjectOutputStream(baos);
            oos.writeObject(demo);

            // 反序列化
            ByteArrayInputStream bais = new ByteArrayInputStream(baos.toByteArray());
            ObjectInputStream ois = new ObjectInputStream(bais);
            Demo o = (Demo) ois.readObject();
            System.out.println(o.toString());
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}

~~~





## 克隆

### 浅克隆

拷贝对象时仅仅拷贝对象本身，而不拷贝对象包含的引用指向的对象。

#### cloneable实现

需要被克隆的类的操作：实现Cloneable接口，并重写Object类的clone()方法（访问修饰符设置为public，默认是protected）。在重写的clone方法中调用super.clone()并返回。

进行浅克隆时，调用对象的clone方法即可。



### 深克隆

拷贝对象本身，并且拷贝对象所包含的引用指向的所有对象。

#### cloneable实现

需要被克隆的类的操作：实现Cloneable接口，并重写Object类的clone()方法。在clone方法中用父类clone创建对象，然后使用对象引用变量的类的clone方法创建变量对象，赋值给自己的clone对象的引用变量。**（Object的clone方法只能完成浅克隆，通过这个基类克隆方法来去构建自己需要的形式）**

对引用的类的操作：实现Cloneable接口，并重写Object类的clone()方法。如果引用的类也引用了别的类的对象，也在clone方法中去构建克隆的对象。

也可以通过序列化对象，把对象转化为输出流，在转化为数据流，在获取到对象，来实现深克隆

#### 序列化实现

通过把对象序列化成二进制流，再反序列化就可以实现深拷贝



## ==、eqauls、hashcode

- ==
  - 如果是基本数据类型用==，就是比较他们的值，如果对象比较用==，就是比较他们的内存地址是否相等。
- eqauls
  - equals()用来判断两个对象是否相等。默认判断两个对象的地址是否相等。可以通过重写来实现比较对象的内容。
- hashcode
  - 返回与这个对象的存储地址和字段等相关的一个值。不同的对象可能生成相同的hashcode值。hashcode值不同，肯定是不同的对象。

## 哈希碰撞和哈希冲突

碰撞指哈希表中计算key，得到了和另一个key相同的hashCode

碰撞强调，计算出同一个索引位置。

## HashMap中的key为什么重写eqauls和hashcode

HashMap比较key的时候，先对key的使用hashcode()，比较其值是否相等，**若hash值相等再用==和equals()比较key**。若不等就认为他们不相等。

自定义的类的hashcode()方法继承与Object类，返回与这个对象的存储地址和字段等相关的一个值。所以逻辑上相等的对象，默认的hashcode()并不相等。

如果认为逻辑上相等的对象就是相等的。那么需要重写类的hashcode()和equals()。如果HashMap判断hashcode()相等后会接着判断equals()，equals()默认是比较对象地址是否相等，所以也要重写equals()，确保相同的对象的hashcode()和equals()是相等的。

hashmap中的判断是否是同一个key的代码

~~~java
if (p.hash == hash && ((k = p.key) == key || (key != null && key.equals(k))))
~~~

规定

> - 如果两个对象相等，则hashcode一定相等。
> - 如果两个对象相等，两对象相互调用equals方法都返回true。
> - 如果两个对象有相等hashcode，他们不一定相等。
> - hashCode()默认是对堆上的对象产生独特值。如果没有重写，那么这个类的两个对象无论如何都不会相等。

### 重写的案例

~~~java

~~~



## String、StringBuilder 和 StringBuffer

### String类

**字符串**：字符串是常量，一旦创建不能更改。然而String类的对象，可以指向不同的字符串。因为字符串对象虽然不能修改，但是他们的地址可以共享。

**String类被final修饰**，不能被继承。String类底层数据结构为一个char型的数组，**被final修饰的**，相当于保存的是某个字符串的内存地址。String类不能修改字符串本身，也不能修改指向字符串地址的变量。String的值是不能改变的，String类是一个不可变类。不可变类的好处是，**保证线程安全。**

new String() 和 new String(“”)都是声明一个新的空字符串，是空串不是null。

String类维护了一个字符串常量池，JDK1.6在方法区、1.7在堆内存、1.8在元空间。常量池中维护着一个存储字符串实例的引用的hash表。

1. 当使用字面值（String s = "hello";）创建时，如果常量池中有对应字符串就直接指向这个地址，如果没有就在常量池中创建字符串并指向地址。也就是说无论常量池中是否有，**引用都指向的是常量池中的字符串**。
2. 当通过new 创建字符串对象时。new String("abc")。在常量池如果没有这个字符串就创建字符串。在堆中创建一个String对象，如常量池中有对应字符串就在String对象中的数据变量就指向常量池中字符串。**无论常量池中是否有，最终都要在堆中创建一个String对象且数据变量都指向常量池中的字符串**
3. String.intern()返回的就是字符串对象在常量池中对应的字符串。
4. 字符串相加
   1. 字符串常量相加，在预编译时会被优化，自动合成一个字符串常量
   2. 字符串变量相加（a += "a"），本质是new了StringBuilder对象进行append操作，然后调用toString()返回String对象

~~~java
String s1 = "111";
String s2 = "111";
System.out.println(s1==s2);//true

String s1 = "111";
String s2 = new String("111");
System.out.println(s1==s2);//false

String s1 = new String("111");
String s2 = new String("111");
System.out.println(s1==s2);//false

String s1 = "111111";
String s2 = "111"+"111";
System.out.println(s1==s2);//true

String s1 = "111111";
String a = "111";
String b = "111";
String s2 = a+b;
System.out.println(s1==s2);//false
~~~



因为字符串是不可变的，所以不用担心线程安全问题。

- StringBuilder

StringBuilder类中修改字符串是针对对象本身，不会像String去生成新的对象重新赋给变量引用。它不是线程安全的。

默认字符数组长度为参数长度+16，扩容时先长度*2+2，如果还不够就直接扩容到需要的长度。

- StringBuffer

StringBuffer类是线程安全的，他的很多方法用了synchronized。

## Java8的字符串拼接

新增了三种方式：

### StringJoiner

JDK1.8，添加了一个新的用于字符串拼接的类，适用于需要分隔符的场景——StringJoiner。在构造时可以指定一个分隔符，然后每连接一个元素，它便会加上一个分隔符。还可以指定前缀和后缀。StringJoiner本质是一个StringBuilder。

~~~java
public static String formatList(List<String> list, String delimiter) {
    StringJoiner result = new StringJoiner(delimiter);
    for (String str : list) {
        result.add(str);
    }
    return result.toString();
}
~~~

### String.join()

JDK1.8还为String类添加了一个新的静态方法——String.join。第一个参数的分隔符，第二个参数是Iterable（实现可迭代）。

### Collectors.joining()

JDK1.8还提供了对于字符串集合的连接操作的专门的流式API——Collectors.joining。

- 无参的 joining() 方法，即不存在连接符（底层实现为 StringBuilder）；
- joining(CharSequence delimiter) 方法，即分隔符为 delimiter（底层实现为 StringJoiner）；
- joining(CharSequence delimiter, CharSequence prefix, CharSequence suffix)方法，即分隔符为 delimiter，前缀为 prefix，后缀为 suffix（底层实现为 StringJoiner）。

~~~java
public static String formatList(
        List<String> list, String delimiter, String prefix, String suffix) {

    return list.stream().collect(Collectors.joining(delimiter, prefix, suffix));
}

public static void main(String[] args) throws Exception {
    List<String> list = Arrays.asList("a", "b", "c", "d", "e", "f", "g");

    System.out.println("使用 Collectors.joining：");
    String format = formatList(list, ", ", "{ ", " }");
    System.out.println(format);
}
~~~



## valueOf判空

~~~java
public static String valueOf(Object obj) {
    return (obj == null) ? "null" : obj.toString();
}
~~~

String.valueOf会对传进来的对象进行判空，如果是空就返回字符串null，否则返回对象的toString()方法。

而null调用toString()方法会报异常。

## 包装类

### 为什么要有包装类

- 万物皆对象，方便使用和管理
- 有些操作要求必须是对象，比如泛型
- 便于完成类型转换

### 什么是装箱拆箱

- 装箱：基本类型转换为包装类型。
- 拆箱：包装类型转换为基本类型。

- 自动装箱：基本数据类型的值赋给包装类对象，会执行隐式的装箱（valueOf）。
- 主动拆箱：包装类对象的值赋给基本数据类型，会执行隐式的拆箱（intValue等等方法）。



### 缓存

包装类有一个缓存，将常用的一定范围的包装类对象存储在缓存中，在装箱时如果缓存中存在，就引用已有的，如果不存在就创建新的对象。

Integer、Byte、Short、Integer、Long、Character。默认缓存中有**[-128,127]**。两个浮点数类型的没有常量池。

拿Integer为例

用一个静态内部类来存储缓存对象，把**[-128,127]**值的对象先存储在Integer数组cache[]中。

在进行装箱时，会优先从缓存中去取包装类对象。

~~~java
public static Integer valueOf(int i) {
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }
~~~









### Integer比较可以用==吗

在两个Integer的值在**[-128,127]**时，用==是返回true的，因为地址是一样的。如果不是这个范围就推荐用equals吧。



## for和foreach

foreach适用于数组或者实现了iterator的集合类。用foreach循环遍历一个数组时，不能改变集合中的元素，如增加元素、修改元素。否则会抛出ConcurrentModificationException异常，这就是单线程状态下产生的 fail-fast 机制。

### 迭代器

迭代器中的引用的数据和结合中是同一份。在迭代器中维护一个自己的操作计数，当remove时自己的计数和集合的计数同步，如果在next或者remove时发现两个计数不一致，说明集合在迭代过程中改动过，就会抛出并发修改异常。

fail-fast 机制：在迭代器迭代过程中，如果对集合进行修改的时，可能会抛出ConcurrentModificationException，单线程下也会出现这种情况，上面已经提到过。是一种错误检测机制。

迭代器：遍历方便，删除方便；（Iterator只有3个方法：hasNext()、next()、remove()）

普通for：遍历时可删除，可修改；

增强for：遍历最方便，不可编辑。（实际上调用的还是迭代器）











## 设计模式

三大类：

- 创建型模式：单例模式、抽象工厂模式、工厂方法模式、建造者模式、原型模式
- 结构性模式：适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式
- 行为型模式：策略模式、模板方法模式、观察者模式、迭代子模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式





## Java实现回调

java提供了 接口帮我们实现 回调函数，俗称 接口回调。

一般使用就是：一个方法的参数中包含一个接口，这个方法在执行中会调用传入接口实现的方法。所以可以通过给方法传入接口来实现，自己期望的方法在之后被执行。

这个父亲有一个儿子，一个女孩，并且父亲开始动筷子了，他们两个才可以动。

~~~java
package zt;

/**
 * 接口回调
 */
public final class App {
    public static void main(String[] args) {
        new Father(new Son(),new Sister()).start();;
    }
}

interface Start{
    void Fstart(Object obj);
}

/**
 * 父亲类，里面有个start函数，表示开始动筷子
 */
class Father{

    private Sister sister;
    private Son son;

    Father(Son son,Sister sister){
        this.son= son;
        this.sister = sister;
    }

    public void start(){
        System.out.println("父亲开始动筷子了");
        son.Fstart("父亲动了筷子");
        sister.Fstart("父亲动了筷子");
    }
}
/**
 * 儿子类，里面有个start函数，表示开始动筷子
 */
class Son implements Start{
    private void start(){
        System.out.println("儿子可以开始动筷子了");
    }

    @Override
    public void Fstart(Object obj) {
        if(obj.toString().equals("父亲动了筷子")){
            start();
        }
    }
}
/**
 * 姐姐类，里面有个start函数，表示开始动筷子
 */
class Sister implements Start{
    private void start(){
        System.out.println("姐姐可以开始动筷子了");
    }

    @Override
    public void Fstart(Object obj) {
        if(obj.toString().equals("父亲动了筷子")){
            start();
        }
    }
}
~~~



## Java IO中涉及到的哪些类以及哪些设计模式

涉及到的类主要有FileInputStream ，InputStreamReader ，BufferedReader 。

###### 装饰者模式

动态地将责任附加到对象上，若要扩展功能，装饰者模提供了比继承更有弹性的替代方案。
通俗的解释：装饰模式就是给一个对象增加一些新的功能，而且是动态的，要求装饰对象和被装饰对象实现同一个接口，装饰对象持有被装饰对象的实例。

###### 适配器模式

将一个类的接口，转换成客户期望的另一个接口。适配器让原本接口不兼容的类可以合作无间。
适配器模式有三种：类的适配器模式、对象的适配器模式、接口的适配器模式。
通俗的说法：适配器模式将某个类的接口转换成客户端期望的另一个接口表示，目的是消除由于接口不匹配所造成的类的兼容性问题。

1、适配器模式 
//file 为已定义好的文件流 

~~~java
FileInputStream fileInput = new FileInputStream(file); 
InputStreamReader inputStreamReader = new InputStreamReader(fileInput);
~~~

以上就是适配器模式的体现，FileInputStream是字节流，而并没有字符流读取字符的一些api，因此通过InputStreamReader将其转为Reader子类，因此有了可以操作文本的文件方法。换句话说，就是将FileInputStream读取一个字节流的方法扩展转换为InputStreamReader读取一个字符流的功能。
2、装饰者模式

~~~java
BufferedReader bufferedReader=new BufferedReader(inputStreamReader);
~~~

构造了缓冲字符流，将FileInputStream字节流包装为BufferedReader过程就是装饰的过程，刚开始的字节流FileInputStream只有read一个字节的方法，包装为inputStreamReader后，就有了读取一个字符的功能，在包装为BufferedReader后，就拥有了read一行字符的功能。



## IO

### 全面认识

传统的IO大致分为四种类型

- InputStream、OutputStream基于字节操作的IO
- Writer、Reader基于字符操作的IO
- File基于磁盘操作的IO
- Socket基于网络操作的IO

划分

- 按照流的流向划分
  - 输入流
  - 输出流
- 按照操作单元划分
  - 字节流
  - 字符流
- 按照流的角色划分
  - 节点流
  - 处理流

java中IO流涉及40多个类。从下面4个抽象类中派生出来。

- InputStream/Reader：所有输入流的基类。前者是字节输入流，后者是字符输入流。
- OutputStream/Writer：所有输出流的基类。前者是字节流输出流，后者是字符输出流。

如果是音频文件、图片等媒体文件用字节流比较好，如果涉及到字符的话使用字符流比较好。

### IO使用

接下来看 InputStream、OutputStream、Writer、Reader 的继承关系图和使用示例。

#### InputStream的使用

![img](Java基础.assets/javacore-io-001.png)

~~~java
InputStream inputStream = new FileInputStream("D:\\log.txt");
byte[] bytes = new byte[inputStream.available()];
inputStream.read(bytes);
String str = new String(bytes, "utf-8");
System.out.println(str);
inputStream.close();
~~~

#### OutputStream的使用

![img](Java基础.assets/javacore-io-002.png)

~~~java
OutputStream outputStream = new FileOutputStream("D:\\log.txt",true); // 参数二，表示是否追加，true=追加
outputStream.write("你好，老王".getBytes("utf-8"));
outputStream.close();
~~~

#### Writer的使用

![img](Java基础.assets/javacore-io-004.png)

~~~java
Writer writer = new FileWriter("D:\\log.txt",true); // 参数二，是否追加文件，true=追加
writer.append("老王，你好");
writer.close();
~~~

#### Reader的使用

![img](Java基础.assets/javacore-io-003.png)

~~~java
Reader reader = new FileReader(filePath);
BufferedReader bufferedReader = new BufferedReader(reader);
StringBuffer bf = new StringBuffer();
String str;
while ((str = bufferedReader.readLine()) != null) {
    bf.append(str + "\n");
}
bufferedReader.close();
reader.close();
System.out.println(bf.toString());
~~~



## 同步、异步、阻塞、非阻塞

### 同步与异步

同步：一个任务的完成需要依赖另一个任务。要么都成功要么都失败，状态可以保持一致。

异步：一个任务的完成不需要一个被依赖的任务的完成，只要通知了被依赖的任务要做什么，然后把自己的任务完成就好了。被依赖的任务是否真的完成，依赖它的任务并不知道。

同步异步强调被调用方的执行和返回方式。所以对调用者的影响很大。

同步就是一个任务的完成需要依赖另一个任务，是一种可靠地任务序列。你调用这个任务，他要把他的事情做完，把全部结果返回给你才结束。

异步不需要等待被依赖的任务完成，只通知被依赖的任务要完成什么工作。你调用一个任务，他立刻给你返回，做任务的过程和结果，他自己去完成，想办法告诉你。

### 阻塞与非阻塞

阻塞：CPU停下来等待一个慢的操作完成

非阻塞：慢的操作在执行时CPU去做其他的事

阻塞非阻塞强调调用方如何处理。

阻塞就是调用方调用一个任务，要拿到任务的返回结果才继续做后面的事。

非阻塞就是调用方调用一个任务，接下来就去做别的是，等任务完成之后再接着做该做接下来的事。一般是通过线程切换，这样导致上下文切换次数多。

### 结合

同步阻塞，最常见最简单的。（BIO）

同步非阻塞，通过多线程的方式实现非阻塞。线程过多会导致cpu资源消耗大。（多线程BIO、NIO）

异步阻塞，比如数据库从库的同步，一般是通过异步阻塞。（多路复用IO）

异步非阻塞，很复杂。

## 优雅的文件读写

### Java7之前

~~~java
// 添加文件
FileWriter fileWriter = new FileWriter(filePath, true);
fileWriter.write(Content);
fileWriter.close();

// 读取文件
FileReader fileReader = new FileReader(filePath);
BufferedReader bufferedReader = new BufferedReader(fileReader);
StringBuffer bf = new StringBuffer();
String str;
while ((str = bufferedReader.readLine()) != null) {
    bf.append(str + "\n");
}
bufferedReader.close();
fileReader.close();
System.out.println(bf.toString());
~~~

Java 7 引入了Files（java.nio包下）的，大大简化了文件的读写，如下：

~~~java
// 写入文件（追加方式：StandardOpenOption.APPEND）
Files.write(Paths.get(filePath), Content.getBytes(StandardCharsets.UTF_8), StandardOpenOption.APPEND);

// 读取文件
byte[] data = Files.readAllBytes(Paths.get(filePath));
System.out.println(new String(data, StandardCharsets.UTF_8));
~~~

Files 下还有很多有用的方法，比如创建多层文件夹，写法上也简单了:

~~~java
// 创建多（单）层目录（如果不存在创建，存在不会报错）
new File("D://a//b").mkdirs();
~~~





## Socket

Socket是描述计算机之间完成相互通信的一种抽象功能。在代码中，可以用一个Socket实例唯一代表一个主机上的一个应用程序的通信链路。

### 建立通信链路

客户端与服务器端通信，**客户端要创建一个Socket实例**，操作系统为这个Socket实例分配一个没有被使用过的本地端口号，并创建一个包含本地和远程地址和端口号的套接字数据结构。这个数据结构一直保存在系统中，直到这个连接关闭。在创建Socket市里的构造函数返回正确之前，要进行TCP的三次握手，握手完，Socket实例对象将创建完成，否则抛出IOException错误。

服务端创建一个ServerSocket实例，ServerSocket创建只要指定的端口号没有被占用，一般实例创建就会成功。操作系统为ServerSocket实例创建一个底层数据结构，包含指定监听的端口号，和包含监听地址的通配符。当调用accept()方法时，将进入阻塞状态，等待客户端的请求。当一个新的请求到来时，将这个连接创建一个新的套接字数据结构，这个套接字的信息包含的地址和端口信息就是请求的。新创建的套接字的数据关联到ServerSocket的一个未完成的列表中（正在与客户端建立TCP连接），三次握手成功后，才返回Socket实例。

### 数据传输

当连接建立成功，服务端和客户端都会有一个Socket实例，每个Socket实例都会有一个InputStream和OutputStream。网络I/O都是以字节流传输的。操作系统会为 InputStream 和 OutputStream 分别分配一定大小的缓冲区，数据的写入和读取都是通过这个缓存区完成的。写入端将数据写到 OutputStream 对应的 SendQ 队列中，当队列填满时，数据将被发送到另一端 InputStream 的 RecvQ 队列中，如果这时 RecvQ 已经满了，那么 OutputStream 的 write 方法将会阻塞直到 RecvQ 队列有足够的空间容纳 SendQ 发送的数据。值得特别注意的是，这个缓存区的大小以及写入端的速度和读取端的速度非常影响这个连接的数据传输效率，由于可能会发生阻塞，所以网络 I/O 与磁盘 I/O 在数据的写入和读取还要有一个协调的过程，如果两边同时传送数据时可能会产生死锁，在后面 NIO 部分将介绍避免这种情况。

https://developer.ibm.com/zh/articles/j-lo-javaio/



## 反射

**反射是Java被视为动态语言的关键**。有了反射可以**在运行时获取类属性和执行类方法**后，在堆内存中生成一个Class类型的对象，这个对象包含了完整的类的结构信息。通过这个Class类得到类的信息，就是反射。

### 好处

增加程序的灵活性，避免将程序写死到代码里。

比如通过new创建对象，这种写死的方式可以改为，通过反射根据类名字去实例化对象，名字写在配置文件中让程序去读取。这样就减少了程序的改动。



### 如何获取Class

- **调用对象的getClass()方法**。这个方法所有类的父类Object类定义的方法，不能被重写。
- 通过**类的class属性**获取。
- **用类的名字通过Class.forName("...")获取**。可能抛出ClassNotFoundException
- 基本数据类型可以用类名.Type获取
- 通过类加载器

### 用Class对象做什么

- **创建对应类的实例**
  - newInstance()。需要无参构造器，如果没有，需要其他方式，获取构造器传入指定类型的参数，然后用过Constructor实例化对象。
- **调用类的方法**
  - 通过Class类的getMethod(String name,Class...parameterTypes)方法获取一个Method对象。设置好参数。使用Object invoke(Object obj,Object[] args)进行调用，并向方法中传递设置的obj对象的参数信息。Object 为方法的返回值



### 为什么反射创建对象慢

是因为编译器无法对发射相关的代码做优化。即时编译器（JIT）在运行期间将字节码文件编译成机器码，并做了一些优化。反射涉及动态解析的类型，因此无法执行某些优化。