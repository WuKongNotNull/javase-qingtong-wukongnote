## **6-多态**

	(1)同一个对象在不同时刻体现出来的不同状态。
	
	(2)多态的前提：
	
		A:有继承或者实现关系。
	
		B:有方法重写。
	
		C:有父类或者父接口引用指向子类对象。
	
		多态的分类：
	
			a:具体类多态
	
				class Fu {}
	
				class Zi extends Fu {}
	
				Fu f = new Zi();
	
			b:抽象类多态
	
				abstract class Fu {}
	
				class Zi extends Fu {}
	
				Fu f = new Zi();
	
			c:接口多态
	
				interface Fu {}
	
				class Zi implements Fu {}
	
				Fu f = new Zi();
	
	(3)多态中的成员访问特点
	
		A:成员变量
	
			编译看左边，运行看左边
	
		B:构造方法
	
			子类的构造都会默认访问父类构造
	
		C:成员方法
	
			编译看左边，运行看右边
	
		D:静态方法
	
			编译看左边，运行看左边
	
	(4)多态的好处：
	
		A:提高代码的维护性(继承体现)
	
		B:提高代码的扩展性(多态体现)
	
	(5)多态的弊端：
	
		父不能使用子的特有功能
	
		现象：
	
			子可以当作父使用，父不能当作子使用。
	
	(6)多态中的转型
	
		A:向上转型
	
			从子到父
	
		B:向下转型
	
			从父到子

#### **向上转型和向下转型**

```java
/**
 * 宠物类，狗狗和猫猫的父类
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
	//去医院
	public void toHospital() {
    }
	
}
```

```java
/**
 * 狗狗类，宠物的子类。
 */
public class Dog extends Pet {
	private String strain="泰迪";// 品种
	
	public Dog(){}

	/**
	 * 有参构造方法。
	 * @param name   昵称
	 * @param strain   品种
	 */
	public Dog(String name, String strain) {
		super(name); 
		this.strain = strain;
	}
	
	public void setStrain(String strain) {
		this.strain = strain;
	}
	public String getStrain() {
		return strain;
	}
	
	//对父类方法进行重写，同时使用supper继承父类方法
	public void print(){
		super.print();
		System.out.println("我是一只"+this.getStrain()+"犬。");
	}
	
	//对父类方法进行重写
	public void toHospital() {
        System.out.println("给狗狗打针、吃药");
        this.setHealth(60);
    }
    
    //狗狗跳墙(狗狗具有的独特方法)
	public void jumpWall(){
		System.out.println("狗急跳墙");
	}

}
```

```java
/**
 * 小猫类，宠物的子类。
 */
public class Cat extends Pet {
	private String sex;// 性别
	/**
	 * 有参构造方法。
	 * @param name 昵称
	 * @param sex 性别
	 */
	public Cat(String name, String sex) {
		super(name);
		this.sex = sex;
	}
	public String getSex() {
		return sex;
	}
	public void setSex(String sex) {
		this.sex = sex;
	}	
	
	//对父类方法进行重写，同时使用supper继承父类的方法
	public void print(){
		super.print();
		System.out.println("我的性别是"+this.getSex()+"。");
	}
	
	//对父类方法进行重写
    public void toHospital() {
        System.out.println("给猫猫吃药、疗养");
        this.setHealth(60);
    }

}
```

```java
public class Master {
	// 给宠物看病,入参为父类
    public void cure(Pet pet) {
        if (pet.getHealth() < 50)
            pet.toHospital();        
    }
}
```

```java
public class Test {
	public static void main(String[] args) {
        //向上转型 Dog 转成 Pet(使用多态思想)
		Pet dogP=new Dog();
		dogP.setHealth(10);
		dogP.print();
		System.out.println("*************************");
		Master master=new Master();
		master.cure(dogP);
		dogP.print();
        
        //向下转型,调用狗狗独有的方法（使用多态思想）
        if(dogP instanceof Dog){
        Dog dog=(Dog)dogP;
        dog.jumpWall();
        }
	}
}
```

#### **工厂模式：父类作为方法的返回值**

````java
//动物类
public abstract class Animal{
	//叫
	public abstract void cry();
}

````

````java
//Dog子类继承Animal父类
public class Dog extends Animal{
	public void cry() {
		System.out.println("狗狗叫");
	}
}
//Cat子类继承Animal父类
public class Cat extends Animal{
	public void cry() {
		System.out.println("猫猫叫");
	}
}
//Duck子类继承Animal父类
public class Duck extends Animal{
	public void cry() {
		System.out.println("鸭鸭叫");
	}
}
````

```java
//主人类(工厂类)
public class Host {
	public void letCry(Animal animal) {
	    animal.cry();//调用动物叫的方法
	}
	//赠送动物，该方法返回父类类型
	public Animal donateAnimal(String type) {
	    Animal animal;
	    if(type.equals("dog")){
	    	//向上转型
	      animal=new Dog();
	    }
	    else if(type.equals("cat")){
	      animal=new Cat();
	    }
	    else{
	      animal=new Duck();
	    }
	    return animal;
	}
}
```

```java
public class Test {
	public static void main(String[] args) {
	    Host host=new Host();
	    Animal animal;
	    animal=host.donateAnimal("dog");
	    animal.cry();//狗叫
	    animal=host.donateAnimal("cat");
	    animal.cry();//猫叫
	}
}
```

### **设计模式之工厂模式**

1） 简单工厂模式

step-1：请用面向对象的思想实现计算机控制程序，要求输入两个数和运算符号得到结果

```java
public** **class** TestDemo {

 

	**public** **static** **void** main(String[] args) {

 

		Scanner sc = **new** Scanner(System.**in**);

		System.**out**.println("请输入数字A:");

		**int** a = sc.nextInt();

		System.**out**.println("请输入运算符号:");

		sc = **new** Scanner(System.**in**);

		String c = sc.nextLine();

		System.**out**.println("请输入数字B:");

		sc = **new** Scanner(System.**in**);

		**int** b = sc.nextInt();

		**int** result = 0;

		**switch** (c) {

		**case** "+":

			result = a + b;

			**break**;

		**case** "-":

			result = a - b;

			**break**;

		**case** "*":

			result = a * b;

			**break**;

		**case** "/":

			result = a / b;

			**break**;

		**default**:

			System.**out**.println(a+c+b+":运算符异常...");

			**return**;

		}

		System.**out**.println(a+c+b+"="+result);

	}

 

}
```

	step-2：使用面向对象的思想进行编程，上面的写法只能说是实现了功能，如果现在有一个层序需要调用这个计算呢？那么上面这种写法就不能满足。根据这个思路我们需要将上面的计算设计成一个类，提供对外访问的接口。让业务逻辑和界面逻辑分开，降低它们之间的耦合度，分开后才能达到容易维护和扩展。

**public** **class** Operation {

 

	**public** **static** **int** getResult(**int** numberA, **int** numberB, String operate) {
	
		**int** result = 0;
	
		**switch** (operate) {
	
		**case** "+":
	
			result = numberA + numberB;
	
			**break**;
	
		**case** "-":
	
			result = numberA - numberB;
	
			**break**;
	
		**case** "*":
	
			result = numberA * numberB;
	
			**break**;
	
		**case** "/":
	
			result = numberA / numberB;
	
			**break**;
	
		**default**:
	
			System.**out**.println("运算符异常...");
	
		}
	
		**return** result;
	
	}

 


}

 

**public** **class** Test {

 

	**public** **static** **void** main(String[] args) {
	
		Scanner sc = **new** Scanner(System.**in**);
	
		System.**out**.println("请输入数字A:");
	
		**int** numberA = sc.nextInt();
	
		System.**out**.println("请输入运算符号:");
	
		sc = **new** Scanner(System.**in**);
	
		String operate = sc.nextLine();
	
		System.**out**.println("请输入数字B:");
	
		sc = **new** Scanner(System.**in**);
	
		**int** numberB = sc.nextInt();

 


		System.**out**.println(Operation.*getResult*(numberA, numberB, operate));

 


	}

 


}

 

	step-3：上面的写法用到了面向对象的封装，将业务逻辑和界面逻辑分开了，业务进一步增加现在需要一个平方根的运算，再进一步需要将+、-、*、/参与到运算中，这样的业务就有了一定的复杂度，那么如果在操作的过程中不小心输错了一个符号，将会导致运行结果错误。下面就要用到所学的继承和多态。

![img](../imageswps3977.tmp.jpg) 

加减乘除无论如何计算，至少都需要的是两个数，可以设计为类的属性，而不同的知识它的操作，最终都需要返回一个结果，中间的操作过程不同。实现的代码如下

**public** **abstract** **class** Operation {

 

	**private** **double** numberA;
	
	**private** **double** numberB;

 


	**public** **abstract** **double** getResult();

 


	**public** **void** setNumberA(**double** numberA) {
	
		**this**.numberA = numberA;
	
	}

 


	**public** **void** setNumberB(**double** numberB) {
	
		**this**.numberB = numberB;
	
	}

 


	**public** **double** getNumberA() {
	
		**return** numberA;
	
	}

 


	**public** **double** getNumberB() {
	
		**return** numberB;
	
	}

 


}

 

**public** **class** Add **extends** Operation {

 

	@Override
	
	**public** **double** getResult() {
	
		**return** getNumberA() + getNumberB();
	
	}

 


}

 

**public** **class** Divide **extends** Operation {

 

	@Override
	
	**public** **double** getResult() {
	
		**return** getNumberA() / getNumberB();
	
	}

 


}

 

**public** **class** Minus **extends** Operation {

 

	@Override
	
	**public** **double** getResult() {
	
		**return** getNumberA() - getNumberB();
	
	}

 


}

 

**public** **class** Multiply **extends** Operation {

 

	@Override
	
	**public** **double** getResult() {
	
		**return** getNumberA() * getNumberB();
	
	}

 


}

 

**public** **class** OperationFactory {

 

	**public** **static** Operation createOperation(String operate) {
	
		Operation op = **null**;
	
		**switch** (operate) {
	
		**case** "+":
	
			op = **new** Add();
	
			**break**;
	
		**case** "-":
	
			op = **new** Minus();
	
			**break**;
	
		**case** "*":
	
			op = **new** Multiply();
	
			**break**;
	
		**case** "/":
	
			op = **new** Divide();
	
			**break**;
	
		}
	
		**return** op;
	
	}

 


}

 

**public** **class** Test {

​	

	**public** **static** **void** main(String[] args) {
	
		Operation o = OperationFactory.*createOperation*("+");
	
		o.setNumberA(9.8);
	
		o.setNumberB(5.6);
	
		System.**out**.println(o.getResult());
	
	}

 


}

 

分析：利用了简单工厂模式实现了解耦，如果现在需要一个平方根的运算，只需要继承Operation这个类重写它的方法就可以了，而不需要去改动加减乘除的类。利于代码的维护和扩展。通过OperationFactory工厂根据运算的需要去创建对象。

2） 工厂方法模式

对于简单工厂模式我们已经实现了初步的解耦，加强了程序的可维护性和可扩展性，在运算类上已经做到了这一点，如果需要增加一个平方根的运算，我们可以去继承Operation这个类，但是在工厂类中我们还是需要增加一个case来判断，这样对于工厂类的维护和扩展就比较麻烦，需要去修改原有的代码，从而给维护和扩展带来了困难。

工厂方法模式的UML图：

![img](../imageswps3987.tmp.jpg) 

实现的代码如下：

**public** **abstract** **class** Operation {

 

	**private** **double** numberA;
	
	**private** **double** numberB;

 


	**public** **abstract** **double** getResult();

 


	**public** **void** setNumberA(**double** numberA) {
	
		**this**.numberA = numberA;
	
	}

 


	**public** **void** setNumberB(**double** numberB) {
	
		**this**.numberB = numberB;
	
	}

 


	**public** **double** getNumberA() {
	
		**return** numberA;
	
	}

 


	**public** **double** getNumberB() {
	
		**return** numberB;
	
	}

 


}

 

**public** **class** Add **extends** Operation {

 

	@Override
	
	**public** **double** getResult() {
	
		**return** getNumberA() + getNumberB();
	
	}

 


}

 

**public** **class** Divide **extends** Operation {

 

	@Override
	
	**public** **double** getResult() {
	
		**return** getNumberA() / getNumberB();
	
	}

 


}

 

**public** **class** Minus **extends** Operation {

 

	@Override
	
	**public** **double** getResult() {
	
		**return** getNumberA() - getNumberB();
	
	}

 


}

 

**public** **class** Multiply **extends** Operation {

 

	@Override
	
	**public** **double** getResult() {
	
		**return** getNumberA() * getNumberB();
	
	}

 


}

 

**public** **abstract** **class** OperationFactory {

 

	**public**  **abstract**  Operation createOperation();

 


}

 

**public** **class** AddFactory **extends** OperationFactory{

 

	@Override
	
	**public** Operation createOperation() {
	
		**return** **new** Add();
	
	}

 


}

 

**public** **class** DivideFactory **extends** OperationFactory{

 

	@Override
	
	**public** Operation createOperation() {
	
		**return** **new** Divide();
	
	}

 


}

 

**public** **class** MinusFactory **extends** OperationFactory{

 

	@Override
	
	**public** Operation createOperation() {
	
		**return** **new** Minus();
	
	}

 


}

 

**public** **class** MultiplyFactory **extends** OperationFactory {

 

	@Override
	
	**public** Operation createOperation() {
	
		**return** **new** Multiply();
	
	}

 


}

 

**public** **class** Test {

 

	**public** **static** **void** main(String[] args) {

 


		OperationFactory of = **new** AddFactory();

 


		Operation o = of.createOperation();

 


		o.setNumberA(10);
	
		o.setNumberB(20);
	
		System.**out**.println(o.getResult());

 


	}

 


}

 

分析：通过工厂方法模式，如果需要增加一个平方根的运算，只需要继承Operation和OperationFactory类重写它的方法，原来的代码结构并不会发生改变，而需要修改的知识客户端。解耦的同时提高了程序的可维护性和可扩展性。