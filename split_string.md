# C++:如何分割字符串到数组中?
2015-10-17 <br />   
给出一个字符串，如`"1 2 3 4 5"`,如何将这个字符串中的数字分割开，并放到一个数组或者**vector**中？本文将讨论这个问题。    
## 使用 stringstream
**`<sstream>`**头文件中的`stringstream`类可以完成这个任务，该类的对象读取一个字符串(字符串各个部分由空格分隔)，然后再将读取的字符串由**stringstream**类对象传送到数组或者**vector**中即可，示例如下：    

    stringstream ss("1 2 3 4 5");
    int tmp;
    vector<int> v;
    while(cin>>tmp)        //转换为int类型
    	v.push_back(tmp);
    for(auto e:v)
    	cout<<e<<" ";    //输出:1 2 3 4 5 
    cout<<endl;
<hr />
## 使用 strtok
除了使用`stringstream`类以外，还可以使用C头文件**`<string.h>`**中的`strtok()`函数，此函数可以接受一个字符指针和一个分隔符集(用于划定字符串中各部分的界限)作为参数，来看下面的例子:   

    string str("1 2 3 4 5");
    vector<int> v;
    // c_str()函数返回const char*,因此使用const_cast来去掉const属性
    char *p = strtok(const_cast<char*>(str.c_str())," ");
    while(p != NULL)
    {
    	v.push_back(atoi(p));
    	p = strtok(NULL," ");
    }
    for(auto e:v)
    	cout<<e<<" ";      //输出:1 2 3 4 5
    cout<<endl;
<br />    
## 使用 string::find()
另外一种分割字符串的方法就是使用`string`类的`find()`成员函数，用该函数找到分隔符的位置，之后再使用`string`类的另一个成员函数`substr()`来提取字符串中的各个部分。使用这种方法的函数如下:    

    void split(const std::string &s,char delimiter,std::vector<std::string> &v)
    {
    	std::string::size_type i = 0;
    	std::string::size_type j = s.find(delimiter);
    
    	while(j != std::string::npos)
    	{
    		v.push_back(s.substr(i,j-i));
    		i = ++j;
    		j = s.find(delimiter,i);
    	}
    	if(j == std::string::npos)
    		v.push_back(s.substr(i,s.length()));
    }
下面是一个使用该函数的例子:    

    string str = "Hello everone,my name is Rodion.";
    vector<string> v;
    split(str,' ',v);     //以空格作为分隔符
    for(auto e:v)
    	cout<<e<<" ";    //Output: Hello everyone,my name is Rodion.
    cout<<endl;
一如既往，如果你对文章中的内容有任何疑问，或者是发现文章中有任何错误，都可以通过下面的地址发邮件告诉我。    
E-mail: rytubuntulinux@gmail.com  <br /><br />    

