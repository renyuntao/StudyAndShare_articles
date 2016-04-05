# C++: NULL与nullptr的区别            
2016-04-05 <br /><br />              
              
## NULL          
`NULL` 通常定义为 `(void*)0`,这使得将 `NULL` 转换为 `int` 类型也是允许的，但这有时会导致一些问题，如下面的例子:             
             
```cpp
#include<iostream>

int func(int x) 
{
	std::cout<<"In func(int x)";
}
int func(char *s) 
{
	std::cout<<"In char *s\n";
}

int main()
{
	func(NULL);
	return 0;
}
```
编译上面的程序会产生如下错误:            
           
> error: call of overloaded ‘func(NULL)’ is ambiguous

因为 `NULL` 可以转换为 `int` 类型，所以导致了调用 `func` 函数时发生歧义.                    
             
## nullptr
与 `NULL` 不同，`nullptr` 是C++11中新增的一个关键字，在所有可以使用 `NULL` 的地方都可以使用它，但是 `nullptr` 不可以转换为 `int` 类型，因此，如果将上面程序中的 `NULL` 替换为 `nullptr`,则编译时的错误就会消除，并且程序输出如下结果:              
       
```
In char *s
```
