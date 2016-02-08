# Lambda表达式:Python与C++中的区别
2016-02-05   <br /> <br />    
Python和C++11中都支持Lambda表达式，但是 [Python中的lambda表达式](http://www.secnetix.de/olli/Python/lambda_functions.hawk) 和 [C++中的lambda表达式](http://en.cppreference.com/w/cpp/language/lambda) 使用方法上却有一些区别，下面总结了其中的一些区别.            

## 区别 #1:return语句             
C++中，lambda表达式要返回一个值，就要显式使用 `return` 语句,如下:         

```cpp       
// 返回一个整数的平方
auto func = [](int x) {return x*x;};
std::cout<<func(5);
```

然而在Python中，lambda表达式要返回一个值，**不用也不能** 使用 `return` 语句，而是直接写要返回的表达式即可,如下:           

```python          
>>> func = lambda x : x * x
>>> func(5)      
25
```

## 区别 #2:是否允许多条语句            
Python中，lambda表达式的函数体部分，只允许包含一条表达式,之后返回这条表达式的结果(**注意返回结果不用也不能使用 `return` 关键字**);而C++中的lambda表达式中则允许包含有多条语句。例如下面的C++代码片段中的lambda表达式中包含了 `if-else` 条件语句:            

```cpp           
int x = 10;
auto func = [=]() 
	 {
	 	if(x < 5)
			std::cout<<"x < 5";
		else
			std::cout<<"x >= 5";
	 };
```

而 `if-else` 条件语句则不允许在Python的lambda表达式中出现，因为Python的lambda表达式的函数体部分只允许包含一条表达式.          

另外需要注意的是，Python中的lambda表达式中只允许包含一条表达式，而不是一条语句，因此像下面的lambda表达式是错误的:           

```python         
>>> func = lambda x : x += 1
```          

上面的lambda表达式中的 `x += 1` 是一条赋值语句，而不是一条表达式，因此是错误的。         

## 区别 #3:捕获变量的规则               
C++中的lambda表达式通过lambda表达式中的中括号部分来设置lambda表达式捕获变量的规则:         

- **[=]** 表示以传值的方式捕获外部变量                  
- **[&]** 表示以引用的方式捕获外部变量             
- **[]** 表示不捕获外部变量             

而在Python中，lambda表达式则是按照 [LEGB](http://spartanideas.msu.edu/2014/05/12/a-beginners-guide-to-pythons-namespaces-scope-resolution-and-the-legb-rule/) 的规则来捕获外部变量的.             

