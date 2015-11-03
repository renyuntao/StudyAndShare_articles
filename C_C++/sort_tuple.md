# C++:对tuple进行排序
2015-10-21 <br />     
&nbsp;&nbsp;&nbsp;之前曾写过一篇文章 —— [Python:排序操作](http://128.199.222.126/python_sort.html),其中讨论了如何在Python中对元组进行排序.[tuple](http://www.cplusplus.com/reference/tuple/tuple/?kw=tuple)对C++来说是一个新的特性，它在C++11中被加入。本文将讨论如何在C++中对一组**tuple**进行排序.     
作为例子，现在假定**tuple**中有两个元素，类型分别为`int`类型和`string`类型，对**tuple**进行排序时，将根据每一个**tuple**的第二个元素(即`string`类型的元素)来进行排序，示例如下:     

    std::vector<std::tuple<int,std::string>> v;
    v.push_back(std::make_tuple(0,"h"));
    v.push_back(std::make_tuple(1,"e"));
    v.push_back(std::make_tuple(2,"l"));
    v.push_back(std::make_tuple(3,"l"));
    v.push_back(std::make_tuple(4,"o"));
    v.push_back(std::make_tuple(5," "));
    v.push_back(std::make_tuple(6,"w"));
    v.push_back(std::make_tuple(7,"o"));
    v.push_back(std::make_tuple(8,"r"));
    v.push_back(std::make_tuple(9,"l"));
    v.push_back(std::make_tuple(10,"d"));
    sort(v.begin(),v.end(),
    [](std::tuple<int,std::string> a,std::tuple<int,std::string> b) -> bool 
    {return (std::get<1>(a) < std::get<1>(b));});
    for(auto e:v) cout<<std::get<0>(e)<<"  "<<std::get<1>(e)<<endl;
**程序输出如下:**    

    5  
    10 d
    1 e
    0 h
    2 l
    3 l
    9 l
    4 o
    7 o
    8 r
    6 w
**注:**上述程序中的**lambda表达式**中的参数如果是按引用传递，则参数必须声明为`const`，因为你不能改变传入的数据。故应写为如下形式:        
`[](const std::tuple<int,std::string> &a,const std::tuple<int,std::string> &b)`        
&nbsp;&nbsp;&nbsp;上面的程序将`tuple`存储在`vector`中，之后调用标准库中的`sort()`函数，并使用[lambda表达式](http://en.cppreference.com/w/cpp/language/lambda)为`sort`函数创建排序准则,该例子中，**lambda表达式**将`tuple`的第二个元素提取出来作为排序数据，并按照升序的顺序来排序.      
<br />    
一如既往，如果你对文章中的内容有任何疑问，或者是发现文章中有任何错误，都可以通过下面的地址发邮件告诉我.    
E-mail: rytubuntulinux@gmail.com <br /><br />    
   
