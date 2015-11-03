#C++:Name Mangling与函数重载
2015-09-18  <br /><br />  
&nbsp;&nbsp;&nbsp;C语言中不允许函数重载，一个函数名只能对应一个函数;而在C++中，因为允许函数重载，一个函数名可能对应多个函数，但是一些低级语言(如C和汇编语言)和工具(如链接器)无法理解函数重载，它们认为一个函数名只能对应一个函数，因此需要有一个方法来区分各个函数。现在的解决方法是在编译程序的时候，编译器进行一个**`Name Mangling`**的过程，为重载的各个函数生成一个唯一的名称,这样在之后的链接过程中就不会因为连接器无法理解而报错了。下面是一个简单的例子:

    #include<iostream>
    void foo(int a)
    {
    	return 0;
    }
    void foo(int a,int b)
    {
    	return 0;
    }
    int main()
    {
    
    }

从上面的程序可以看出，foo函数进行了重载;假设上面的程序保存在test.cxx文件中,下面进行编译:  

    $ g++ -c test.cxx  
    $ nm test.o
                      U __cxa_atexit
                      U __dso_handle
    0000005d t _GLOBAL__sub_I__Z3fooi
    00000014 T main
    00000000 T _Z3fooi
    0000000a T _Z3fooii
    0000001e t _Z41__static_initialization_and_destruction_0ii
                      U _ZNSt8ios_base4InitC1Ev
                      U _ZNSt8ios_base4InitD1Ev
    00000000 b _ZStL8__ioinit

注:**`nm`**是一个可以查看目标文件中的符号名称的小程序(**`objdump`**同样可以完成这个任务).  
&nbsp;&nbsp;&nbsp;上面的输出中，**`_Z3fooi`**和**`_Z3fooii`**就是编译器为重载的foo函数生成了两个名称，用以区分两个函数。由于**`Name Mangling`**是编译器的行为，而且这个过程没有一个标准，所以不同的编译器输出的结果也会不同。  
******************  
##拓展
在C++中，使用`<typeinfo>`头文件中的`name()`,该函数返回的结果也是经过了`Name Mangling`的过程，如下:  

    #include<iostream>
    #include<typeinfo>
    int main()
    {
        int in; 
        char ca; 
        double dou;
        char ch[10];
        char *ptr;
        std::cout<<"in is:"<<typeid(in).name()<<"\n";
        std::cout<<"ca is:"<<typeid(ca).name()<<"\n";
        std::cout<<"dou is:"<<typeid(dou).name()<<"\n";
        std::cout<<"ch is:"<<typeid(ch).name()<<"\n";
        std::cout<<"ptr is:"<<typeid(ptr).name()<<"\n";
        return 0;
    }
假设上述程序保存在test.cxx文件中，下面编译并执行程序:  

    $ g++ test.cxx && ./a.out
    in is:i
    ca is:c
    dou is:d
    ch is:a10_c
    ptr is:pi

由于上面的程序的输出结果是经过`name mangling`的，所以结果不容易理解此时,可以利用命令行程序`c++filt`来对这些`mangled name`进行`demangle`,如下:  

    $ ./a.out | c++filt  
    in is:int
    ca is:char
    dou is:double
    ch is:char [10]
    ptr is:char*

经过`c++filt`处理之后就可以清楚地看出各个变量的类型了。  <br /><br />
一如既往，如果你对文章中的内容有任何疑问，或者是发现文章中有任何错误，都可以通过下面的地址发邮件告诉我。  
E-mail: rytubuntulinux@gmail.com <br /><br /><br />

