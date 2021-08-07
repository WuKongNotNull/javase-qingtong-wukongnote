# **5-内部类**

```java
/*
	内部类概述:
		把类定义在其他类的内部，这个类就被称为内部类。
		举例：在类A中定义了一个类B，类B就是内部类。
	
	内部的访问特点：
		A:内部类可以直接访问外部类的成员，包括私有。
		B:外部类要访问内部类的成员，必须创建对象。
	
*/
class Outer {
	private int num = 10;
	
	class Inner {
		public void show() {
			System.out.println(num);
		}
	}
	
	public void method() {
		//找不到符号
		//show();
	
		Inner i = new Inner();
		i.show();
	}
	
}

class InnerClassDemo {
	public static void main(String[] args) {
	
	}
}
```

```java
/*
	内部类位置
		成员位置:在成员位置定义的类，被称为成员内部类。	
		局部位置:在局部位置定义的类，被称为局部内部类。
		
		
	成员位置:在成员位置定义的类，被称为成员内部类。	
		
*/
class Outer {
	private int num = 10;

	//成员位置
	/*
	class Inner {
		
	}
	*/
	

	public void method() {
		//局部位置
		class Inner {
		
		}
	}
}

class InnerClassDemo2 {
	public static void main(String[] args) {
		
	}
}
```

```java
/*
	成员内部类:
		如何直接访问内部类的成员。
		外部类名.内部类名 对象名 = 外部类对象.内部类对象;
*/
class Outer {
	private int num = 10;
	
	class Inner {
		public void show() {
			System.out.println(num);
		}
	}
}

class InnerClassDemo3 {
	public static void main(String[] args) {
		//需求：我要访问Inner类的show()方法
		//Inner i = new Inner();
		//i.show();
		
		//格式：外部类名.内部类名 对象名 = 外部类对象.内部类对象;
		Outer.Inner oi = new Outer().new Inner();
		oi.show();
	}
}
```

```java
/*
	成员内部类的修饰符：
		private 为了保证数据的安全性
		static 为了方便访问数据
			注意：静态内部类访问的外部类数据必须用静态修饰。
	
	案例：我有一个人(人有身体，身体内有心脏。)
		
		class Body {
			private class Heart {
				public void operator() {
					System.out.println("心脏搭桥");
				}
			}
			
			public void method() {
				if(如果你是外科医生) {
					Heart h = new Heart();
					h.operator();
				}
			}
		}
		
		按照我们刚才的讲解，来使用一下
		Body.Heart bh = new Body().new Heart();
		bh.operator();
		//加了private后，就不能被访问了，那么，怎么玩呢?
		Body b =  new Body();
		b.method();
*/
class Outer {
	private int num = 10;
	private static int num2 = 100;
	
	//内部类用静态修饰是因为内部类可以看出是外部类的成员
	public static class Inner {
		public void show() {
			//System.out.println(num);
			System.out.println(num2);
		}

		public static void show2() {
			//System.out.println(num);
			System.out.println(num2);
		}		
	}
}

class InnerClassDemo4 {
	public static void main(String[] args) {
		//使用内部类
		// 限定的新静态类
		//Outer.Inner oi = new Outer().new Inner();
		//oi.show();
		//oi.show2();
		
		//成员内部类被静态修饰后的访问方式是:
		//格式：外部类名.内部类名 对象名 = new 外部类名.内部类名();
		Outer.Inner oi = new Outer.Inner();
		oi.show();
		oi.show2();
		
		//show2()的另一种调用方式
		Outer.Inner.show2();
	}
}
```

```java
/*
	局部内部类
		A:可以直接访问外部类的成员
		B:在局部位置，可以创建内部类对象，通过对象调用内部类方法，来使用局部内部类功能
	
	面试题：
		局部内部类访问局部变量的注意事项?
		A:局部内部类访问局部变量必须用final修饰
		B:为什么呢?
			局部变量是随着方法的调用而调用，随着调用完毕而消失。
			而堆内存的内容并不会立即消失。所以，我们加final修饰。
			加入final修饰后，这个变量就成了常量。既然是常量。你消失了。
			我在内存中存储的是数据20，所以，我还是有数据在使用。
*/
class Outer {
	private int num  = 10;
	
	public void method() {
		//int num2 = 20;
		//final int num2 = 20;
		class Inner {
			public void show() {
				System.out.println(num);
				//从内部类中访问本地变量num2; 需要被声明为最终类型
				System.out.println(num2);//20
			}
		}
		
		//System.out.println(num2);
		
		Inner i = new Inner();
		i.show();
	}
}

class InnerClassDemo5 {
	public static void main(String[] args) {
		Outer o = new Outer();
		o.method();
	}
}
```

```java
/*
	匿名内部类
		就是内部类的简化写法。

	前提：存在一个类或者接口
		这里的类可以是具体类也可以是抽象类。
	
	格式：
		new 类名或者接口名(){
			重写方法;
		}
		
	本质是什么呢?
		是一个继承了该类或者实现了该接口的子类匿名对象。
*/
interface Inter {
	public abstract void show();
	public abstract void show2();
}

class Outer {
	public void method() {
		//一个方法的时候
		/*
		new Inter() {
			public void show() {
				System.out.println("show");
			}
		}.show();
		*/
		
		//二个方法的时候
		/*
		new Inter() {
			public void show() {
				System.out.println("show");
			}
			
			public void show2() {
				System.out.println("show2");
			}
		}.show();
		
		new Inter() {
			public void show() {
				System.out.println("show");
			}
			
			public void show2() {
				System.out.println("show2");
			}
		}.show2();
		*/
		
		//如果我是很多个方法，就很麻烦了
		//那么，我们有没有改进的方案呢?
		Inter i = new Inter() { //多态
			public void show() {
				System.out.println("show");
			}
			
			public void show2() {
				System.out.println("show2");
			}
		};
		
		i.show();
		i.show2();
	}
}

class InnerClassDemo6 {
	public static void main(String[] args) {
		Outer o = new Outer();
		o.method();
	}
}
```

**练习**

```java
/*
	面试题：
		要求请填空分别输出30，20，10。
		
	注意：
		1:内部类和外部类没有继承关系。
		2:通过外部类名限定this对象
			Outer.this
*/
class Outer {
	public int num = 10;
	class Inner {
		public int num = 20;
		public void show() {
			int num = 30;
			System.out.println(num);
			System.out.println(this.num);
			//System.out.println(new Outer().num);
			System.out.println(Outer.this.num);
		}
	}
}
class InnerClassTest {
	public static void main(String[] args) {
		Outer.Inner oi = new Outer().new Inner();
		oi.show();
	}	
}
```

```java
/*
	匿名内部类在开发中的使用
*/
interface Person {
	public abstract void study();
}

class PersonDemo {
	//接口名作为形式参数
	//其实这里需要的不是接口，而是该接口的实现类的对象
	public void method(Person p) {
		p.study();
	}
}

//实现类
class Student implements Person {
	public void study() {
		System.out.println("好好学习,天天向上");
	}
}

class InnerClassTest2 {
	public static void main(String[] args) {
		//测试
		PersonDemo pd = new PersonDemo();
		Person p = new Student();
		pd.method(p);
		System.out.println("--------------------");
		
		//匿名内部类在开发中的使用
		//匿名内部类的本质是继承类或者实现了接口的子类匿名对象
		pd.method(new Person(){
			public void study() {
				System.out.println("好好学习,天天向上");
			}
		});
	}
}
```

```java
/*
	匿名内部类面试题：
		按照要求，补齐代码
			interface Inter { void show(); }
			class Outer { //补齐代码 }
			class OuterDemo {
				public static void main(String[] args) {
					  Outer.method().show();
				  }
			}
			要求在控制台输出”HelloWorld”
*/
interface Inter { 
	void show(); 
	//public abstract
}

class Outer { 
	//补齐代码
	public static Inter method() {
		//子类对象 -- 子类匿名对象
		return new Inter() {
			public void show() {
				System.out.println("HelloWorld");
			}
		};
	}
}

class OuterDemo {
	public static void main(String[] args) {
		Outer.method().show();
		/*
			1:Outer.method()可以看出method()应该是Outer中的一个静态方法。
			2:Outer.method().show()可以看出method()方法的返回值是一个对象。
				又由于接口Inter中有一个show()方法,所以我认为method()方法的返回值类型是一个接口。
		*/
	}
}
```