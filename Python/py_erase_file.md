#Python:如何清空一个文件中的内容?
2015-10-09 <br />   
这里有两种方法清空一个文件的内容，第一种方法可以使用在还没有建立文件对象的情况下，第二种方法可以使用在已经建立了文件对象的情况下，下面一一来讨论这两种情况.    
##尚未建立文件对象
在尚未建立文件对象时，可以使用如下语句来清空一个文件中的内容:  

    >>> open('file.txt','w').close()
上面的语句中，首先会执行**`open('file.txt','w')`**语句来创建一个文件对象，因为采用**w(rite)**模式打开文件，所以会首先清空文件中的内容，此后再通过新建的文件对象调用**`close()`**来终止与文件的连接,这样就完成了清空文件内容的操作。     
##已建立文件对象
当已存在一个文件对象，**且这个文件对象可以对文件进行写(Write)操作(注意不是追加(append)操作)**,则可以通过如下语句来清空一个文件的内容:   

    >>> f = open('file.txt','r+')
    >>> f.truncate()
当一个文件对象可以对文件进行写(Write)操作时,则可以使用文件对象的成员函数**truncate()**来清空一个文件.      
<br /><br />      
一如既往，如果你对文章中的内容有任何疑问，或者是发现文章中有任何错误，都可以通过下面的地址发邮件告诉我.   
E-mail: rytubuntulinux@gmail.com  <br /><br />   

