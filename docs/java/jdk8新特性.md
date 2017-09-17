### Optional

[JDK8新特性Optional 类](http://blog.csdn.net/canot/article/details/52956230)

Optional 类是一个可以为null的容器对象。如果值存在则isPresent()方法会返回true，调用get()方法会返回该对象。

Optional 是个容器：它可以保存类型T的值，或者仅仅保存null。Optional提供很多有用的方法，这样我们就不用显式进行空值检测。

Optional 类的引入很好的解决空指针异常。
~~~java
//显式判断null，代码多
if(null == str) {
    return 0;
}
return str.length();

//Optional更简洁
return Optional.ofNullable(str).map(String::length).orElse(0);
~~~

### Lambda表达式
[Java SE 8: Lambda表达式](http://www.infoq.com/cn/articles/Java-se-8-lambda)

要理解lambda表达式，首先要了解的是函数式接口（functional interface）。简单来说，函数式接口是只包含一个抽象方法的接口。在使用lambda表达式时，只需要提供形式参数和方法体。由于函数式接口只有一个抽象方法，所以通过lambda表达式声明的方法体就肯定是这个唯一的抽象方法的实现，而且形式参数的类型可以根据方法的类型声明进行自动推断。
~~~java
//传统方法
public void runThread() {
    new Thread(new Runnable() {
        public void run() {
            System.out.println("Run!");
        }
    }).start();
}

//lambda
public void runThreadUseLambda() {
    new Thread(() -> {
        System.out.println("Run!");
    }).start();
}

//Lambda表达式“(x, y) -> y - x“实现了java.util.Comparator接口。
Collections.sort(list, (x, y) -> y - x);
~~~

### 接口的默认方法
[默认方法](http://www.infoq.com/cn/articles/Java-se-8-lambda)，在文章的后半部分

- 接口的默认方法的主要目标之一是解决接口的演化问题。当往一个接口中添加新的方法时，可以提供该方法的默认实现。对于已有的接口使用者来说，代码可以继续运行。新的代码则可以使用该方法，也可以覆写默认的实现。
- 实现行为的多继承。

~~~java
//在方法前加default关键字
default List convert(Currency from, Currency to, List amounts) {
        List result = new ArrayList();
            for (BigDecimal amount : amounts) {
                result.add(convert(from, to, amount));
            }
            return result;
}
~~~

### 流式处理
[流式数据处理](http://www.cnblogs.com/shenlanzhizun/p/6027042.html)
