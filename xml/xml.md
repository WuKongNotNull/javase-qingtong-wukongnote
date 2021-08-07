# **7-XML**

**XML简介**

可扩展标记语言

**特点**

- 与操作系统、编程语言的开发平台无关

- 实现不同系统之间的数据交换

**作用**

	- 数据交互
	- 配置应用程序和网站
	- Ajax基石

## **XML保存衣服尺码信息**

```java
<?xml version="1.0" encoding="UTF-8"?>
<clothesSize>
    <size range="height&lt;165">S</size>
    <size range="165&lt;height&lt;170">M</size>
    <size range="110&lt;height&lt;175">L</size>
    <size range="175&lt;height&lt;180">XL</size>
    <size range="180&lt;height&lt;185">XXL</size>
</clothesSize>
```



## **Dom解析XML：手机品牌和型号的增删改查**

```java
<?xml version="1.0" encoding="GB2312"?>
<PhoneInfo>
    <Brand name="华为">
        <Type name="U8650"/>
        <Type name="HW123"/>
        <Type name="HW321"/>
    </Brand>
    <Brand name="苹果">
    	<Type name="iPhone4"/>
    </Brand>
</PhoneInfo>
```

```java
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStreamWriter;
import java.io.UnsupportedEncodingException;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.transform.OutputKeys;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerConfigurationException;
import javax.xml.transform.TransformerException;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;

import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import org.xml.sax.SAXException;


public class ParseXMLDemo {
	private Document document=null;
	public static void main(String[] args) {
		ParseXMLDemo pd=new ParseXMLDemo();
		pd.getDocument();
		pd.showInfo();
//		pd.add();
//		pd.update();
//		pd.savaXML("new.xml");
//		pd.delete();
	}
	
	public void getDocument(){
		DocumentBuilderFactory  factory=DocumentBuilderFactory.newInstance();
		try {
			DocumentBuilder builder=factory.newDocumentBuilder();
			document=builder.parse("收藏信息.xml");
		} catch (ParserConfigurationException e) {
			e.printStackTrace();
		} catch (SAXException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
	
	
	//获取手机品牌和属性
	public void showInfo(){
		NodeList brands=document.getElementsByTagName("Brand");
		for(int i=0;i<brands.getLength();i++){
			Node node=brands.item(i);
			Element eleBrand=(Element)node;
			System.out.println(eleBrand.getAttribute("name"));
			
			NodeList types=eleBrand.getChildNodes();
			for(int j=0;j<types.getLength();j++){
				Node typeNode=types.item(j);
				if(typeNode.getNodeType()==Node.ELEMENT_NODE){
					Element eleType=(Element)typeNode;
					System.out.println(eleType.getAttribute("name"));
				}
			}
		}
	}
	
	//保存XML文件
	public void savaXML(String path){
		TransformerFactory factory=TransformerFactory.newInstance();
		factory.setAttribute("indent-number", "4");
		try {
			Transformer transformer=factory.newTransformer();
			transformer.setOutputProperty(OutputKeys.ENCODING, "gb2312");
			transformer.setOutputProperty(OutputKeys.INDENT, "yes");
//			StreamResult result=new StreamResult(new FileOutputStream(path));
			StreamResult result=new StreamResult(new OutputStreamWriter(new FileOutputStream(path), "gb2312"));
			DOMSource  source=new DOMSource(document);
			transformer.transform(source, result);
		} catch (TransformerConfigurationException e) {
			e.printStackTrace();
		} catch (TransformerException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (FileNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (UnsupportedEncodingException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
	
	//添加
	public void add(){
		Element element=document.createElement("Brand");
		element.setAttribute("name", "三星");
		Element ele1=document.createElement("Type");
		ele1.setAttribute("name", "Note3");
		element.appendChild(ele1);
		document.getElementsByTagName("PhoneInfo").item(0).appendChild(element);
		this.savaXML("new.xml");
	}
	
	//修改
	public void update(){
		NodeList brands=document.getElementsByTagName("Brand");
		for(int i=0;i<brands.getLength();i++){
			Node brand=brands.item(i);
			Element eleBrand=(Element)brand;
			eleBrand.setAttribute("id", i+"");
		}
		this.savaXML("new.xml");
	}
	
	//删除
	public void delete(){
		NodeList brands=document.getElementsByTagName("Brand");
		for(int i=0;i<brands.getLength();i++){
			Node brand=brands.item(i);
			Element eleBrand=(Element)brand;
			if(eleBrand.getAttribute("name").equals("华为")){
				eleBrand.getParentNode().removeChild(eleBrand);
			}
		}
		this.savaXML("new.xml");
	}
}

```

## **Dom4j解析XML：手机品牌和型号的增删改查**

```java
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.util.Iterator;

import org.dom4j.Document;
import org.dom4j.Element;
import org.dom4j.io.OutputFormat;
import org.dom4j.io.SAXReader;
import org.dom4j.io.XMLWriter;



public class Dom4j {

	public static Document doc;
	
	public static void main(String[] args) {
		loadDocument();
//		showPhoneInfo();
//		saveXML("src/新收藏.xml");
//		addNewPhoneInfo();
//		updatePhoneInfo();
		deleteItem();
		showPhoneInfo();
	}

	public static void loadDocument(){
		try{
			SAXReader saxReader = new SAXReader();
			doc = saxReader.read(new File("src/收藏信息.xml"));
		}catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		}
	}
	
	public static void updatePhoneInfo(){
		// 获取XML的根节点
		Element root = doc.getRootElement();
		int id = 0;
		for (Iterator itBrand = root.elementIterator(); itBrand.hasNext();) {
			Element brand = (Element) itBrand.next();
			id++;
			brand.addAttribute("id", id + "");
		}

		saveXML("src/收藏信息.xml");

	}
	
	public static void deleteItem(){
		// 获取XML的根节点
		Element root = doc.getRootElement();
		int id = 0;
		for (Iterator itBrand = root.elementIterator(); itBrand.hasNext();) {
			Element brand = (Element) itBrand.next();
			if (brand.attributeValue("name").equals("华为")) {
				brand.getParent().remove(brand);
			}
		}
//		saveXML("src/收藏信息.xml");			
	}
	
	public static void showPhoneInfo() {
		// 获取XML的根节点
		Element root = doc.getRootElement();
		// 遍历所有的Brand标签
		for (Iterator itBrand = root.elementIterator(); itBrand.hasNext();) {
			Element brand = (Element) itBrand.next();
			// 输出标签的name属性
			System.out.println("品牌：" + brand.attributeValue("name"));
			// 遍历Type标签
			for (Iterator itType = brand.elementIterator(); itType.hasNext();) {
				Element type = (Element) itType.next();
				// 输出标签的name属性
				System.out.println("\t型号：" + type.attributeValue("name"));
			}
		}

	}
	
	public static void saveXML(String path){
		try {
			OutputFormat format = OutputFormat.createPrettyPrint();
			format.setEncoding("GBK"); // 指定XML编码
			XMLWriter writer;
			writer = new XMLWriter(new FileWriter(path), format);
			writer.write(doc);
			writer.close();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
	
	public static void addNewPhoneInfo(){
		// 获取XML的根节点
		Element root = doc.getRootElement();
		// 创建Brand标签
		Element el = root.addElement("Brand");
		// 给Brand标签设置属性
		el.addAttribute("name", "三星");
		// 创建Type标签
		Element typeEl = el.addElement("Type");
		// 给Type标签设置属性
		typeEl.addAttribute("name", "Note4");
		saveXML("src/收藏信息.xml");
	}
}

```

```java
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.transform.OutputKeys;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerConfigurationException;
import javax.xml.transform.TransformerException;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;

import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import org.xml.sax.SAXException;

public class Main {

	public static Document doc;
	
	public static void main(String[] args) {
		deleteItem();
	}

	public static void updatePhoneInfo(){
		try {
			// 1、得到DOM解析器的工厂实例
			DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
			// 2、从DOM工厂获得DOM解析器
			DocumentBuilder db = dbf.newDocumentBuilder();
			// 3、解析XML文档，得到一个Document，即DOM树
			doc = db.parse("src/收藏信息.xml");
			
			NodeList list = doc.getElementsByTagName("Brand");
			for(int i = 0; i < list.getLength(); i++){
				Element el = (Element) list.item(i);
				el.setAttribute("id", i+"");
			}
			
			saveXML("src/收藏信息.xml");
		} catch (Exception e) {
			// TODO: handle exception
		}
	}
	
	public static void deleteItem(){
		try {
			// 1、得到DOM解析器的工厂实例
			DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
			// 2、从DOM工厂获得DOM解析器
			DocumentBuilder db = dbf.newDocumentBuilder();
			// 3、解析XML文档，得到一个Document，即DOM树
			doc = db.parse("src/收藏信息.xml");
			
			NodeList list = doc.getElementsByTagName("Brand");
			for(int i = 0; i < list.getLength(); i++){
				Element el = (Element) list.item(i);
				if(el.getAttribute("name").equals("华为")){
					el.getParentNode().removeChild(el);
				}
			}
			
			saveXML("src/收藏信息.xml");
		} catch (Exception e) {
			// TODO: handle exception
		}
	}
	
	public static void showPhoneInfo(){
		try {
			// 1、得到DOM解析器的工厂实例
			DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
			// 2、从DOM工厂获得DOM解析器
			DocumentBuilder db = dbf.newDocumentBuilder();
			// 3、解析XML文档，得到一个Document，即DOM树
			doc = db.parse("src/收藏信息.xml");

			//获取所有Brand标签
			NodeList brandList = doc.getElementsByTagName("Brand");
			//遍历所有的Brand标签
			for(int i = 0 ; i < brandList.getLength(); i++){
				Node brand = brandList.item(i);
				//判断节点类型是否是Element
				if(brand.getNodeType() != 1){
					continue;
				}
				//输出标签的name属性
				Element el = (Element)brand;
				System.out.println("品牌：" + el.getAttribute("name"));
				
				//获取Brand标签的所有子节点
				NodeList typeList = el.getChildNodes();
				//遍历Brand标签
				for(int j = 0; j < typeList.getLength(); j++){
					Node type = typeList.item(j);
					if(type.getNodeType() != 1){
						continue;
					}
					el = (Element)type;
					System.out.println("\t型号：" + el.getAttribute("name"));
				}
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	public static void saveXML(String path){
		try {
			//获得TransformerFactory对象
			TransformerFactory transformerFactory = TransformerFactory.newInstance();
			Transformer transformer = transformerFactory.newTransformer();
			DOMSource domSource = new DOMSource(doc);
			// 设置编码类型
			transformer.setOutputProperty(OutputKeys.ENCODING, "gb2312");
			StreamResult result = new StreamResult(new FileOutputStream(path));
			// 把DOM树转换为XML文件
			transformer.transform(domSource, result);
		} catch (TransformerConfigurationException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (FileNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (TransformerException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
	
	public static void addNewPhoneInfo(){
		try {
			// 1、得到DOM解析器的工厂实例
			DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
			// 2、从DOM工厂获得DOM解析器
			DocumentBuilder db;

			db = dbf.newDocumentBuilder();

			// 3、解析XML文档，得到一个Document，即DOM树
			doc = db.parse("src/收藏信息.xml");

			//创建Brand标签
			Element el = doc.createElement("Brand");
			//给Brand标签设置属性
			el.setAttribute("name", "三星");
			//创建Type标签
			Element typeEl = doc.createElement("Type");
			//给Type标签设置属性
			typeEl.setAttribute("name", "Note4");
			//将Type标签
			el.appendChild(typeEl);
			doc.getElementsByTagName("PhoneInfo").item(0).appendChild(el);
		} catch (ParserConfigurationException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (SAXException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}
```

