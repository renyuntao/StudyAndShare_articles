# 不能被C++编译器编译的C程序(二)
2016-01-28  <br /><br />        

&nbsp;&nbsp;&nbsp;虽然C++被设计为向后兼容C，但是仍然有很多C程序在被C++编译器编译时会产生编译错误，二者毕竟不是同一种语言。之前写过的一篇文章 [不能被C++编译器编译的C程序](http://www.studyandshare.info/c_pro_canot_compile.html) 中总结了一些不能被C++编译器编译的C程序，本文将继续分享另外一些不能被C++编译器编译的C程序.            

### 复合文字(Compound Literals)           
复合文字是在C99标准中新增的特性,它允许我们创建一个无名的数组或者结构体，例如创建一个无名的一维数组的复合文字如下:          

	(int []){1,2,3,4};

上述语句创建了一个包含4个int型元素的一维数组.          
又例如，使用复合文字向函数中传递结构体参数:            

	struct Point
	{
		int x,y;
	};

	int func(struct Point val)
	{
		// do something
	}

	int main(void)
	{
		func((struct Point){1,2});
		return 0;
	}

目前C++中并不支持复合文字这种特性，因此如果一个C程序中包含了复合文字的相关语句，则该程序只能用C编译器来编译，如果使用C++编译器，则会报错。     

### 字符串字面值向char*的转换
在C中，允许一个char类型的指针指向一个字符串字面值，即下面的语句是合法的:          

	char *p = "Hello World!";

而C++中，字符串字面值向char*类型的转换已经被废弃了，因此在C++程序中，如果包含类似上面的语句，则使用 `g++` 进行编译的时候，虽然能够通过编译，但是会产生下面的警告:            

> warning: deprecated conversion from string constant to ‘char*’ [-Wwrite-strings]

C++中废弃这种类型的转换，原因在于字符串字面值是一个常量，它通常存储在RAM中，如果用一个非常量指针指向一个常量，则可能发生非常量指针修改常量字符串的错误。        

