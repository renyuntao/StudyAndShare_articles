# delete与free()的区别
2015-11-07 <br />    
这篇文章总结了一些free()和delete的区别，下面一一列出.       
**1)** **free()**是函数，而**delete**是操作符，这一点与**new**和**malloc()**的区别相同.    

****
**2)** 先来看看**delete**和**free()**的作用对象.    
在C++中，delete的左右对象有两个:      

1. 指向由new操作符分配的内存的指针
2. NULL指针

同样，free()的作用对象也有两个:     

1. 指向由malloc()函数分配的内存的指针
2. NULL指针

&nbsp;&nbsp;&nbsp;由此可见，delete操作符和free()函数的另一个区别是在排除NULL指针的情况下，delete操作符只能与new操作符对应使用，而free()函数也只能与malloc()函数对应使用，二者不能交叉使用。     
