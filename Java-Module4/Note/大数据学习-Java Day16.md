

# 大数据学习-Java Day16

## IO流

### 1 概念

-  IO就是Input和Output的简写，也就是输入和输出的含义。 
- IO流就是指读写数据时像流水一样从一端流到另外一端，因此得名为“流"。 

### 2 基本分类

-  按照读写数据的基本单位不同，分为 字节流 和 字符流。 
  - 其中字节流主要指以字节为单位进行数据读写的流，可以读写任意类型的文件。 
  - 其中字符流主要指以字符(2个字节)为单位进行数据读写的流，只能读写文本文件。 
- 按照读写数据的方向不同，分为 输入流 和 输出流（站在程序的角度）。 
  - 其中输入流主要指从文件中读取数据内容输入到程序中，也就是读文件。 
  - 其中输出流主要指将程序中的数据内容输出到文件中，也就是写文件。 
- 按照流的角色不同分为节点流和处理流。 
  - 其中节点流主要指直接和输入输出源对接的流。 
  - 其中处理流主要指需要建立在节点流的基础之上的流。 

### 3 体系结构

![](./picture/02 IO流的体系图.png)

![](./picture/03 IO流的框架图.png)

### 4 相关流的详解

- **FileWriter类** 

  - 概念

    -  java.io.FileWriter类主要用于将文本内容写入到文本文件。 

  - 常用方法

    | 方法声明                                    | 功能介绍                                                    |
    | ------------------------------------------- | ----------------------------------------------------------- |
    | FileWriter(String fileName)                 | 根据参数指定的文件名构造对象                                |
    | FileWriter(String fileName, boolean append) | 以追加的方式根据参数指定的文件名来构造对象                  |
    | void write(int c)                           | 写入单个字符                                                |
    | void write(char[] cbuf, int off, int len)   | 将指定字符数组中从偏移量off开始的len个字符写入此 文件输出流 |
    | void write(char[] cbuf)                     | 将cbuf.length个字符从指定字符数组写入此文件输出 流中        |
    | void flush()                                | 刷新流                                                      |
    | void close()                                | 关闭流对象并释放有关的资源                                  |
    
    ```java
    
    import java.io.FileWriter;
    import java.io.IOException;
    
    public class FileWriterTest {
    
        public static void main(String[] args) {
            // 选中代码后可以使用 ctrl+alt+t 来生成异常的捕获代码等
            FileWriter fw = null;
    
            try {
                // 1.构造FileWrite类型的对象与d:/a.txt文件关联
                // 若文件不存在，该流会自动创建新的空文件
                // 若文件存在，该流会清空文件中的原有内容
                fw = new FileWriter("d:/a.txt");
                // 以追加的方式创建对象去关联文件
                // 若文件不存在则自动创建新的空文件，若文件存在则保留原有数据内容
                //fw = new FileWriter("d:/a.txt", true);
                // 2.通过流对象写入数据内容  每当写入一个字符后则文件中的读写位置向后移动一位
                fw.write('a');
    
                // 准备一个字符数组
                char[] cArr = new char[]{'h', 'e', 'l', 'l', 'o'};
                // 将字符数组中的一部分内容写入进去
                fw.write(cArr, 1, 3);  // ell
                // 将整个字符数组写进去
                fw.write(cArr); // hello
    
                // 刷新流
                fw.flush();
                System.out.println("写入数据成功！");
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                // 3.关闭流对象并释放有关的资源
                if (null != fw) {
                    try {
                        fw.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }
    ```
    
    

- **FileReader类** 

  -  基本概念

    -  java.io.FileReader类主要用于从文本文件读取文本数据内容。 

  - 常用方法

    | 方法声明                                      | 功能介绍                                                     |
    | --------------------------------------------- | ------------------------------------------------------------ |
    | FileReader(String fileName)                   | 根据参数指定的文件名构造对象                                 |
    | int read()                                    | 读取单个字符的数据并返回，返回-1表示读取到末尾               |
    | int read(char[] cbuf, int offset, int length) | 从输入流中将最多len个字符的数据读入一个字符数组中，返回读取 到的字符个数，返回-1表示读取到末尾 |
    | int read(char[] cbuf)                         | 从此输入流中将最多 cbuf.length 个字符的数据读入字符数组中，返 回读取到的字符个数，返回-1表示读取到末尾 |
    | void close()                                  | 关闭流对象并释放有关的资源                                   |
    
    ```java 
    
    import java.io.FileReader;
    import java.io.IOException;
    
    public class FileReaderTest {
    
        public static void main(String[] args) {
            FileReader fr = null;
    
            try {
                // 1.构造FileReader类型的对象与d:/a.txt文件关联
                fr = new FileReader("d:/a.txt");
                //fr = new FileReader("d:/b.txt");
                // 2.读取数据内容并打印
                /*
                int res = fr.read();
                System.out.println("读取到的单个字符是：" + (char)res); // 'a'
                 */
                /*
                int res = 0;
                while ((res = fr.read()) != -1) {
                    System.out.println("读取到的单个字符是：" + (char)res + "，对应的编号是：" + res);
                }
    			*/
                // 准备一个字符数组来保存读取到的数据内容
                char[] cArr = new char[5];
                // 期望读满字符数组中的一部分空间，也就是读取3个字符放入数组cArr中下标从1开始的位置上
                /*int res = fr.read(cArr, 1, 3);
                System.out.println("实际读取到的字符个数是：" + res); // 3
                for (char cv : cArr) {
                    System.out.println("读取到的单个字符是：" + (char)cv); // 啥也没有 a e l 啥也没有
                }*/
    
                // 期望读满整个字符数组
                int res = fr.read(cArr);
                System.out.println("实际读取到的字符个数是：" + res); // 5
                for (char cv : cArr) {
                    System.out.println("读取到的单个字符是：" + (char)cv); // a e l l h
                }
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                // 3.关闭流对象并释放有关的资源
                if (null != fr) {
                    try {
                        fr.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }
    
    ```
    
    

-  **FileOutputStream类** 

  -  基本概念 

    - java.io.FileOutputStream类主要用于将图像数据之类的原始字节流写入到输出流中。

  - 常用的方法 

    | 方法声明                                      | 功能介绍                                                    |
    | --------------------------------------------- | ----------------------------------------------------------- |
    | FileOutputStream(String name)                 | 根据参数指定的文件名来构造对象                              |
    | FileOutputStream(String name, boolean append) | 以追加的方式根据参数指定的文件名来构造对象                  |
    | void write(int b)                             | 将指定字节写入此文件输出流                                  |
    | void write(byte[] b, int off, int len)        | 将指定字节数组中从偏移量off开始的len个字节写入 此文件输出流 |
    | void write(byte[] b)                          | 将 b.length 个字节从指定字节数组写入此文件输出 流中         |
    | void flush()                                  | 刷新此输出流并强制写出任何缓冲的输出字节                    |
    | void close()                                  | 关闭流对象并释放有关的资源                                  |

- **FileInputStream类** 

  -  基本概念 

    - java.io.FileInputStream类主要用于从输入流中以字节流的方式读取图像数据等。 

  - 常用的方法  

    | 方法声明                             | 功能介绍                                                     |
    | ------------------------------------ | ------------------------------------------------------------ |
    | FileInputStream(String name)         | 根据参数指定的文件路径名来构造对象                           |
    | int read()                           | 从输入流中读取单个字节的数据并返回，返回-1表示读取到末尾     |
    | int read(byte[] b, int off, int len) | 从此输入流中将最多len个字节的数据读入字节数组中，返回读取到的 字节个数，返回-1表示读取到末尾 |
    | int read(byte[] b)                   | 从此输入流中将最多 b.length 个字节的数据读入字节数组中，返回读 取到的字节个数，返回-1表示读取到末尾 |
    | void close()                         | 关闭流对象并释放有关的资源                                   |
    | int available()                      | 获取输入流所关联文件的大小                                   |

    ##### 案例 编程实现两个文件之间的拷贝功能。 
    
    ```java
    
    import java.io.FileReader;
    import java.io.FileWriter;
    import java.io.IOException;
    
    //文件字符流实现文件的拷贝
    public class FileCharCopyTest {
    
        public static void main(String[] args) {
            FileReader fr = null;
            FileWriter fw = null;
    
            try {
                // 1.创建FileReader类型的对象与d:/a.txt文件关联
                fr = new FileReader("d:/a.txt");
                //fr = new FileReader("d:/03 IO流的框架图.png");
                // 2.创建FileWriter类型的对象与d:/b.txt文件关联
                fw = new FileWriter("d:/b.txt");
                //fw = new FileWriter("d:/IO流的框架图.png");   拷贝图片文件失败！！！
                // 3.不断地从输入流中读取数据内容并写入到输出流中
                System.out.println("正在玩命地拷贝...");
                int res = 0;
                while ((res = fr.read()) != -1) {
                    fw.write(res);
                }
                System.out.println("拷贝文件成功！");
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                // 4.关闭流对象并释放有关的资源
                if (null != fw) {
                    try {
                        fw.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
                if (null != fr) {
                    try {
                        fr.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }
    
    ```
    
    ```java
    
    import java.io.FileInputStream;
    import java.io.FileOutputStream;
    import java.io.IOException;
    
    //文件字节流实现文件的拷贝
    
    public class FileByteCopyTest {
    
        public static void main(String[] a rgs) {
    
            // 获取当前系统时间距离1970年1月1日0时0分0秒的毫秒数
            long g1 = System.currentTimeMillis();
    
            FileInputStream fis = null;
            FileOutputStream fos = null;
    
            try {
                // 1.创建FileInputStream类型的对象与d:/03 IO流的框架图.png文件关联
                //fis = new FileInputStream("d:/03 IO流的框架图.png");
                fis = new FileInputStream("d:/02_IO流的框架结构.mp4");
                // 2.创建FileOutputStream类型的对象与d:/IO流的框架图.png文件关联
                //fos = new FileOutputStream("d:/IO流的框架图.png");
                fos = new FileOutputStream("d:/IO流的框架结构.mp4");
                // 3.不断地从输入流中读取数据内容并写入到输出流中
                System.out.println("正在玩命地拷贝...");
                // 方式一：以单个字节为单位进行拷贝，也就是每次读取一个字节后再写入一个字节
                // 缺点：文件稍大时，拷贝的效率很低
                /*int res = 0;
                while ((res = fis.read()) != -1) {
                    fos.write(res);
                }*/
                // 方式二：准备一个和文件大小一样的缓冲区，一次性将文件中的所有内容取出到缓冲区然后一次性写入进去
                // 缺点：若文件过大时，无法申请和文件大小一样的缓冲区，真实物理内存不足
                /*int len = fis.available();
                System.out.println("获取到的文件大小是：" + len);
                byte[] bArr = new byte[len];
                int res = fis.read(bArr);
                System.out.println("实际读取到的文件大小是：" + res);
                fos.write(bArr);*/
                // 方式三：准备一个相对适当的缓冲区，分多次将文件拷贝完成
                byte[] bArr = new byte[1024];
                int res = 0;
                while ((res = fis.read(bArr)) != -1) {
                    fos.write(bArr, 0, res);
                }
    
                System.out.println("拷贝文件成功！");
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                // 4.关闭流对象并释放有关的资源
                if (null != fos) {
                    try {
                        fos.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
                if (null != fis) {
                    try {
                        fis.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }
    
            long g2 = System.currentTimeMillis();
            System.out.println("使用文件流拷贝视频文件消耗的时间为：" + (g2-g1));  // 165
        }
    }
    
    ```
    
    

- ​    **BufferedOutputStream类** 

  -  基本概念 

    - java.io.BufferedOutputStream类主要用于描述缓冲输出流，此时不用为写入的每个字节调用底层 系统。 

  - 常用的方法 

    | 方法声明                                         | 功能介绍                                  |
    | ------------------------------------------------ | ----------------------------------------- |
    | BufferedOutputStream(OutputStream out)           | 根据参数指定的引用来构造对象              |
    | BufferedOutputStream(OutputStream out, int size) | 根据参数指定的引用和缓冲区大小来构造 对象 |
    | void write(int b)                                | 写入单个字节                              |
    | void write(byte[] b, int off, int len)           | 写入字节数组中的一部分数据                |
    | void write(byte[] b)                             | 写入参数指定的整个字节数组                |
    | void flush()                                     | 刷新流                                    |
    | void close()                                     | 关闭流对象并释放有关的资源                |

- **BufferedInputStream类** 

  -  基本概念 

    - java.io.BufferedInputStream类主要用于描述缓冲输入流。 

  - 常用的方法 

    | 方法声明                                      | 功能介绍                               |
    | --------------------------------------------- | -------------------------------------- |
    | BufferedInputStream(InputStream in)           | 根据参数指定的引用构造对象             |
    | BufferedInputStream(InputStream in, int size) | 根据参数指定的引用和缓冲区大小构造对象 |
    | int read()                                    | 读取单个字节                           |
    | int read(byte[] b, int off, int len)          | 读取len个字节                          |
    | int read(byte[] b)                            | 读取b.length个字节                     |
    | void close()                                  | 关闭流对象并释放有关的资源             |
    
    ```java
    
    import java.io.*;
    
    // 字节缓冲流实现文件的拷贝
    public class BufferedByteCopyTest {
    
        public static void main(String[] args) {
    
            // 获取当前系统时间距离1970年1月1日0时0分0秒的毫秒数
            long g1 = System.currentTimeMillis();
    
            BufferedInputStream bis = null;
            BufferedOutputStream bos = null;
    
            try {
                // 1.创建BufferedInputStream类型的对象与d:/02_IO流的框架结构.mp4文件关联
                bis = new BufferedInputStream(new FileInputStream("d:/02_IO流的框架结构.mp4"));
                // 2.创建BufferedOuputStream类型的对象与d:/IO流的框架结构.mp4文件关联
                bos = new BufferedOutputStream(new FileOutputStream("d:/IO流的框架结构.mp4"));
    
                // 3.不断地从输入流中读取数据并写入到输出流中
                System.out.println("正在玩命地拷贝...");
    
                byte[] bArr = new byte[1024];
                int res = 0;
                while ((res = bis.read(bArr)) != -1) {
                    bos.write(bArr, 0, res);
                }
    
                System.out.println("拷贝文件成功！");
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                // 4.关闭流对象并释放有关的资源
                if (null != bos) {
                    try {
                        bos.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
                if (null != bis) {
                    try {
                        bis.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }
    
            long g2 = System.currentTimeMillis();
            System.out.println("使用缓冲区拷贝视频文件消耗的时间为：" + (g2-g1)); // 44
        }
    }
    
    ```
    
    

- ​    **BufferedWriter类** 

  -  基本概念 

    - java.io.BufferedWriter类主要用于写入单个字符、字符数组以及字符串到输出流中。 

  - 常用的方法 

    | 方法声明                                  | 功能介绍                                              |
    | ----------------------------------------- | ----------------------------------------------------- |
    | BufferedWriter(Writer out)                | 根据参数指定的引用来构造对象                          |
    | BufferedWriter(Writer out, int sz)        | 根据参数指定的引用和缓冲区大小来构造对象              |
    | void write(int c)                         | 写入单个字符到输出流中                                |
    | void write(char[] cbuf, int off, int len) | 将字符数组cbuf中从下标off开始的len个字符写入输出流 中 |
    | void write(char[] cbuf)                   | 将字符串数组cbuf中所有内容写入输出流中                |
    | void write(String s, int off, int len)    | 将参数s中下标从off开始的len个字符写入输出流中         |
    | void write(String str)                    | 将参数指定的字符串内容写入输出流中                    |
    | void newLine()                            | 用于写入行分隔符到输出流中                            |
    | void flush()                              | 刷新流                                                |
    | void close()                              | 关闭流对象并释放有关的资源                            |

- **BufferedReader类** 

  -  基本概念 

    - java.io.BufferedReader类用于从输入流中读取单个字符、字符数组以及字符串。

  - 常用的方法 

    | 方法声明                                | 功能介绍                                                     |
    | --------------------------------------- | ------------------------------------------------------------ |
    | BufferedReader(Reader in)               | 根据参数指定的引用来构造对象                                 |
    | BufferedReader(Reader in, int sz)       | 根据参数指定的引用和缓冲区大小来构造对象                     |
    | int read()                              | 从输入流读取单个字符，读取到末尾则返回-1，否则返回实际读取到 的字符内容 |
    | int read(char[] cbuf, int off, int len) | 从输入流中读取len个字符放入数组cbuf中下标从off开始的位置上， 若读取到末尾则返回-1，否则返回实际读取到的字符个数 |
    | int read(char[] cbuf)                   | 从输入流中读满整个数组cbuf                                   |
    | String readLine()                       | 读取一行字符串并返回，返回null表示读取到末尾                 |
    | void close()                            | 关闭流对象并释放有关的资源                                   |
    
    ```java
    
    import java.io.*;
    
    public class BufferedCharCopyTest {
    
        public static void main(String[] args) {
            BufferedReader br = null;
            BufferedWriter bw = null;
    
            try {
                // 1.创建BufferedReader类型的对象与d:/a.txt文件关联
                br = new BufferedReader(new FileReader("d:/a.txt"));
                // 2.创建BufferedWriter类型的对象与d:/b.txt文件关联
                bw = new BufferedWriter(new FileWriter("d:/b.txt"));
                // 3.不断地从输入流中读取一行字符串并写入到输出流中
                System.out.println("正在玩命地拷贝...");
                String str = null;
                while ((str = br.readLine()) != null) {
                    bw.write(str);
                    bw.newLine(); // 当前系统中的行分隔符是：\r\n
                }
                System.out.println("拷贝文件成功！");
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                // 4.关闭流对象并释放有关的资源
                if (null != bw) {
                    try {
                        bw.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
                if (null != br) {
                    try {
                        br.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }
    ```
    
    

-  PrintStream类 

  -  基本概念 

    - java.io.PrintStream类主要用于更加方便地打印各种数据内容。 

  - 常用的方法 

    | 方法声明                      | 功能介绍                           |
    | ----------------------------- | ---------------------------------- |
    | PrintStream(OutputStream out) | 根据参数指定的引用来构造对象       |
    | void print(String s)          | 用于将参数指定的字符串内容打印出来 |
    | void println(String x)        | 用于打印字符串后并终止该行         |
    | void flush()                  | 刷新流                             |
    | void close()                  | 用于关闭输出流并释放有关的资源     |

- PrintWriter类 

  -  基本概念

    -  java.io.PrintWriter类主要用于将对象的格式化形式打印到文本输出流。

  - 常用的方法 

    | 方法声明                | 功能介绍                       |
    | ----------------------- | ------------------------------ |
    | PrintWriter(Writer out) | 根据参数指定的引用来构造对象   |
    | void print(String s)    | 将参数指定的字符串内容打印出来 |
    | void println(String x)  | 打印字符串后并终止该行         |
    | void flush()            | 刷新流                         |
    | void close()            | 关闭流对象并释放有关的资源     |

    #####  案例 不断地提示用户输入要发送的内容，若发送的内容是"bye"则聊天结束，否则将用户输入的内容写 入到文件d:/a.txt中。 要求使用BufferedReader类来读取键盘的输入 System.in代表键盘输入 要求使用PrintStream类负责将数据写入文件 
    
    ```java
    
    import java.io.*;
    import java.text.SimpleDateFormat;
    import java.util.Date;
    
    public class PrintStreamChatTest {
    
        public static void main(String[] args) {
    
            // 由手册可知：构造方法需要的是Reader类型的引用，但Reader类是个抽象类，实参只能传递子类的对象  字符流
            // 由手册可知： System.in代表键盘输入， 而且是InputStream类型的 字节流
            BufferedReader br = null;
            PrintStream ps = null;
            try {
                br = new BufferedReader(new InputStreamReader(System.in));
                ps = new PrintStream(new FileOutputStream("d:/a.txt", true));
    
                // 声明一个boolean类型的变量作为发送方的代表
                boolean flag = true;
    
                while(true) {
                    // 1.提示用户输入要发送的聊天内容并使用变量记录
                    System.out.println("请" + (flag? "张三": "李四") + "输入要发送的聊天内容：");
                    String str = br.readLine();
                    // 2.判断用户输入的内容是否为"bye"，若是则聊天结束
                    if ("bye".equals(str)) {
                        System.out.println("聊天结束！");
                        break;
                    }
                    // 3.若不是则将用户输入的内容写入到文件d:/a.txt中
                    //else {
                    // 获取当前系统时间并调整格式
                    Date d1 = new Date();
                    SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
                    ps.println(sdf.format(d1) + (flag?" 张三说：":" 李四说：") + str);
                    //}
                    flag = !flag;
                }
                ps.println(); // 写入空行 与之前的聊天记录隔开
                ps.println();
                ps.println();
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                // 4.关闭流对象并释放有关的资源
                if (null != ps) {
                    ps.close();
                }
                if (null != br) {
                    try {
                        br.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }
    
    ```
    
    

-   OutputStreamWriter类 

  - 基本概念

    -  java.io.OutputStreamWriter类主要用于实现从字符流到字节流的转换。

  - 常用的方法 

    | 方法声明                                                 | 功能介绍                          |
    | -------------------------------------------------------- | --------------------------------- |
    | OutputStreamWriter(OutputStream out)                     | 根据参数指定的引用来构造对象      |
    | OutputStreamWriter(OutputStream out, String charsetName) | 根据参数指定的引用和编码构造 对象 |
    | void write(String str)                                   | 将参数指定的字符串写入            |
    | void flush()                                             | 刷新流                            |
    | void close()                                             | 用于关闭输出流并释放有关的资 源   |

-   InputStreamReader类 

  - 基本概念

    -  java.io.InputStreamReader类主要用于实现从字节流到字符流的转换。 

  - 常用的方法 

    | 方法声明                                              | 功能介绍                            |
    | ----------------------------------------------------- | ----------------------------------- |
    | InputStreamReader(InputStream in)                     | 根据参数指定的引用来构造对象        |
    | InputStreamReader(InputStream in, String charsetName) | 根据参数指定的引用和编码来构造对 象 |
    | int read(char[] cbuf)                                 | 读取字符数据到参数指定的数组        |
    | void close()                                          | 用于关闭输出流并释放有关的资源      |

- 字节编码

  -  编码表的由来 
    - 计算机只能识别二进制数据，早期就是电信号。为了方便计算机可以识别各个国家的文字，就需要 将各个国家的文字采用数字编号的方式进行描述并建立对应的关系表，该表就叫做编码表。 	
  -  常见的编码表 ASCII：
    - 美国标准信息交换码， 使用一个字节的低7位二位进制进行表示。 ISO8859-1：
    - 拉丁码表，欧洲码表，使用一个字节的8位二进制进行表示。 
    - GB2312：中国的中文编码表，最多使用两个字节16位二进制为进行表示。
    -  GBK：中国的中文编码表升级，融合了更多的中文文字符号，最多使用两个字节16位二进制位表 示。
    -  Unicode：国际标准码，融合了目前人类使用的所有字符，为每个字符分配唯一的字符码。所有的 文字都用两个字节16位二进制位来表示。 
  -  编码的发展
    -  面向传输的众多 UTF（UCS Transfer Format）标准出现了，UTF-8就是每次8个位传输数据，而 UTF-16就是每次16个位。这是为传输而设计的编码并使编码无国界，这样就可以显示全世界上所 有文化的字符了 
    -  Unicode只是定义了一个庞大的、全球通用的字符集，并为每个字符规定了唯一确定的编号，具体 存储成什么样的字节流，取决于字符编码方案。推荐的Unicode编码是UTF-8和UTF-16。
    -  UTF-8：变长的编码方式，可用1-4个字节来表示一个字符。 

-    DataOutputStream类 

    -    基本概念

        -    java.io.DataOutputStream类主要用于以适当的方式将基本数据类型写入输出流中。 

    -   常用的方法 

        | 方法声明                           | 功能介绍                                                     |
        | ---------------------------------- | ------------------------------------------------------------ |
        | DataOutputStream(OutputStream out) | 根据参数指定的引用构造对象 OutputStream类是个抽象 类，实参需要传递子类对象 |
        | void writeInt(int v)               | 用于将参数指定的整数一次性写入输出流，优先写入高字 节        |
        | void close()                       | 用于关闭文件输出流并释放有关的资源                           |

        ```java
        
        import java.io.DataOutputStream;
        import java.io.FileOutputStream;
        import java.io.IOException;
        
        public class DataOutputStreamTest {
        
            public static void main(String[] args) {
                DataOutputStream dos = null;
        
                try {
                    // 1.创建DataOutputStream类型的对象与d:/a.txt文件关联
                    dos = new DataOutputStream(new FileOutputStream("d:/a.txt"));
                    // 2.准备一个整数数据66并写入输出流
                    // 66: 0000 0000 ... 0100 0010    =>   B
                    int num = 66;
                    //dos.writeInt(num);  // 写入4个字节
                    dos.write(num);       // 写入1个字节
                    System.out.println("写入数据成功！");
                } catch (IOException e) {
                    e.printStackTrace();
                } finally {
                    // 3.关闭流对象并释放有关的资源
                    if (null != dos) {
                        try {
                            dos.close();
                        } catch (IOException e) {
                            e.printStackTrace();
                        }
                    }
                }
            }
        }
        
        ```

        

-   DataInputStream类 
  -  基本概念

    -  java.io.DataInputStream类主要用于从输入流中读取基本数据类型的数据。

  - 常用的方法

    | 方法声明                        | 功能介绍                                                     |
    | ------------------------------- | ------------------------------------------------------------ |
    | DataInputStream(InputStream in) | 根据参数指定的引用来构造对象 InputStream类是抽象类， 实参需要传递子类对象 |
    | int readInt()                   | 用于从输入流中一次性读取一个整数数据并返回                   |
    | void close()                    | 用于关闭文件输出流并释放有关的资源                           |
  
    ```java
    
    import java.io.DataInputStream;
    import java.io.FileInputStream;
    import java.io.IOException;
    
    public class DataInputStreamTest {
    
        public static void main(String[] args) {
            DataInputStream dis = null;
    
            try {
                // 1.创建DataInputStream类型的对象与d:/a.txt文件关联
                dis = new DataInputStream(new FileInputStream("d:/a.txt"));
                // 2.从输入流中读取一个整数并打印
                //int res = dis.readInt(); // 读取4个字节
                int res = dis.read();      // 读取1个字节
                System.out.println("读取到的整数数据是：" + res); // 66
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                // 3.关闭流对象并释放有关的资源
                if (null != dis) {
                    try {
                        dis.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }
    
    ```
    
    
  
- **ObjectOutputStream类** 

  -  基本概念

    -  java.io.ObjectOutputStream类主要用于将一个对象的所有内容整体写入到输出流中。 
    - 只能将支持 java.io.Serializable 接口的对象写入流中。
    -  类通过实现 java.io.Serializable 接口以启用其序列化功能。 
    - 所谓序列化主要指将一个对象需要存储的相关信息有效组织成字节序列的转化过程。

  - 常用的方法 

    | 方法声明                             | 功能介绍                               |
    | ------------------------------------ | -------------------------------------- |
    | ObjectOutputStream(OutputStream out) | 根据参数指定的引用来构造对象           |
    | void writeObject(Object obj)         | 用于将参数指定的对象整体写入到输出流中 |
    | void close()                         | 用于关闭输出流并释放有关的资源         |

    ```java
    
    import java.io.FileOutputStream;
    import java.io.IOException;
    import java.io.ObjectOutputStream;
    
    public class ObjectOutputStreamTest {
    
        public static void main(String[] args) {
            ObjectOutputStream oos = null;
    
            try {
                // 1.创建ObjectOutputStream类型的对象与d:/a.txt文件关联
                oos = new ObjectOutputStream(new FileOutputStream("d:/a.txt"));
                // 2.准备一个User类型的对象并初始化
                User user = new User("qidian", "123456", "13511258688");
                // 3.将整个User类型的对象写入输出流
                oos.writeObject(user);
                System.out.println("写入对象成功！");
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                // 4.关闭流对象并释放有关的资源
                if (null != oos) {
                    try {
                        oos.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }
    
    ```
    
    ```java
    
    public class User implements java.io.Serializable {
        private static final long serialVersionUID = -5814716593800822421L;
    
        private String userName;  // 用户名
        private String password;  // 密码
        private transient String phoneNum;  // 手机号  表示该成员变量不参与序列化操作
    
        public User() {
        }
    
        public User(String userName, String password, String phoneNum) {
            this.userName = userName;
            this.password = password;
            this.phoneNum = phoneNum;
        }
    
        public String getUserName() {
            return userName;
        }
    
        public void setUserName(String userName) {
            this.userName = userName;
        }
    
        public String getPassword() {
            return password;
        }
    
        public void setPassword(String password) {
            this.password = password;
        }
    
        public String getPhoneNum() {
            return phoneNum;
        }
    
        public void setPhoneNum(String phoneNum) {
            this.phoneNum = phoneNum;
        }
    
        @Override
        public String toString() {
            return "User{" +
                    "userName='" + userName + '\'' +
                    ", password='" + password + '\'' +
                    ", phoneNum='" + phoneNum + '\'' +
                    '}';
        }
    }
    
    ```
    
    

-  **ObjectInputStream类** 
  -  基本概念 

    - java.io.ObjectInputStream类主要用于从输入流中一次性将对象整体读取出来。 
    - 所谓反序列化主要指将有效组织的字节序列恢复为一个对象及相关信息的转化过程。 

  - 常用的方法 

    | 方法声明                          | 功能介绍                                                     |
    | --------------------------------- | ------------------------------------------------------------ |
    | ObjectInputStream(InputStream in) | 根据参数指定的引用来构造对象                                 |
    | Object readObject()               | 主要用于从输入流中读取一个对象并返回 无法通过返回值 来判断是否读取到文件的末尾 |
    | void close()                      | 用于关闭输入流并释放有关的资源                               |

    ```java
  
    import java.io.FileInputStream;
  import java.io.IOException;
    import java.io.ObjectInputStream;
  
    public class ObjectInputStreamTest {
  
        public static void main(String[] args) {
          ObjectInputStream ois = null;
    
            try {
                // 1.创建ObjectInputStream类型的对象与d:/a.txt文件关联
                ois = new ObjectInputStream(new FileInputStream("d:/a.txt"));
                // 2.从输入流中读取一个对象并打印
                Object obj = ois.readObject();
                System.out.println("读取到的对象是：" + obj); // qidian 123456 13511258688  null
            } catch (IOException e) {
                e.printStackTrace();
            } catch (ClassNotFoundException e) {
                e.printStackTrace();
            } finally {
                // 3.关闭流对象并释放有关的资源
                if (null != ois) {
                    try {
                        ois.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }
    
    ```
  
    
  
  -  序列化版本号 
  
    - 序列化机制是通过在运行时判断类的serialVersionUID来验证版本一致性的。在进行反序列化时， JVM会把传来的字节流中的serialVersionUID与本地相应实体类的serialVersionUID进行比较，如 果相同就认为是一致的，可以进行反序列化，否则就会出现序列化版本不一致的异常 (InvalidCastException)。  
  
  -  transient关键字 
  
    - transient是Java语言的关键字，用来表示一个域不是该对象串行化的一部分。当一个对象被串行 化的时候，transient型变量的值不包括在串行化的表示中，然而非transient型的变量是被包括进 去的。 
  
  -   当希望将多个对象写入文件时，通常建议将多个对象放入一个集合中，然后将集合这个整体看做一 个对象写入输出流中，此时只需要调用一次readObject方法就可以将整个集合的数据读取出来， 从而避免了通过返回值进行是否达到文件末尾的判断。 
  
- **RandomAccessFile类** 

  - 基本概念 

    - java.io.RandomAccessFile类主要支持对随机访问文件的读写操作。 

  - 常用的方法 

    | 方法声明                                   | 功能介绍                                                     |
    | ------------------------------------------ | ------------------------------------------------------------ |
    | RandomAccessFile(String name, String mode) | 根据参数指定的名称和模式构造对象 <br />r: 以只读方式打开 <br />rw：打开以便读取和写入<br />rwd:打开以便读取和写入，同步文件内容的更新 <br />rws:打开以便读取和写入，同步文件内容和元数据 的更新 |
    | int read()                                 | 读取单个字节的数据                                           |
    | void seek(long pos)                        | 用于设置从此文件的开头开始测量的文件指针偏移量               |
    | void write(int b)                          | 将参数指定的单个字节写入                                     |
    | void close()                               | 用于关闭流并释放有关的资源                                   |


```java

import java.io.IOException;
import java.io.RandomAccessFile;

public class RandomAccessFileTest {

    public static void main(String[] args) {
        RandomAccessFile raf = null;

        try {
            // 1.创建RandomAccessFile类型的对象与d:/a.txt文件关联
            raf = new RandomAccessFile("d:/a.txt", "rw");
            // 2.对文件内容进行随机读写操作
            // 设置距离文件开头位置的偏移量，从文件开头位置向后偏移3个字节    aellhello
            raf.seek(3);
            int res = raf.read();
            System.out.println("读取到的单个字符是：" + (char)res); // a l
            res = raf.read();
            System.out.println("读取到的单个字符是：" + (char)res); // h 指向了e
            raf.write('2'); // 执行该行代码后覆盖了字符'e'
            System.out.println("写入数据成功！");

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            // 3.关闭流对象并释放有关的资源
            if (null != raf) {
                try {
                    raf.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

```

