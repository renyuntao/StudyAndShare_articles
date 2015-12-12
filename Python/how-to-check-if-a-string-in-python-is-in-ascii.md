# 如何检验一个字符串中是否都是ASCII字符?
2015-12-11  <br />           
## Python
### 使用内置函数 all
Python中的内置函数`all`可以检查一个可迭代对象中的所有元素是否都为True或者都满足某一条件，如果是的话，`all()`就返回True;否
则返回False。此外，Python中的另一个内置函数`ord()`可以将一个ASCII字符转换为一个整数，如果`ord()`接受的参数不是ASCII字符，
`ord()`就会抛出异常，因此可以结合使用`all()`和`ord()`来检测一个字符串中的字符是否都为ASCII字符,如下:       

    >>> str = 'ab¥'
    >>> all(ord(c) < 128 for c in str)
    False
    >>> str = 'abc'
    >>> all(ord(c) < 128 for c in str)
    True
### 使用Python异常机制
在Python中，字符串类型的对象都会有一个`encode()`成员函数，这个成员函数会按照参数指定的编码方式对字符串进行编码，如果不能够
编码成功，就会抛出`UnicodeEncodeError`的异常。当使用ASCII的编码方式对非ASCII字符进行编码时，就一定会编码失败，从而抛出异常，因此可以使用这个特点来检测一个字符串中是否含有非ASCII字符,如下:           

    str = 'ab¥'
    try:
    	str.encode('ascii')
    except UnicodeEncodeError:
    	print('Not a ascii-encode unicode string')
    else:
    	print('It is a ascii-encode unicode string')
## C/C++
char类型本来就是一种整数类型，而ASCII表中所有可打印字符的范围是[32,127],因此可以通过检查字符是否在[32,127]这个整数范围内，
来判断该字符是否为ASCII中的可打印字符。           
在C++中可以使用string类型来保存字符串，C中使用字符数组即可，如下:          

	std::string str = "abc¥";
	int len = str.length();
	for(int i = 0; i < len; ++i)
	{
		if(str[i] > 32 and str[i] < 128)
		{
			cout<<"is ascii\n";
		}
		else
		{
			cout<<"is not ascii\n";
			break;
		}
	}
**注意:** 在Python中不能使用这种方法,因为Python中只有字符串类型，而没有和C/C++中类似的char类型，不能够和int类型进行比较.           
