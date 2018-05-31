### [try-with-resources](http://blog.csdn.net/hengyunabc/article/details/18459463)

  实际上就是自动调用资源的close()函数
  ~~~java
  static String readFirstLineFromFile(String path) throws IOException {
    try (BufferedReader br = new BufferedReader(new FileReader(path))) {
        return br.readLine();
    }
  }
  ~~~

  ### [Fork/Join](https://www.liaoxuefeng.com/article/001493522711597674607c7f4f346628a76145477e2ff82000)
map-reduce原理
