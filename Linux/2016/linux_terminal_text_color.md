# Linux:如何在终端设置字体颜色和背景颜色?          
2016-04-22 <br /><br />                
            
## 原理
&nbsp;&nbsp;&nbsp;现代的Linux终端都是采用 **美国国家标准协会转义码([ANSI escape code](https://en.wikipedia.org/wiki/ANSI_escape_code))** 来设置字体颜色和背景颜色的。因此我们可以通过在终端输出相应字体颜色和背景颜色的转义码，即可控制终端的字体颜色和背景颜色。

## 使用说明
&nbsp;&nbsp;&nbsp;ANSI 转义码以 **Esc** 和 **[** 这两个字符开头，其中，**Esc** 字符在 ASCII 码中的十进制表示为 **27**, 十六进制表示为 **\x1B**, 八进制表示为 **\033**.在书写 ANSI 转义码的时候，开头的两个字符可以是一下两种格式中的任意一种:               
           
- **\x1B[**        
- **\033[**

&nbsp;&nbsp;&nbsp;此外，如果你使用的是 **Bash** 的话，你还可以使用 **\e** 来表示 **Esc** 这个字符，即 ANSI 转义码开头的两个字符可以写作　**\e[**, 不过要注意，**\e** 只能够在 **Bash** 中使用，而在其它的Shell 中，则不能够使用. 因为 **\e** 并不属于 **POSIX** 标准。           

### 改变字体颜色
要改变字体颜色，需要使用如下格式的字符串序列:              
          
```
ESC[#m
```
其中字符 `#` 表示一个在 [30,37] 范围内的整数，每一个整数都代表一个颜色，30-37 这8个数字表示的颜色依次为:           
        
```
黑 红 绿 黄 蓝 品红 青 灰
```           
此外，还可以通过如下的语法来改变颜色的亮暗:              
         
```
ESC[#,#m
```
上述语法中，第一个 `#` 表示 0 或者 1,当为0的时候，颜色为暗色，当为1时，颜色为亮色。第二个 `#` 仍然表示 [30,37] 范围内的一个整数.     
### 改变背景颜色              
改变背景颜色的语法与改变字体颜色的语法相同,如下:            
           
```
ESC[#m
```
不同的是，字符 `#` 表示的是一个在 [40,47] 范围内的整数，同样，每一个数字代表一个整数。40-47 这8个数字表示的颜色依次为:       
         
```
黑 红 绿 黄 蓝 品红 青 灰            
```
### 使用 256 种颜色            
使用上面所说的方法只能表示8中颜色，如果想要使用更多的颜色，需要使用另外的语法格式.                
**改变字体颜色:**              
          
```
ESC[38;5;#m
```
**改变背景颜色:**           
            
```
ESC[48;5;#m
```
上面语法中的 `#` 都表示一个在 [0,255] 范围内的整数，每一个整数代表一种颜色，关于数字与颜色对应的列表可以查看 **[这里](https://en.wikipedia.org/wiki/ANSI_escape_code#Colors)**.

## 恢复默认字体颜色和背景颜色
要恢复默认的字体颜色和背景颜色，可以使用下面的语法:             
            
```
ESC[0m
```

## 示例            
### 在终端中的用法            
**设置字体颜色**            
假如现在你想要输出红色字体的字符串 "Hello World",可以使用如下语句:             
  
```bash
$ echo -e "\x1B[31mHelloWorld"
```
同理，要将背景设置为红色，使用如下语句即可:             
         
```bash
echo -e "\x1B[41m"
```

## 在C程序中的用法
下面是一个改变字体颜色的C程序的例子:         
     
```cpp
#include <stdio.h>

#define NORMAL   "\x1B[0m"
#define RED      "\x1B[31m"
#define GREEN    "\x1B[32m"
#define YELLOW   "\x1B[33m"
#define BLUE     "\x1B[34m"
#define MAGENTA  "\x1B[35m"
#define CYAN     "\x1B[36m"
#define GRAY     "\x1B[37m"

int main()
{
    printf(RED"red\n");
    printf(GREEN"green\n");
    printf(YELLOW"yellow\n");
    printf(BLUE"blue\n");
    printf(MAGENTA"magenta\n");
    printf(CYAN"cyan\n");
    printf(GRAY"gray\n");
    printf(NORMAL"normal\n");

    return 0;
}
```
## 相关资源              
如果你使用的是 **Bash**, 还可以设置字体的格式，具体用法可以参见下面的链接:                     
**[Bash tips: Colors and formatting (ANSI/VT100 Control sequences)](http://misc.flogisoft.com/bash/tip_colors_and_formatting)**             
如果你想更改你的终端中命令提示符的的风格，可以查看下面的链接:                 
**[Bash Shell PS1: 10 Examples to Make Your Linux Prompt like Angelina Jolie](http://www.thegeekstuff.com/2008/09/bash-shell-ps1-10-examples-to-make-your-linux-prompt-like-angelina-jolie/)**
