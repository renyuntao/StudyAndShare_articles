# AWK:一些使用规范                 
2016-03-18  <br /><br />              
               
## awk中花括号的位置
在编写awk脚本时，BEGIN,END的左大括号必须与BEGIN,END位于同一行，否则会提示语法错误，而BEGIN，END的右大括号则要求并不严格，可以和BEGIN，END位于同一行，也可以独自位于一行。               
               
同样，在主输入循环中，左大括号必须与要匹配的模式位于同一行，否则会提示语法错误;而右大括号则要求并不严格，可以随意放置，并且其后也可以在接其他内容。   
**与sed进行比较:** sed中，在右大括号所在行，有大括号后面不能在跟后其他内容。       
            
## 主输入(main input)循环中的大括号               
主输入循环中的大括号只是括住要执行的语句，对于其他部分则不用括住,否则会提示语法错误。                 
示例如下:            

**YES:**           

```awk
BEGIN { 
    print("There are two files!") 
} 
    FILENAME == "test" { 
        print($1 * $1) 
    }   
    FILENAME == "haha"{ 
        print("I'm",$1) 
    }   
END{ 

    print("end") 
}
```
**NO:**                  
           
```awk
BEGIN { 
    print("There are two files!") 
} 
{
    FILENAME == "test" { 
        print($1 * $1) 
    }   
    FILENAME == "haha"{ 
        print("I'm",$1) 
    }   
}
END{ 

    print("end") 
} 
```

## 在Shell中使用awk              
在shell脚本中使用awk与在命令行中使用awk类似，示例如下:            
          
```bash
$ cat test.sh
echo $1 | awk ' 
BEGIN{ 
	print("In shell script") 
} 
{ 
    print 
} 
END{ 
    print("end") 
}'
```
## 使用split函数产生的数组的索引从1开始            
awk中有一个函数——split()，该函数产生的数组的索引由1开始，而不是通常的程序设计语言中的由0开始。示例如下:               
            
```bash           
$ cat test.sh
echo $1 | awk ' 
BEGIN{ 
    split("I,II,III,IV,V,VI,VII,VIII,IX,X",arr,",") 
} 
$1 > 0 && $1 <= 10 { 
    print("ok") 
    print(arr[$1]) 
    exit 
} 
{ 
    print("invalid number") 
    exit 
}'
```
 
## 命令行参数数组           
命令行参数数组的索引从0开始，这与通常的awk数组不同，而与C语言中的数组相同。示例如下:             
            
```bash
$ cat awksrc
BEGIN{
	for(x=0;x<ARGC;++x)
		print(ARGV[x])
	print(ARGC)
}
$ awk -f awksrc hello world
awk
hello
world
3
```             
如果实在Shell脚本中，可以像下面这样做:               
         
```bash
$ cat awksrc.sh
awk '
BEGIN{
	for(x=0;x<	ARGC;++x)
		print(ARGV[x])
	print(ARGC)
}'  $*
$ chmod u+x awksrc.sh && ./awksrc.sh hello world
awk
hello
world
3
```             
         
## index(s,t) 函数              
awk中的index函数返回子串t在字符串s中的位置。           
**注意:** 这里的位置从1开始计算，而不是通常的程序语言中的从0开始计算
            
## **1** 的作用             
假设现在有一个 **CSV** 文件，名为 **example.csv**,其内容如下:             
          
```
0003812,3,2
0000808,0,0
0003346,1,0
0018003,8,1
0044477,4,1
0197183,0,0
```
现在执行如下命令:           
         
```awk
$ awk '1' example.csv
0003812,3,2
0000808,0,0
0003346,1,0
0018003,8,1
0044477,4,1
0197183,0,0
```
上述语句输出了文件 **example.csv** 中的内容，这是因为在 **AWK** 中，**1** 等同于如下语句:           
           
```awk
{print $0}
```
又因为 `$0` 是 `print` 的默认值，所以又等同于              
          
```awk
{print}
```
