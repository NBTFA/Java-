# Java基础

Owner: Terrence
Verification: Verified
Tags: Java
Created time: January 15, 2024 2:00 PM

# 1. Java基础

## 1.1 为什么Java代码可以实现一次编写，到处运行？

Java之所以可以实现一次编写，到处运行，是因为Java采用了一种称为"Write Once, Run Anywhere"（一次编写，到处运行）的原理。

JVM(Java Virtual Machine)是java跨平台的关键。

.java(java源代码) —> .class(字节码) —>特定平台的机器码

编译器：.java —> .class

JVM：.class —> 机器码

JVM是实现跨平台的关键，java程序是跨平台的，JVM不是

## 1.2 一个java文件里可以有多个类吗（不含内部类）？

一个java文件里可以有多个类，但只能有一个被public修饰的类

包含public修饰的类，他的名字需要和java文件名一致

修饰类时只能使用default和public

default：可以被同一包下其他类访问

public：可以被任意访问

## 1.3 java成员变量/成员方法访问权限

|  | 类内部 | 本包 | 子类 | 外部包 |
| --- | --- | --- | --- | --- |
| public  | ✅ | ✅ | ✅ | ✅ |
| protected | ✅ | ✅ | ✅ | ❌ |
| default | ✅ | ✅ | ❌ | ❌ |
| private | ✅ | ❌ | ❌ | ❌ |

private：可以被内部成员访问

default：可以被内部成员访问，也可以被同一包下的其他类访问

protected：可以被内部成员访问，也可以被同一包下的其他类访问，还可以被子类访问

public：可以被任意类访问

## 1.4 java数据类型

基本数据类型

- 整数类型(byte/short/int/long)
- 浮点类型(float/double)
- 字符类型(char)
- 布尔类型(boolean)

除了布尔类型，其他都是数字类型，可以进行类型转换

引用类型

- 数组
- 类
- 接口

通过指针，指向堆中对象所持有的内存空间

|  | 字节 | 数据范围 | 默认值 |
| --- | --- | --- | --- |
| byte | 1 byte(8 bits) | $-2^7$ ~ $2^7-1$ | 0 |
| short | 2 byte(16 bits) |  -2^15 ~ 2^15-1 | 0 |
| int | 4 byte(32 bits) | -2^31 ~ 2^31-1 | 0 |
| long | 8 byte(64 bits) | -2^63 ~ 2^63-1 | 0L |
| float | 4 byte(32 bits) | -3.4*10^38 ~ 3.4*10^38 | 0.0F |
| double | 8 byte(64 bits) | -1.8*10^308 ~ 1.8*10^308 | 0.0 |
| char | 2 byte(16 bits) | \u0000 ~ \uffff | ‘\u0000’ |
| boolean | none | none | false |

## 1.5 全局变量和局部变量的区别

|  | 全局变量 | 局部变量 |
| --- | --- | --- |
| 定义位置 | 类中，但是方法外 | 在方法内或者方法参数中 |
| 访问范围 | 在类的任何地方都可以访问 | 只能在定义他的方法内访问 |
| 生命周期 | 与对象的生命周期一致 | 与方法的执行过程一致 |
| 存储位置 | 堆内存 | 栈内存 |
| 默认值 | 有默认值，int为0，对象为null | 无默认值，使用前必须初始化 |
| 访问修饰符 | 可以用任何访问修饰符（public，private） | 不能使用修饰符 |

## 1.6 包装类

包装类将java基本数据类型封装成对象

1. **对象操作：**包装类允许将基本类型作为对象处理
2. **集合框架：**java的集合框架（arraylist，hashmap等）只能存储对象，包装类可以使集合中存储基本类型的数据
3. **提供更多方法：**例如，可以将一个字符串转换为一个基本类型的值，或者检查一个数值的大小
4. **************************************null值的支持：**************************************基本类型不能赋值`null`，他们的包装类可以
5. ********************泛型支持：********************java泛型不支持基本数据类型，通过包装类，可以在泛型如`List<Integer>`中使用基本数据类型

### 1.6.1 自动装箱、自动拆箱

自动装箱：把一个基本类型的数据直接赋值给对应的包装类型

自动拆箱：把一个包装类型的对象直接赋值给对应的基本类型

### 1.6.2 如何对Integer和Double类型判断相等

1. 不能使用==进行比较，因为他们是不同的数据类型
2. 不能转为字符串比较，因为浮点值带小数点，整数不带，永远不相等
3. 不能使用compareTo比较，compareTo只能用于相同类型比较

正确做法：整数、浮点类型的包装类，都继承于Number。Number定义了将数字转换为byte、short、int、long、float、double的方法。

```java
Integer i = 100;
Double d = 100.00;
i.doubleValue() == d.doubleValue();
```

### 1.6.3 如何比较Integer和int

```java
int a = 5;
Integer b = new Integer(5);
System.out.println(a==b);
```

自动拆箱会把b变成int

### 1.6.4 如何比较Integer和Integer

```java
Integer a = new Integer(5);
Integer b = new Integer(5);
System.out.println(a.equals(b));
```

对象比较要用equals()

## 1.7 面向对象

将软件视为一系列互相作用的对象

- 核心概念
    1. 对象（object）
        - 对象是类的实例，它包含了数据和能够操作这些数据的方法。
    2. 类（class）
        - 类是创建对象的蓝图或模版，它定义了对象的数据和方法。
    3. 封装（Encapsulation）
        - 将数据（属性）和代码（方法）绑定，隐藏对象的内部细节。
        - 隐藏类的实现细节
        
        ```java
        public class Person{
        
        	private String name;
        	private int age;
        
        	public String getName(){
        		return name;
        	}
        
        }
        ```
        
    4. 继承（Inheritance）
        - 子类继承父类的特性，并可以有自己的独特特性。
        
        ```java
        //父类
        public class Animal{
        	public void eat(){
        		System.out.println("animal is eating");
        	}
        }
        
        //子类
        public class Dog extends Animal{
        	public void bark(){
        		System.out.println("dog is barking");
        	}
        }
        
        Dog dog = new Dog();
        dog.eat();
        dog.bark();
        ```
        
    5. 多态（Polymorphism）
        - 多态意味着一个类的借口的不同实现。
        - 在如下例子中，dog编译时的类型是Animal，运行时的类型是Dog。
        
        ```java
        public class Animal{
        	public void eat(Shit shit){
        		System.out.println("dog is eating");
        	}
        	public void eat(Food food){
        		System.out.println("human is eating");
        	}
        }
        
        Animal dog = new Dog(shit);
        dog.eat();//Dog is eating
        ```
        
    6. 抽象（Abstraction）
        - 隐藏具体实现，只展示必要的信息。
        
        ```java
        //抽象类
        abstract class Shape{
        	//抽象方法
        	abstract void draw();
        }
        
        //继承抽象类的子类
        class Circle extends Shape{
        	@Override
        	void draw(){
        		System.out.println("Drawing a circle");
        	}
        }
        
        Shape shape = new Circle();
        shape.draw();
        ```
        

## 1.8 Java为什么是单继承，为什么不能多继承

单继承：一个类只能有一个直接的父类

多继承：一个类继承多个父类

Java没有多继承是因为多继承容易产生混淆，比如，两个父类中包含相同方法时，子类在调用该方法或重写该方法时会迷惑。

## 1.9 重写（Override）和重载（Overload）

1. 重写发生在子类父类中，如果子类方法想和父类方法构成重写关系，则他的方法名、参数列表必须与父类相同。返回值要≤父类方法，异常要≤父类方法，修饰符要≥父类方法。如果父类为private，子类无法对其重写。

```java
//父类
public class Animal{
	public void eat(){
		System.out.println("animal is eating");
	}
}

//子类
public class Dog extends Animal{
	@Override
	public void eat(){
		System.out.println("dog is barking");
	}
}

Dog dog = new Dog();
dog.eat();
```

1. 重载发生在同一个类中，多个方法拥有相同名字但参数列表不同。

```java
class MathOperation{
	public int multiply(int a, int b){
		return a*b;
	}

	public int multiply(int a, int b, int c){
		return a*b*c;
	}
}

MathOperation op = new MathOperation();
op.multiply(4, 5);
op.multiply(1, 2, 3);
```

1. 构造方法不能重写。因为构造方法需要和类同名，重写需要子类方法和父类方法同名。如果允许重写构造方法，那么子类中将会存在于类名不同的构造方法。

## 1.10 Object类

### Object类是所有类的根类。所有类都使用`Object`作为super类。所有对象（包括数组）都实现了这个类的方法。

- Class<?> getClass()：返回该对象的运行时类。
- boolean equals(Object obj)：判断对象与对象是否相等
- int hashCode()：返回hashCode值。默认情况下Object类的hashCode()方法根据对象的地址来计算。
- String toString()：返回String。当使用System.out.println()方法输出对象的时候，系统会自动调用toString()返回String。Object类的toString()方法返回**`运行时类名@十六进制hashCode值`**格式的字符串。
- Object clone()：返回对象的一个副本，并不是原对象。

## 1.11 hashCode()和equals()的关系

hashCode()：返回对象的hashCode，即一个整数值。

equals()：默认实现是比较对象的引用，但通常被重写以比较对象内容。

### 他们的关系

****一致性：****如果两个对象通过`equals()`方法比较是相等的，那么他们的hashCode()必须返回相同的值

******************************************************************非一致性：******************************************************************如果两个对象通过`equals()`方法比较不相等，那么他们hashCode()可以返回相同或不同的值。

### HashSet加入元素

当向HashSet中加入元素时，他会获取对象的hashCode()，然后调用equals()方法来判断是否存在该元素。

### 为什么要重写hashCode()和equals()

因为Object类的equals()方法默认使用==比较，这样比较的是地址，他们是否为同一对象而并非是内容。

### ==和equals()有什么区别？

==：

- 用于基本数据类型的时候，比较的是两个数值是否相等。
- 用于引用数据类型的时候，比较的是两个对象的内存地址是否相同，即判断他们是否为同一对象。

equals()：

- 没有重写时，默认使用==实现。
- 重写后，一般会按照对象的内容来比较。

## 1.12 String类

### String可以被继承吗？

String类是一个final类，这意味着他不能被继承。

1. 不变性：String对象一旦被创建，他的值就不能被改变
2. 安全性：String被广泛用于参数传递，网络通信等，不变性可以防止值被改变，增加安全性。多线程中，只有不变的对象和值是线程安全的，可以在多个线程中共享数据。
3. 性能：字符串常量池，减少创建相同字面量的字符串，让不同的引用指向池中的同一字符串，节约很多堆内存。

### String和StringBuffer的区别

String类是不可变类，一旦String对象被创建后，包含在这个对象中的字符序列是不可改变的。

StringBuffer对象则代表一个字符序列可变的字符串，当一个StringBuffer被创建后，通过StringBuffer提供的方法可以改变这个对象的字符序列，并通过toString()方法转换为String对象。

### StringBuffer和StringBuilder的区别

StringBuffer和StringBuilder都是可变的字符串对象，他们有共同的父类`AbstractStringBuilder`，并且两个类的构造方法和成员方法也基本相同。不同的是，StringBuffer是线程安全的，而StringBuilder是非线程安全的，所以StringBuilder的性能略高。

### 使用字符串时，new和””推荐使用哪种？

“hello”

- 当java直接使用”hello”时，JVM会使用常量池来管理这个字符串。

new String(”hello”)

- JVM会先使用常量池来管理”hello”直接量，在调用String类构造器来创建一个新的String对象，新创建的String对象被保存在堆内存中。

**new的方式会多创建一个对象出来，会占用更多的内存，所以一般使用直接量的方式创建字符串。**

### 字符串拼接

1. +运算符：如果拼接的是字符串直接量，则使用+运算符。”he”+”llo”。
    - 如果拼接的都是直接量，那么编译器会优化为一个完整的字符串。”he”+”llo”=”hello”
    - 如果包含变量，那么编译器会使用StringBuilder并调用append()方法。如果在循环中，就相当于每次循环都执行`new StringBuilder().append(str)`，效率很低。
2. StringBuilder/StringBuffer：如果两个字符串中包含变量，并不要求线程安全，使用StringBuilder。如果两个字符串中包含变量，要求线程安全，使用StringBuffer。
    - StringBuilder/StringBuffer都有字符串缓冲区，缓冲区的容量在创建对象时确定，默认为16。
    - 当拼接的字符串超过缓冲区容量，那么会出发扩容机制。频繁的扩容会降低拼接的性能，所以最好能够提前制定缓冲区容量为预估的字符串长度。
3. String类的concat方法：如果只是对两个字符串进行拼接，包含变量，则使用concat。
    - 先创建一个足以容纳待拼接字符串的字节数组（charArray），然后把字符串拼到数组里，最后转换为字符串。
    - 大量字符串时，concat效率低于StringBuilder。只拼接两个字符串时，concat效率优于StringBuilder。

### String a = “abc”;

JVM会使用常量池来管理字符串直接量。

1. JVM检查常量池中是否已经有“abc”
2. 如果没有将“abc”放入常量池，否则就复用常量池中的“abc”
3. 将引用赋值给变量a

### new String(”abc”)

1. JVM使用常量池
2. 创建新的String对象，数据会指向常量池中的直接量，对象会被保存在堆内存中

## 1.13 接口和抽象类

接口是一种规范，规定了实现者必须向外提供哪些服务；对于调用者而言，接口规定了可以调用哪些服务，以及如何调用服务。当在一个程序中使用接口，接口时多个模块间的耦合标准。当在多个应用使用接口，接口时多个程序的通信标准。使用implements

抽象类时一种模版式设计。是系统实现过程中的中间产品，必须有进一步的完善。使用extends

- 接口只能包含抽象方法（abstract），静态方法（static），默认方法（default）和私有方法（private）；抽象类可以包含普通方法。
- 接口只能定义静态常量，不能定义普通成员变量；抽象类可以定义普通成员变量也可以定义静态常量。
- 接口不包含构造器；抽象类可以包含构造器，不是为了创建对象，而是让子类调用构造器来完成抽象类的初始化操作。
- 接口不能包含初始化块；抽象类可以。
- 一个类最多只能有一个父类，包括抽象类；但一个类可以实现多个接口。

## 1.14 异常

### Java异常机制

1. 异常处理
    - Java异常处理由try, catch, 和finally组成。
    - try用于包裹业务代码，catch用于捕获并处理异常，finally用于回收资源。
    - 当系统代码异常，系统会创建异常对象，JVM寻找可以处理这个异常的catch块。
    - 无论是否发生异常，finally一定会执行。
2. 抛出异常
    - Java允许程序主动抛出异常，使用throw。
3. 异常跟踪栈
    - 异常从发生异常的方法向外传播。

### Java异常接口

Throwable（异常顶层父类）

- Error
    - 一般指与虚拟机相关的问题，如系统崩溃，虚拟机错误，动态连接失败等。
    - 无法恢复或不可能捕捉，将导致程序中断，因此不该用catch来捕获Error。
- Exception
    - Checked异常
        - 需要使用`throws`声明，否则编译器会报错。
        - 示例：`IOException`, `SQLException`等
    - Runtime异常
        - RuntimeException类及其子类都为Runtime异常
        - 运行时可能被抛出的异常。
        - 示例：`NullPointerException`, `IndexOutOfBoundsException`等

### finally

- 无论try，catch是否异常捕获，或者是否return。Finally都会执行。除了在try，catch中使用`System.exit(1);`
- 在finally中使用return，throw语句会导致try，catch中的return，throw语句失效。
    - 系统会寻找是否有finally，如果有系统会立即开始执行finally，只有当执行完成后，才会执行try，catch中的return或throw语句。

## 1.15 static修饰的类

1. 非内部类
    - 不能被声明为static
2. 静态内部类（Static Nested Class）
    - 可以被继承。因为他们是静态的，所以不需要外部类的实例就可以被创建和使用，他们可以被看作是其外部类的一个静态成员。
    - 不能访问外部类的实例成员，只能访问静态成员
    
    ```java
    public class OuterClass{
    	//静态内部类
    	public static class StaticNestedClass{
    		public void display() {
    			System.out.println("11");
    		}
    	}
    }
    
    //继承静态内部类
    public class ExtendedClass extends OuterClass.StaticNestedClass {
    	@Override
    	public void display() {
    		System.out.println("22");
    	}
    }
    ```
    

### static和final的区别

static

- 声明static的都属于类本身，而非某个实例
- 静态变量：可以被所有类的实例共享，在类第一次被加载的时候创建，程序运行期间只有一份拷贝。
- 静态方法：可以在没有类实例下被调用，只能访问静态成员。

```java
public class MyClass {
	public static int a;//可以使用
	public int b;//不能使用

	public static int staticMethod() {
		return a;
	}
}
```

final

- 不能被更改或继承
- final变量：无法修改
- final方法：无法被重写
- final类：无法被继承

## 1.16 泛型

Java集合在编译时会把对象的编译类型变成Object类型。当取出集合元素的时候需要进行类型转换

`List<String>` 这是一个泛型列表，可以帮助集合自动记住元素的数据类型，无需进行强制类型转换

### 泛型擦除

```java
List<String> list1 = ...;
List list2 = list1;

for(Object item : list2) {
	String str = (String) item; //强制类型转换
}

List list1 = ...;
List<String> list2 = list1; //编译时警告“未经检查的转换“
```

在编译时，list2中将为Object类型。当需要取出时，需要进行强制类型转换。

### List<? extends T>和List<? super T>

List<? extends T>

- 这种list可以接受T或者T的任何子类。上界通配符。
- 读取是安全的，因为无论是T的哪个子类，都可以把它当成T。
- 写入是不安全的，无法确定List到底接受哪种具体子类
- 用于安全读取数据，例如在设计只读方法。

```java
List<? extends Number> numList = new ArrayList<Integer>();
Number num = numList.get(0);
//不可以写入具体数据类型，因为无法确定，只能用Number
```

List<? super T>

- 这种List可以接受T和T任何父类，下界通配符。
- 读取是不安全的，因为可能是T的任何父类，无法确定返回对象。
- 写入是安全的，无论List具体类型，它至少接受T。
- 用于安全写入数据，例如设计输入参数时。

```java
List<? super Integer> intList = new ArrayList<Number>();
Object obj = intList.get(0);
//可以写入Integer，因为无论List具体是什么，至少接受Integer
intList.add(new Integer(10));
```

## 1.17 Java反射机制

反射允许绕过访问控制，访问或修改私有字段的值。

反射允许程序在运行中检查或修改类和对象的行为。

主要通过`java.lang.reflect`包中的类和接口来实现。

```java
Class<?> clazz = Class.forName("java.util.ArrayList");
Method[] methods = clazz.getDeclaredMethods(); //获取类的methods
Field[] fields = clazz.getDeclaredFields(); //获取类的fields
Constructor<?>[] constructors = clazz.getDeclaredConstructors(); //获取类的构造器

//实例化对象
Object instance = clazz.newInstance();

//访问字段和方法
clazz = Myclass.class;
Method method = clazz.getDeclaredMethod("myMethod", String.class);
method.setAccessible(true); //如果myMethod是私有方法
method.invoke(instanceOfMyClass, "parameter");
```

反射机制是实现动态代理的基础。

动态代理允许在运行时创建一个实现了一组给定接口的新对象。

优点

- 灵活性：可以在不知道具体类的情况下与对象交互
- 通用性：可以编写不依赖于特定类的通用代码

缺点

- 性能：反射比直接的Java代码慢，因为涉及动态类型解析
- 安全：反射破坏了封装性，因为允许访问私有字段和方法
- 复杂

反射的应用场景

- JDBC：创建数据库连接，需要先通过反射机制加载数据库的驱动程序
- XML：解析出来的类是String，需要利用反射机制实例化
- AOP（面向切面编程）：是在程序运行时创建目标对象的代理类，必须使用反射实现

## 1.18 Java的四种引用方式

1. 强引用（Strong Reference）
    - 如果一个对象具有强引用，垃圾收集器不会回收
    - 当使用`new`创建一个对象时，就是创建强引用
    - 只要强引用存在，垃圾收集器不会回收该对象
2. 软引用（Soft Reference）
    - 用来描述一些还有用但非必需的对象
    - 通过`java.lang.ref.SoftReference`类实现
    - 在内存足够的情况下，垃圾收集器不会回收软引用指向的对象；如果内存不足，就会回收这些对象的内存。
    - 适合实现缓存
3. 弱引用（Weak Reference）
    - 比软引用更弱的引用
    - 通过`java.lang.ref.WeakReference`实现
    - 只要垃圾收集器发现弱引用，不管当前内存是否足够，都会回收对象的内存。
    - 适合用于实现无需显示删除的缓存，例如WeakHashMap
4. 虚引用（Phantom Reference）
    - 最弱的引用类型
    - 通过`java.lang.ref.PhantomReference`类实现，必须与`ReferenceQueue`一起使用
    - 主要用来跟踪对象被垃圾收集器回收的活动。
    - 虚引用对对象本身没有太大影响，完全类似于没有引用。