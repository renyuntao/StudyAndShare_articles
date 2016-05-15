# C/C++:编写独立于操作系统的代码                  
2016-04-01   <br /><br />               
            
要编写独立于操作系统的代码，可以在同一个程序中，针对依赖于操作系统部分的代码，来编写不同的代码，之后在不同的操作系统中执行不同部分的代码。这里的关键点在于如何检测操作系统的类型，现在很多编译器定义了一些宏，可以用来检测不同的操作系统类型。例如，**GCC** 编译器中有如下宏:             
            
```
_WIN32: 标记系统为32位或64位Windows操作系统               
_WIN64: 标记系统为64位Windows操作系统             
            
unix, __unix, __unix__: 标记系统为 UNIX 操作系统
```
**参阅:** [Stack Overflow](http://stackoverflow.com/questions/142508/how-do-i-check-os-with-a-preprocessor-directive)                  
在 Linux 系统中，使用 `ls` 命令来列出当前目录下的文件，而在 Windows 系统中，则是使用 `dir` 命令，下面是一个在 Linux 和 Windows 平台下都可以运行的程序的例子:               
            
```cpp
#include<cstdlib>

int main()
{
#ifdef _WIN32
	system("dir");
#else
	system("ls");
#endif
	return 0;
}
```

