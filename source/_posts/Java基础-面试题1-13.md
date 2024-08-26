---
title: 一、Java基础-面试题1-13
date: 2024-08-24 22:32:50
tags: Java
---
##### 1 Java和C++的区别？

C++是编译型语言，Java是解释型语言。

##### 2 如何理解面向过程和面向对象？

2.1面向过程把问题分解成每一步骤，每一个步骤用函数实现，面向对象把步骤分解，步骤抽象，形成对象，通过对象的调用解决问题。

2.2面向对象的三大特点

* **封装**：抽象成一个具体的Java类

* **继承**：子类继承父类（基类）的方法，方便方法复用

* **多态**：`override`运行时多态 `overload`编译时多态

##### 3接口和抽象类的区别？

* 抽象类可以有构造器，接口不能有构造器，抽象类和接口都不能被实例化。

* 接口可以被实现，抽象类可以被继承。

* 一个类可以实现多个接口，但是只能继承一个抽象类，接口支持多重继承。

抽象类示例如下：

```java
abstract class AnimalTwo {
    // 抽象方法，子类必须实现
    abstract void makeSound();
    // 构造器
    public AnimalTwo() {
        System.out.println("I am father");
    }
    // 抽象类中的具体方法
    void breathe() {
        System.out.println("Breathing...");
    }
}

class DogTwo extends AnimalTwo {
    @Override
    void makeSound() {
        System.out.println("Bark!");
    }

    public DogTwo() {
        super();
        System.out.println("I am son");
    }

}

public class AbstractExample {
    public static void main(String[] args) {
//        AnimalTwo animalTwo = new AnimalTwo(); 抽象类不能被实例化
        DogTwo dogTwo = new DogTwo();
        dogTwo.makeSound();  // 输出：Bark!
        dogTwo.breathe();    // 输出：Breathing...
    }
}
```

接口示例如下：

```java
interface Flyable {
    void fly();
}

// 不能这样做：
// Flyable flyable = new Flyable(); // 编译错误，因为接口不能被实例化

// 这样是可以的：
class Bird implements Flyable {
    public void fly() {
        System.out.println("Bird is flying");
    }
}
public class InterfaceExample {
    public static void main(String[] args) {
        Flyable flyable = new Bird(); // 使用实现了接口的类来实例化
        flyable.fly(); // 输出：Bird is flying
    }

}
```

##### 4 Java中已经有了基本数据类型，为什么还需要包装类？

4.1区别：

* 基本类型默认值为0，false或\u0000等，包装类为null。
* 基本直接使用，不需要new，包装需要new。

4.2自动拆箱和装箱

* 自动拆箱：包装类转成基本数据类型；自动装箱：基本数据类型转换成包装类。

4.3自动拆箱和装箱应用场景：包装类型和基本类型比较大小，包装类型的运算等

```java
public class AutoBoxingUnboxingExample {
    public static void main(String[] args) {
        /*
        在 c == d 的比较中，c 和 d 是 Integer 对象，因此它们的引用地址不同，因为它们的值超过了 Java 的整数缓存范围（-128 到 127）。
        如果 c 和 d 在这个范围内，== 比较结果为 true，因为 Java 对这些值进行了缓存。
         */

        // 基本类型
        int a = 10;
        // 包装类型
        Integer b = 10;

        // 比较基本类型和包装类型
        if (a == b) {
            System.out.println("a 和 b 相等 (自动拆箱)");
        } else {
            System.out.println("a 和 b 不相等");
        }

        // 比较两个包装类型
        Integer c = 128;
        Integer d = 128;

        if (c == d) {
            System.out.println("c 和 d 引用相同");
        } else {
            System.out.println("c 和 d 引用不同");
        }

        // 使用 equals() 方法比较两个包装类型的值
        if (c.equals(d)) {
            System.out.println("c 和 d 的值相等");
        } else {
            System.out.println("c 和 d 的值不相等");
        }

        // 基本类型和包装类型的运算
        int sum = a + b; // b 自动拆箱为 int 类型
        System.out.println("a + b = " + sum);
    }
}
```

##### 5 为什么不能用float,double表示金额？

避免造成精度丢失，`Java`提供了`BigDecimal`来进行精确计算。

##### 6 为什么不能用BigDecimal中的equals方法来做值比较？

因为使用`BigDecimal`中的`equals`方法会比较值和标度，如比较0.1和0.10，他们的值虽然是一样的，但是精度是不一样的。通常使用`compareTo`进行值的比较。

##### 7 BigDecimal(double)和BigDecimal(String)有什么区别？

`BigDecimal(double)`创建出的值并不是准确的数字，而是一个近似值，而使用`BigDecimal(String)`所创建出的值就等于其本身。

如：`new BigDecimal(0.1)`所创建出的值并不等于0.1，而`BigDecimal("0.1")`创建出的值正好等于0.1。

##### 8 String、StringBuilder、StringBuffer的区别？

8.1 `String`类被声明为`final`，`final`修饰的类是不能被继承的，所以`String`类中的方法无法被重写。

8.2 ` String`类没有提供用于修改字符串内容的方法，任何对字符串的修改，都会产生一个新的`String`对象。如下：

```java
String s = "abcd"
s = s.concat("ef")
```

虽然字符串内容看似已经成功修改，但是实际上s已经创建了一个新的对象了。

![](https://qinyunjian-1316017204.cos.ap-guangzhou.myqcloud.com/images/typora/1693569145559-1464948e-b069-4234-8f03-40dba93f044b.jpeg)

所以当需要创建可变的字符串对象时，通常使用`StringBuilder`或`StringBuffer`。

8.3 String的"+"是如何实现的

```java
String s1 = "a";
String s2 = "b";
String s3 = s1 + "," + s2 //等同于(new StringBuilder()).append(s1).append(",").append(s2).toString()
```

使用`+`进行字符拼接，实际上是通过`StringBuilder`的`append`方法进行处理的。

8.4 为什么不要再循环中频繁的使用字符串拼接，而是使用`StringBuffer`和`StringBuilder`进行替代

因为每次循坏都会创建临时对象，造成性能下降和内存浪费，如下：

```java
String result = "";
for (int i = 0; i < 1000; i++) {
    result += i;  //等同于(new StringBuilder()).append(result).append(i).toString()
}
```

##### 9 String为什么设计成不可变的？

可以从缓存，安全性，线程安全等角度进行解释。

9.1 **缓存**：`Java`中会存在一个字符串常量池，当创建对象时，常量池会先检查是否已经存在改对象，如已经存在则指向同一对象，如不存在则创建新的对象，这种机制依赖于`String`的不可变性。

9.2 **安全性**：在实际应用中，用户密钥，文件路径等敏感信息都是用`String`类来进行存储的，如果`String`类是可变的，在某些情况下内容会被恶意篡改，从而引发安全性问题。

9.3 **线程安全**：不可变对象在多个线程之间共享，它们的线程是安全的，当某个线程更改了值，会在字符串常量池中创建一个新的字符串，而不是修改相同的值，因此，字符串对于多线程来说是安全的。

##### 10 String str=new String(hollis)创建了几个对象？

通常情况下，这行代码会创建两个对象，

1. **字符串常量池中的对象**：
   - `hollis` 是一个字符串字面量。在代码执行时，Java 会检查字符串常量池中是否已经存在内容为 `hollis` 的字符串对象。如果不存在，Java 会在字符串常量池中创建一个新的 `hollis` 字符串对象。
   - 如果常量池中已经存在 `hollis`，则不会创建新的对象。
2. **堆中的 `String` 对象**：
   - `new String(hollis)` 明确表示创建一个新的 `String` 对象，即使 `hollis` 已经存在于字符串常量池中。这个新的 `String` 对象会存储在堆（heap）中，并且它的内容会是指向常量池中 `hollis` 的引用。
   - 这个 `String` 对象是通过 `new` 关键字创建的，因此在每次执行这行代码时都会生成一个新的对象。

**结论**：

- 如果 `hollis` 字符串字面量在常量池中不存在，那么 `String str = new String(hollis);` 这行代码会创建两个对象：一个在字符串常量池中，一个在堆中。
- 如果 `hollis` 字符串字面量已经在常量池中存在，那么这行代码只会创建一个对象，即堆中的 `String` 对象。

总结： **通常情况下，这行代码会创建两个对象**，一个在常量池中（如果字面量 `hollis` 还不存在），一个在堆中（无论如何都会创建）。

##### 11 String a = "ab"; String b = "a" + "b"; a == b 吗？

结果为`true`，因为==比较的是对象的引用，因为a和b都是**字面量**组成的字符串，引用地址在编译的时候已经确定了，在编译时，会把字面量直接拼接在一起，所以二者都是引用同一个对象。

**字面量**：说简单点，字面量就是指有数字、字母等构成的字符串或数值，字面量只能以右值出现，即右值等于左边的值，如下：

```java
int a = 1;
String s = "hollis";
```

##### 12 RPC接口返回中，使用基本类型还是包装类？

尽量使用包装类，因为基本数据类型在发生异常的时候可能会返回默认值，如`int` 默认返回0，而包装类则会返回`null`。

##### 13 在开发过程中常见的语法糖？

所谓语法糖就是方便开发人员使用，对语法进行简化；但在编译的时候会还原成最基础的语法，这个就是解语法糖。

13.1 **`switch`支持使用`String`类**

`Java`中的`switch`原本就是支持基本类型，比如`int`、`char`等，对于`int`类型，会直接比较数值，对于`char`，则会比较ASCII码。对于编译器来说，

任何类型的比较都要转成整型。如`short`、`char`（ASCII码是整型）、以及`int`。

```java
String str = "world";
switch (str) {
    case "hello":
        System.out.println("hello");
        break;
    case "world":
        System.out.println("world");
        break;
    default:
        break;
}
```

实际在编译器中的代码如下：

```java
String str ="world";
String s;
switch((s=str).hashcode()) {
	case 99162322:
		if(s.equals("hello”))
			System.out.println("hello");
		break;
	case 113318802:
		if(s.equals("world"))
			System.out.println("world");
		break;
	default:
		break;
}
```

字符串的`switch`是通过`equals()`和`hasdCode()`方法来实现的

13.2 **泛型**

13.2.1 **定义**：泛型允许类、接口、和方法在定义的时候使用类型参数，这能使代码更加通用和类型安全。

**类**：

```java
// 泛型类
public class Box<T> {
    private T item;

    public void setItem(T item) {
        this.item = item;
    }

    public T getItem() {
        return item;
    }
}
```

上面的代码中，`Box` 类是一个泛型类，`T` 是一个类型参数，可以在创建 `Box` 对象时指定具体的类型。

**接口**：

```java
interface InterfaceName<T> {
    // 在接口中使用类型参数T
    T Method(T param);
}
```

**方法**：方法也可以是泛型的，即方法定义中可以有一个或多个类型参数。这使得方法能够处理不同类型的对象，而不需要定义多个重载方法。

```java
public <T> void printArray(T[] array) { // 方法声明使用了泛型类型（如 T）
    for (T element : array) {
        System.out.println(element);
    }
}
```

如果去掉方法中的`<T>`，则编译器会报警：Cannot resolve symbol 'T'，这意味着编译器不知道`T`是什么类型。

13.2.2 **泛型的边界**：对泛型类型参数进行约束，比如要求类型参数必须是某个类的子类或实现某个接口。这可以通过使用 `extends` 关键字来实现。

```java
public <T extends Number> void printNumber(T number) {
    System.out.println(number);
}
```

在这个方法中，`T` 必须是 `Number` 的子类或 `Number` 本身。这样就限制了 `printNumber` 方法只能接受数字类型的参数

13.2.3 **通配符**：

在泛型中，通配符用于表示未知类型。常见的通配符有两种：

- **无界通配符（?）**：可以接受任何类型。
- **有界通配符**：
  - **`? extends T`**：表示可以接受 `T` 类型及其子类型。
  - **`? super T`**：表示可以接受 `T` 类型及其父类型。

示例：

```java
public void processElements(List<? extends Number> list) {
    for (Number number : list) {
        System.out.println(number);
    }
}
```

在这个方法中，`List<? extends Number>` 表示可以接受一个 `Number` 或 `Number` 的子类的列表。

13.2.4 **类型擦除**

泛型在编译时被擦除，实际上运行时并不保留类型信息。例如，`List<String>` 在运行时就是 `List`。

13.3 **自动拆箱与装箱**

**自动装箱**：原始类型转换成对应的对象，如int变量转换成Integer对象。

**自动拆箱**：对应的对象转成成原始类型，Integer对象转换成int类型值。

原始类型byte,short,char,int,long,float,double,boolean 对应的封装类为Byte,Short,Character,Integer,Long,Float,Double,Boolean。装箱过程是通过调用包装器的`valueOf`方法实现的，而拆箱过程则是调用包装器的`xxxValue`方法实现的，如下：

```java
Integer i = 10;  // 自动装箱
int n = i;       // 自动拆箱
//等效于
Integer i = Integer.valueOf(10);  // 自动装箱
int n = i.intValue();             // 自动拆箱
```

13.4 **枚举**

枚举是一种特殊的数据类型，用于表示有限的一组常量。当我们使用`enum`来定义一个枚举类型的时候，编译器会自动帮我们创建一个`final`类型的类继承`enum`类，所以枚举类型不能被继承。

13.5 **for-each**

for-each的实现原理其实就是使用了普通的for循环和迭代器，迭代器示例如下：

```java
public static void main(String[] args) {
    // 创建一个 ArrayList 并添加一些元素
    ArrayList<String> fruits = new ArrayList<>();
    fruits.add("Apple");
    fruits.add("Banana");
    fruits.add("Cherry");
    fruits.add("Date");

    // 获取该 ArrayList 的迭代器
    Iterator<String> iterator = fruits.iterator();

    // 使用迭代器遍历集合
    while (iterator.hasNext()) //检查集合是否存在下一元素
    {
        String fruit = iterator.next();//获取当前元素
        System.out.println(fruit);
    }
}
```

**迭代器的特点**

- **顺序访问**：`Iterator` 提供了一种顺序访问集合元素的方式。
- **移除元素**：`Iterator` 还提供了 `remove()` 方法，可以在遍历时移除当前元素，但需要注意，它只能在调用 `next()` 之后调用，且只能移除当前遍历的元素

13.6 **try-with-resource**

基本语法

```java
try (ResourceType resource = new ResourceType()) {
    // 使用资源
} catch (ExceptionType e) {
    // 处理异常
}
```

* **ResourceType**: 资源的类型，它必须实现 `AutoCloseable` 接口。

* **resource**: 声明并初始化要使用的资源。

* **try 块**: 在此块中使用资源，资源在此块结束时自动关闭。

* **catch 块**: 用于处理可能出现的异常。

