# Ubuntu 14.04中安装及配置新版本GCC             
2016-05-27   <br /><br />              
                 
## 为什么要安装新版本的GCC?           
Ubuntu 14.04 中，GCC 的默认版本为 `gcc-4.8`,该版本的 gcc 编译器对 C++11 的支持并不完备，例如，`g++-4.8` 对 C++11 引入的正则表达式并不能很好的支持(详细内容可以参见 **[这里](https://gcc.gnu.org/bugzilla/show_bug.cgi?id=53631)** 和 **[这里](http://stackoverflow.com/questions/12530406/is-gcc-4-8-or-earlier-buggy-about-regular-expressions)**)。因此，为了正常使用 C++11, 就需要使用新版本的 GCC 了。         
    
## 如何安装新版本的GCC?             
Ubuntu 14.04 软件仓库中，GCC 默认的版本只有 `gcc-4.8`, 因此要安装新版本的　GCC, 首先需要手动添加 **[PPA](https://en.wikipedia.org/wiki/Personal_Package_Archive)**:             
          
```bash
$ sudo add-apt-repository ppa:ubuntu-toolchain-r/test
```
然后进行更新:
        
```bash
$ sudo apt-get update
```
接下来就是安装 gcc 和 g++ 了，在写本文时，最新版本的 GCC 为 `gcc-6.1`, 因此下面来安装 `gcc-6.1` 和 `g++-6.1`:              
            
```bash
$ sudo apt-get install gcc-6.1 g++-6.1
```
接着就是一段时间的下载过程。      

## 配置 GCC-6.1 为默认 GCC 版本
在下载完毕后，你可以使用下面的命令来查看当前 GCC 版本:                
        
```bash
$ gcc --version
gcc-4.8 (Ubuntu 4.8.4-2ubuntu1~14.04.1) 4.8.4
Copyright (C) 2013 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```
可以看到当前 GCC 的版本仍然是 `gcc-4.8`。现在你的系统中有两个 GCC 版本 —— `gcc-4.8` 和 `gcc-6.1`,而默认版本是 `gcc-4.8`, 如果你想要使用 `gcc-6.1`, 就得在命令行中显式地指出要用 `gcc-6.1`:             
        
```bash
$ gcc-6 example.c
```
显然这样做很麻烦，我们更喜欢像下面这样使用 gcc:           
       
```bash
$ gcc example.c
```
这就需要将 GCC-6.1 配置为当前系统的默认 GCC 版本了，具体做法如下:       

1. 首先对 gcc 和 g++ 的候选设置做一些清除工作

	```bash
	$ sudo update-alternavies --remove-all gcc 
	$ sudo update-alternavies --remove-all g++
	```
	如果你的系统中本来就没有对 gcc 和 g++ 进行候选设置，那么上述两条命令会提示如下错误:              

	```bash
	update-alternatives: 错误: 无 gcc 的候选项
	update-alternatives: 错误: 无 g++ 的候选项
	```
	不过没有关系，你可以继续向下进行。                   
2. 对 gcc 和 g++ 进行候选设置               
        
	```bash
	$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 10
	$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-6.1 20

	$ sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.8 10
	$ sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-6.1 20

	$ sudo update-alternatives --install /usr/bin/cc cc /usr/bin/gcc 30
	$ sudo update-alternatives --set cc /usr/bin/gcc

	$ sudo update-alternatives --install /usr/bin/c++ c++ /usr/bin/g++ 30
	$ sudo update-alternatives --set c++ /usr/bin/g++
	```

设置完毕后，在执行如下命令查看当前系统中 GCC 的默认版本:          
       
```bash
$ gcc --version
gcc (Ubuntu 6.1.1-3ubuntu11~14.04.1) 6.1.1 20160511
Copyright (C) 2016 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```
可以看到，GCC 的默认版本已经变成了 `gcc-6.1`.

## 一个更加简单的配置方法         
如果感觉上面的方法太麻烦，下面就来介绍一种更加简单的方法，就是使用别名(**alias**),具体操作如下:              
           
1. 使用你喜欢的编辑器打开主目录下的文件 `.bashrc`,下面使用 `vim` 打开:               
	
	```bash
	$ vim ~/.bashrc
	```
2. 在文件末尾添加如下两条语句:               

	> alias gcc='/usr/bin/gcc'    
	>alias g++='/usr/bin/g++'	

3. 保存文件并退出
4. 执行下面的命令进行更新:               

	```bash
	$ . ~/.bashrc
	```
	或          

	```bash
	$ source ~/.bashrc
	```

接下来查看 GCC 的版本:         
        
```bash
$ gcc --version
gcc (Ubuntu 6.1.1-3ubuntu11~14.04.1) 6.1.1 20160511
Copyright (C) 2016 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```
可以看到，GCC 的版本同样变为了 `gcc-6.1`。                 

<!--
Reference: http://askubuntu.com/questions/26498/choose-gcc-and-g-version
-->
