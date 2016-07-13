# AWK: 消除文件中的重复行               
<!--2016-03-23 --> <br /><br />               
 
&nbsp;&nbsp;&nbsp;&nbsp;在对文件中的数据进行分析时，消除文件中的重复行是一个很常用的操作， 在Linux系统中，完成这个任务的最简单的方法就是使用 **[awk](https://en.wikipedia.org/wiki/AWK)** 了，下面将演示如何使用 **awk** 来去除文件中的重复行。        
假设现在有一个文本文件 `example.txt`,其中的内容为:                 
                  
```
one
one
two
two
three
three
```
上述文件内容包含了一些重复的行，使用 awk 消除该文件中的重复行，只要执行如下语句即可:                
            
```bash
$ awk '!seen[$0]++' example.txt
one
two
three
```
从上面的输出结果中可以看到，我们已经成功的消除了重复的行。              
&nbsp;&nbsp;&nbsp;&nbsp;上述语句中的 `$0` 表示的当前要处理的那一行的内容，`seen` 是一个关联数组, `awk` 每处理一行内容，都会对 `seen` 关联数组中的某个元素的值进行加一操作，如果某一行内容在之前从未出现过，则 `seen[$0]` 的值为0, 此时 `!seen[$0]` 即为 True, 那么就会打印该行。简单地说，该语句就是告诉 `awk`，对于文件 `example.txt` 中的每一行内容，如果该行内容之前没有出现过，则打印该行;如果出现过，则不做处理。     
                   
如果觉得这个 awk 语句难以理解，可以使用下面的更为清楚的表示方法:               
            
```bash
$ awk '{if(!seen[$0]) print $0; ++seen[$0]}' example.txt
one
two
three
```

<!--
Reference:
https://stackoverflow.com/questions/11532157/unix-removing-duplicate-lines-without-sorting/
-->
