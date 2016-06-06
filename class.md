## 面向对象

+ 什么是对象和类
+ 构造函数
+ 类变量
+ 类方法
+ 包
+ 依赖
+ 访问权限
+ 接口
+ 抽象类
+ 继承

#### 什么是对象和类？

- 对象：万事万物皆对象，狗是对象，它们颜色不同，种类不同，年龄不同，它们都能吃饭，都是奔跑，都能吠叫。
- 类：用来描述对象。

		public class Dog {

			private String color;
			private int age;
			private String type;

			public void eat() {
				System.out.println("Dog eat");
			}

			public void run() {
				System.out.println("Dog run");
			}

			public void bark() {
				System.out.println("Dog bark");
			}
		}

#### 构造函数

- 定义

		public class Dog {

			public Dog() {

			}

			public Dog(String color) {
				this.color = color;
			}

			private String color;
			private int age;
			private String type;

			public void eat() {
				System.out.println("Dog eat");
			}

			public void run() {
				System.out.println("Dog run");
			}

			public void bark() {
				System.out.println("Dog bark");
			}
		}

#### 类变量

	private String color;
	private int age;
	private String type;

#### 类方法
	
	public void eat() {
		System.out.println("Dog eat");
	}

	public void run() {
		System.out.println("Dog run");
	}

	public void bark() {
		System.out.println("Dog bark");
	}

#### 包

	package com.acxiom.java.training.base;

	public class Dog {

	}

#### 依赖

	package com.acxiom.training;

	import java.util.ArrayList;
	import java.util.List;

	public class Class1 {
		
		public List<String> list = new ArrayList<String>();
		
	}


#### 访问权限

+ 控制修饰符
+ 非控制修饰符


控制修饰符

+ public
+ private
+ 缺省(friendly/package)
+ protected

非控制修饰符

+ static
+ final
+ abstract
+ synchronized

方法访问限制：
|案例		|public	| protected	|  friendly	|  private	|
| ---		|---	| ---    	|-----|  ---- 	| 
|同类		|	Y	|	Y	  	|	Y 		|	Y  		
|同包子类	|	Y	|	Y		|	Y		|	N		
|同包非子类	|	Y	|	Y		|	Y		|	N		
|异包子类	|	Y	|	Y		|	N		|	N		
|异包非子类	|	Y	|	N		|	N		|	N		

#### 接口
Java接口是一系列方法的声明，是一些方法特征的集合，一个接口只有方法的特征没有方法的实现，因此这些方法可以在不同的地方被不同的类实现，而这些实现可以具有不同的行为（功能）

定义接口格式：

	[public]interface 接口名称 [extends父接口名列表] {

		//静态常量
		[public] [static] [final] 数据类型变量名=常量值;
		//抽象方法
		[public] [abstract] [native] 返回值类型方法名（参数列表）;

	}

	public interface Logger {

		public abstract void log(String message);
	}

	public class ConsoleLogger implements Logger {

		public void log(String message) {
			System.out.println(message);
		}
	}

#### 抽象类
使用了关键词abstract声明的类叫作“抽象类”。如果一个类里包含了一个或多个抽象方法，类就必须指定成abstract（抽象）。“抽象方法”,属于一种不完整的方法，只含有一个声明，没有方法主体。
	
	public abstract class Logger {

		public abstract void log(String message);
	}

	public class ConsoleLogger extends Logger {

		public void log(String message) {
			System.out.println(message);
		}
	}

#### 继承
继承是面向对象最显著的一个特性。继承是从已有的类中派生出新的类，新的类能吸收已有类的数据属性和行为，并能扩展新的能力。

	public class Dog {

		public void run() {
			System.out.println("Dog run");
		}

		public void bark() {
			System.out.println("Dog bark");	
		}
	}

	public class Taidi extends Dog {

		public void run() {
			System.out.println("Taidi run");
		}

		public static void main(String[] args) {
			Dog dog = new Taidi();
			dog.run();
			dog.bark();
		}
	}

[返回上页](basic.md)