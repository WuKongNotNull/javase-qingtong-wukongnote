# **10-异常**

(1)程序出现的不正常的情况，跳出提示。

(2)异常的体系

		Throwable
	
			|--Error	严重问题，程序员一般无法处理的异常。
	
			|--Exception
	
				|--RuntimeException	运行期异常，我们需要修正代码
	
				|--非RuntimeException 编译期异常，必须处理的，否则程序编译不通过

(3)异常的处理

		A:JVM的默认处理
	
			把异常的名称,原因,位置等信息输出在控制台，但是呢程序不能继续执行了
	
		B:自己处理
	
			a:try...catch...finally
	
				自己编写处理代码,后面的程序可以继续执行
	
			b:throws
	
				把自己处理不了的，在方法上声明，告诉调用者，这里有问题

### **程序中的异常**
```java
import java.util.Scanner;

/**
 * 演示程序中的异常
 */
public class Test1 {
	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		System.out.print("请输入被除数（整型）:");
		int num1 = in.nextInt();
		System.out.print("请输入除数（整型）:");
		int num2 = in.nextInt();
		System.out.println(num1+"/"+ num2 +"="+ num1/ num2);
		System.out.println("感谢使用本程序！");
	}
}
```

### **传统处理程序中的异常**
```java
import java.util.Scanner;

/**
 * 传统处理程序中的异常
 */
public class Test2 {
	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		System.out.print("请输入被除数:");
		int num1 = in.nextInt();
		System.out.print("请输入除数:");
		int num2 = 0;
		if (in.hasNextInt()) { // 如果输入的除数是整数
			num2 = in.nextInt();
			if (0 == num2) { // 如果输入的除数是0
				System.err.println("输入的除数是0，程序退出。");
				System.exit(1);
			}
			System.out.println(num1+"/"+ num2 +"="+ num1/ num2);
			System.out.println("感谢使用本程序！");
		} else { // 如果输入的除数不是整数
			System.err.println("输入的除数不是整数，程序退出。");
			System.exit(1);
		}
		
	}
}

```
### **使用try-catch处理异常**
```java
import java.util.Scanner;

/**
 * 使用try-catch进行异常处理。
 */
public class Test3 {
    public static void main(String[] args) {
	    Scanner in = new Scanner(System.in);
            System.out.print("请输入被除数:");        
	    try {
            int num1 = in.nextInt();
            System.out.print("请输入除数:");
            int num2 = in.nextInt();
            System.out.println(num1+"/"+ num2 +"="+ num1/ num2);          
        } catch (Exception e) {
            System.err.println("出现错误:被除数和除数必须是整数,除数不能为零。");
            e.printStackTrace();
        }
       System.out.println("感谢使用本程序!");
    }
}
```
### **使用try-catch-finally处理异常**
```java
import java.util.Scanner;
/**
 * 使用try-catch-finally进行异常处理。
 */
public class Test4 {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        System.out.print("请输入被除数:");
        try {
            int num1 = in.nextInt();
            System.out.print("请输入除数:");
            int num2 = in.nextInt();
            System.out.println(num1+"/"+ num2 +"="+ num1/ num2);
        } catch (Exception e) {
            System.err.println("出现错误:被除数和除数必须是整数,除数不能为零。");
            //System.exit(1); // finally语句块不执行的唯一情况
        } finally {
            System.out.println("感谢使用本程序!");
        }
    }
}

```
### **try块和catch块中return语句的执行**
```java
import java.util.Scanner;

/**
 * 测试try块和catch块中return语句的执行。
 * 
 */
public class Test5 {
	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
        System.out.print("请输入被除数:");
        try {
            int num1 = in.nextInt();
            System.out.print("请输入除数:");
            int num2 = in.nextInt();
            System.out.println(num1+"/"+ num2 +"="+ num1/ num2);
            return; //finally语句块仍旧会执行
        } catch (Exception e) {
            System.err.println("出现错误:被除数和除数必须是整数,除数不能为零");               
            return; //finally语句块仍旧会执行
        } finally {
            System.out.println("感谢使用本程序!");
        }
	}
}

```
### **使用多重catch处理异常**
```java
import java.util.Scanner;
import java.util.InputMismatchException;
/**
 * 多重catch块
 * 
 */
public class Test6 {
  public static void main(String[] args) {
      Scanner in = new Scanner(System.in);
      System.out.print("请输入被除数:");
	  	try {
          int num1 = in.nextInt();
          System.out.print("请输入除数:");
          int num2 = in.nextInt();
          System.out.println(num1+"/"+ num2 +"="+ num1/ num2);
      } catch (InputMismatchException e) {
          System.err.println("被除数和除数必须是整数。");
      } catch (ArithmeticException e) {
          System.err.println("除数不能为零。");
      } catch (Exception e) {  //该异常捕捉在前，报错
          System.err.println("其他未知异常。");
      } finally {
          System.out.println("感谢使用本程序!");
      }
   }
}

```
### **使用throws声明异常**
```java
import java.util.Scanner;
/**
 * 使用throws声明异常。
 */
public class Test7 {
    /**
     * 通过try-catch捕获并处理异常。
     * @param args
     */
    public static void main(String[] args) {
        try {
            divide();
        } catch (Exception e) {
            System.err.println("出现错误:被除数和除数必须是整数,除数不能为零");
            e.printStackTrace();
        }
    }
//  /**
//   * 通过throws继续声明异常。
//   */
//  public static void main(String[] args) throws Exception {
//      divide();
//  }

    /**
     * 输入被除数和除数,计算商并输出。
     * @throws Exception
     */
    public static void divide() throws Exception {
        Scanner in = new Scanner(System.in);
        System.out.print("请输入被除数:");
        int num1 = in.nextInt();
        System.out.print("请输入除数:");
        int num2 = in.nextInt();
        System.out.println(num1+"/"+ num2 +"="+ num1/ num2);
    }
}

```
### **使用throw抛出异常**
```java
/**
 * 使用throw在方法内抛出异常。
 */
public class Person {
	private String name = "";// 姓名
	private int age = 0;// 年龄
	private String sex = "男";// 性别
	/**
	 * 设置性别。
	 * @param sex 性别
	 * @throws Exception
	 */
	public void setSex(String sex) throws Exception {
		if ("男".equals(sex) || "女".equals(sex))
			this.sex = sex;
		else {
			throw new Exception("性别必须是“男”或者“女”！");
		}
	}
	/**
	 * 打印基本信息。
	 */
	public void print() {
		System.out.println(this.name + "（" + this.sex 
				+ "，" + this.age + "岁）");
	}
}
```
```java
/**
 * 捕获throw抛出的异常。
 */
public class Test8 {
	public static void main(String[] args) {
		Person person = new Person();
		try {
			person.setSex("Male");
			person.print();
		} catch (Exception e) {			
			e.printStackTrace();
		}
	}
}
```

### **Checked异常必须处理**
```java
import java.io.*;
/**
 * 不处理Checked异常。
 */
public class Test9 {
	public static void main(String[] args) {
		FileInputStream fis = null;
		// 创建指定文件的流。
		fis = new FileInputStream(new File("accp.txt"));
		// 创建指定文件的流。
		fis.close();
	}
}
```
```java
import java.io.*;

/**
 * 处理Checked异常。
 */
public class Test10 {
	public static void main(String[] args) {
		FileInputStream fis = null;
		try {
			// 创建指定文件的流。
			fis = new FileInputStream(new File("accp.txt"));
		} catch (FileNotFoundException e) {
			System.err.println("无法找到指定文件！");
			e.printStackTrace();
		}
		try {
			// 关闭指定文件的流。
			fis.close();
		} catch (IOException e) {
			System.err.println("关闭指定文件输入流时出现异常！");
			e.printStackTrace();
		}
	}
}
```

### **自定义异常**
```java
public class GenderException extends Exception{
	//构造方法1
	public GenderException(String message) {
		  super(message);
		}
}
```

```java
class Person{
	private String name="";			//姓名
	private int age=0;				//年龄
	private String sex="男";		//性别
	 //设置性别
	public void setSex(String sex) throws GenderException {
		  if ("男".equals(sex) || "女".equals(sex))
		    this.sex = sex;
		  else {
		    throw new GenderException("性别必须是“男”或者“女”！");
		  }
	}

	//打印基本信息
	public void print(){
		System.out.println(this.name+"（"+this.sex+"，"+this.age+"岁）");
	}
}
public class Test{
	public static void main(String[] args){
		Person person = new Person();
		  try {
		    person.setSex("Male");
		    person.print();
		  } catch (GenderException e) {
		    e.printStackTrace();
		  }

	}
}
```

### **练习**

1、根据编号输出课程名称?

```java
import java.util.Scanner;

/**
 * 测试try-catch-finally的使用，根据编号输出课程名称
 */
public class TestException1 {
	public static void main(String[] args) {
		System.out.print("请输入课程代号(1～3之间的数字):");
		Scanner in = new Scanner(System.in);
		try {
			int courseCode = in.nextInt();
			switch (courseCode) {
			case 1:
				System.out.println("C#编程");
				break;
			case 2:
				System.out.println("Java编程");
				break;
			case 3:
				System.out.println("SQL基础");
			}
		} catch (Exception ex) {
			System.out.println("输入不为数字!");
			ex.printStackTrace();
		} finally {
			System.out.println("欢迎提出建议!");
		}
	}
}

```

2、使用throw抛出年龄异常?

```java
/**
 * 使用throw在方法内抛出异常。
 */
public class Person {
	private String name = "";// 姓名
	private int age = 0;// 年龄
	private String sex = "男";// 性别
	/**
	 * 设置性别。
	 * @param sex 性别
	 * @throws Exception
	 */
	public void setSex(String sex) throws Exception {
		if ("男".equals(sex) || "女".equals(sex))
			this.sex = sex;
		else {
			throw new Exception("性别必须是“男”或者“女”！");
		}
	}
	
	public void setAge(int age) throws Exception {
		if (age>=1 && age<=100)
			this.age = age;
		else {
			throw new Exception("年龄必须在1到100之间！");
		}
	}

	/**
	 * 打印基本信息。
	 */
	public void print() {
		System.out.println(this.name + "（" + this.sex 
				+ "，" + this.age + "岁）");
	}
}
```

```java
/**
 * 捕获throw抛出的异常。
 */
public class TestException2{
	public static void main(String[] args) {
		Person person = new Person();
		try {
			person.setAge(110);
			person.print();
		} catch (Exception e) {			
			e.printStackTrace();
		}
	}
}
```