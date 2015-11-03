#C++:从类类型向数据类型转换
2015-09-24 <br /><br />
&nbsp;&nbsp;&nbsp;之前写了一篇文章[C++:explicit与=delete](http://www.studyandshare.info/explicit_delete.html),其中介绍了使用单参构造函数将从一个数据类型向类类型做转换;其实这种转换操作也可以逆向来进行，即将一个类类型向一个数据类型来做转换，此时就需要用到C++中所谓的`转换函数`了，`转换函数`是一个形如`operator TypeName();`形式的函数，`TypeName`即为你想要将类类型转换到的数据类型，下面来看一个例子.

    #include<iostream>
    using namespace std;
    class Test
    {
    	private:
    		int a;
    	public:
    		Test():a(10) {}
    		operator int()
    		{
    			return int(a);
    		}
    
    };
    int main()
    {
    	Test t;
    	int foo = t;
		int foo2 = int(t);
    	cout<<"foo:"<<foo<<endl;
    	cout<<"foo2:"<<foo2<<endl;
    	return 0;
    }
假设上面的程序保存在`test.cxx`文件中:  

    $ g++ test.cxx && ./a.out
    foo:10
	foo2:10
由输出可见，类对象`t`确实被转换为了`int`类型。   
&nbsp;&nbsp;&nbsp;**说明:**定义好转换函数后，当编译器发现`int foo = t;`这样的语句，即等号左边是一个数据类型，而等号右边是一个类类型，会自动在类中寻找相应的转换函数，如果没有找到，就会报错;如果找到了，就将类类型转换为相应的数据类型。同时，用户也可以手动将类强制转换为相应的数据类型，例如`foo2 = int(t);`这样的语句手动将类类型转换为相应的数据类型,尽管是手动进行的强制类型转换，但这种强制转换操作仍是调用的转换函数来完成的。  <br /><hr />
##注意事项
在使用转换函数时，也是有一定的限制条件的，如下:
 
- 转换函数必须是类方法  
- 转换函数不能指定返回类型  
- 转换函数不能有参数  

<hr />
##替换的方法
&nbsp;&nbsp;&nbsp;上面程序中的`int foo = t;`语句，其转换操作是自动的、隐式的，因此它可能会带来一个问题，比如在不希望进行转换操作时，转换函数也可能会自动、隐式的进行这种转换操作。解决这个问题的一个方法是定义一个类函数来显示完成这样的转换操作，例如可以将上面程序中的转换函数替换为如下函数:

    int ToInt()
    {
    	return a;
    }
之后将语句

    int foo = t;
    int foo2 = int(t);
替换为

    int foo = t.ToInt();
    int foo2 = t.ToInt();
这样显示来完成转换操作看起来会更加清晰、明确。  <br /><br /><br />
一如既往，如果你对文章中的内容有任何疑问，或者是发现文章中有任何错误，都可以通过下面的地址发邮件告诉我.  
E-mail: rytubuntulinux@gmali.com<br /><br /><br />
