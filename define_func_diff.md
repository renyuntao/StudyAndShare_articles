# C++ vs Python:函数定义的区别
2015-10-25 <br />     
&nbsp;&nbsp;&nbsp;C++和Python的函数定义的区别除了Python的函数定义不用声明返回值，而C++的函数定义必须声明返回值类型这一明显的区别外，另一个重要的区别是**在C++中，函数体内所用的到变量必须在使用之前声明过;而Python的函数体中，使用某一个变量时，该变量不一定已经声明过，函数体中的变量只要在调用该函数的位置之前有定义即可**,下面来看一个例子:      
#### C++:

    void compare(int a)
    {
    	if(a > N)
    		cout<<"a > N"<<endl;
    	else
    		cout<<"a <= N"<<endl;
    }
#### Python:

    def compare(a):
    	if a > N:
    		print('a > N')
    	else:
    		print('a <= N')
&nbsp;&nbsp;&nbsp;上面使用C++和Python写的两个功能相同的函数，但是C++所写的那个函数却是错误的，该函数会在编译时报错，因为使用了未定义的变量`N`;与C++不同，虽然Python所写的那个函数同样使用了未定义的变量`N`,但是这样的函数定义在Python中却是正确的，Python只要求在调用该函数的位置之前有变量`N`的定义即可，如下:

    #some code
    N = 5        #在compare函数调用之前定义变量`N`即可
    compare(8)   #调用compare函数
<br />    
一如既往，如果你对文章中的内容有任何疑问，或者是发现文章中有任何错误，都可以通过下面的地址发邮件告诉我.     
E-mail: rytubuntulinux@gmail.com   <br /><br />    

