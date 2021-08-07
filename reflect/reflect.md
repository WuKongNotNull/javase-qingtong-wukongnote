# 反射

主要是指程序可以访问，检测和修改它本身状态或行为的一种能力，并能根据自身行为的状态和结果，调整或修改应用所描述行为的状态和相关的语义。
反射是Java中一种强大的工具，能够使我们很方便的创建灵活的代码，这些代码可以再运行时装配，无需在组件之间进行源代码链接。但是反射使用不当会成本很高！
PS:通过反射的机制创建对象，获取类中的属性和方法，并调用类中的属性和方法;
二、反射的作用
1.反编译：.class-->.java
2.通过反射机制访问java对象的属性，方法，构造方法等；这样好像更容易理解一些，下边我们具体看怎么实现这些功能。
三、JDK提供的反射类
java.lang.Class;                
java.lang.reflect.Constructor;
java.lang.reflect.Field;        
java.lang.reflect.Method;
java.lang.reflect.Modifier;
四、反射的API
1.获取类的方法：
a)Class.forName (“类的全路径”);
b)类名.class
c)对象.getClass();

2.创建对象
a)newInstance();

3.获取属性
public Field[] getFields()；				获取当前class中所有的公共属性
public Field getField(String name)；		获取当前class中指定属性名的公共属性
public Field[] getDeclaredFields()；		获取当前class中所有的属性包含私有的
public Field getDeclaredField(String name)；	获取当前class中指定属性名的属性包含私有的
4.获取方法
public Method getMethod(String name, Class<?>... parameterTypes) 	获取指定方法名和参数列表的方法
public Method[] getMethods()								获取所有的方法
public Method getDeclaredMethod(String name,Class<?>... parameterTypes) 获取指定方法名和参数列表的方法，包含私有的
public Method[] getDeclaredMethods()						获取所有的方法，包含私有的
5.执行方法：
调用Method对象中的public Object invoke(Object obj, Object... args)方法。

五、反射的应用
1.反射的应用：Servlet、各大框架… 等都利用了java的反射机制；
例：简单工厂模式+反射改进
连接不同的数据库
/**
* 数据库接口
* @author Administrator
*
*/
public interface DataSource {

public void init();

public void select();

public void insert();

}

/**
* 数据库抽象类：可以扩展
* @author Administrator
*
*/
public abstract class AbstractDataSource implements DataSource {

private String driverClass;
private String url;
private String user;
private String password;


}

public class Oralce extends AbstractDataSource {

	@Override
	public void init() {
		System.out.println("Oracle连接成功...");
	}
	
	@Override
	public void select() {
		System.out.println("Oralce查询....");
	}
	
	@Override
	public void insert() {
		System.out.println("Oralce添加....");
	}

}

public class MySql extends AbstractDataSource {

	@Override
	public void init() {
		System.out.println("Mysql连接成功...");
	}
	
	@Override
	public void select() {
		System.out.println("Mysql查询...");
	}
	
	@Override
	public void insert() {
		System.out.println("Mysql添加...");
	}

}

public class Factory {

	public static DataSource getDataSource(String className) {
		DataSource dataSource = null;
		try {
			Class c = Class.forName(className);
			dataSource=(DataSource) c.newInstance();
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (InstantiationException e) {
			e.printStackTrace();
		} catch (IllegalAccessException e) {
			e.printStackTrace();
		}
		return dataSource;
	}
}

public class Test {

	public static void main(String[] args) {
		//com.mxp.db.MySql
		//com.mxp.db.Oralce
		DataSource dataSource = Factory.getDataSource("com.mxp.db.Oralce");
		dataSource.init();
		dataSource.select();
		dataSource.insert();
	}

}

模仿servlet
/**
* 预订义好的抽象类
* @author Administrator
*
*/
public abstract class HttpServlet {

public abstract void doGet();

public abstract void doPost();

}
/**
* 反射并调用HttpServlet中的方法
* @author Administrator
*
*/
public class Servlet {
/**
    * @param method :get  post
    * @param className
      */
      public void execute(String method, String className) {
      try {
      Class c = Class.forName(className);
      HttpServlet servlet = (HttpServlet) c.newInstance();
      method = "do" + String.valueOf(method.charAt(0)).toUpperCase() + method.substring(1);
      Method m = c.getMethod(method);
      m.invoke(servlet);
      } catch (Exception e) {
      e.printStackTrace();
      }
      }

}
/**

* 用户自定义的实现类
* @author Administrator
*
*/
public class UserServlet extends HttpServlet {

@Override
public void doGet() {
System.out.println("UserServlet is doGet");
}

@Override
public void doPost() {
System.out.println("UserServlet is doPost");
}

}
/**

* 测试类
* @author Administrator
*
*/
public class Test {

public static void main(String[] args) {

	String className = "com.mxp.servlet.UserServlet";
	String method = "post";
	Servlet servlet = new Servlet();
	servlet.execute(method, className);
}

}




