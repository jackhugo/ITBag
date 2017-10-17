今天同事在看我的代码时，给我提了一个建议，说在代码中尽量少使用try catch，因为这个会影响性能。之前真没注意这个，就去详细查查。

在网上找到这篇国外文章，从字节码执行层面进行了讲解。
[How the Java virtual machine handles exceptions](https://www.javaworld.com/article/2076868/learn-java/how-the-java-virtual-machine-handles-exceptions.html)

**先说结论：try catch在不抛异常的情况下，并不影响性能。**

文章大家去看，我引用部分来说明。

以下是remainder方法及其字节码。可以看出，在方法执行未发生异常情况下（字节码中是0-3），并未涉及异常部分；当有异常发生，才会从异常表中查找执行模块。

>~~~java
static int remainder(int dividend, int divisor)
    throws DivideByZeroException {
    try {
        return dividend % divisor;
    }
    catch (ArithmeticException e) {
        throw new DivideByZeroException();
    }
}
>~~~
>~~~java
The main bytecode sequence for remainder:
   0 iload_0               // Push local variable 0 (arg passed as divisor)
   1 iload_1               // Push local variable 1 (arg passed as dividend)
   2 irem                  // Pop divisor, pop dividend, push remainder
   3 ireturn               // Return int on top of stack (the remainder)
The bytecode sequence for the catch (ArithmeticException) clause:
   4 pop                   // Pop the reference to the ArithmeticException
                           // because it isn't used by this catch clause.
   5 new #5 <Class DivideByZeroException>
                           // Create and push reference to new object of class
                           // DivideByZeroException.
DivideByZeroException
   8 dup           // Duplicate the reference to the new
                           // object on the top of the stack because it
                           // must be both initialized
                           // and thrown. The initialization will consume
                           // the copy of the reference created by the dup.
   9 invokenonvirtual #9 <Method DivideByZeroException.<init>()V>
                           // Call the constructor for the DivideByZeroException
                           // to initialize it. This instruction
                           // will pop the top reference to the object.
  12 athrow                // Pop the reference to a Throwable object, in this
                           // case the DivideByZeroException,
                           // and throw the exception.
>~~~
