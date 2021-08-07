# 第一个java程序

1．HelloWorld.java

**public** **class** HelloWorld {

​	

​	**public** **static** **void** main(String[] args) {

​		System.**out**.println("HelloWorld");

​	}

 

}

代码说明：

1） public:如果有public修饰需要class名和文件名相同；

2） class：修饰类。

3） main函数：java程序的入口和出口，表示一个程序从main函数的第一行开始，到最后一行结束；要想正确运行一个java程序必须要有一个main函数(固定写法)；

4） System.out.println("HelloWorld")：标准输出，表示在控制台输出字符串“HelloWorld”（字符串必须使用双引号）；

5） 编译.java源程序：javac  HelloWorld.java；会在当前路径下生成HelloWorld.class

6） 运行程序：java   HelloWorld；不需要加.class，因为JVM虚拟机去找的HelloWorld类，而不是HelloWorld.class文件；

2.CLASSPATH变量的说明

CLASSPATH：当使用java去运行.class文件的时候，首先从当前路径查找，如果当前路径没有，则在CLASSPATH所指定的路径下继续查找；

 

## **练习**

1.使用记事本编辑‘’hello world‘’程序。

2.使用eclipse或者Idea编写‘’hello world‘’程序。

3.JDK和JRE分别是什么？它们的区别是什么？

4.JVM的优势是什么？

# 