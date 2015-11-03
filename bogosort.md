# Bogosort
2015-10-15 <br />  
**Bogosort**是一种效率很低下的排序方法(它是我所见到过的排序方法中效率最为低下的一种排序方法),它的时间平均时间复杂度为**O(n!)**,正因为如此，**Bogosort**并未在实际生产中被使用.尽管如此，还是会有一些地方会用到**Bogosort**的，[维基百科](https://en.wikipedia.org/wiki/Bogosort)上列出了几处用到**Bogosort**的地方，感兴趣的话可以查看那里.          
## Bogosort的排序原理
**Bogosort**的排序原理很简单，只是简单的随机打乱数组中元素的顺序，直到数组中的元素已序,可以用下面的伪代码来表示:   

    while not InOrder(list)
    do
    	Shuffle(list)
    done
## Bogosort的实现
下面是我用Python对**Bogosort**的实现:


    #!/usr/bin/env python
    import random
    def inOrder(ls):
    	length = len(ls)
    	if length == 0:
    		return True
    	for i in range(length-1):
    		if ls[i] > ls[i+1]:
    			return False
    	return True
    
    ls = [1,0,-1,20,7,15,23]
    while not inOrder(ls):
    	random.shuffle(ls)
    print(ls)
<br />   
一如既往，如果你对文章中的内容有任何疑问，或者是发现文章中有任何错误，都可以通过下面的地址发邮件告诉我.   
E-mail: rytubuntulinux@gmail.com <br /><br />  

