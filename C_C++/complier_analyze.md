# 一个需要注意的C++编译器分析机制
2015-11-01  <br />       
在说这个问题前，先来看三个等价的函数声明:     

- int foo(double d);
- int foo(double (d));
- int foo(double);

第一种声明方法是最基本也最常见的函数声明方法;第二种方法中在参数d两边加上了小括号，这是多余的，但却是合法的;最后一种声明方法是省略了参数名称，这也是常见的一种声明方法，它也是合法的。       
下面在来看三个等价的函数声明，该函数的参数是一个指向不带任何参数的函数的指针(即函数指针)，如下:     

- int func(double (*pf)());
- int func(double pf());
- int func(double ());

第三种方法中同样是省略了参数名称。       
现在假设有一个文件，其中包含了一些整数，如果想要把这个文件中的整数保存到一个vector中，你可能会这样做:      

    ifstream fin("num.txt");
    vector<int> v(istream_iterator<int>(fin),
    			  istream_iterator<int>());
上述程序在编译时能够通过，但事实上却什么都没做,这是因为C++编译器将上述程序的第二条语句视为一条函数声明，依据上面看到的6条函数声明方法，C++认为这条语句:     

    vector<int> v(istream_iterator<int>(fin),
    			  istream_iterator<int>());
该语句声明了一个函数，函数名为v，该函数的参数有两个，第一个函数参数的类型是istream\_iterator<int>,参数名称是fin;第二个函数参数是一个函数指针，它指向一个不带任何参数，并且返回一个istream_iterator<int>的函数，并且该参数省略的参数名称。      
这样我们就明白了为什么C++编译器会将上述语句视为一条函数名称了,解决这个问题有如下两种方法:     
**1)** 为参数加上小括号
因为为参数加上小括号是合法的，所以我们可以用下面的方法来绕过C++编译器的这种分析机制:     

    vector<int> v((istream_iterator<int>(fin)),      
    			  (istream_iterator<int>()));        //为参数加上小括号
但是这种方法并不是在所有的编译器上都可行的，一些编译器被接受上面形式的语句，不过我用的g++还是能够正常编译通过的.       
**2)** 一种更好更通用的方法就是避免使用匿名的istream_iterator对象，像下面这样做:     

    ifstream fin("num.txt");
    istream_iterator<int> begin(fin);
    istream_iterator<int> end;
    vector<int> v(begin,end);
<br />      
