参考：[java instrument 初探](http://blog.csdn.net/pwlazy/article/details/5109742)

java在1.5引入java.lang.instrument，你可以由此实现一个java agent,通过此agent来修改类的字节码即改变一个类。

- 通过实现ClassFileTransformer的transform方法，具体实现字节码增强的逻辑
- 实现javaagent的premain方法，将上一步中的ClassFileTransformer实例注册到instrument中。当类加载时，transform方法会执行。
