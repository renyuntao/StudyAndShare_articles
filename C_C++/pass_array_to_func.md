# 如何将数组作为参数传递给函数?  
2015-11-10   <br />      
&nbsp;&nbsp;&nbsp;将一个数组作为参数传递给函数的方式有很多种，而且形式各异，传递一维数组和传递二维数组的形式又有很大差异，很容易导致混乱。因此这篇文章总结了一些将常用的一维数组和二维数组作为参数传递给函数的方式，以供参考和对比.        
## 将一维数组作为参数传递给函数
将一维数组作为参数传递给函数的方式比较简单，有如下三种:     
### 方法一

    int arr[10];
    void passArr(int *param)
    {
    	// do something
    }
	passArr(arr);
### 方法二

    int arr[10];
    void passArr(int param[10])
    {
    	// do something
    }
    passArr(arr);
### 方法三

    int arr[10];
    void passArr(int param[])
    {
    	// do something
    }
    passArr(arr);

******
## 将二维数组作为参数
相对于一维数组而言，将二维数组作为参数传递给函数就比较复杂了，有如下几种方式:       
### 方法一

    int arr[10][10];
    void passArr(int arr[][10])      // 需指定列数
    {
    	// do something
    }
    passArr(arr);
### 方法二

    int arr[10][10];
    void passArr(int (*arr)[10])     // 需指定列数
    {
    	// do something
    }
### 方法三

	int **arr;
	arr = new int* [10];
	for(int i = 0; i < 10; ++i)
		arr[i] = new int[10];
	void passArr(int **arr)
	{
		// do something
	}
	passArr(arr);
	for(int i = 0; i < 10; ++i)
		delete [] arr[i];
	delete [] arr;
### 方法四 (仅适用于C99)
在C99中增加了一个新的特性 —— 变长数组(**VLA**),对于一个二维数组，可以使用变量来表示数组的行数和列数.        

	int rows = 2;
	int cols = 2;
	int arr[rows][cols];
	void passArr(int rows,int cols,int arr[rows][cols])
	{
		// do something
	}
	passArr(rows,cols,arr);
**注意:** 上面程序中passArr函数中参数的顺序应该是变量rows，cols要在数组arr之前，因为数组arr用到了变量rows和cols.       
