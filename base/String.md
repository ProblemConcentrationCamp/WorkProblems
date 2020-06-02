

#### String ####
1.String 被final修饰。是不可变类。



2.String 保存数据 是一个 cahr 类型的数组。

       private final char value[];



3.String与二进制互转乱码。

主要是因为在二进制转化的时候，没有强制规定文件编码。在不同环境默认的文件编码是不一致的。 所以在所有编码的地方，都强制使用UTF-8

str.getBytes("UTF-8");

new String(bytes,"UTS-8")



4.相等判断。在源码中，两个String是否相等。

  A. 通过指针判断是否相等。

      if (this == anObject) 

  B. 判断数据类型是否一致

       if (anObject instanceof String)

  C. 根据String的底层数据结构特征。判断char数组的长度是否相等。

  D. 在数组长度也相等的情况下。判断数据中每一个字符是否相等。

  

5.字符串替换。

replace 替换所有匹配的字符或字符串

replaceFirst 替换第一个匹配的字符或字符串

replaceAll 替换所有匹配的字符或字符串。不同的是，第一个参数可以使要匹配的字符，也可以是正则。



6.字符串拆分 split

将字符串按照指定的字符查分成数组。

str.split(",")  或者 str.split(","，1)

第二个参数是limit，表示限制拆分成几个元素。拆分结果不会出现拆分的字段。

在一些特殊情况下：

String a =",a,,b,";

a.split(",") 结果:["","a","","b"] 

我们希望拆分得到的结果是去除空值或者空格的。

这时候可以考虑使用Google的Guava。

String a =",a, , b c ,";

// Splitter 是 Guava 提供的 API 

List<String> list = Splitter.on(',')

  .trimResults()// 去掉空格

  .omitEmptyStrings()// 去掉空值

  .splitToList(a);

log.info("Guava 去掉空格的分割方法：{}",JSON.toJSONString(list));

// 打印出的结果为：

["a","b c"]



7.数组或者list合并成字符串 String.join

两个参数，一个是合并后的分隔符，第二个是要合并的数组或者list。String.join 如果join的是一个list。无法自动过滤null，无法依次join。此时可以使用Guava。

Joiner joiner = Joiner.on(",").skipNulls();
