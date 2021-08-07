# **2-泛型**

是一种把类型明确的工作推迟到创建对象或者调用方法的时候才去明确的特殊类型。也称为参数化类型，把类型当作参数进行传递。

```java
/*
* 回想一下，我们的数组
 * 		String[] strArray = new String[3];
 * 		strArray[0] = "hello";
 * 		strArray[1] = "world";
 * 		strArray[2] = 10;
 * 集合也模仿着数组的这种做法，在创建对象的时候明确元素的数据类型。这样就不会在有问题了。
 * 而这种技术被称为：泛型。
 * 
 * 泛型：是一种把类型明确的工作推迟到创建对象或者调用方法的时候才去明确的特殊的类型。参数化类型，把类型当作参数一样的传递。
 * 格式：
 * 		<数据类型>
 * 		此处的数据类型只能是引用类型。
 * 好处：
 * 		A:把运行时期的问题提前到了编译期间
 * 		B:避免了强制类型转换
 * 		C:优化了程序设计，解决了黄色警告线
 */
public class GenericDemo {
	public static void main(String[] args) {
		// 创建
		ArrayList<String> array = new ArrayList<String>();

		// 添加元素
		array.add("hello");
		array.add("world");
		array.add("java");
		// array.add(new Integer(100));
		//array.add(10); // JDK5以后的自动装箱
		// 等价于：array.add(Integer.valueOf(10));

		// 遍历
		Iterator<String> it = array.iterator();
		while (it.hasNext()) {
			// ClassCastException
			// String s = (String) it.next();
			String s = it.next();
			System.out.println(s);
		}

		// 看下面这个代码
		// String[] strArray = new String[3];
		// strArray[0] = "hello";
		// strArray[1] = "world";
		// strArray[2] = 10;
	}
}
```

```java
/*
 * 泛型在哪些地方使用呢?
 * 		看API，如果类，接口，抽象类后面跟的有<E>就说要使用泛型。一般来说就是在集合中使用。
 */
public class ArrayListDemo {
	public static void main(String[] args) {
		// 用ArrayList存储字符串元素，并遍历。用泛型改进代码
		ArrayList<String> array = new ArrayList<String>();

		array.add("hello");
		array.add("world");
		array.add("java");

		Iterator<String> it = array.iterator();
		while (it.hasNext()) {
			String s = it.next();
			System.out.println(s);
		}
		System.out.println("-----------------");

		for (int x = 0; x < array.size(); x++) {
			String s = array.get(x);
			System.out.println(s);
		}
	}
}
```

```java
/*
 * 泛型类：把泛型定义在类上
 */
public class ObjectTool<T> {
	private T obj;

	public T getObj() {
		return obj;
	}

	public void setObj(T obj) {
		this.obj = obj;
	}
}


/*
 * 泛型类的测试
 */
public class ObjectToolDemo {
	public static void main(String[] args) {
		// ObjectTool ot = new ObjectTool();
		//
		// ot.setObj(new String("吴亦凡"));
		// String s = (String) ot.getObj();
		// System.out.println("姓名是：" + s);
		//
		// ot.setObj(new Integer(30));
		// Integer i = (Integer) ot.getObj();
		// System.out.println("年龄是：" + i);

		// ot.setObj(new String("鹿晗"));
		// // ClassCastException
		// Integer ii = (Integer) ot.getObj();
		// System.out.println("姓名是：" + ii);

		System.out.println("-------------");

		ObjectTool<String> ot = new ObjectTool<String>();
		// ot.setObj(new Integer(27)); //这个时候编译期间就过不去
		ot.setObj(new String("王力宏"));
		String s = ot.getObj();
		System.out.println("姓名是：" + s);

		ObjectTool<Integer> ot2 = new ObjectTool<Integer>();
		// ot2.setObj(new String("吴彦祖"));//这个时候编译期间就过不去
		ot2.setObj(new Integer(27));
		Integer i = ot2.getObj();
		System.out.println("年龄是：" + i);
	}
}
```

```java
/*
 * 泛型方法：把泛型定义在方法上
 */
public class ObjectTool {
	public <T> void show(T t) {
		System.out.println(t);
	}
}


public class ObjectToolDemo {
	public static void main(String[] args) {
		// ObjectTool ot = new ObjectTool();
		// ot.show("hello");
		// ot.show(100);
		// ot.show(true);

		// ObjectTool<String> ot = new ObjectTool<String>();
		// ot.show("hello");
		//
		// ObjectTool<Integer> ot2 = new ObjectTool<Integer>();
		// ot2.show(100);
		//
		// ObjectTool<Boolean> ot3 = new ObjectTool<Boolean>();
		// ot3.show(true);

		// 定义泛型方法后
		ObjectTool ot = new ObjectTool();
		ot.show("hello");
		ot.show(100);
		ot.show(true);
	}
}
```

```java
/*
 * 泛型接口：把泛型定义在接口上
 */
public interface Inter<T> {
	public abstract void show(T t);
}

public class InterDemo {
	public static void main(String[] args) {
		// 第一种情况的测试
		// Inter<String> i = new InterImpl();
		// i.show("hello");

		// // 第二种情况的测试
		Inter<String> i = new InterImpl<String>();
		i.show("hello");

		Inter<Integer> ii = new InterImpl<Integer>();
		ii.show(100);
	}
}
```

```java
/*
 * 泛型高级(通配符)
 * ?:任意类型，如果没有明确，那么就是Object以及任意的Java类了
 * ? extends E:向下限定，E及其子类
 * ? super E:向上限定，E极其父类
 */
public class GenericDemo {
	public static void main(String[] args) {
		// 泛型如果明确的写的时候，前后必须一致
		Collection<Object> c1 = new ArrayList<Object>();
		// Collection<Object> c2 = new ArrayList<Animal>();
		// Collection<Object> c3 = new ArrayList<Dog>();
		// Collection<Object> c4 = new ArrayList<Cat>();

		// ?表示任意的类型都是可以的
		Collection<?> c5 = new ArrayList<Object>();
		Collection<?> c6 = new ArrayList<Animal>();
		Collection<?> c7 = new ArrayList<Dog>();
		Collection<?> c8 = new ArrayList<Cat>();

		// ? extends E:向下限定，E及其子类
		// Collection<? extends Animal> c9 = new ArrayList<Object>();
		Collection<? extends Animal> c10 = new ArrayList<Animal>();
		Collection<? extends Animal> c11 = new ArrayList<Dog>();
		Collection<? extends Animal> c12 = new ArrayList<Cat>();

		// ? super E:向上限定，E极其父类
		Collection<? super Animal> c13 = new ArrayList<Object>();
		Collection<? super Animal> c14 = new ArrayList<Animal>();
		// Collection<? super Animal> c15 = new ArrayList<Dog>();
		// Collection<? super Animal> c16 = new ArrayList<Cat>();
	}
}

class Animal {
}

class Dog extends Animal {
}

class Cat extends Animal {
}
```



**demo**

```java
/**
* 宠物类，狗狗父类。
*/
public abstract class Pet {
	protected String name = "无名氏";// 昵称
	protected int health = 100;// 健康值
	protected int love = 0;// 亲密度
	
	public abstract void eat();  //抽象方法eat(),负责宠物吃饭功能。
	
	/**
	 * 无参构造方法。
	 */
	public Pet() {	
	}
	/**
	 * 有参构造方法。
	 * @param name  昵称
	 */
	public Pet(String name) {
		this.name = name;
	}
	public String getName() {
		return name;
	}
	public int getHealth() {
		return health;
	}
	public int getLove() {
		return love;
	}
	/**
	 * 输出宠物信息。
	 */
	public void print() {
		System.out.println("宠物的自我介绍：\n我的名字叫" + this.name + 
				"，健康值是"	+ this.health + "，和主人的亲密度是"
				+ this.love + "。");
	}
}
```

```java
/**
 * 狗狗类，宠物的子类。
 */
public class Dog extends Pet {
	private String strain;// 品种
	/**
	 * 有参构造方法。
	 * @param name   昵称
	 * @param strain   品种
	 */
	public Dog(String name, String strain) {
		super(name); 
		this.strain = strain;
	}
	public String getStrain() {
		return strain;
	}
	/**
	 * 重写父类的print方法。
	 */
	public void print(){
		super.print(); //调用父类的print方法
		System.out.println("我是一只 " + this.strain + "。");
	}
	/**
	 * 实现吃饭方法。 
	 */
	public void eat() {
		super.health = super.health + 3;
		System.out.println("狗狗"+super.name + "吃饱啦！健康值增加3。");
	}
}
```

```java
import java.util.ArrayList;
import java.util.List;
/**
 * 对List应用使用泛型。
 */
public class TestList {
    public static void main(String[] args) {
        /* 1、创建多个狗狗对象*/
        Dog ououDog = new Dog("偶偶", "哈士奇");
		Dog yayaDog = new Dog("丫丫", "藏獒");
		Dog meimeiDog = new Dog("美眉", "金毛");
		Dog feifeiDog = new Dog("菲菲", "贵宾犬");     
        /* 2、创建ArrayList集合对象并把多个狗狗对象放入其中*/
        List<Dog> dogs = new ArrayList<Dog>();//标记元素类型
        dogs.add(ououDog);
	    dogs.add(yayaDog);
	    dogs.add(meimeiDog);
	    dogs.add(2, feifeiDog); // 添加feifeiDog到指定位置
	    //dogs.add("hello"); //出现编译错误,元素类型不是Dog。
        /* 3、 显示第三个元素的信息*/
        Dog dog3 = dogs.get(2); //无需类型强制转换
        System.out.println("第三个狗狗的信息如下:");
        System.out.println(dog3.getName() + "\t" + dog3.getStrain());
        /*4、使用foreach语句遍历dogs对象*/
		System.out.println("\n所有狗狗的信息如下：");
		for(Dog dog:dogs){//无需类型强制转换
			System.out.println(dog.getName() + "\t" + dog.getStrain());
		}   
    }
}
```

```java
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;
/**
 * 对Map应用使用泛型。
 */
public class TestMap {
	public static void main(String[] args) {
		/* 1、创建多个狗狗对象*/
		Dog ououDog = new Dog("偶偶", "哈士奇");
		Dog yayaDog = new Dog("丫丫", "金毛");
		Dog meimeiDog = new Dog("美眉", "藏獒");
		Dog feifeiDog = new Dog("菲菲", "贵宾犬");
		/* 2、创建Map集合对象并把多个狗狗对象放入其中*/
		Map<String,Dog> dogMap=new HashMap<String,Dog>();
		dogMap.put(ououDog.getName(),ououDog);
		dogMap.put(yayaDog.getName(),yayaDog);
		dogMap.put(meimeiDog.getName(),meimeiDog);
		dogMap.put(feifeiDog.getName(),feifeiDog);
		/*3、通过迭代器依次输出集合中所有狗狗的信息*/
		System.out.println("使用Iterator遍历，所有狗狗的昵称和品种分别是：");
		Set<String> keys=dogMap.keySet();//取出所有key的集合
		Iterator<String> it=keys.iterator();//获取Iterator对象
		while(it.hasNext()){
			String key=it.next();  //取出key
			Dog dog=dogMap.get(key);  //根据key取出对应的值
			System.out.println(key+"\t"+dog.getStrain());
		}
		/*//使用foreach语句输出集合中所有狗狗的信息
		 for(String key:keys){
			Dog dog=dogMap.get(key);  //根据key取出对应的值
			System.out.println(key+"\t"+dog.getStrain());	
		}*/		
	}
}
```





