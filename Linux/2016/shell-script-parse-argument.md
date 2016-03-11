# Shell Script:如何解析命令行参数?         
2016-03-09    <br />               
           
当你希望以类似下面的格式执行的你当Shell脚本时:                
       
$ **cmd** **-x** **-f** _arg_ filename.txt            
           
你就需要对命令行参数进行解析了，不过我们不必自己动手来解析这些命令行参数，有一个名为 **getopts** 的工具可以帮助我们来解析命令行参数，下面将分享该工具的一些简单用法。       
            
## 一些术语
对于下面的命令:              
           
$ **cmd** **-x** **-f** _arg_ filename.txt            
             
- **cmd** 即为所要执行的命令(你所写的Shell脚本)             
- **-x** 为一个 **选项**      
- **-f** 与 **-x** 类似，同样为一个 **选项**,不同的是该选项还有一个参数,即 *arg*                 
- **filename.txt** 是剩余的参数，它不与任何选项有联系                    
             
## 使用 getopts              
在Shell脚本中使用 **getopts** 的格式如下:              
           
```bash
while getopts OPTSTRING VARNAME
do
	...
done
```
当我们使用 **getopts** 时，我们需要首先告诉 **getopts** 要解析的选项有哪些,这就是上面的 **OPTSTRING**,而解析的选项名称则会保存在 **VARNAME** 中。                     
            
还以下述命令为例:            
           
$ **cmd** **-x** **-f** _arg_ filename.txt            
           
我们要解析的选项是 **-x** 和 **-f**,因此就应该使用 **"xf:"** 来表示 **OPTSTRING**,注意 **"xf:"** 中ｆ后面还有一个冒号 **:** ,这是在告诉 **getopts** f选项后面要跟一个参数.而解析到 **-f** 选项时，**-f** 选项后面的参数会保存到一个名为 **OPTARG** 的特殊变量中去.                
             
下面是一个例子:            
        
```bash
while getopts "xf:" opt
do
	case $opt in
		x)
			echo "x option is triggered"
			;;
		f)
			echo "f option is triggered, and it's argument is $OPTARG"
			;;
		\?)
			;;
	esac
done
```
假设上述脚本放在文件 **example.sh** 文件中:            
         
```bash
$ chmod u+x example.sh
$ ./example.sh -x -f example.txt filename.txt   # 运行脚本
x option is triggered
f option is triggered, and it's argument is example.txt            
```
可以看出，**getopts** 正确解析了命令行参数。         
            
本文只是简单介绍了一下 **getopts** 的用法，关于 **getopts** 的详细用法，你可以参见下面的内容:                  
[Small getopts tutorial](http://wiki.bash-hackers.org/howto/getopts_tutorial)                   
               
## 相关内容                
在 **C/C++** 程序中解析命令行参数:                 
[Example of Parsing Arguments with getopt](http://www.gnu.org/software/libc/manual/html_node/Example-of-Getopt.html)                
              
在 **Python** 程序中解析命令行参数:               
[Python文档](https://docs.python.org/3.4/library/argparse.html)                     
