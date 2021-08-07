# **9-接口**

	(1)接口是一种能力和约定，解决了单继承的局限。
	
	(2)接口的特点：
	
		A:接口用关键字interface修饰
	
			interface 接口名 {}
	
		B:类实现接口用implements修饰
	
			class 类名 implements 接口名 {}
	
		C:接口不能实例化
	
		D:接口的实现类
	
			a:是一个抽象类。
	
			b:是一个具体类，这个类必须重写接口中的所有抽象方法。
	
	(3)接口的成员特点：
	
		A:成员变量
	
			只能是常量
	
			默认修饰符：public static final
	
		B:构造方法
	
			没有构造方法
	
		C:成员方法
	
			只能是抽象的
	
			默认修饰符：public abstract

```java
//父类
public  abstract class Door {
	//防盗门是一个门  is a  的关系
	//开门功能
	public  void openDoor() {};
	//关门功能
	public void closeDoor() {};
}
```

```java
//接口
public interface Lock {
	//防盗门上有一个把锁  has  a  的关系
	//锁门功能
	public void lockDoor();
	//解锁功能
	public void unlockDoor();

}
```

```java
public class TheftDoor extends Door implements Lock{
	//开门的实现方法
	public void openDoor() {
		System.out.println("开始开门。开门中。开门结束");
	}
	//关门的实现方法
	public  void closeDoor() {
		System.out.println("准备关门。关门中。关门结束");
	}
	//锁门功能
	@Override
	public void lockDoor() {
		System.out.println("插入钥匙，转动，锁上，拔出钥匙");
	}
	//解锁功能
	@Override
	public void unlockDoor() {
		System.out.println("插入钥匙，转动，解锁，拔出钥匙");
	}
}
```

```java
import org.junit.Test;
public class Mytest {
	@Test
	public void test1() {		
		//不使用接口调用
		TheftDoor theftDoor= new TheftDoor();		
		//出门步骤：关门---》锁门		
		theftDoor.closeDoor();
		theftDoor.lockDoor();		
		//回家步骤：开锁---》开门
		theftDoor.unlockDoor();
		theftDoor.openDoor();		
		System.out.println("+++++++++++++++++++++++++++");		
		//调用接口中的方法，实现实现类中的方法
		Lock lock= new TheftDoor();
		 lock.unlockDoor();
		 lock.lockDoor();		 
		 System.out.println("++++++++++++++++++++++++++");		 
		 //调用抽象类中的方法，实现实现类中的方法
		 Door door= new TheftDoor();
		 door.closeDoor();
		 door.openDoor();		 	 
		 /**
		  * 门和防盗门是继承关系；
		  * 但是门和锁是非继承关系，如何实现继承呢？
		  * java的设计者想到了interface接口。
		  * 
		  */	
	}
}
```

### **练习**

#### **1、设计USB接口？**

```java
/**
 * USB接口。
 */
public interface UsbInterface {
	/**
	 * USB接口提供服务。
	 */
	void service();
}
```

```java
/**
 * U盘。
 */
public class UDisk implements UsbInterface {
	public void service() {
		System.out.println("连接USB口，开始传输数据。");
	}
}
```
```java
/**
 * USB风扇。
 */
public class UsbFan implements UsbInterface {
	public void service() {
		System.out.println("连接USB口，获得电流，风扇开始转动。");
	}
}
```
```java
/**
 * 测试类。
 * @param args
 */
public class Test {	
	public static void main(String[] args) {	
		//1、U盘
		UsbInterface uDisk = new UDisk();
		uDisk.service();	
		//2、USB风扇
		UsbInterface usbFan= new UsbFan();
		usbFan.service();
	}
}
```

#### **2、设计打印机？**

```java
/**
 * 墨盒接口。
 */
public interface InkBox {
	
	/**
	 * 得到墨盒颜色
	 * @return 墨盒颜色	 
	 */
	public String getColor();
}
```
```java
/**
 * 彩色墨盒。
 */
public class ColorInkBox implements InkBox {
	
	public String getColor() {
		return "彩色";
	}
}

```
```java
/**
 * 黑白墨盒。
 */
public class GrayInkBox implements InkBox {
	
	public String getColor() {
		return "黑白";
	}
}
```
```java
/**
 * 纸张接口。
 */
public interface Paper {
	
	/**
	 * 得到纸张大小
	 * @return 纸张大小
	 */
	public String getSize();
}
```
```java
/**
 * A4纸类。
 */
public class A4Paper implements Paper {

	public String getSize() {
		return "A4";
	}

}
```

```java
/**
 * B5纸类。
 */
public class B5Paper implements Paper {

	public String getSize() {
		return "B5";
	}

}
```

```java
/**
 * 打印机类。
 */
public class Printer  {	
	InkBox inkBox;  //墨盒
	Paper paper;   //纸张
		
	/**
	 * 设置打印机墨盒
	 * @param inkBox 打印使用的墨盒
	 */
	public void setInkBox(InkBox inkBox){
		this.inkBox=inkBox;
	}
	
	/**
	 * 设置打印机纸张
	 * @param paper 打印使用的纸张
	 */
	public void setPaper(Paper paper){
		this.paper=paper;
	}

	/**
	 * 使用墨盒在纸张上打印
	 */
	public void print(){
		System.out.println("使用"+inkBox.getColor()+
				"墨盒在"+paper.getSize()+"纸张上打印。");
	}
}
```

```java
/**
 * 测试类
 */
public class Test {
	public static void main(String[] args) {
		//1、定义打印机
		InkBox inkBox = null;
		Paper paper = null;
		Printer printer=new Printer();
		
		//2.1、使用黑白墨盒在A4纸上打印
		inkBox=new GrayInkBox();
		paper=new A4Paper();		
		printer.setInkBox(inkBox);
		printer.setPaper(paper);
		printer.print();

		//2.2、使用彩色墨盒在B5纸上打印
		inkBox=new ColorInkBox();
		paper=new B5Paper();		
		printer.setInkBox(inkBox);
		printer.setPaper(paper);
		printer.print();
		
		//2.3使用彩色墨盒在A4纸上打印
		paper=new  A4Paper();
		printer.setPaper(paper);
		printer.print();	
	}
}
```