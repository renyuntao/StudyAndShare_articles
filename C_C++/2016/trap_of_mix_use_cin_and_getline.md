# 从输入缓冲区读取内容时的一些陷阱
2016-05-22  <br /><br />

&nbsp;&nbsp;&nbsp;&nbsp;在 C 和 C++ 中，当使用 `cin`, `getline`, `scanf` 等从标准输入读取数据时，常常会产生一些令人困惑的行为，下面总结了一些从标准输入读取数据时遇到问题的例子，以及相应的解决方案。          
## C++

当使用 cin 读取了一个整数，紧接着又使用 getline() 来从 stdin 读取一个字符串，无论输入内容如何，这个字符串将读不到任何值:     
  
```cpp
int i; 
std::string str;
std::cin>>i;
getline(std::cin, str);
std::cout<<”i:”<<i<<”  str:”<<str<<”\n”;   // str 值总为空
```

使用 cin 读取一个整数后，紧接着使用 cin 读取一个字符，却可以正常工作:       

```cpp
int i; 
char c;  
std::cin>>i;
std::cin>>c; 
```

使用 getline() 读取一个字符串，紧接着使用 getline() 读取另外一个字符串，也可以正常工作:         
     
```cpp
std::string str1;
std::string str2;
getline(std::cin, str1);
getline(std::cin, str2);
```

上述问题的原因在于，使用 cin 读取内容的时候，cin 会将换行符 '\n' 留在输入缓冲区，而 cin 本身在从输入缓冲区中读取内容的时候，会忽略空白字符(如空格，换行符等)，因此，如下语句可以正常工作:      

```cpp
int i;
char c; 
std::cin>>i;
std::cin>>c;
```

但是，两个参数的 getline() 是以换行符为分隔符的，当使用 cin 读取了一个整数后，会在输入缓冲区中留下一个换行符，接下来在调用两个参数的 getline() 读取时，遇到上次 cin 留下来的换行符，即停止读取，因此下面的程序中，无论输入什么内容，调用 getline() 后，字符串变量的值都为空:     

```cpp
int i;  
std::string str;
std::cin>>i;
getline(std::cin, str);
```

解决方法是在使用 cin 读取完整数后，调用 `std::cin.ignore()` 来除去输入缓冲区中的换行符:

```cpp
int i;  
std::string str;
std::cin>>i;
std::cin.ignore();
getline(std::cin, str);
```
 
而在使用 getline() 读取字符串时，会把分隔符(如换行符)从输入缓冲区中取出并丢掉，因此，在连续两次调用 getline() 时，第一次调用 getline() 并不会对第二次调用 getline() 造成影响, 故下面的程序会正常工作:           

```cpp
std::string str1;
std::string str2;
getline(std::cin, str1);
getline(std::cin, str2);
```

## C

&nbsp;&nbsp;&nbsp;对于 C 语言中的 scanf() 函数，其特点与 cin 有些不同，当scanf() 使用 %d 来从输入缓冲区中读取整数的时候， scanf() 会忽略掉空白字符(如空格符、换行符等)；而当 scanf() 使用 %c 来从输入缓冲区内读取字符的时候， scanf() 则不会忽略掉空白字符，这一点与 cin 不同，cin 对于输入缓冲区中的所有空白字符，都采取忽略的策略。因此，下面的程序将不能正常工作:          
 
```cpp
int i;
char c;
scanf(“%d”, &i);
scanf(“%c”, &c);
printf(“i: %d    c:%c \n”, i, c);   
```

上面的程序片段中，第一个 scanf() 从输入缓冲区中读取一个整数后，会留下一个换行符在输入缓冲区中，当第二个 scanf() 从输入缓冲区中读取字符的时候，变量 c 将会读取到换行符，因此变量 c 的值将总为一个换行符。

### 解决方法
#### 方法一: 
在第一个 scanf() 后面调用 getchar() 函数来去除掉第一个 scanf() 遗留在输入缓冲区中的换行符, 从而消除对第二个 scanf() 的影响:       

```cpp
int i;
char c;
scanf(“%d”, &i);
getchar();
scanf(“%c”, &c);
printf(“i: %d    c:%c \n”, i, c);   
```
#### 方法二:      

将语句

```cpp
scanf(“%c”, &c);
```

修改为

```cpp
scanf(“ %c”, &c);
```
即在 %c 前面加一个空格，这样 scanf() 函数就会忽略掉空白字符:    

```cpp
int i;
char c;
scanf(“%d”, &i);
scanf(“ %c”, &c);
printf(“i: %d    c:%c \n”, i, c);   
```

关于更多输入输出方面的内容，可以参考 **[这里](http://www.augustcouncil.com/~tgibson/tutorial/iotips.html)** 和 **[这里](https://gsamaras.wordpress.com/code/caution-when-reading-char-with-scanf-c/)**.
