# **4-抽象类**

### **抽象类与抽象方法**

(1)把多个共性的东西提取到一个类中，这是继承的做法。

	   但是呢，这多个共性的东西，在有些时候，方法声明一样，但是方法体。
	
	   也就是说，方法声明一样，但是每个具体的对象在具体实现的时候内容不一样。
	
	   所以，我们在定义这些共性的方法的时候，就不能给出具体的方法体。
	
	   而一个没有具体的方法体的方法是抽象的方法。
	
	   在一个类中如果有抽象方法，该类必须定义为抽象类。

(2)抽象类的特点

		A:抽象类和抽象方法必须用关键字abstract修饰
	
		B:抽象类中不一定有抽象方法,但是有抽象方法的类一定是抽象类
	
		C:抽象类不能实例化
	
		D:抽象类的子类
	
			a:是一个抽象类。
	
			b:是一个具体类。这个类必须重写抽象类中的所有抽象方法。

(3)抽象类的成员特点：

		A:成员变量
	
			有变量，有常量
	
		B:构造方法
	
			有构造方法
	
		C:成员方法
	
			有抽象，有非抽象

```java
public abstract class Base {
	//抽象类中可以没有抽象方法,但包含了抽象方法的类就必须被定义为抽象类
	public abstract void method1();
	public abstract void method2();
	public void method3(){}
	//没有抽象的构造方法
	//public abstract Base(){}
	//没有抽象的静态方法
	//static abstract void method4();
	
	public Base(){
		System.out.println("父类的无参构造方法");
	}
	
	static void method4(){
		System.out.print("静态方法表示类所特有的功能，这种功能的实现不依赖于类的具体实例，也不依赖于它的子类。因此，当前类必须为静态方法提供实现");
	}
}
```

```java
//如果子类没有实现父类的所有抽象方法，子类必须被定义为抽象类

public abstract class Sub1 extends Base {

	public void method1() {
		System.out.println("重写父类的method1");
	}
	
}
```

```java
//否则就必须实现父类的所有抽象方法
public class Sub2 extends Base {
	public Sub2(){
		System.out.println("子类的无参构造方法");
	}

	@Override
	public void method1() {
		System.out.println("重写父类的抽象方法method1");
	}

	@Override
	public void method2() {
		System.out.println("重写父类的抽象方法method2");
	}

}

```

```java
public class Test {
	public static void main(String[] args) {
		//抽象类不允许实例化
		//Base base=new Base();
		//抽象类中可以有非抽象的构造方法，创建子类的实例时可能调用
		//抽象类不能被实例化,但可以创建一个引用变量，其类型是一个抽象类，指向非抽象的子类实例
		Base sub=new Sub2();
		sub.method1();
		sub.method4();
	}
}
```
```java

```