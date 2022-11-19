# 类的基本结构

## 成员变量

成员变量（也叫成员属性）是属于对象的变量，在类中可以包含多个成员变量，通过类创建的对象使用`.`来访问和操作成员变量。

成员变量默认初始值为数据类型的默认值，也可以自己定义初始值。

```java
public class Student {//定义一个Student类
	int age;//成员变量age
    	String name;//成员变量name
	String word = "我会说话"
}

public static void main(String[] args) {
    Student student = new Student();//创建一个Student对象student
    student.name = "小明";//为
    System.out.println(student.name + "的默认初始年龄为：" + student.age + "，自定义初始说：" + student.word);
}
//输出：小明的默认年龄为：0，自定义初始说：我会说话
```

## 成员方法

方法是代码语句的集合，用来完成某种操作

方法可以有返回值，比如计算两个数的和要返回计算结果；也可以没有返回值，比如要在控制台打印某些内容

成员方法和成员变量一样，是属于对象的，只能通过对象去调用

### 方法的定义

```java
[返回值类型] 方法名([形参]){
  //方法体
  return 返回值;
}
```

- 返回值类型：可以是引用类型和基本类型，也可以是void，表示没有返回值
- 方法名称：和标识符的规则一致，规范为小写字母开头使用驼峰命名法
- 形参：要计算两个数的和，就需要把两个数的值传入方法，在方法中表示这两个数的变量就叫形参
- 方法体：方法要执行的代码都写在方法体中
- 返回值：方法执行完后要返回给调用者的值，比如计算两个数的和要返回计算结果（如果返回值类型为void，可以省略return）
- 非void方法中，必须有返回值，且`return`不一定要放在最后，但无论`return`在哪执行到`return`都会立即结束整个方法
- 如果传入方法的参数（实参）是基本类型，在调用方法时会对参数的值进行复制，方法中的参数变量，不是我们传入的变量本身（形参不改变实参）
- 传入方法的参数，如果是引用类型，那么传入的依然是该对象的引用

```java
public static void main(String[] args) {
    int a = 1, b = 2;
  	new Test().test(a, b);
  	System.out.println("方法外：a="+a+", b="+b);
}

public class Test{
 	void test(int a, int b){  //传递的是值
  		int temp = a;
  		a = b;
 		b = temp;
        System.out.println("方法内：a="+a+", b="+b);
	} 
}
//输出：
//方法内：a=2，b=1
//方法外：a=1，b=2
```

```java
public class B{
    String name;
}

public class A{
    void test(B b){  //传递的是对象的引用
        System.out.println(b.name);
    }
}

public static void main(String[] args) {
    int a = 1;
  	B b = new B();
  	b.name = "name";
  	new A().test(b);
  	System.out.println("a="+a+", b="+b);
}
//输出：
//name
//a=1, b=B@16b98e56
```

### 方法的使用

#### 方法之间相互调用

```java
void a(){
  //xxxx
}

void b(){
  a();
}
```

#### 方法的递归调用

方法在内部调用自身称为递归调用

```java
i = 0;
int a(){
    i++;
    if(i < 10){
        a();
    }
}
```

#### 方法的重载

一个类中可以包含多个同名但形参不同的方法。方法的返回类型，可以相同，也可以不同，但形参必须不同

```java
public class Test {
    int a(int i){
        return i;
    }
    
    void a(String i){
        System.out.println(i); 
    }
    
    void a(){
        System.out.println("void a()"); 
    }
}

public static void main(String[] args) {
    Test test = new Test();
    int a1 = test.a(1);
    test.a("void a(String i)");
    test.a();
	System.out.println(a1);
}
//输出：
//void a(String i)
//void a()
//1
```

## 构造方法

所有类都默认有一个无参构造方法，构造方法没有返回值，也可以理解为，返回的是当前对象的引用，每个类都必须具有这个方法

```java
//定义一个Test类
public class Test {
}

//反编译的结果
public class Test {
    //默认无参构造方法
	public Test() {
    }
}
```

- 只有调用类的构造方法，才能创建类的对象

```java
// new + 构造方法
Test test = new Test();
```

- 类要在一开始就要执行的语句，都在构造方法里面，完成构造方法里的内容后，才会创建对象，一般最常用的就是给成员属性赋初始值

```java
public class Test {
    String word;
    
    Test(){
        word = "Word";
    }
}
```

- 可以手动创建有参构造方法，当定义了新的有参构造方法后，默认的无参构造会被覆盖
- 如果同时需要有参和无参构造，那么就需要用到方法的重载！手动再去定义一个无参构造。
- 当遇到名称冲突时，需要用到`this`关键字

```java
public class Test {
    String word;

    Test(){
	word = "word";
    }
    
    Test(String word){   //形参和类成员变量名称相同，Java会优先使用形式参数定义的变量！
        this.word = word;  //通过this指代当前的对象属性，this就代表当前对象
    }
}
```

- #### this

  - `this`只能用于指代当前对象的内容，只有属于对象的部分可以使用`this`

  - 只能在类的成员方法中使用`this`，不能在静态方法中使用`this`关键字。

- 成员变量的初始化始终在构造方法执行之前

```java
public class Test {
    String a = "12qwaszx";

    Test(){
        System.out.println(a);
    }

    public static void main(String[] args) {
        Test s = new Test();
    }
}
//输出：12qwaszx
```

## 静态变量和静态方法

静态变量和静态方法是类具有的属性，可以理解为所有对象共享的内容。通过使用`static`关键字来声明一个变量或一个方法为静态的，一旦被声明为静态，那么通过这个类创建的所有对象，操作的都是同一个目标，对象再多，也只有这一个静态的变量或方法。一个对象改变了静态变量的值，其他的对象读取的就是被改变的值。

```java
public class Test {
    static int a = 0;
}

public static void main(String[] args) {
	Test test1 = new Test();
	test1.a = 10;
	Test test2 = new Test();
	System.out.println(test2.a);
}
//输出：10
```

- 推荐直接通过`类名.xxx`的形式直接访问被标记为静态的内容

```java
public static void main(String[] args) {
   Test.a = 10;
   System.out.println(Test.a);
}
//输出：10
```

## 代码块和静态代码块

### 代码块

代码块是属于类的内容，在对象创建时执行，在构造方法执行之前执行（和成员变量初始值一样），且每创建一个对象时，只执行一次（相当于构造之前的准备工作）

```java
public class Test {
    {
        System.out.println("我是代码块");
    }

    Test(){
        System.out.println("我是构造方法");
    }
}
//输出：
//我是代码块
//我是构造方法
```

### 静态代码块

静态代码块跟静态方法和静态变量一样，在类刚加载时就会调用；

```java
public class Test {
    static int a;

    static {
        a = 10;
    }
    
    public static void main(String[] args) {
        System.out.println(Test.a);
    }
}
//输出：1
```

## String和StringBuilder类

### String

字符串类是一个特殊的类，是Java中唯一重载运算符的类(Java不支持运算符重载)

`String`的对象直接支持使用`+`或`+=`运算符来进行拼接，并形成新的`String`对象（`String`的字符串是不可变的）

```java
String a = "abcd", b = "bcde";
String ab = a+b;
System.out.println(ab);
//输出：abcdbcde
```

`String`的拼接有可能会被编译器优化为`StringBuilder`来减少对象创建

```java
String result="String1"+"String2"; //会被优化成一句：String1String2
```

```java
String str1="String1";
String str2="String2";
String result=str1+str2;
//变量随时可变，在编译时无法确定result的值，无法被优化
```

### StringBuilder

`StringBuilder` 类和 `String` 类不同，`StringBuilder` 类的对象能够被多次的修改，并且不产生新的未使用对象。

```java
String str1="String2";
String str2="String2";
String result=(new StringBuilder(String.valueOf(str1))).append(str2).toString();
System.out.println(str1);
//输出：String1String2
```

`StringBuilder` 能够存储可变长度的字符串！

```java
StringBuilder builder = new StringBuilder();
builder
       .append("ab")
       .append("bc")
       .append("cd");   //StringBuilder的链式调用
String str = builder.toString();
System.out.println(str);
//输出：abbccd
```































