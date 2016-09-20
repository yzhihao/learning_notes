#流
## 分类
字节流：Inputstream（输入）,Iutputstream，读出来的数据就是010101...一个一个字节读，一个字节8位
字符流：Reader（输入），Writer，一个字符一个字符往外读，2个字节一个字符（在java中）
输入指文件对程序输入。就是Inputstream，字节字符流的主体都是文件。
节点流：直接输入输出，
处理流：对节点流处理的流，套在节点流外面，像水流中的过滤管道（高级）
##方法
buffer就是像小桶一样先读满后输入到程序。
read（）是一个一个字节读。
##字符流很有必要
##
/文件名分隔符，也可以有一个\\
##
Fileoutputstream如果没有文件就会自动生成一个文件。注意不能健目录。
缓冲区可以大大减少对硬盘的读写，减少对硬盘的伤害。

##IO——对象的序列化和反序列化
###一、概念
直接实现Serializable接口的类是JDK自动把这个类的对象序列化，而如果实现public interface Externalizable extends Serializable的类则可以自己控制对象的序列化，建议能让JDK自己控制序列化的就不要让自己去控制。<br>

1. 对象序列化，就是将Object转换成byte序列，反之叫对象的反序列化
2. 序列化流（ObjectOutputStream)，字节的过滤流 —— writeObject()方法
  反序列化流（ObjectInputStream）—— readObject（）方法
3. 序列化接口（Serializable）
对象必须实现序列化接口，才能进行序列化，否则将出现异常。
这个接口，没有任何方法，只是一个【标准】
###二、transient关键字
1. transient修饰的元素，不会进行JVM默认的序列化：如int transient age = 10;在序列化和反序列化后，age的值为默认分配的值0
2. 可以自己通过重写序列化操作方式，来对transient修饰的元素进行想要的序列化。
3. 
序列化：
transient 关键字：被transient修饰的元素，该元素不会进行jvm默认的序列化，但可以自己完成这个元素的序列化
###注意：
（1）在以后的网络编程中，如果有某些元素不需要传输，那就可以用transient修饰，来节省流量;对有效元素序列化，提高性能。<br>
（2）可以使用writeObject自己完成这个元素的序列化。ArrayList就是用了此方法进行了优化操作。ArrayList最核心的容器Object[] elementData使用了transient修饰，但是在writeObject自己实现对elementData数组的序列化。只对数组中有效元素进行序列化。readObject与之类似。<br>

	（3）java.io.ObjectOutputStream.defaultWriteObject();
	// 把jvm能默认序列化的元素进行序列化操作
	java.io.ObjectOutputStream.writeInt(age);// 自己完成序列化<br>
	（4）
	java.io.ObjectOutputStream.defaultReadObject();// 把jvm能默认反序列化的元素进行反序列化
	this.age = java.io.ObjectOutputStream.readInt(); // 自己完成age的反序列化操作
	4. 再对于无法默认序列化的成员，可以进行.writeObject(obj)和this.obj = s.readObject()完成序列化
3. 这样做的目的是提高效率。如ArrayList里，对数组的有效对象进行序列化

##序列化过程中子父类构造函数问题
###一、父类实现了serializable接口，子类继承就可序列化。
1、子类在反序列化时，父类实现了序列化接口，则不会递归调用其构造函数。
###二、父类未实现serializable接口，子类自行实现可序列化
2、子类在反序列化时，父类没有实现序列化接口，则会递归调用其构造函数。