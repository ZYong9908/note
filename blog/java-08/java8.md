# 包和访问控制

## 包(package)

为了更好地组织类、防止命名冲突、访问控制、提供搜索和定位类（class）、接口、枚举（enumerations）和注释（annotation）等。，Java 提供了包机制，用于区别类名的命名空间。

### 包的作用

- 把功能相似或相关的类或接口组织在同一个包中，方便类的查找和使用。
- 如同文件夹一样，包也采用了树形目录的存储方式。同一个包中的类名字是不同的，不同的包中的类的名字是可以相同的，当同时调用两个不同包中相同类名的类时，应该加上包名加以区别。因此，包可以避免名字冲突。
- 包也限定了访问权限，拥有包访问权限的类才能访问某个包中的类。

Java 使用包（package）这种机制是为了防止命名冲突，访问控制，提供搜索和定位类（class）、接口、枚举（enumerations）和注释（annotation）等。

### 包声明和导入

#### 包的声明

包用来区分类的位置的，也可以用来将类进行分类

- 包语句的语法格式为

```java
package pkg1[．pkg2[．pkg3…]];
```

- 包本质是文件夹，如 test 就是一个 test 文件夹中包含一个 test.java 文件，再包含Test类

```java
package test;

public class Test{
  
}
```

#### 包的导入

如果需要使用其他包里的类，需要使用`import`，也可以导入包内的全部类

```java
import test.Test;//导入Test类
import test.*；//导入test包内的全部类
```

Java默认为我们导入了`java.lang`包，不需要手动导入

#### 静态导入

静态导入可以直接导入某个类的静态方法或者是静态变量，导入后，相当于这个方法或类定义在当前类中，可以直接调用该方法。

```java
import static test.Test.test;

public class Main {
    public static void main(String[] args) {
        test();
    }
}
```

注意：静态导入不会进行类的初始化

## 访问控制

Java中，可以使用访问控制修饰符来保护对类、变量、方法和构造方法的访问（不希望外部类访问类中的属性或是方法，只允许内部调用）。Java 支持 4 种不同的访问权限。

- **default** (默认，什么也不写）: 在同一包内可见，不使用任何修饰符。使用对象：类、接口、变量、方法
- **private** : 在同一类内可见。使用对象：变量、方法。 **注意：不能修饰类（外部类）**
- **public** : 对所有类可见。使用对象：类、接口、变量、方法
- **protected** : 对同一包内的类和所有子类可见。使用对象：变量、方法。 **注意：不能修饰类（外部类）**

| 访问控制修饰符 | 当前类 | 同一包内的类 | 子类 | 其他 |
| -------------- | :----: | :----------: | :--: | :--: |
| public         |   ✓    |      ✓       |  ✓   |  ✓   |
| protected      |   ✓    |      ✓       |  ✓   |  ✗   |
| 默认           |   ✓    |      ✓       |  ✗   |  ✗   |
| private        |   ✓    |      ✗       |  ✗   |  ✗   |

- 权限控制符可以声明在方法、成员变量、类前面，一旦声明为private，只能在类内部访问

```java
public class Test {
    private int a = 1;   //具有私有访问权限，只能类内部访问
}

public static void main(String[] args) {
    Test test = new Test();
    System.out.println(test.a);
}
//报错：java: a 在 Test 中是 private 访问控制
```

- 和文件名称相同的类，只能是public，并且一个java文件中只能有一个public class！

```java
// Test.java
public class Test {
    
}
class Test{	//不能添加修饰符,只能是default
	
}
```
