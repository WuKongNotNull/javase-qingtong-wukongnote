# **3-继承**

### **3.1继承定义**

    (1)把多个类中相同的成员给提取出来定义到一个独立的类中。然后让这多个类和该独立的类产生一个关系，
    
       这多个类就具备了这些内容。这个关系叫继承。
    
    (2)Java中如何表示继承呢?格式是什么呢?
    
    	A:用关键字extends表示
    
    	B:格式：
    
    		class 子类名 extends 父类名 {}
    
    (3)继承的好处：
    
    	A:提高了代码的复用性
    
    	B:提高了代码的维护性
    
    	C:让类与类产生了一个关系，是多态的前提
    
    (4)继承的弊端：
    
    	A:让类的耦合性增强。这样某个类的改变，就会影响其他和该类相关的类。
    
    		原则：低耦合，高内聚。
    
    		耦合：类与类的关系
    
    		内聚：自己完成某件事情的能力
    
    	B:打破了封装性
    
    (5)Java中继承的特点
    
    	A:Java中类只支持单继承
    
    	B:Java中可以多层(重)继承(继承体系)
    
    (6)继承的注意事项：
    
    	A:子类不能继承父类的私有成员
    
    	B:子类不能继承父类的构造方法，但是可以通过super去访问
    
    	C:不要为了部分功能而去继承
    
    (7)什么时候使用继承呢?
    
    	A:继承体现的是：is a的关系。
    
    	B:采用假设法
    
    (8)Java继承中的成员关系
    
    	A:成员变量
    
    		a:子类的成员变量名称和父类中的成员变量名称不一样，这个太简单
    
    		b:子类的成员变量名称和父类中的成员变量名称一样，这个怎么访问呢?
    
    			子类的方法访问变量的查找顺序：
    
    				在子类方法的局部范围找，有就使用。
    
    				在子类的成员范围找，有就使用。
    
    				在父类的成员范围找，有就使用。
    
    				找不到，就报错。
    
    	B:构造方法
    
    		a:子类的构造方法默认会去访问父类的无参构造方法
    
    			是为了子类访问父类数据的初始化
    
    		b:父类中如果没有无参构造方法，怎么办?
    
    			子类通过super去明确调用带参构造
    
    			子类通过this调用本身的其他构造，但是一定会有一个去访问了父类的构造
    
    			让父类提供无参构造
    
    	C:成员方法
    
    		a:子类的成员方法和父类中的成员方法名称不一样，这个太简单
    
    		b:子类的成员方法和父类中的成员方法名称一样，这个怎么访问呢?
    
    			通过子类对象访问一个方法的查找顺序：
    
    				在子类中找，有就使用
    
    				在父类中找，有就使用
    
    				找不到，就报错

**继承总结：**

1. 子类继承父类，继承了父类的所有属性和方法(包含私有的)，私有的不能直接访问；

2. 一个类如果没有使用extends，那么它将继承Object类，Object类是所有类的父类，始祖类；

3. 一个类可以继承多个类，但java中规定一个类只能直接继承一个类；可以间接继承；

4. 子类具有扩展的功能，扩展子类特有的属性和方法；

5. 继承大大提供了代码的重复利用性；

继承是Java中实现代码重用的重要手段之一。Java中只支持单根继承，即一个类只能有一个直接父类。将重复的代码抽取到父类。子类和父类是is-a的关系，比如猫是一个宠物。小狗是一个宠物。

```java
/**
 * 宠物类，小狗和小猫的父类
 */
public class Pet {
	private String name = "待定";// 昵称
	private int health = 100;// 健康值
	private int love = 20;// 亲密度
	
	/**
	 * 无参构造方法
	 */
	public Pet() {
	}
	/**
	 * 有参构造方法
	 * @param name  昵称
	 */
	public Pet(String name) {
		this.name = name;
	}
	
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getHealth() {
		return health;
	}
	public void setHealth(int health) {
		if(health<0||health>100){
			System.out.println("健康值应该在0至100之间，默认值为60。");
			this.health=60;
            return;
		}
		this.health = health;
	}

	public int getLove() {
		return love;
	}

	public void setLove(int love) {
		if(love<0||love>100){
			System.out.println("亲密度应该在0至100之间，默认值为10。");
			this.love=10;
			return;
		}
		this.love = love;
	}

	/**
	 * 输出宠物信息
	 */
	public void print() {
		System.out.println("宠物的自我介绍：\n我的名字叫" + 
				this.name + "，我的健康值是" + this.health 
				+ "，我和主人的亲密程度是" + this.love + "。");
	}
}
```



```java
/**
 * 狗狗类，宠物的子类
 */
public class Dog extends Pet {
	private String strain="泰迪";// 品种

	//无参构造方法
	public Dog() {
	}
	
	public String getStrain() {
		return strain;
	}

	public void setStrain(String strain) {
		this.strain = strain;
	}
	
}

```

```java
/**
 * 小猫类，宠物的子类
 */
public class Cat extends Pet {
	private String sex="公猫";// 性别

	//无参构造方法
	public Cat() {
	}
	
	public String getSex() {
		return sex;
	}
	public void setSex(String sex) {
		this.sex = sex;
	}	
}
```

```java
//测试类
public class Test {
	public static void main(String[] args) {
		// 1、创建宠物对象pet并输出信息
		Pet pet = new Pet("欢喜宠物");
		pet.print();
		// 2、创建狗狗对象dog并输出信息
		Dog dog = new Dog();
		dog.setName("旺财狗");
		dog.setHealth(90);
		dog.setLove(80);
		dog.setStrain("土狗");
		dog.print();
		// 3、创建小猫对象cat并输出信息
		Cat cat = new Cat();
		cat.setName("大黑猫");
		cat.setHealth(98);
		cat.setLove(99);
		cat.setSex("母猫");
		cat.print();
	}
}
```

### **3.2super关键字**

（1）使用super关键字,super代表父类对象 。
（2）super只能出现在子类的方法和构造方法中。
（3）在子类构造方法中调用且必须是第一句。
（4）不可以访问父类中定义为private的属性和方法。

#### **子类访问父类构造器**

```java
/**
 * 狗狗类，宠物的子类
 */
/*继承中构造方法的关系
		A:子类中所有的构造方法默认都会访问父类中空参数的构造方法
		B:为什么呢?
			因为子类会继承父类中的数据，可能还会使用父类的数据。
			所以，子类初始化之前，一定要先完成父类数据的初始化。
			
			注意：子类每一个构造方法的第一条语句默认都是：super();*/

public class Dog extends Pet {
	private String strain="泰迪";// 品种

	//无参构造方法
	public Dog() {
	}
    //通过supper，子类访问父类构造器
    public Dog(String name,String strain){
        supper(name); //supper的使用
        this.strain=strain;
    }
	public String getStrain() {
		return strain;
	}
	public void setStrain(String strain) {
		this.strain = strain;
	}
}

/*----------------------------------分割线--------------------------------------------*/
public class Test {
	public static void main(String[] args) {
		Dog dog=new Dog("傻蛋狗","哈士奇");
		dog.print();
	}
}

```

```java
/*
	如果父类没有无参构造方法，那么子类的构造方法会出现什么现象呢?
		报错。
	如何解决呢?	
		A:在父类中加一个无参构造方法
		B:通过使用super关键字去显示的调用父类的带参构造方法
		C:子类通过this去调用本类的其他构造方法
			子类中一定要有一个去访问了父类的构造方法，否则父类数据就没有初始化。
			
	注意事项：
		this(...)或者super(...)必须出现在第一条语句上。
		如果不是放在第一条语句上，就可能对父类的数据进行了多次初始化，所以必须放在第一条语句上。
*/
class Father {
	/*
	public Father() {
		System.out.println("Father的无参构造方法");
	}
	*/
	
	public Father(String name) {
		System.out.println("Father的带参构造方法");
	}
}

class Son extends Father {
	public Son() {
		super("随便给");
		System.out.println("Son的无参构造方法");
		//super("随便给");
	}
	
	public Son(String name) {
		//super("随便给");
		this();
		System.out.println("Son的带参构造方法");
	}
}

class ExtendsDemo7 {
	public static void main(String[] args) {
		Son s = new Son();
		System.out.println("----------------");
		Son ss = new Son("吴亦凡");
	}
}
```



#### **子类访问父类属性**

super.成员变量 调用父类的成员变量

```java
class Father {
	public int num = 10;
}

class Son extends Father {
	public int num = 20;
	
	public void show() {
		int num = 30;
		System.out.println(num);
		System.out.println(this.num);
		System.out.println(super.num);
	}
}

class ExtendsDemo5 {
	public static void main(String[] args) {
		Son s = new Son();
		s.show();
	}
}
```



#### **子类访问父类方法**

当子类需要父类的功能，而功能主体子类有自己特有内容时，可以重写父类中的方法。这样，即沿袭了父类的功能，又定义了子类特有的内容。

```java
class Phone {
	public void call(String name) {
		System.out.println("给"+name+"打电话");
	}
}

class NewPhone extends Phone {
	public void call(String name) {
		//System.out.println("给"+name+"打电话");
		super.call(name);
		System.out.println("可以听天气预报了");
	}
}

class ExtendsDemo9 {
	public static void main(String[] args) {
		NewPhone np = new NewPhone();
		np.call("吴亦凡");
	}
}
```

### **3.3final关键字**

	(1)是最终的意思，可以修饰类，方法，变量。
	
	(2)特点：
	
		A:它修饰的类，不能被继承。
	
		B:它修饰的方法，不能被重写。
	
		C:它修饰的变量，是一个常量。
	
	(3)面试相关：
	
		A:局部变量
	
			a:基本类型 值不能发生改变
	
			b:引用类型 地址值不能发生改变，但是对象的内容是可以改变的
	
		B:初始化时机
	
			a:只能初始化一次。
	
			b:常见的给值
	
				定义的时候。(推荐)
	
				构造方法中

```java
/*
	final可以修饰类，方法，变量
	
	特点：
		final可以修饰类，该类不能被继承。
		final可以修饰方法，该方法不能被重写。(覆盖，复写)
		final可以修饰变量，该变量不能被重新赋值。因为这个变量其实常量。
		
	常量：
		A:字面值常量
			"hello",10,true
		B:自定义常量
			final int x = 10;
*/

//final class Fu //无法从最终Fu进行继承

class Fu {
	public int num = 10;
	public final int num2 = 20;

	/*
	public final void show() {
	
	}
	*/
}

class Zi extends Fu {
	// Zi中的show()无法覆盖Fu中的show()
	public void show() {
		num = 100;
		System.out.println(num);
		
		//无法为最终变量num2分配值
		//num2 = 200;
		System.out.println(num2);
	}
}

class FinalDemo {
	public static void main(String[] args) {
		Zi z = new Zi();
		z.show();
	}
}
```

```java
/*
	面试题：final修饰局部变量的问题
		基本类型：基本类型的值不能发生改变。
		引用类型：引用类型的地址值不能发生改变，但是，该对象的堆内存的值是可以改变的。
*/
class Student {
	int age = 10;
}

class FinalTest {
	public static void main(String[] args) {
		//局部变量是基本数据类型
		int x = 10;
		x = 100;
		System.out.println(x);
		final int y = 10;
		//无法为最终变量y分配值
		//y = 100;
		System.out.println(y);
		System.out.println("--------------");
		
		//局部变量是引用数据类型
		Student s = new Student();
		System.out.println(s.age);
		s.age = 100;
		System.out.println(s.age);
		System.out.println("--------------");
		
		final Student ss = new Student();
		System.out.println(ss.age);
		ss.age = 100;
		System.out.println(ss.age);
		
		//重新分配内存空间
		//无法为最终变量ss分配值
		ss = new Student();
	}
}
```

```java
/*
	final修饰变量的初始化时机
		A:被final修饰的变量只能赋值一次。
		B:在构造方法完毕前。(非静态的常量)
*/
class Demo {
	//int num = 10;
	//final int num2 = 20;
	
	int num;
	final int num2;
	
	{
		//num2 = 10;
	}
	
	public Demo() {
		num = 100;
		//无法为最终变量num2分配值
		num2 = 200;
	}
}

class FinalTest2 {
	public static void main(String[] args) {
		Demo d = new Demo();
		System.out.println(d.num);
		System.out.println(d.num2);
	}
}
```

```java
/*
	继承的代码体现
	
	由于继承中方法有一个现象：方法重写。
	所以，父类的功能，就会被子类给覆盖调。
	有些时候，我们不想让子类去覆盖掉父类的功能，只能让他使用。
	这个时候，针对这种情况，Java就提供了一个关键字：final
	
	final:最终的意思。常见的是它可以修饰类，方法，变量。
*/
class Fu {
	public final void show() {
		System.out.println("这里是绝密资源,任何人都不能修改");
	}
}

class Zi extends Fu {
	// Zi中的show()无法覆盖Fu中的show()
	public void show() {
		System.out.println("这是一堆垃圾");
	}
}

class ZiDemo {
	public static void main(String[] args) {
		Zi z = new Zi();
		z.show();
	}
}
```



### **3.4方法重写**

**方法重写**

子类根据需求对从父类继承的方法进行重新编写。
重写时，可以用super.方法的方式来保留父类的方法。
构造方法不能被重写。

方法名相同,参数列表相同,返回值类型相同或是其子类。
访问权限不能严于父类。
父类的静态方法不能被子类覆盖为非静态方法,父类的非静态方法不能被子类覆盖为静态方法
子类可以定义与父类同名的静态方法，以便在子类中隐藏父类的静态方法(注：静态方法中无法使用super)
父类的私有方法不能被子类覆盖
不能抛出比父类方法更多的异常

```java
public class Base {
	public void method1(){
		System.out.println("父类的实例方法");
	}
	public static void method2(){
		System.out.println("父类的静态方法");
	}
	public Base method3(){
		System.out.println("父类返回值类型为base的方法");
		return new Base();
	}
	
	private void method4(){
		System.out.println("父类的私有方法");
	}
}
```

```java
public class Sub extends Base{
	//private void method1(){    //访问权限不能严于父类
	//public static void method1(){    //父类的非静态方法不能被子类覆盖为静态方法
	public void method1(){
		System.out.println("子类的实例方法");
	}
	
	//public void method2(){  //父类的静态方法不能被子类覆盖为非静态方法
	//子类可以定义与父类同名的静态方法，以便在子类中"隐藏"父类的静态方法
	public static void method2(){
		System.out.println("子类的静态方法");
	}
	
	//返回值类型相同或者是其子类
	public Sub method3(){
		System.out.println("子类返回值为Sub的方法");
		return new Sub();
	}
	
	//父类的私有方法不能被子类覆盖,这样写可以，但是是独立的方法
	public void method4(){
		System.out.println("子类的私有方法");
	}
}
```

示例：重写Object的equal（）方法

```java
//学生类
public class Student extends Object {
	//属性
	private int sid;//学员学号
	private String name;//姓名
	private int age;//年龄
	private double weight;//体重
	
	//无参构造方法
	public Student() {
		//使用无参构造方法构造学员时，为其属性赋值
		this.name="无名氏";
		this.age=18;
		this.weight=50;
	}

	//构造方法重载 ：带参构造方法
	public Student(int sid,String name, int age, double weight) {
		this.sid=sid;
		this.name = name;
		this.weight = weight;
		if(age<18 || age>30){
			System.out.println("***输入的年龄为："+age+"，该年龄不合法，将重置!***");
			this.age=18;
		}else{
			this.age = age;
		}
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		if(age<18 || age>30){
			System.out.println("***输入的年龄为："+age+"，该年龄不合法，将重置!***");
			return;

		}
		this.age = age;
	}

	public double getWeight() {
		return weight;
	}

	public void setWeight(double weight) {
		this.weight = weight;
	}

	public int getSid() {
		return sid;
	}

	public void setSid(int sid) {
		this.sid = sid;
	}

	//重写equals()方法，如果学员学号、姓名都相同，证明是同一个学生
	//instanceof操作符用于判断一个引用类型所引用的对象是否是一个类的实例
	public boolean equals(Object o){
		if(this==o){
			return true;
		}
		if(! (o instanceof Student)){
			return false;
		}
		Student obj=(Student)o;
		if(this.sid==obj.sid && this.name.equals(obj.name)){
			return true;
		}else{
			return false;
		}		
	}
	
	public static void main(String[] args) {
		//Student没有重写equals()时,下列两行代码结果：false；重写后，结果：true
		Student s1=new Student(1,"张三",20,55);
		Student s2=new Student(1,"张三",20,55);
		System.out.println(s1.equals(s2));
	}
}

```