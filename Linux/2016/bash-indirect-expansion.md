# Bash: Indirect Expansion                  
2016-03-27   <br /><br />                 
                
先来看一个例子:               
              
```bash
$ VALUE=value
$ FOO=VALUE
```              
上面的片段中定义了两个变量 `VALUE` 和 `FOO`,现在想要通过变量 `FOO` 来获取变量 `VALUE` 的值，该如何做呢？我们知道要获取变量 `FOO` 的值可以直接使用如下语句:               
         
```bash
$ echo "$FOO"
VALUE
```
那么要通过变量 `FOO` 来获取变量 `VALUE` 的值是不是要使用如下语句:                
              
```bash
$ echo "$$FOO"
```
如果你尝试执行上面的语句的话，会发现并不能输出预期的结果，而是输出了 **_PID_FOO**, 其中 **_PID_** 为你当前shell的进程ID.                  
要完成上述任务，就要用到Bash中所谓的 **Indirect Expansion** 特性了，`man bash` 你会看到下面的一段话:               
           
> If the first character of parameter is an exclamation point (!), a level of variable indirection is introduced. Bash uses the value of the variable formed from the rest of parameter as the name of the variable; this variable is then expanded and that value is used in the rest of the substitution, rather than the value of parameter itself. This is known as indirect expansion.              
             
简单的说，如果在Bash中使用了感叹号，Bash就会将感叹号后面的变量的值作为一个变量名来处理，这个变量名随后也会拓展为该变量的值。                 
下面使用Bash的 **Indirect Expansion** 特性来完成上面的任务:             
           
```bash 
$ VALUE=value
$ FOO=VALUE
$ echo "${!FOO}"
value
```
从输出结果中可以看出，我们得到了想要的内容.                     
            
## 缺点
**Indirect Expansion** 只是Bash Shell的一个特性，其它的Shell中并没有该特性，因此，如果你使用的不是Bash Shell,就不能够使用上面介绍的 **Indirect Expansion** 特性，不过你可以使用下面的语句来代替:            
         
```bash
$ VALUE=value
$ FOO=VALUE
$ eval "echo \$${FOO}"
value
```
上面的语句在所有类型的Shell中都可以正常使用。            

