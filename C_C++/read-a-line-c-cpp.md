# 从标准输入读取一行的几种方法
2016-01-03   <br />         
要从标准输入读取一行字符串，就不能使用 `cin` 或 `scanf()` 了，因为它们在遇到空格时就会停止读取，因此就要采取其他的方法，下面是我总结的在 C/C++ 中读取一行字符串的几种方法.      

## C
### 方法 #1:使用 fgetc     
`fgetc` 是位于头文件 `<stdio.h>` 中的一个函数，函数原型如下:      

> int fgetc(FILE *stream);

它每次读取输入流中的一个字符，并返回字符对应的 `int` 值，如果读取到输入流的尾部就返回 `EOF`，下面是使用该函数读取一行字符串的例子:         

    void readLine(char *line, int maxlen)
    {
    	int i = 0;
    	char c;
    	while(1)
    	{
    		c = fgetc(stdin);
    		if(c == EOF)
    			break;
    		line[i++] = c;
    		if(i > maxlen-1)
    			break;
    	}
    }
### 方法 #2:使用fgets
`fgets` 同样是位于头文件 `<stdio.h>` 中的一个函数，它的函数原型如下:        

> char *fgets(char *s, int size, FILE *stream);

该函数从流 _stream_ 中读取字符串，直到读取了 _size_ 个字符或者是遇到换行符，并将读取的字符串保存到 _s_ .如果遇到换行符，换行符也会保存到字符串的尾部.此外，终结符 `\0` 也会被添加到字符串的末尾。下面是使用该函数读取一行字符的例子:          

	char str[MAXLEN] = {0,};
	fgets(str,MAXLEN-1,stdin);
**注意:** 不要使用 `gets()` 函数，原因可以参见这篇文章: ['gets' is deprecated](http://www.studyandshare.info/gets_func.html).
### 方法 #3:使用 getline
`getline` 是位于头文件 `<stdio.h>` 中的一个函数，它应该是C中读取一行字符串的最佳选择了。`getline` 的函数原型如下:          
 
> ssize_t getline(char **lineptr, size_t *n, FILE *stream);

`getline` 从 _stream_ 中读取一行字符串保存在 _lineptr_ 所指的内存中;如果 _lineptr_ 为NULL，`getline` 会自动调用 `malloc` 来为 _lineptr_ 分配内存;如果 _lineptr_ 的空间不足以存储读取的字符串， `getline` 还会自动调用 `realloc` 来为 _lineptr_ 分配更大的内存.此外，换行符也会保存到字符串尾部，并以空字符 `\0` 结尾。下面是使用 `getline` 来读取一行字符串的例子:         

    int bytes_read;
    int nbytes = 100;
    char *my_string;
    
    puts ("Please enter a line of text.");
    
    /* These 2 lines are the heart of the program. */
    my_string = (char *) malloc (nbytes + 1);
    bytes_read = getline (&my_string, &nbytes, stdin);
    puts(my_string);
    free(my_string);

----------
## C++
在 C++ 中，可以使用头文件 `<string>` 中的 `getline` 来从标准输入读取一行字符， **注意此处的 `getline` 与 C 中的 `getline` 并不相同** ,它的函数原型如下:         

> istream& getline (istream& is, string& str); 

该函数从输入流 _is_ 中读取一行字符串，并将字符串保存到 `string` 类对象 `str` 中去，结尾的换行符并不保存到字符串末尾。该函数的一个使用例子如下:        

    std::string line;
    getline(cin,line);
    std::cout<<line;
