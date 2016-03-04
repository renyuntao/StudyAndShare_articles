# Python:如何接受命令行参数?             
2016-03-04   <br />           
     
在一些情况下，我们需要向Python程序中传递一些命令行参数，下面将分享几种向Python程序中传递命令行参数的方法。　　　
     
## 使用 sys.argv
在 **C/C++** 中，**main()** 函数有两个参数 ——　**argc** 和 **argv** ,其中 **argv** 中保存了传递给 **C/C++** 程序的命令行参数。在Python中，同样有一个 **argv** 用来接受命令行参数，这个 **argv** 位于模块 **sys** 中。
        
下面举例来说明其用法:             
        
```python
from sys import argv

filename, x, y = argv
sum_ = int(x) + int(y)
print('sum: ',sum_)
```
上面这个程序用来计算两个整数的和，这两个整数从命令行参数中获取。与 **C/C++** 中的 **argv** 相同，从命令行中接受的参数被视为字符串，因此在对两个数相加之前，首先要将它们转换为整数（这里用 `int()` 来进行转换）.            
       
假设上述程序保存在文件 `example.py` 文件中，下面运行这个程序:            
      
```bash
$ python example.py 1 2
sum: 3
```           
## 使用Linux风格的方式
当从命令行接受的参数比较复杂时，**sys.argv** 就难以胜任了，此时可以采用Linux风格的方式来从命令行接受参数，即按照如下格式从命令行中接受参数:    
     
**program** **-option1** _arg1_ **-option2** _arg2_ ...             
      
这时就需要解析命令行中的选项和参数了，而 **argparse** 模块则可以完成这个任务.    
  
使用 **argparse** 解析参数时，首先要创建一个用于解析参数的类对象,之后将要解析的参数告诉这个类对象即可对命令行中的参数进行解析。    
    
由于这个模块用法十分多，这里不再赘述，你可以查看 [Python文档](https://docs.python.org/3/howto/argparse.html) 来了解该模块的用法.        
　

