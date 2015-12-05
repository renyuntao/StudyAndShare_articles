# 'x < y < z' 在C和Python中的区别
2015-11-03 <br />     
&nbsp;&nbsp;&nbsp;像`x < y < z`这样的语句，无论是在C中还是在Python中，都是合法的语句，然而这样的一条语句在C和Python中却有着不
同的含义.             
你可能没有在C中见过像`x < y < z`这样语句，在C中通常是使用`x < y && y < z`这样的语句来表达数学中`x < y < z`这样的关系。这是因
为，`x < y < z`这样的语句虽然在C中是合法的，但是其在C中所表达的含义却和你想要表达的意思是不同的。对于`x < y < z`,C首先进行x和
y的比较操作，之后返回一个bool值，真为1,假则为0,接下来，C再用这个bool值(0或1)来和`z`进行比较。来看下面的例子:      

    int a = 3;
    int b = 2;
    int c = 1;
    if(a < b < c)
    	printf("'a < b < c' is true\n");
    else
    	printf("'a < b < c' is false\n");
输出:    

    'a < b < c' is true        
&nbsp;&nbsp;&nbsp;上面的程序中，a和b先进行比较，a等于3,b等于2,因此`a < b`返回false，即0,之后0再和c进行比较操作，因为c等于1,因
此`0 < c`返回true，故整个if条件表达式的结果为True。

---------
&nbsp;&nbsp;&nbsp;Python是一门更加接近于自然语言的程序设计语言，在Python中，`x < y < z`操作所表达的含义与我们通常所理解的含义
是相同的，即y大于x而小于z，下面来看一个例子:       

    a,b,c = 3,2,1
    if a < b < c:
    	print("'a < b < c' is true")
    else:
    	print("'a < b < c' is false")
输出:         

    'a < b < c' is false         
&nbsp;&nbsp;&nbsp;在Python中，`x < y and y < z`和`x < y < z`表达同样的含义，但是Python更加提倡使用`x < y < z`,因为这种形式的
操作中`y`的值只计算一次;而在`x < y and y < z`形式的操作中，`y`的值要计算两次,因此，`x < y < z`形式的操作更加高效.          
