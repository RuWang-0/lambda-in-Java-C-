# Java的Lambda表达式机制

## Lambda表达式

lambda表达式是一个匿名函数，也可称为闭包，它所抽象出来的东西是一组运算。

lambda表达式​允许把函数作为一个方法的参数（函数作为参数传递进方法中）。

使用lambda​表达式可以使代码变的更加简洁紧凑。

## Java里的Lambda表达式

Java 8的一个大亮点是引入lambda表达式，它显著增强了Java。首先，它们增加了新的语法元素，使Java语言的表达能力得以提升（更简洁，如用lambda语法来代替匿名的内部类），并流线化了一些常用结构的实现方式。其次，lambda表达式的加入也导致API 库中增加了新的功能，包括利用多核环境的并行处理功能（尤其是在处理for-each 风格的操作时）变得更加容易，以及支持对数据执行管道操作的新的流API。*（第二点有点深入，值得研究一下）*

> Java 8 includes a couple of other parallel array operations that utilize lambda expressions outside of the streams framework. 
>
> Event-driven architectures are easy to implement using lambda-based callbacks.

当开发者在编写lambda​表达式时，也会随之被编译成一个函数式接口。

### 语法

lambda表达式在Java语言中引入了一个新的语法元素和操作符，这个操作符是`->`，称为lambda操作符或箭头操作符。用法如下：

```Java
(parameters) -> expression		// lambda体包含单独一个表达式（表达式体）
or
(parameters) -> {statements;}	// lambda体包含一个代码块（块体）
```

`->`将lambda表达式分为两个部分。左侧指定了lambda表达式需要的所有参数，右侧指定了lambda体，即lambda表达式要执行的动作。在用语言描述时，可以把`->`表达成“成了”或“进入”。

可作为参数传递lambda表达式。

也可以在lambda表达式中抛出异常，但是该异常必须与函数式接口的抽象方法的`throws`子句中列出的异常兼容。

### 例子

#### 表达式体

```Java
// before
double myMath() { return 123.45; }
// after
() -> 123.45		// 没有参数，参数列表为空，返回常量值123.45
// before
Integer add(Integer x, Integer y) { return x + y; }
// after
(x, y) -> x + y		// 或者指定类型
(Integer x, Integer y) -> x + y
```

……

#### 作为参数传递lambda表达式

```Java
button.addActionListener(new ActionListener(){
    public void actionPerformed(ActionEvent actionEvent){
        System.out.println("button detected");  
    }
});
button.addActionListener( 
    event -> System.out.println("button detected"));
```

……

#### 块体

Java 8能够将“一块代码”赋值给一个Java变量。在块lambda中必须显式使用`return`语句来返回值，因为块lambda体代表的不是单独一个表达式。

### 变量捕获/变量作用域

在lambda表达式中，可以访问其外层作用域内定义的变量。

==lambda表达式可以获取或设置其外层类的实例或静态变量的值，以及调用其外层类定义的方法？==

对于lambda表达式外部作用域内定义的局部变量，lambda表达式只能引用标记了`final`或者隐性具有`final`语义(`effectively final`)的外部局部变量，即不能在lambda表达式内部修改定义在域外的局部变量。（`final`类型变量：只能赋值一次）

>The implication of being effectively final is that you can assign to the variable only
>once. 
>
>Encourage people to use lambda expressions to capture values rather than capturing variables.

lambda​表达式当中不允许声明一个与局部变量同名的参数或者局部变量。

因此，lambda表达式也常被称为闭包。

## Java里的Lambda表达式机制

lambda表达式本质上是一个匿名方法，但这个方法不是独立执行的，而是用于实现由函数式接口(Functional Interfaces)定义的另一个方法。因此，lambda表达式会导致产生一个匿名类。

### 函数式接口

函数式接口是仅包含一个抽象方法的接口，通常表示单个动作。例如，标准接口`Runnable`是一个函数式接口，因为它只定义了一个方法`run()`，`run()`定义了`Runnable`的动作。

```Java
// functional interface
interface Runnalbe{
    void run();
}
// before
Runnable runnable1 = new Runnable(){
    @Override public void run(){  
        System.out.println("Running");
    }
};
// after
Runnable runnable2 = () -> System.out.println("Running");
// 稍微解释简化过程：public是多余的，函数名Runnalbe是多余的（接口名），方法名run是多余的（只有一个动作），返回类型void是多余的（接口里定义了，或者编译器自己判断），最后加上操作符"->"，完美。
```

下面通过例子来说明如何在参数上下文中使用lambda表达式。

```Java
// 定义一个函数式接口
interface MyNumber{
    double getValue();
}
// 声明对函数式接口MyNumber的一个引用
MyNumber myNum;
// 将一个lambda表达式赋值给该接口引用
myNum = () -> 123.45;
// lambda表达式变成了getValue()方法的实现
System.out.println(myNum.getValue());
```

当目标类型上下文中出现lambda表达式是，会自动创建实现了函数式接口的一个类的实例，函数式接口声明的抽象方法的行为由lambda表达式定义。当通过目标调用该方法时，就会执行lambda表达式。因此，**lambda表达式提供了一种将代码片段转换为对象的方法。**

### 泛型函数式接口

lambda表达式自身不能指定类型参数。因此，lambda表达式不能是泛型（当然，由于存在类型腿短，所有lambda表达式都展现出一些类似于泛型的特征）。然而，与lambda表达式关联的函数式接口可以是泛型。此时，lambda表达式的目标类型部分由声明函数式接口引用时指定的参数类型决定。

```Java
// 定义一个泛型函数式接口
interface SomeFunc<T>{
    <T> func();
}
SomeFunc<String> s = () -> "This is a String";
SomeFunc<Integer> n = () -> 123.45;
```



## 阅读顺序

Java 8编程参考官方教程（第9版）

Java 8 Lambdas FUNCTIONAL PROGRAMMING FOR THE MASSES by Richard Warburton

*Beginning Java8 Language Features by Kishori Sharan

### 小阅读

[Java中的final和effectively final——csdn](https://blog.csdn.net/qing_gee/article/details/104306797)