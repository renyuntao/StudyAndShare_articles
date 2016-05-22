# Python中的64(32)位整型数的最值                   
2016-05-16  <br /><br />

&nbsp;&nbsp;&nbsp;&nbsp;在 C/C++ 中，经常会使用整型数的最值来表示一些特殊的含义，比如在使用临接矩阵来表示图的时候，常常会用整型数的最大值 **INT_MAX** 或 **UINT_MAX** 来表示图中的两个结点之间的距离是无穷，即这两个结点之间没有边.其中，**INT_MAX** 和 **UINT_MAX** 是两个宏，定义在头文件 **<climits>** 或 **<limits.h>** 中。此外，该头文件中还定义有 **INT_MIN** 宏，用来表示有符号整型数的最小值。                                      

在 Python 该如何表示64(32)位整型数的最值呢?下面以 Python 2.x 和 Python 3.x 为例，分别介绍在 Python 中表示相应最值的方法。               

### Python 2.x              
在 Python 2.x 中，可以使用 `sys` 模块下的 `maxint` 来表示整型数的最大值，在表示整型数的最小值时可以使用 `-maxint-1` 来表示（查看 **[这里](https://docs.python.org/2/library/stdtypes.html#numeric-types-int-float-long-complex)**）。　　        
示例如下:                 

```python
>>> import sys
>>> sys.maxint
9223372036854775807
>>> -sys.maxint-1
-9223372036854775808
```
上面是在64位系统下的输出结果，如果是32为系统，输出结果会与上述结果不同，此时，最大值和最小值应该分别为 `2147483647` 和 `-2147483648`。      

### Python 3.x
在 Python 3.x 中，`sys` 模块下的 `maxint` 被移除了，因此在 Python 3.x 中，`sys.maxint` 已经不能再使用了，不过，你可以使用 `sys` 模块下的 `maxsize` 来代替 `maxint`,`maxsize` 可以实现和 `maxint` 相同的效果。                          
示例如下:             
           
```python
>>> import sys
>>> sys.maxsize
9223372036854775807
>>> -sys.maxsize-1
-9223372036854775808
```
同样，上面是64位系统下的输出结果, 如果是在32位系统下，输出的最大值和最小值应分别为 `2147483647` 和 `-2147483648`。       
