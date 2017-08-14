参考：[Spring Boot整合UEditor，解决找不到上传文件的问题](http://blog.csdn.net/yry0304/article/details/53462974)

我按照上面集成，改动小，但UEditorController中的rootPath获取不对，改成：
~~~java
ResourceUtils.getURL("classpath:static").getPath()
~~~
