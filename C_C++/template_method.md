# 模板类成员函数在何处实现?
2015-10-23 <br />   
&nbsp;&nbsp;&nbsp;模板类成员函数的实现通常与模板累的声明放在同一个头文件下，这与普通类有些不同。模板类这样做的原因是每当在实例化一个模板的时候，编译器都会依据给予的模板参数来创建一个新类，例如:     

    template<typename T>
    struct Test
    {
    	T val;
    	void foo(T param)  {/* do something using T */}
    };
之后若在某个`*.cpp`文件中有如下语句:   

    Test<int> a;
则当编译器读到该行语句时，它会创建一个类似下面这样的类:    

    struct TestInt
    {
    	int val;
    	void foo(int param)  {/* do someting using int */}
    };
&nbsp;&nbsp;&nbsp;因此，编译器需要能够访问类的成员函数的具体实现，然后用给定的模板参数(该例中为int)来实例化它。如果头文件中没有包含模板类成员函数的实现，则编译器就不能访问到类成员函数的实现，也就不能实例化它们了。    
## 将声明与实现分开的办法
虽然通常情况下是将类模板成员函数的声明和实现放在同一个头文件中，但还是有其它方法来将实现和声明分开的，下面有两种方法:     
### 使用include语句
&nbsp;&nbsp;&nbsp;这个方法是在**\*.h**文件中声明模板类，之后在**\*.tpp**文件中试下吗模板类成员函数，然后再在声明模板类的**\*.h**头文件的末尾`include`包含模板类成员函数实现的**\*.tpp**文件。示例如下：    

    //test.h
    template<typename T>
    struct Test
    {
    	T val;
    	void foo(T param);
    };
    
    //test.tpp
    template<typename T>
    void Test<T>::foo(T param)
    {
    	//implementation
    }
这样，实现和声明就分开了，但是编译器仍然可以访问到模板类成员函数的实现.     
<hr />
### 明确实例化所有需要的模板实例
这个方法仍然可以将声明和实现分开，但是有一个缺点，就是你只能使用明确实例化的模板实例.示例如下:    

    //test.h
    template<typename T>
    struct Test {...}     //没有实现
    
    //test.cpp
    
    ...     //实现Test的成员函数     
    
    //明确实例化
    template class Test<int>;
    template class Tstt<char>;
    //你只能够使用int和char类型的Test
[GitHub](https://github.com/renyuntao/implement_template)有这三种方法的使用的例子，具体的使用可以查看那里.    
<br />     
一如既往，如果你对文章中的内容有任何疑问，或者是发现文章中有任何错误，都可以通过下面的地址发邮件告诉我.    
E-mail: rytubuntulinux@gmail.com    
<br /><br />
