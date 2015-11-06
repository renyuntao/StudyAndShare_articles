# Linux:如何将PDF文件转换为TXT文件?   
2015-11-05  <br />     
&nbsp;&nbsp;&nbsp;在很多情况下，你想将一个PDF文件转换为一个可编辑的TXT文件，比如发现一个PDF文件中有一些错误，你想修改这个错误，此时就有将这个PDF文件转换为TXT文件了，本文将介绍如何使用Linux中的一个命令行工具 —— **pdftotext**,来将一个PDF文件转换为一个可编辑的TXT文件，通常Ubuntu中默认自带了该工具，你也可以输入下面的命令来检查一下:      

     $ dpkg -s poppler-utils
如果执行该命令后产生类似下面的输出，则表明你的系统中已经安装了**pdftotext**:       

    Package: poppler-utils
    Status: install ok installed
    Priority: optional
    Section: utils
    Installed-Size: 432
    Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>
	...
如果并没有产生类似上面的输出，你的Ubuntu系统中可能没有安装该软件，可以通过下面的命令来安装:     

    $ sudo apt-get update && sudo apt-get install poppler-utils -y
安装完成后，就可以继续往下看了。      

*****
## PDF格式转换为TXT格式
现在假设你当前所在目录下有一个PDF文件，比如example.pdf,如果想要将该文件转换为TXT文件，可以像下面这样做:    

    $ pdftotext example.pdf example.txt
上述命令执行后，当前目录下会多出一个example.txt文件，它就是由example.pdf这个PDF文件转换而来的TXT文件。         

*****
## 保持原本的布局
上述转换而来的TXT文件中的内容布局可能会与PDF文件中的内容布局有所不同，要想使转换来的TXT文件的内容布局与原本的PDF文件的内容布局相同，就要使用`-layout`参数了，如下:     

    $ pdftotext -layout example.pdf example.txt

*****
## 只转换部分PDF文件
如果你只想转换PDF文件中的某几页，可以使用`-f`参数来指定起始页，使用`-l`参数来指定终止页，看下面的例子:    

    $ pdftotext -f 10 -l 15 example.pdf example.txt
上述命令仅仅将example.pdf文件中的第10～15也的内容转换为了TXT文件

*****
## 控制结尾字符
&nbsp;&nbsp;&nbsp;由于在Linux、Mac、Windows这三个平台下，行结尾符是不同的，Linux平台下是**LF**(Line Feed),Mac平台下是**CR**(Carriage Return),Windows平台下是**CR\LF**,因此，在将PDF文件转换为TXT文件时，最好将行结尾符设置为与你所使用的系统相适应的行结尾符，设置行结尾符可以使用`-eof`参数，`-eol mac`,`-eol dos`,`-eol unix`分别对应设置Mac、Windows、Unix/Linux平台下的行结尾符。这里以Unix/Linux平台为例，将行结尾符设置为与Unix/Linux平台下的行结尾符相适应,如下:   

    $ pdftotext -eol unix example.pdf example.txt
**pdftotext**还有很多的功能，你可以同过**man pdftotext**来获取更过的信息。
