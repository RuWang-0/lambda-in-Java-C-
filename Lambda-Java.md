# Java的Lambda表达式机制

## 首先Lambda表达式是什么

$\lambda$表达式是一个匿名函数，也可称为闭包，它所抽象出来的东西是一组运算。

$\lambda$允许把函数作为一个方法的参数（函数作为参数传递进方法中）。

使用$\lambda$表达式可以使代码变的更加简洁紧凑。



## 其次，如何使用Java里的Lambda表达式

Java 8的一个大亮点是引入$\lambda$表达式，使用它设计的代码会更加简洁。当开发者在编写$\lambda$表达式时，也会随之被编译成一个函数式接口。

### 例子

下面这个例子就是使用$\lambda$语法来代替匿名的内部类，代码不仅简洁，而且还可读。

不使用$\lambda$的老方法：

```java
button.addActionListener(new ActionListener(){
    public void actionPerformed(ActionEvent actionEvent){
        System.out.println("Action detected");  
    }
});
```

使用$\lambda$：

```Java
button.addActionListener( actionEvent -> {
    System.out.println("Action detected");
});
```

让我们来看一个更明显的例子。

不使用$\lambda$的老方法：

```Java
Runnable runnable1=new Runnable(){
    @Override public void run(){  
        System.out.println("Running without Lambda");
    }
};
```

使用$\lambda$：

```Java
Runnable runnable2=()->System.out.println("Running from Lambda");
```

### 语法

```Java
(parameters) -> expression
or
(parameters) -> {statements;}
```

Java 8能够将“一块代码”赋值给一个Java变量。如：

```Java
addActionListener = 
    public void actionPerformed(ActionEvent actionEvent){
        System.out.println("Action detected");  
    }
```

简化一下，`public`是多余的：

```Java
addActionListener = void actionPerformed(ActionEvent actionEvent){
    System.out.println("Action detected");  
}
```

函数名字是多余的，因为已经赋给了`addActionListener`：

```Java
addActionListener = void(ActionEvent actionEvent){
    System.out.println("Action detected");  
}
```

编译器可以自己判断返回类型：

```Java
addActionListener = (ActionEvent actionEvent){
    System.out.println("Action detected");  
}
```

编译器可以自己判断参数类型：

```Java
addActionListener = (actionEvent){
    System.out.println("Action detected");  
}
```

只有一行，所以可以不要大括号，在参数和函数之间加上一个箭头符号`->`：

```Java
addActionListener = (actionEvent) -> System.out.println("Action detected"); 
```



### 变量作用域

$\lambda$表达式~~只能~~引用标记了`final`的外层局部变量，即不能在$\lambda$内部修改定义在域外的局部变量。

$\lambda$表达式可以访问未标记为`final`的外层局部变量，但是该变量不能在之后被修改，即隐性的具有`final`的语义。

$\lambda$表达式当中不允许声明一个与局部变量同名的参数或者局部变量。

## 最后，Java是如何实现Lambda表达式机制的





