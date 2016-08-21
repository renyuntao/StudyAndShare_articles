# 如何将一个十进制整数以二进制的格式输出?              
<!--
2016-08-21
--> <br /><br />

有时候，由于特定的功能，我们在写程序时，需要将一个整数以二进制的格式来输出，我之前在 **[C/C++:如何将一个十进制数转换为一个二进制数?](https://www.techforgeek.info/decimal_to_bin.html)** 一文中解释了如何在C语言中使用 **除2取余法** 来将一个整数转换为二进制格式，以及如何在C++中使用 `bitset` 来将一个整数转换为二进制格式，本文将继续介绍另外几种将一个整数转换为二进制格式的方法。                 
         
推荐阅读: [C/C++:如何将一个十进制数转换为一个二进制数?](https://www.techforgeek.info/decimal_to_bin.html)

## C/C++            

### 方法一

该方法是定义下面的两个宏:          

```cpp
#define BYTE_TO_BINARY_PATTERN "%c%c%c%c%c%c%c%c"
#define BYTE_TO_BINARY(byte) \
	(byte & 0x80 ? '1' : '0'), \
	(byte & 0x40 ? '1' : '0'), \
	(byte & 0x20 ? '1' : '0'), \
	(byte & 0x10 ? '1' : '0'), \
	(byte & 0x08 ? '1' : '0'), \
	(byte & 0x04 ? '1' : '0'), \
	(byte & 0x02 ? '1' : '0'), \
	(byte & 0x01 ? '1' : '0')
```
#### 单字节整数
对于单字节整数（例如 `char` 类型的整数），上面的两个宏可以直接使用，下面是将一个单字节整数以二进制格式输出的一个例子：          

```cpp
#include<stdio.h>
#define BYTE_TO_BINARY_PATTERN "%c%c%c%c%c%c%c%c"
#define BYTE_TO_BINARY(byte) \
	(byte & 0x80 ? '1' : '0'), \
	(byte & 0x40 ? '1' : '0'), \
	(byte & 0x20 ? '1' : '0'), \
	(byte & 0x10 ? '1' : '0'), \
	(byte & 0x08 ? '1' : '0'), \
	(byte & 0x04 ? '1' : '0'), \
	(byte & 0x02 ? '1' : '0'), \
	(byte & 0x01 ? '1' : '0')

int main()
{
	char num = 10;
	printf("Binary of num: "BYTE_TO_BINARY_PATTERN, BYTE_TO_BINARY(num));
	puts("");
	return 0;
}
```
**输出:**            

```
Binary of num: 00001010
```

#### 多字节整数

对于多字节整数，你仍然可以使用上面定义的两个宏，不过在 `printf()` 函数中要使用移位操作，下面通过例子来说明这一点。              
           
假设现在你想将一个 `int` 类型的整数以二进制格式输出:             

```cpp
#include<stdio.h>
#define BYTE_TO_BINARY_PATTERN "%c%c%c%c%c%c%c%c"
#define BYTE_TO_BINARY(byte) \
	(byte & 0x80 ? '1' : '0'), \
	(byte & 0x40 ? '1' : '0'), \
	(byte & 0x20 ? '1' : '0'), \
	(byte & 0x10 ? '1' : '0'), \
	(byte & 0x08 ? '1' : '0'), \
	(byte & 0x04 ? '1' : '0'), \
	(byte & 0x02 ? '1' : '0'), \
	(byte & 0x01 ? '1' : '0')

int main()
{
	int num = 10;
	printf("Binary of num: "BYTE_TO_BINARY_PATTERN" "\
			BYTE_TO_BINARY_PATTERN" "BYTE_TO_BINARY_PATTERN" "\
			BYTE_TO_BINARY_PATTERN,BYTE_TO_BINARY(num>>24),\
			BYTE_TO_BINARY(num>>16), BYTE_TO_BINARY(num>>8),\
			BYTE_TO_BINARY(num));
	puts("");
	return 0;
}
```
**输出:**            

```
Binary of num: 00000000 00000000 00000000 00001010
```

### 方法二
下面的方法是定义一个函数，在函数中将一个整数以二进制的格式输出，函数如下:        

```cpp
// 假定数据以小端序(little-endian)格式存储
void printBits(size_t const size, void const *ptr)
{
	unsigned char *b = (unsigned char*)ptr;
	unsigned char byte;
	int i, j;

	for(i = size-1; i >= 0; --i)
	{
		for(j=7; j >= 0; --j)
		{
			byte = (b[i]>>j) & 1;
			printf("%u", byte);
		}
	}
	
	puts("");
}
```

现在多数系统都是采用 **小端序(Little-endian)** 来存储数据的，关于**大端序(big-endian)** 和**小端序(little-endian)** 的区别，可以参见 **[维基百科](https://en.wikipedia.org/wiki/Endianness)**。           

这种方法对任意类型的整数都适用，无论是单字节的整数还是多字节的整数,下面是使用该函数的一个例子:           

```cpp
#include<stdio.h>

void printBits(size_t const size, void const *ptr)
{
	unsigned char *b = (unsigned char*)ptr;
	unsigned char byte;
	int i, j;

	for(i = size-1; i >= 0; --i)
	{
		for(j=7; j >= 0; --j)
		{
			byte = (b[i]>>j) & 1;
			printf("%u", byte);
		}
	}
	
	puts("");
}

int main()
{
	int i = 23;
	unsigned int ui = 0xffffffff;
	float f = 23.45f;

	printBits(sizeof(i), &i);
	printBits(sizeof(ui), &ui);
	printBits(sizeof(f), &f);

	return 0;
}
```
**输出:**            
       
```
00000000000000000000000000010111      
11111111111111111111111111111111        
01000001101110111001100110011010         
```

## Python

在Python中将一个整数以二进制格式输出就十分简单了，因为Python已经提供了一个这样的函数 —— `bin()`, 下面是一个使用该函数的例子:            
        
```python
>>> num = 10
>>> bin(10)
'0b1010'
```

## Bash Shell
要在Bash Shell中将一个整数转换为二进制格式，我们可以借助 `bc` 这个命令行工具，该工具定义有一个特殊的变量 `obase`, 下面是 `bc` 的man手册中关于该变量的一段描述:           

> There are four special variables, scale, ibase, obase, and last.  scale          
> defines  how  some  operations use digits after the decimal point.  The      
> default value of scale is 0. ibase and obase define the conversion base    
> for input and output numbers.  The default for both input and output is     
> base 10.  last (an extension) is a variable that has the value  of  the     
> last  printed  number.     

从中我们可以了解到 `obase` 定义了 `bc` 中数据的输出格式, 我们可以借助 `bc` 的这个特性来将一个十进制整数转换为二进制，下面是一个例子:          

```bash
$ num=10
$ echo "obase=2; $num" | bc
```
**输出:**            

```bash
1010
```

## 结束语               
在编写程序时，有时候我们需要将一个十进制整数转换为二进制的格式来输出，而这样的转换往往需要一些技巧（尤其是在C/C++中），这篇文章总结了一些在 **C/C++**, **Python** 和 **Bash Shell** 中将十进制整数转换为二进制格式来输出的方法，如果你需要在程序中需要将一个十进制数转换为二进制来输出，希望这篇文章可以帮助到你。                
