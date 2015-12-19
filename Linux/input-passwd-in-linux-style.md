# Linux风格的密码输入方式        
2015-12-14      <br />            
&nbsp;&nbsp;&nbsp;在Linux中，当要求你在终端输入密码的时候，你所输入的密码在终端上不会有任何显示，Linux的这一风格常常使刚
学Linux的新手误以为自己的键盘坏了 :) 。不过Linux的这一密码输入风格确实可以提高安全性，自己在写程序时，如果要求用户输入密
码，该如何实现Linux风格的密码输入方式呢?下面将分享一些方法.       
## C
`<unistd.h>`头文件(这是Linux系统特有的一个头文件，Windows系统中并不存在)中有一个名为`getpass()`的函数，该函数可以实现Linux
风格的密码输入方式。该函数的函数原型如下:          

    char *getpass( const char *prompt);
下面是一个示例程序:            

    #include<unistd.h>
    #include<stdio.h>
    
    int main(void)
    {
    	char *passwd;
    	passwd = getpass("Password:");
    	printf("passwd:%s\n",passwd);
    	return 0;
    }

----------
## Shell Script       
如果使用Shell Script来编写程序的话，可以轻松的实现Linux风格的密码输入方式,示例如下:         

    echo -n "Password:"
    read -s passwd
    echo $passwd
上述程序的关键在与`read`的`-s`参数，该参数可以使输入的密码不再终端显示.                    

----------
## Python
与C类似,Python中也有一个名为`getpass()`的函数，该函数位于`getpass`模块中，其用法也与C中的`getpass()`函数类似，下面是一个
示例:         

    >>> import getpass
    >>> passwd = getpass.getpass('Password:')
    >>> print(passwd)
