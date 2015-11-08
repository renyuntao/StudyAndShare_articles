# malloc() vs new
2015-10-31 <br />     
下面是一些`malloc()`和`new`的区别:          
<br />
**1)** `malloc()`是函数，而`new`是操作符。         

----
<br />
**2)** 使用`new`时可以调用构造函数进行初始化，而使用`malloc()`时则不会，看下面的例子:    

    struct Foo
    {
    	Foo():weight(50) {}
    	int weight;
    };
    
    int main()
    {
    	Foo *useNew = new Foo;
    	Foo *useMalloc = (Foo*)malloc(sizeof(Foo));
    	cout<<*useNew<<endl;      //输出50
    	cout<<*useMalloc<<endl;   //输出0
    	return 0;
    }
对于基本数据类型(int,float,char等)，也可以使用`new`操作符来进行初始化，如下:      

    int main()
    {
    	int *p = new int(10);
    	cout<<*p<<endl;    //输出10
    	return 0;
    }

----
<br />
**3)** new返回确定类型的指针，而malloc()返回void*,又由于C++中有如下规定:    

>“一般情况下，对指针赋值时，等号两边的指针类型应该相等。”    

因此，如果在C++程序中使用malloc()时，应该显式地将malloc()返回的void*转换为相应的指针类型，下面的例子说明了这一点:   

    int main()
    {
    	int *p = (int*)malloc(sizeof(int));   //显示地将void*转换为int*
    	free(p);
    	return 0;
    }
<br />   
一如既往，如果你对文章中的内容有任何疑问，或者是发现文章中有任何错误，都可以通过下面的地址发邮件告诉我.    
E-mail: rytubuntulinux@gmail.com     <br /><br />      

