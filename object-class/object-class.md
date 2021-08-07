## **7-Object类**

	(1)Object是类层次结构的根类，所有的类都直接或者间接的继承自Object类。
	
	(2)Object类的构造方法有一个，并且是无参构造
	
		这其实就是理解当时我们说过，子类构造方法默认访问父类的构造是无参构造
	
	(3)要掌握的方法：
	
		A:toString()
	
			返回对象的字符串表示，默认是由类的全路径+'@'+哈希值的十六进制表示。
	
			这个表示其实是没有意义的，一般子类都会重写该方法。
	
			如何重写呢?过程我也讲解过了，基本上就是要求信息简单明了。
	
			但是最终还是自动生成。
	
		B:equals()
	
			比较两个对象是否相同。默认情况下，比较的是地址值是否相同。
	
			而比较地址值是没有意义的，所以，一般子类也会重写该方法。
	
			重写过程，我也详细的讲解和分析了。
	
			但是最终还是自动生成。
	
	(4)要了解的方法：
	
		A:hashCode() 返回对象的哈希值。不是实际地址值，可以理解为地址值。
	
		B:getClass() 返回对象的字节码文件对象，反射中我们会详细讲解。	
	
		C:finalize() 用于垃圾回收，在不确定的时候。
	
		D:clone() 可以实现对象的克隆，包括成员变量的数据复制，但是它和两个引用指向同一个对象是有区别的。
	
	(5)两个注意问题；
	
		A:直接输出一个对象名称，其实默认调用了该对象的toString()方法。
	
		B:面试题 
	
			==和equals()的区别?
	
			A:==
	
				基本类型：比较的是值是否相同
	
				引用类型：比较的是地址值是否相同
	
			B:equals()
	
				只能比较引用类型。默认情况下，比较的是地址值是否相同。
	
				但是，我们可以根据自己的需要重写该方法。

```java
/*
 * Object:类 Object 是类层次结构的根类。每个类都使用 Object 作为超类。
 * 每个类都直接或者间接的继承自Object类。
 * 
 * Object类的方法：
 * 		public int hashCode():返回该对象的哈希码值。
 * 			注意：哈希值是根据哈希算法计算出来的一个值，这个值和地址值有关，但是不是实际地址值。
 * 			           你可以理解为地址值。
 * 
 *		public final Class getClass():返回此 Object 的运行时类
 *			Class类的方法：
 *				public String getName()：以 String 的形式返回此 Class 对象所表示的实体
 */
public class StudentTest {
	public static void main(String[] args) {
		Student s1 = new Student();
		System.out.println(s1.hashCode()); // 11299397
		Student s2 = new Student();
		System.out.println(s2.hashCode());// 24446859
		Student s3 = s1;
		System.out.println(s3.hashCode()); // 11299397
		System.out.println("-----------");

		Student s = new Student();
		Class c = s.getClass();
		String str = c.getName();
		System.out.println(str); // cn.companyName_01.Student
		
		//链式编程
		String str2  = s.getClass().getName();
		System.out.println(str2);
	}
}
```

```java
/*
 * public String toString():返回该对象的字符串表示。
 * 
 * Integer类下的一个静态方法：
 * 		public static String toHexString(int i)：把一个整数转成一个十六进制表示的字符串
 * 
 * 这个信息的组成我们讲解完毕了，但是这个信息是没有任何意义的。所以，建议所有子类都重写该方法。
 * 怎么重写呢?
 * 		把该类的所有成员变量值组成返回即可。
 * 重写的最终版方案就是自动生成toString()方法。
 * 
 * 注意：
 * 	 直接输出一个对象的名称，其实就是调用该对象的toString()方法。
 */
public class StudentDemo {
	public static void main(String[] args) {
		Student s = new Student();
		System.out.println(s.hashCode());
		System.out.println(s.getClass().getName());
		System.out.println("--------------------");
		System.out.println(s.toString());// cn.companyName.Student@42552c
		System.out.println("--------------------");
		// toString()方法的值等价于它
		// getClass().getName() + '@' + Integer.toHexString(hashCode())
		// this.getClass().getName()+'@'+Integer.toHexString(this.hashCode())

		// cn.companyName.Student@42552c
		// cn.companyName.Student@42552c

		System.out.println(s.getClass().getName() + '@'
				+ Integer.toHexString(s.hashCode()));

		System.out.println(s.toString());

		// 直接输出对象的名称
		System.out.println(s);
	}
}
```

```java
/*
 * public boolean equals(Object obj):指示其他某个对象是否与此对象“相等”。 
 * 这个方法，默认情况下比较的是地址值。比较地址值一般来说意义不大，所以我们要重写该方法。
 * 怎么重写呢?
 * 		一般都是用来比较对象的成员变量值是否相同。
 *      重写的代码优化：提高效率，提高程序的健壮性。
 * 最终版：
 * 		其实还是自动生成。
 * 
 * 看源码：
 * 		public boolean equals(Object obj) {
 * 			//this - s1
 * 			//obj - s2
 *       	return (this == obj);
 *   	}
 * 
 * ==:
 * 		基本类型：比较的就是值是否相同
 * 		引用类型：比较的就是地址值是否相同
 * equals:
 * 		引用类型：默认情况下，比较的是地址值。
 * 		不过，我们可以根据情况自己重写该方法。一般重写都是自动生成，比较对象的成员变量值是否相同
 */
public class StudentDemo {
	public static void main(String[] args) {
		Student s1 = new Student("王菲", 27);
		Student s2 = new Student("王菲", 27);
		System.out.println(s1 == s2); // false
		Student s3 = s1;
		System.out.println(s1 == s3);// true
		System.out.println("---------------");

		System.out.println(s1.equals(s2)); // obj = s2; //false
		System.out.println(s1.equals(s1)); // true
		System.out.println(s1.equals(s3)); // true
		Student s4 = new Student("王力宏",30);
		System.out.println(s1.equals(s4)); //false
		
		Demo d = new Demo();
		System.out.println(s1.equals(d)); //ClassCastException

	}
}

class Demo {}
```

```java
/*
 *	protected void finalize()：当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法。用于垃圾回收，但是什么时候回收不确定。
 *	protected Object clone():创建并返回此对象的一个副本。
 *		A:重写该方法
 *
 *  Cloneable:此类实现了 Cloneable 接口，以指示 Object.clone() 方法可以合法地对该类实例进行按字段复制。 
 *  	这个接口是标记接口，告诉我们实现该接口的类就可以实现对象的复制了。
 */
public class StudentDemo {
	public static void main(String[] args) throws CloneNotSupportedException {
		//创建学生对象
		Student s = new Student();
		s.setName("王菲");
		s.setAge(27);
		
		//克隆学生对象
		Object obj = s.clone();
		Student s2 = (Student)obj;
		System.out.println("---------");
		
		System.out.println(s.getName()+"---"+s.getAge());
		System.out.println(s2.getName()+"---"+s2.getAge());
		
		//以前的做法
		Student s3 = s;
		System.out.println(s3.getName()+"---"+s3.getAge());
		System.out.println("---------");
		
		//其实是有区别的
		s3.setName("鹿晗");
		s3.setAge(30);
		System.out.println(s.getName()+"---"+s.getAge());
		System.out.println(s2.getName()+"---"+s2.getAge());
		System.out.println(s3.getName()+"---"+s3.getAge());
		
	}
}
```