## 数组

+ 什么是数组
+ 定义数组
+ 操作数组
+ 多维数组

#### 什么是数组？

数组就是相同类型数据的集合

#### 定义数组
	
	int[] array = new int[5];

	String[] stringArray = new String[]{"Hello", "World", "Hello", "Acxiom"};

	byte[] byteArray = {1, 2, 3, 4};

#### 操作数组

###### 长度 (.length)	
	
	int[] array = new int[5];

	System.out.println(array.length);

###### 获取元素 (array[ index ])

index 从0开始，到array.length - 1

	String[] stringArray = {"Hello", "World", "Hello", "Acxiom"};	

	System.out.println(stringArray[0]);
	System.out.println(stringArray[1]);
	System.out.println(stringArray[4]);

###### 克隆数组 (array.clone())
	
	int[] array = {1, 2, 3};
	int[] copy = array.clone();

	System.out.println(array.length);
	System.out.println(copy == array);

###### 数组默认初始化值

	int[] array = new int[2];
	char[] charArray = new char[2];
	Object[] objArray = new Object[2];

	System.out.println(array[0]); // 0
	System.out.println(charArray[0]); // ""
	System.out.println(objArray[0]); // null

#### 多维数组

	int[][] array = new int[2][3];
	
	int[][] array2 = new int[2][];
	
	int[][] array3 = {};
	
	int[][] array4 = {{1, 2, 3}, {2, 3, 4}};
	
	int[][] array5 = new int[][]{new int[]{1, 2, 3}, new int[]{2, 3, 4}};
	
	System.out.println(array4[0][0]); //1
	System.out.println(array5[1][1]); //3

## 练习
1. 完成数组合并去重功能：

		/**
		 * Merge two arrays and remove the duplicated ones.
		 * @param array1
		 * @param array2
		 * @return
		 * merge({"Hello", "World"}, {"Hello", "Acxiom"}) ==> {"Hello", "World", "Acxiom"}
		 * merge({"Hello", "World", "World"}, null) ==> {"Hello", "World"}
		 * merge(null, null) ==> null
		 */
		public String[] merge(String[] array1, String[] array2) {
			
			//TODO
			return null;
		}

2. 数组排序

		/**
		 * {3, 5, 4, 2, 5, 1, 6} => {1, 2, 3, 4, 5, 5, 6}
		 * @param array
		 * @return
		 */
		public int[] sort(int[] array) {
			
			//TODO
			return null;
		}


[返回主页](32.html)