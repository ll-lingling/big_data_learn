# 大数据学习-Java Day11

## String类的概述和使用

##  1 String类的概念（重点） 

-   java.lang.String类用于描述字符串，Java程序中所有的字符串字面值都可以使用该类的对象加以描 述，如："abc"。 

- 该类由final关键字修饰，表示该类不能被继承。 

- 从jdk1.9开始该类的底层不使用char[]来存储数据，而是改成 byte[]加上编码标记，从而节约了一 些空间。 

- 该类描述的字符串内容是个常量不可更改，因此可以被共享使用。 

  - 如： 

    ```java
    String str1 = “abc”; // 其中"abc"这个字符串是个常量不可改变。 
    str1 = “123”; // 将“123”字符串的地址赋值给变量str1。 
    			  //改变str1的指向并没有改变指向的内容 
    ```

    

##  常量池的概念

- 由于String类型描述的字符串内容是常量不可改变，因此Java虚拟机将首次出现的字符串放入常量 池中，若后续代码中出现了相同字符串内容则直接使用池中已有的字符串对象而无需申请内存及创建对 象，从而提高了性能。  

  ```java
  // 常量池验证
  
  public class StringPoolTest {
  
      public static void main(String[] args) {
  
          // 1.验证一下常量池的存在
          // 到目前为止，只有String这个特殊类除了new的方式外还可以直接字符串赋值（包装类除外）
          String str1 = "abc";
          String str2 = "abc";
          System.out.println(str1 == str2); // 比较地址  true
      }
  }
  
  ```

  

## 3  常用的构造方法 

| 方法声明                                     | 功能介绍                                                     |
| -------------------------------------------- | ------------------------------------------------------------ |
| String()                                     | 使用无参方式构造对象得到空字符序列                           |
| String(byte[] bytes, int offset, int length) | 使用bytes数组中下标从offset位置开始的length个字节来 构造对象 |
| String(byte[] bytes)                         | 使用bytes数组中的所有内容构造对象                            |
| String(char[] value, int offset, int count)  | 使用value数组中下标从offset位置开始的count个字符来构 造对象  |
| String(char[] value)                         | 使用value数组中的所有内容构造对象                            |
| String(String original)                      | 根据参数指定的字符串内容来构造对象，新创建对象为参 数对象的副本 |

```java

public class StringConstructorTest {

    public static void main(String[] args) {

        // 1.使用无参方式构造对象并打印
        String str1 = new String();
        // "" 表示空字符串对象，有对象只是里面没有内容
        // null 表示空，连对象都没有
        System.out.println("str1 = " + str1); // ""  自动调用toString方法

        System.out.println("----------------------------------------------------");
        // 2.使用参数指定的byte数组来构造对象并打印
        // 'a' - 97
        byte[] bArr = {97, 98, 99, 100, 101};
        // 使用字节数组中的一部分内容来构造对象，表示使用数组bArr中下标从1开始的3个字节构造字符串对象
        // 构造字符串的思路：就是先将每个整数翻译成对应的字符，再将所有的字符串起来。
        // 98 - 'b'   99 - 'c'  100 - 'd'   => bcd
        String str2 = new String(bArr, 1, 3);
        System.out.println("str2 = " + str2); // bcd

        // 使用整个字节数组来构造字符串对象
        String str3 = new String(bArr);
        System.out.println("str3 = " + str3); // abcde

        System.out.println("----------------------------------------------------");
        // 3.使用字符数组来构造字符串对象
        char[] cArr = {'h', 'e', 'l', 'l', 'o'};
        // 使用字符数组中的一部分内容来构造对象
        // 思路：直接将字符串起来
        String str4 = new String(cArr, 2, 2);
        System.out.println("str4 = " + str4); // ll
        // 使用整个字符数组来构造对象
        String str5 = new String(cArr);
        System.out.println("str5 = " + str5); // hello

        System.out.println("----------------------------------------------------");
        // 4.使用字符串来构造字符串对象
        String str6 = new String("world");
        System.out.println("str6 = " + str6); // world
    }
}

```

#### 笔试考点

```java

public class StringExamTest {

    public static void main(String[] args) {

        // 1.请问下面的代码会创建几个对象？分别存放在什么地方？
        //String str1 = "hello";  // 1个对象  存放在常量池中
        //String str1 = new String("hello"); // 2个对象  1个在常量池中，1个在堆区

        // 2.常量池和堆区对象的比较
        String str1 = "hello";  // 常量池
        String str2 = "hello";  // 常量池
        String str3 = new String("hello"); // 堆区
        String str4 = new String("hello"); // 堆区

        System.out.println(str1 == str2);       // 比较地址  true
        System.out.println(str1.equals(str2));  // 比较内容  true
        System.out.println(str3 == str4);       // 比较地址  false
        System.out.println(str3.equals(str4));  // 比较内容  true
        System.out.println(str2 == str4);       // 比较地址  false
        System.out.println(str2.equals(str4));  // 比较内容 true

        System.out.println("------------------------------------------------------------");
        // 3.常量有优化机制，变量没有
        String str5 = "abcd";
        String str6 = "ab" + "cd";  // 常量优化机制  "abcd"
        System.out.println(str5 == str6); // 比较地址  true

        String str7 = "ab";
        String str8 = str7 + "cd"; // 没有常量优化
        System.out.println(str5 == str8); // 比较地址 false


    }
}

```



## 4  常用的成员方法

| 方法声明             | 功能介绍                                 |
| -------------------- | ---------------------------------------- |
| String toString()    | 返回字符串本身                           |
| byte[] getBytes()    | 将当前字符串内容转换为byte数组并返回     |
| char[] toCharArray() | 用于将当前字符串内容转换为char数组并返回 |

```java

public class StringByteCharTest {
 
    public static void main(String[] args) {

        // 1.创建String类型的对象并打印
        String str1 = new String("world");
        System.out.println("str1 = " + str1); // world

        System.out.println("-----------------------------------------------");
        // 2.实现将String类型转换为byte数组类型并打印
        // 思路：先将字符串拆分为字符，将再每个字符转换为byte类型，也就是获取所有字符的ASCII
        byte[] bArr = str1.getBytes();
        for (int i = 0; i < bArr.length; i++) {
            System.out.println("下标为i的元素是：" + bArr[i]);
        }
        // 将byte数组转回String类型并打印
        String str2 = new String(bArr);
        System.out.println("转回字符串为：" + str2); // world

        System.out.println("-----------------------------------------------");
        // 3.实现将String类型转换为char数组类型并打印
        // 思路：将字符串拆分为字符并保存到数组中
        char[] cArr = str1.toCharArray();
        for (int i = 0; i < cArr.length; i++) {
            System.out.println("下标为" + i + "的字符是：" + cArr[i]);
        }
        // 将char数组转回String类型并打印
        String str3 = new String(cArr);
        System.out.println("转回字符串为：" + str3); // world
    }
}

```



| 方法声明               | 功能介绍                                 |
| ---------------------- | ---------------------------------------- |
| char charAt(int index) | 方法charAt用于返回字符串指定位置的字符。 |
| int length()           | 返回字符串字符序列的长度                 |
| boolean isEmpty()      | 判断字符串是否为空                       |

```java

public class StringCharTest {

    public static void main(String[] args) {

        // 1.构造String类型的对象并打印
        String str1 = new String("hello");
        System.out.println("str1 = " + str1); // hello
        // 2.获取字符串的长度和每个字符并打印
        System.out.println("字符串的长度是：" + str1.length()); // 5
        System.out.println("下标为0的字符是：" + str1.charAt(0)); // h
        System.out.println("下标为1的字符是：" + str1.charAt(1)); // e
        System.out.println("下标为2的字符是：" + str1.charAt(2)); // l
        System.out.println("下标为3的字符是：" + str1.charAt(3)); // l
        System.out.println("下标为4的字符是：" + str1.charAt(4)); // o
        //System.out.println("下标为5的字符是：" + str1.charAt(5)); // StringIndexOutOfBoundsException 字符串下标越界异常

        System.out.println("----------------------------------------------");
        // 3.使用for循环获取所有字符
        for (int i = 0; i < str1.length(); i++) {
            System.out.println("下标为" + i + "的字符是：" + str1.charAt(i)); // h e l l o
        }

        System.out.println("----------------------------------------------");
        // 4.判断字符串是否为空
        System.out.println(0 == str1.length()? "字符串为空": "字符串不为空"); // 不为空
        System.out.println(str1.isEmpty()? "字符串为空": "字符串不为空");     // 不为空

        System.out.println("----------------------------------------------");
        // 5.笔试考点
        // 使用两种方式实现字符串"12345"转换为整数12345并打印
        String str2 = new String("12345");
        // 方式一：调用Integer类中的parseInt()方法即可
        int ia = Integer.parseInt(str2);
        System.out.println("转换出来的整数是：" + ia); // 12345
        // 方式二：利用ASCII来实现类型转换并打印
        // '1' - '0' => 1  '2' - '0' => 2  ...
        int ib = 0;
        for (int i = 0; i < str2.length(); i++) {
            ib = ib*10 + (str2.charAt(i) - '0'); // 1 12 ...
        }
        System.out.println("转换出来的结果是：" + ib); // 12345

        System.out.println("----------------------------------------------");
        // 如何实现整数到字符串的转换
        //String str3 = String.valueOf(ib);
        String str3 = "" + ib; // 推荐使用
        System.out.println("str3 = " + str3); // 12345
    }
}
```



##### 案例  判断字符串“上海自来水来自海上”是否为回文并打印，所谓回文是指一个字符序列无论从左向右读 还是从右向左读都是相同的句子。 

```java

public class StringJudgeTest {

    public static void main(String[] args) {

        // 1.创建字符串对象并打印
        String str1 = new String("上海自来水来自海上");
        System.out.println("str1 = " + str1); // 上海自来水来自海上   9
        // 2.判断该字符串内容是否为回文并打印
        for (int i = 0; i < str1.length()/2; i++) {
            if (str1.charAt(i) != str1.charAt(str1.length()-i-1)) {  // 0和8   1和7  2和6  3和5
                System.out.println(str1 + "不是回文！");
                return;  // 仅仅是用于实现方法的结束
            }
        }
        System.out.println(str1 + "是回文！");
    }
}

```



| 方法声明                            | 功能介绍                                 |
| ----------------------------------- | ---------------------------------------- |
| int compareTo(String anotherString) | 用于比较调用对象和参数对象的大小关系     |
| int compareToIgnoreCase(String str) | 不考虑大小写，也就是'a'和'A'是相等的关系 |

##### 案例  编程实现字符串之间大小的比较并打印。 

```java

public class StringCompareTest {

    public static void main(String[] args) {

        // 1.构造String类型的对象并打印
        String str1 = new String("hello");
        System.out.println("str1 = " + str1); // hello

        // 2.使用构造好的对象与其它字符串对象之间比较大小并打印
        System.out.println(str1.compareTo("world"));  // 'h' - 'w' => 104 - 119 => -15
        System.out.println(str1.compareTo("haha"));   // 'e' - 'a' => 101 - 97  => 4
        System.out.println(str1.compareTo("hehe"));   // 'l' - 'h' => 108 - 104 => 4
        System.out.println(str1.compareTo("heihei")); // 'l' - 'i' => 108 - 105 => 3
        System.out.println(str1.compareTo("helloworld")); // 长度： 5 - 10 => -5
        System.out.println(str1.compareToIgnoreCase("HELLO")); // 0
    }
}

```



| 方法声明                                       | 功能介绍                                 |
| ---------------------------------------------- | ---------------------------------------- |
| String concat(String str)                      | 用于实现字符串的拼接                     |
| boolean contains(CharSequence s)               | 用于判断当前字符串是否包含参数指定的内容 |
| String toLowerCase()                           | 返回字符串的小写形式                     |
| String toUpperCase()                           | 返回字符串的大写形式                     |
| String trim()                                  | 返回去掉前导和后继空白的字符串           |
| boolean startsWith(String prefix)              | 判断字符串是否以参数字符串开头           |
| boolean startsWith(String prefix, int toffset) | 从指定位置开始是否以参数字符串开头       |
| boolean endsWith(String suffix)                | 判断字符串是否以参数字符串结尾           |

##### 案例   编程实现上述方法的使用。 

```java

public class StringManyMethodTest {

    public static void main(String[] args) {

        // 1.构造String类型的对象并打印
        String str1 = new String("     Let Me Give You Some Color To See See!");
        System.out.println("str1 = " + str1); //      Let Me Give You Some Color To See See!

        // 2.实现各种成员方法的调用和测试
        boolean b1 = str1.contains("some");
        System.out.println("b1 = " + b1); // false  区分大小写
        b1 = str1.contains("Some");
        System.out.println("b1 = " + b1); // true

        System.out.println("----------------------------------------------");
        // 将所有字符串转换为大写  小写  以及去除两边的空白字符
        String str2 = str1.toUpperCase();
        System.out.println("str2 = " + str2); //    LET ME GIVE YOU SOME COLOR TO SEE SEE!
        System.out.println("str1 = " + str1); //    Let Me Give You Some Color To See See!   常量

        String str3 = str1.toLowerCase();
        System.out.println("str3 = " + str3); //    let me give you some color to see see!
        System.out.println("str1 = " + str1); //    Let Me Give You Some Color To See See!

        String str4 = str1.trim();
        System.out.println("str4 = " + str4); //Let Me Give You Some Color To See See!     奇点

        System.out.println("----------------------------------------------");
        // 判断字符串是否以...开头  以...结尾
        b1 = str1.startsWith("Let");
        System.out.println("b1 = " + b1); // false
        b1 = str1.startsWith(" ");
        System.out.println("b1 = " + b1); // true
        // 从下标5开始是否以"Let"开头
        b1 = str1.startsWith("Let", 5);
        System.out.println("b1 = " + b1); // true

        b1 = str1.endsWith("See");
        System.out.println("b1 = " + b1); // false
        b1 = str1.endsWith("See!");
        System.out.println("b1 = " + b1); // true
    }
}

```



| 方法声明                                       | 功能介绍                                                     |
| ---------------------------------------------- | ------------------------------------------------------------ |
| boolean equals(Object anObject)                | 用于比较字符串内容是否相等并返回                             |
| int hashCode()                                 | 获取调用对象的哈希码值                                       |
| boolean equalsIgnoreCase(String anotherString) | 用于比较字符串内容是否相等并返回，不考虑大小写， 如：'A'和'a'是相等 |

#####  案例   提示用户从键盘输入用户名和密码信息，若输入”admin”和”123456”则提示“登录成功，欢迎使 用”，否则提示“用户名或密码错误，您还有n次机会”，若用户输入三次后依然错误则提示“账户已 冻结，请联系客服人员！ 

```java

import java.util.Scanner;

public class StringEqualsTest {

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        for (int i = 3; i > 0; i--) {
            // 1.提示用户从键盘输入用户名和密码信息并使用变量记录
            System.out.println("请输入您的用户名和密码信息：");
            String userName = sc.next();
            String password = sc.next();

            // 2.判断用户名和密码是否为"admin"和"123456"并给出提示
            //if ("admin".equals(userName) && "123456".equals(password)) {
            if ("admin".equalsIgnoreCase(userName) && "123456".equals(password)) { // 防止空指针异常
                System.out.println("登录成功，欢迎使用！");
                break;
            } //else {
            if (1 == i) {
                System.out.println("账户已冻结，请联系客服人员！");
            } else {
                System.out.println("用户名或密码错误，您还有" + (i - 1) + "次机会！");
            }
            //}
        }
        // 关闭扫描器
        sc.close();
    }
}

```



| 方法声明                                   | 功能介绍                                                 |
| ------------------------------------------ | -------------------------------------------------------- |
| int indexOf(int ch)                        | 用于返回当前字符串中参数ch指定的字符第一次出现的 下标    |
| int indexOf(int ch, int fromIndex)         | 用于从fromIndex位置开始查找ch指定的字符                  |
| int indexOf(String str)                    | 在字符串中检索str返回其第一次出现的位置，若找不到 返回-1 |
| int indexOf(String str, int fromIndex)     | 表示从字符串的fromIndex位置开始检索str第一次出现 的位置  |
| int lastIndexOf(int ch)                    | 用于返回参数ch指定的字符最后一次出现的下标               |
| int lastIndexOf(int ch, int fromIndex)     | 用于从fromIndex位置开始查找ch指定字符出现的下标          |
| int lastIndexOf(String str)                | 返回str指定字符串最后一次出现的下标                      |
| int lastIndexOf(String str, int fromIndex) | 用于从fromIndex位置开始反向搜索的第一次出现的下 标。     |

##### 案例  编写通用的代码可以查询字符串"Good Good Study, Day Day Up!"中所有"Day"出现的索引位置并 打印出来。  

```java

public class StringIndexTest {

    public static void main(String[] args) {

        // 1.构造String类型的对象并打印
        String str1 = new String("Good Good Study, Day Day Up!");
        System.out.println("str1 = " + str1); // Good Good Study, Day Day Up!

        // 2.实现字符串中指定字符和字符串的查找功能
        int pos = str1.indexOf('g');
        System.out.println("pos = " + pos); // -1  代表查找失败
        pos = str1.indexOf('G');
        System.out.println("pos = " + pos); // 0   该字符第一次出现的索引位置
        // 表示从下标0开始查找字符'G'第一次出现的索引位置，包含0
        pos = str1.indexOf('G', 0);
        System.out.println("pos = " + pos); // 0
        pos = str1.indexOf('G', 1);
        System.out.println("pos = " + pos); // 5

        System.out.println("------------------------------------------------------");
        // 查找字符串
        pos = str1.indexOf("day");
        System.out.println("pos = " + pos); // -1
        pos = str1.indexOf("Day");
        System.out.println("pos = " + pos); // 17   字符串中第一个字符的下标
        pos = str1.indexOf("Day", 17);
        System.out.println("pos = " + pos); // 17   字符串中第一个字符的下标
        pos = str1.indexOf("Day", 18);
        System.out.println("pos = " + pos); // 21   字符串中第一个字符的下标

        System.out.println("------------------------------------------------------");
        // 编写通用代码实现将字符串str1中所有"Day"出现的索引位置找到并打印出来
        pos = str1.indexOf("Day");
        while (-1 != pos) {
            System.out.println("pos = " + pos); // 17
            pos = str1.indexOf("Day", pos+1);
        }

        System.out.println("------------------------------------------------------");
        // 优化一下
        pos = 0;
        while ((pos = str1.indexOf("Day", pos)) != -1) {
            System.out.println("pos = " + pos);
            pos += "Day".length();
        }

        System.out.println("------------------------------------------------------");
        // 3.实现字符和字符串内容的反向查找
        pos = str1.lastIndexOf('G');
        System.out.println("pos = " + pos); // 5
        // 从下标5的位置开始反向查找
        pos = str1.lastIndexOf('G', 5);
        System.out.println("pos = " + pos); // 5

        pos = str1.lastIndexOf('G', 6);
        System.out.println("pos = " + pos); // 5

        pos = str1.lastIndexOf('G', 4);
        System.out.println("pos = " + pos); // 0

        System.out.println("------------------------------------------------------");
        pos = str1.lastIndexOf("Day");
        System.out.println("pos = " + pos); // 21
        pos = str1.lastIndexOf("Day",  21);
        System.out.println("pos = " + pos); // 21
        pos = str1.lastIndexOf("Day", 20);
        System.out.println("pos = " + pos); // 17
        pos = str1.lastIndexOf("Day", 15);
        System.out.println("pos = " + pos); // -1
    }
}

```



| 方法声明                                       | 功能介绍                                                     |
| ---------------------------------------------- | ------------------------------------------------------------ |
| String substring(int beginIndex, int endIndex) | 返回字符串中从下标beginIndex（包括）开始到 endIndex（不包括）结束的子字符串 |
| String substring(int beginIndex)               | 返回字符串中从下标beginIndex（包括）开始到字符串结尾 的子字符串 |

##### 案例   提示用户从键盘输入一个字符串和一个字符，输出该字符(不含)后面的所有子字符串。 

```java

import java.util.Scanner;

public class SubStringTest {

    public static void main(String[] args) {

        // 1.构造String类型的对象并打印
        String str1 = new String("Happy Wife, Happy Life!");
        System.out.println("str1 = " + str1); // Happy Wife, Happy Life!

        // 2.获取字符串中的一部分内容并打印
        // 表示从当前字符串中下标12开始获取子字符串
        String str2 = str1.substring(12);
        System.out.println("str2 = " + str2); // Happy Life!
        // 可以取到6但是取不到10
        String str3 = str1.substring(6, 10);
        System.out.println("str3 = " + str3); // Wife

        System.out.println("---------------------------------------------------------");
        // 3.获取输入字符串中从输入字符起的子字符串内容
        System.out.println("请输入一个字符串：");
        Scanner sc = new Scanner(System.in);
        String str4 = sc.next();
        System.out.println("请输入一个字符：");
        String str5 = sc.next();
        // 从str4中查找str5第一次出现的索引位置
        int pos = str4.indexOf(str5);
        System.out.println("pos = " + pos);
        // 根据该位置获取子字符串
        String str6 = str4.substring(pos+1);
        System.out.println("获取到的子字符串是：" + str6);
    }
}

```



## 5  正则表达式

- 基本概念
  -  正则表达式本质就是一个“规则字符串”，可以用于对字符串数据的格式进行验证，以及匹配、查 找、替换等操作。该字符串通常使用^运算符作为开头标志，使用$运算符作为结尾标志，当然也可以省 略。  
- 规则

| 正则表达式  | 说明                                                        |
| ----------- | ----------------------------------------------------------- |
| [abc]       | 可以出现a、b、c中任意一个字符                               |
| [^abc]      | 可以出现任何字符，除了a、b、c的任意字符                     |
| [a-z]       | 可以出现a、b、c、……、z中的任意一个字符                      |
| [a-zA-Z0-9] | 可以出现a~z、A~Z、0~9中任意一个字符                         |
| .           | 任意一个字符（通常不包含换行符）                            |
| \d          | 任意一个数字字符，相当于[0-9]                               |
| \D          | 任意一个非数字字符                                          |
| \s          | 空白字符，相当于[\t\n\x0B\f\r]                              |
| \S          | 非空白字符                                                  |
| \w          | 任意一个单词字符，相当于[a-zA-Z_0-9]                        |
| \W          | 任意一个非单词字符                                          |
| X?          | 表示X可以出现一次或一次也没有，也就是0 ~ 1次                |
| X*          | 表示X可以出现零次或多次，也就是0 ~ n次                      |
| X+          | 表示X可以出现一次或多次，也就是1 ~ n次                      |
| X{n}        | 表示X可以出现恰好 n 次                                      |
| X{n，}      | 表示X可以出现至少 n 次，也就是>=n次                         |
| X{n，m}     | 表示X可以出现至少 n 次，但是不超过 m 次，也就是>=n并且<=m次 |



- 相关方法

| 方法名称                      | 方法说明                                                  |
| ----------------------------- | --------------------------------------------------------- |
| boolean matches(String regex) | 判断当前正在调用的字符串是否匹配参数指定的正则表达式规 则 |

##### 案例    使用正则表达式描述一下银行卡密码的规则：要求是由6位数字组成。   

##### 			使用正则表达式描述一下QQ号码的规则：要求是由非0开头的5~15位数组成。                 

##### 			使用正则表达式描述一下手机号码的规则：要求是由1开头，第二位数是3、4、5、7、8中的一 位，总共11位 

##### 			描述身份证号码的规则：总共18位，6位数字代表地区，4位数字代表年，2位数字代表月，2位数 字代表日期， 3位数字代表个人，最后一位可能数字也可能是X。 

```java

import java.util.Scanner;

public class StringRegTest {

    public static void main(String[] args) {

        // 1.定义描述规则的正则表达式字符串并使用变量记录
        // 正则表达式只能对数据格式进行验证，无法对数据内容的正确性进行检查，内容的正确性检查需要后台查询数据库
        // 描述银行卡密码的规则：由6位数字组成
        //String reg = "^[0-9]{6}$";
        //String reg = "[0-9]{6}";
        //String reg = "\\d{6}";
        // 使用正则表达式描述一下QQ号码的规则：要求是由非0开头的5~15位数字组成。
        //String reg = "[1-9]\\d{4,14}";
        //使用正则表达式描述一下手机号码的规则：要求是由1开头，第二位数是3、4、5、7、8中的一位，总共11位
        //String reg = "1[34578]\\d{9}";
        //描述身份证号码的规则：总共18位，6位数字代表地区，4位数字代表年，2位数字代表月，2位数字代表日期， 3位数字代表个人，
        // 最后一位可能数字也可能是X。
        String reg = "(\\d{6})(\\d{4})(\\d{2})(\\d{2})(\\d{3})([0-9|X])";
        // 2.提示用户从键盘输入指定的内容并使用变量记录
        Scanner sc = new Scanner(System.in);
        while(true) {
            //System.out.println("请输入您的银行卡密码：");
            //System.out.println("请输入您的QQ号码：");
            //System.out.println("请输入您的手机号码：");
            System.out.println("请输入您的身份证号码：");
            String str = sc.next();

            // 3.判断用户输入的字符串内容是否满足指定的规则并打印
            if (str.matches(reg)) {
                //System.out.println("银行卡密码的格式正确！");
                System.out.println("输入字符串的格式正确！");
                break;
            } else {
                //System.out.println("银行卡密码的格式错误！");
                System.out.println("输入字符串的格式错误！");
            }
        }
    }
}

```



| 方法名称                                              | 方法说明                                                     |
| ----------------------------------------------------- | ------------------------------------------------------------ |
| String[] split(String regex)                          | 参数regex为正则表达式，以regex所表示的字符串为分隔 符，将字符串拆分成字符串数组 |
| String replace(char oldChar, char newChar)            | 使用参数newChar替换此字符串中出现的所有参数 oldChar          |
| String replaceFirst(String regex, String replacement) | 替换此字符串匹配给定的正则表达式的第一个子字符串             |
| String replaceAll(String regex, String replacement)   | 将字符串中匹配正则表达式regex的字符串替换成 replacement      |

```java

public class StringRegMethodTest {

    public static void main(String[] args) {

        // 1.准备一个字符串对象并打印
        String str1 = "1001,zhangfei,30";
        System.out.println("str1 = " + str1); // 1001,zhangfei,30
        // 2.按照逗号对字符串内容进行切割
        String[] sArr = str1.split(",");
        for (int i = 0; i < sArr.length; i++) {
            System.out.println("下标为" + i + "的字符串是：" + sArr[i]); // 1001 zhangfei 30
        }

        System.out.println("--------------------------------------------------------------");
        // 3.准备一个字符串内容并进行替换
        String str2 = "我的小名叫大帅哥";
        // 将字符串中所有的字符'我'替换为'你'
        String str3 = str2.replace('我', '你');
        System.out.println("str2 = " + str2); // 我的小名叫大帅哥
        System.out.println("str3 = " + str3); // 你的小名叫大帅哥
        // 将字符串中所有的字符'大'替换为'小'
        String str4 = str3.replace('大', '小');
        System.out.println("str4 = " + str4); // 你的小名叫小帅哥
        // 将字符串中所有的字符'小'替换为'大'
        String str5 = str4.replace('小', '大');
        System.out.println("str5 = " + str5); // 你的大名叫大帅哥

        System.out.println("--------------------------------------------------------------");
        // 4.准备一个字符串进行字符串内容的替换
        String str6 = "123abc456def789ghi";
        // 将第一个数字字符串替换为"#"
        String str7 = str6.replaceFirst("\\d+", "#");
        System.out.println("替换第一个字符串后的结果是：" + str7); // #abc456def789ghi
        // 将所有字母字符串替换为"$$$"
        String str8 = str7.replaceAll("[a-z]+", "A");
        System.out.println("str8 = " + str8); // #A456A789A

    }
}

```

