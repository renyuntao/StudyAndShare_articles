# 使用GCC创建静态、动态链接库             
2015-05-31 <br /><br />              
            
## 静态链接库
### 创建一个新的静态链接库
创建静态连接库其实就是将程序的目标文件打包到一个后缀为.a的文件中(也就是静态库)，这个任务可由 `ar` 这个归档器程序来完成。
示例如下:

**foo.h:**          

```cpp
//******* foo.h *******
void func1();
```

**foo.c:**

```cpp
#include<stdio.h>
#include"foo.h"

void func1()
{
	printf("In func1()...\n");
}
```

执行如下命令:

```bash
$ gcc -c foo.c    # 之后会得到一个名为foo.o的目标文件
$ ar rcs libfoo.a foo.o    # 归档器以目标文件作为输入,生成静态库
```
之后就得到了静态连接库libfoo.a(关于ar的选项含义可以参阅man手册)             
调用：            

**main.c:**         
```cpp
#include"foo.h"

int main()
{
	func1();
	return 0;
}
```

在终端执行下面的命令来生成可执行文件并执行:            

```cpp
$ gcc main.c -lfoo -L . -static -o main          //假定静态链接库与call.c位于同一目录下,使用-static选项阻止
$ ./main                                        //使用动态库，但在某些系统下，该选项不起作用
In func1()...
```
### 向一个已存在的静态链接库中添加文件
现在新建了两个文件 —— foo1.h 和 foo1.c:             
           
**foo1.h:**            

```cpp
void func2();
```
**foo1.c:**

**foo1.c:**       
```cpp
#include<stdio.h>
#include"foo1.h"

void func2()
{
	printf("In func2()...\n");
}
```
现在想要在上面生成的静态链接库 `libfoo.a` 中调用 `func2()` 函数，这就需要将 `foo1.c` 所对应的目标文件添加到 `libfoo.a` 中去，具体做法如下:         

```bash
$ gcc -c foo1.c  # 产生一个名为 foo1.o 的目标文件
$ ar rs libfoo.a foo1.o   # 将 foo1.o 添加到 libfoo.a 中
```
之后就可以在 `libfoo.a` 中调用 `func2()` 函数了，下面在做个测试。           
首先修改 `main.c`,在其中添加对 `func2()` 函数的调用，修改后的 `main.c` 文件如下:             
  
```cpp
#include"foo.h"
#include"foo1.h"

int main()
{
	func1();
	func2();
	return 0;
}
```
之后在终端执行如下命令来生成可执行文件并执行:             
        
```bash
$ gcc main.c -lfoo -L . -static -o main
$ ./main
In func1()...
In func2()...
```

## 动态链接库
创建动态链接库其实是将程序的目标文件打包到一个后缀为.so的文件(即动态链接库)中，但是此过程中不需要ar程序,因为ar是一个归档器,而该过程需要的是一个链接器,链接器以目标文件或库作为输入,生成动态库或可执行文件.
示例如下：

**foo.h:**        

```cpp
void func1();
```
**foo.c:**          

```cpp
#include"foo.h"

void func1()
{
	printf("In func1() ...\n");
}
```
接下来在终端执行如下命令:            

```bash
$ gcc -fpic -c site.c   // 之后会产生一个名为site.o的目标文件
$ gcc site.o -shared -o libsite.so  // 使用 -shared 选项告知链接器创建动态链接库,而不是可执行文件
```
如果执行上述命令报错,可以尝试将'-fpic'改成'-fPIC',因为这两个选项是基于特定类型的机器的
关于 `-fpic` 的作用，可以参考下面的链接:                  
[GCC -fPIC option](http://stackoverflow.com/questions/5311515/gcc-fpic-option)                

执行完上述的命令后即可得到动态链接库libsite.so
下面在 `main()` 函数中调用动态链接库中的 `func1()` 函数:           
**main.c:**             

```cpp
#include"foo.h"
int main()
{
	func1();
	return 0;
}
```

最后，执行如下命令生成可执行文件并执行:           

```bash
$ gcc main.c -lfoo -L. -o main
$ ./main
```
但是程序并不会正确执行，而是会产生如下错误:            

> ./main: error while loading shared libraries: libfoo.so: cannot open shared object file: No such file or directory

这是因为我们并没有将动态链接库放到标准的位置（动态链接库的标准位置是 `/usr/lib/` 或 `/usr/local/lib/`),不过我们可以通过环境变量 `LD_LIBRARY_PATH`　或 `rpath` 来解决这个问题,下面来一一介绍这两种方法。                       
### 使用 LD_LIBRARY_PATH
通过设置环境变量 `LD_LIBRARY_PATH` 的值，可以告诉加载器(**loader**)我们所要使用的动态链接库的位置，假设动态链接库在 `/home/ryt/example/` 目录下，则可以用下面的方法设置 `LD_LIBRARY_PATH`:            
        
```bash
$ export LD_LIBRARY_PATH=/home/ryt/example/:$LD_LIBRARY_PATH
``
之后再次运行程序:             
        
```bash
$ ./main
In func1() ...
```
可以看到，这次程序可以正确执行了。               

### 使用 rpath
`rpath(run path)` 是 GCC 的一个命令行选项，使用它可以在命令行中指定动态链接库的位置，用法如下(仍然假设动态链接库位于 `/home/ryt/example/` 目录下):           
         
```bash
$ unset LD_LIBRARY_PATH  # 消除 LD_LIBRARY_PATH 变量的影响
$ gcc main.c -lfoo -L. -Wl,-rpath=/home/ryt/example/ -o main
$ ./main
In func1() ...
```

### 将动态链接库放到标准位置         
除了上面所说的两种方法外，还可以将生成的动态链接库放到标准位置(即 `/usr/lib/` 或 `/usr/local/lib/` 目录下),这样，无需额外的指示，即可找到所需的动态链接库。这个过程包含如下两个步骤:          

1. 将动态链接库 `libfoo.so` 放到标准位置
	
	```bash
	$ sudo cp -v ./libfoo.so /usr/lib/  
	```
2. 更新缓冲区        

	```bash
	$ sudo ldconfig
	```
下面重新生成可执行文件:             

```bash
$ gcc main.c -lfoo -L . -o main
```
使用 `ldd` 命令对生成的可执行文件做一个检测,看是否找到了动态链接库的位置:           
      
```bash
$ ldd main | grep 'foo'
libfoo.so => /usr/lib/libfoo.so (0x00007fcafcded000)
```
一切正常，下面来运行这个程序:            
       
```bash
$ ./main
In func1() ...
```
可以看到程序也正常运行。        

<!--

Shared libraries with GCC on Linux
http://www.cprogramming.com/tutorial/shared-libraries-linux-gcc.html#fn:3

-->
