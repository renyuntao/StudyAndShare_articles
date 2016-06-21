# Linux:如何获取终端的大小?           
<!--2016-05-01-->  <br /><br />            
              
&nbsp;&nbsp;&nbsp;在Linux平台下编写程序，有时候需要获取终端的大小，之后根据终端的大小来调整输出到终端的信息的格式，使得显示结果更加美观。下面介绍几种获取终端大小的方法。                
            
## 在Bash Shell中获取终端大小
&nbsp;&nbsp;&nbsp;在Shell Script中获取终端大小的方法很简单，Bash Shell 中有两个环境变量，分别是 `LINES` 和 `COLUMNS`,只要输出这两个环境变量的值，即可查看当前终端的大小了。             
查看终端的高度:         
        
```bash
$ echo $LINES
```
查看终端的宽度:             
     
```bash
$ echo $COLUMNS
```
此外，使用 `tput` 和 `stty` 这两个命令行工具也可以查看当前终端的大小:            
查看终端的高度:           
    
```bash
$ tput lines
```
查看终端的宽度:           

```bash
$ tput cols
```
使用 `stty` 可以同时返回终端的高度和宽度:              
          
```bash
$ stty size
```
## 在C/C++中获取终端的大小
&nbsp;&nbsp;&nbsp;在C/C++中获取终端的大小，需要借助 `<sys/ioctl.h>` 头文件中的 `ioctl()` 函数，关于该函数的用法，你可以通过 `man ioctl` 来查看，下面只给出使用该函数获取终端大小的方法:              
            
```cpp
#include<sys/ioctl.h>
#include<unistd.h>
#include<iostream>

int main()
{
	struct winsize w;
	ioctl(STDOUT_FILENO,TIOCGWINSZ,&w);
	std::cout<<w.ws_row<<"\n";  // 终端高度
	std::cout<<w.ws_col<<"\n";   // 终端宽度
	return 0;
}
```
## 在Python中获取终端的大小             
在Python中，可以使用位于 **[shutil](https://docs.python.org/3/library/shutil.html)** 模块下的 `get_terminal_size()` 函数来获取当前终端的大小，如下:           
          
```python
>>> import shutil
>>> res = shutil.get_terminal_size()
>>> print(res.lines, res.columns)  # 输出终端的高度和宽度
```
