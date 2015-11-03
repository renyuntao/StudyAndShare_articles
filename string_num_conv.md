# C++:字符串与数字之间的转换
2015-10-15 <br />   
## 将字符串转换为数字
### C风格的转换
可以使用**`<stdlib.h>`**头文件中的如下几个函数:   

- atoi()    将字符串转化为int型
- atol()    将字符串转换为long型
- atoll()   将字符串转换为long long型
- atoq()    同atoll()

下面以函数**`atoi()`**来举一个例子:

    string str = "45";
    int val = atoi(str.c_str());   //val = 45
因为上面所述的几个函数都是接受C风格的字符串，因此**string**类型的对象要使用**c_str()**成员函数来做一下转换.   
此外，还可以使用**`<stdlib.h>`**头文件中的如下函数:   

- strtol()
- strtoll()   

这两个函数的特点是它们可以根据根据指定的进制数来将字符串转换为整型十进制数，下面以**strtol()**来举一个例子:

    string str = "45 1101 A";
    char *end;
    long int a = strtol(str.c_str(),&end,10);     // a = 45
    long int b = strtol(end,&end,2);            // b = 13
    long int c = strtol(end,NULL,16);        // c = 10
上面程序中**strtol()**函数中的参数**10**,**2**,**16**分别表示字符串为10进制，2进制和16进制。   
### C++风格的转换
在**C++11**中，有多个函数可以将一个字符串转换为数字，下面简单的列出几个:   

- stoi()     将字符串转换为int型
- stol()     将字符串转换为long int型
- stof()     将字符串转换为float型

以**stoi()**函数来举一个例子:   

    string str1 = "45";
    string str2 = "A";
    int a = stoi(str1,nullptr,10);     // a = 45
    int b = stoi(str2,nullptr,16);     // b = 10
**stoi()**函数可以像**strtol()**函数一样，指定字符串的进制数。   
*************
## 将数字转换为字符串
### stringstream
在**`<sstream>`**头文件中包含有一个**stringstream**类，使用该类对象读取一个数字，之后再使用该类对象的**str()**函数将读取的数字转换为字符串即可，示例如下:

    int a = 100;
    stringstream ss;
    ss<<a;
    cout<<ss.str()<<endl;     //输出100
### std::to_string()  (C++11)
除了上面的方法外，C++11还增加了一个函数**to_string()**,该函数可以接受`int`,`long`,`double`,`float`等类型的数值，然后将他们转换为字符串，示例如下:

    int a = 100;
    float b = 20.4;
    cout<<std::to_string(a)<<endl;     //输出100
    cout<<std::to_string(b)<<endl;     //输出25.400000
<br />    
## 使用 sprintf
另外一种将数字转换为字符串的方法是使用`<stdio.h>`头文件中的`sprintf()`函数，示例如下:   

    char arr[10] = {0,};
    int a = 100;
    sprintf(arr,"%d",a);
    cout<<arr<<endl;      //输出100
一如既往，如果你对文章中的内容有任何疑问，或者是发现文章中有任何错误，都可以通过下面的地址发邮件告诉我。   
E-mail: rytubuntulinux@gmail.com   <br /><br />    

