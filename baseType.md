## 基础数据类型
| 类型        	| 取值范围          | 位  | 默认值|
| ------------- |:-------------:	| :---------- |:-----:| 
| byte      	| [-128, 127] 		| 8 	| 0　	|
| short      	| [- 2^15, 2^15-1]	| 16	| 0 	|
| int 			| [- 2^31, 2^31-1]  | 32	| 0		|
| long 			| [- 2^63, 2^63-1]	| 64	| 0L 	|
| float 		|-3.40292347E+38-3.40292347E+38| 32 | 0.0f	|
| double 		|-1.79769313486231570E+308-1.79769313486231570E+308|64| 0.0d	|
| char 			| \u0000 - \uffff 	|16 	| \u0000	|
| boolean 		| true/false		|1		| false 	|

## 基本数据类型之间的转换
byte <（short=char）< int < long < float < double
如果从小转换到大，可以自动完成，而从大到小，必须强制转换。short和char两种相同类型也必须强制转换。

+ 自动转换
+ 强制转换
+ 表达式转换

#### 自动转换
自动转换时发生扩宽（widening conversion）。因为较大的类型（如int）要保存较小的类型（如byte），内存总是足够的，不需要强制转换。如果将字面值保存到byte、short、char、long的时候，也会自动进行类型转换。

#### 强制转换
	int a = 300;
	byte b = (byte)a;

#### 表达式转换
除了赋值以外，表达式计算过程中也可能发生一些类型转换。在表达式中，类型提升规则如下：

- 所有byte/short/char都被提升为int。
- 如果有一个操作数为long，整个表达式提升为long。float和double情况也一样。

		int a = 300;
		byte b = 1;
		int c = a + b;

## 练习
1. 完成加减乘除算法：

		/**
		 * calculate
		 * @param algorithm: (add(a)|subtract(s)|multiply(m)|division(d))
		 * @param arg1
		 * @param arg2
		 * @return
		 */
		public double calculate(String algorithm, double arg1, double arg2) {

			//TODO
			return 0.0d;
		}


[返回上页](basic.md)