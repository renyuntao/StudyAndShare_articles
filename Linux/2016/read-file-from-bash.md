# Bash:如何一行一行地从文件中读取内容              
2016-04-07   <br /><br />                
            
假设现在有一个文本文件 `example.txt`,其内容如下:                 
            
```
one 
two
three
```
现在想要在 Shell 脚本中一行一行地读取该文件中的内容，可以像下面这样做:    
     
```bash
#!/bin/bash

while read -r line
do
	echo "$line"
done < "$1"
```
上面的程序中 `read` 命令的 `-r` 选项表示不转义任何字符，如果不指定该选项，如果文件 `example.txt` 中存在 `\\` 这样的字符串的话，那么 `read` 会将 `\\` 转义为 `\`.        
假设上面的 Shell 脚本保存在文件 `read.sh` 中，则可以按照下面的方式来读取文件 `example.txt` 中的内容:              
           
```bash
$ chmod u+x read.sh
$ ./read.sh example.txt
one
two
three
```              
从输出结果中，可以看到这个脚本已经实现了从文件中一行一行读取内容的功能。                  

