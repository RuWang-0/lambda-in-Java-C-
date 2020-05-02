# C++和Java的Lambda表达式机制

### 首先Lambda表达式是什么

Lambda表达式是一个匿名函数，



### 其次，如何使用Java里的Lambda表达式

Java 8的一个大亮点是引入Lambda表达式，使用它设计的代码会更加简洁。当开发者在编写Lambda表达式时，也会随之被编译成一个函数式接口。下面这个例子就是使用Lambda语法来代替匿名的内部类，代码不仅简洁，而且还可读。

不使用Lambda的老方法：

```java
button.addActionListener(new ActionListener(){
    public void actionPerformed(ActionEvent actionEvent){
        System.out.println("Action detected");  
        }
});
```

使用Lambda：

```Java
button.addActionListener( actionEvent -> {
    System.out.println("Action detected");
});
```

让我们来看一个更明显的例子。

不使用Lambda的老方法：

```Java
Runnable runnable1=new Runnable(){
    @Override public void run(){  
        System.out.println("Running without Lambda");
    }
};
```

使用Lambda：

```Java
Runnable runnable2=()->System.out.println("Running from Lambda");
```

可见，用符号`->`就能使用Lambda表达式。

### 最后，Java是如何实现Lambda表达式机制的



[Lambda表达式——百度百科]([https://baike.baidu.com/item/Lambda%E8%A1%A8%E8%BE%BE%E5%BC%8F/4585794](https://baike.baidu.com/item/Lambda表达式/4585794))

