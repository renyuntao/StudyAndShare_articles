# Linux:如何将一个TXT文件转换为PDF文件?
2015-11-06 <br />     
&nbsp;&nbsp;&nbsp;我在[Linux:如何将一个PDF文件转换为TXT文件?](http://128.199.222.126/pdftotext.html)一文中介绍了在Linux平台下如何使用命令行工具将一个PDF文件转换为TXT文件，而做相反的操作，即将一个TXT文件转换为一个PDF文件，也是常常要进行的，甚至这种操作更加常用。因此，本文将讨论在Linux平台下如何将一个TXT文件转换为PDF文件，下面列出了几种转换的方法.      
## 使用LibreOffice
&nbsp;&nbsp;&nbsp;这种方法可能是进行PDF到TXT转换的最简单的一种方法了。**LibreOffice**这款软件在Ubuntu中是自带的软件，在**LibreOffice**中编辑好文本后，点击**LibreOffice**上面的一个按钮即可生成对应的PDF文件，这个按钮的位置很明显，你可以自己找找看。     

******
## 使用命令行方式
作为一名Linux用户，你可能更加钟爱于使用命令行的方式来解决问题(我本人即是如此)，下面来介绍如何使用命令行的方式来完成PDF到TXT文件的转换.       
要完成这个任务，你必须要安装两个必备的软件包,在Ubuntu或Debian系统下，你可以使用下面的命令来安装:      

     $ sudo apt-get update && sudo apt-get install enscript ghostscript -y
安装完毕后，下面的两步操作将会完成TXT到PDF文件的转换:        
首先，将文本文件转换为[Post Script](https://en.wikipedia.org/wiki/PostScript)格式，这个任务可由**enscript**命令来完成:      

     $ enscript -p output.ps input.txt
最后，将上面生成的postscript文件转换为PDF文件.      

     $ ps2pdf output.ps output.pdf
由上面的说明可见，要将TXT文件转换为PDF文件，有一个中间过程，即首先要将TXT文件转换为Post Script格式，之后再将生成的postscript文件转换为PDF格式。   

****
**使用enscript的缺点:** enscript这个命令行工具只能够将英文字符转换为Post Script格式，但是对于像汉语这样的文字就无能为力了.     

****
&nbsp;&nbsp;&nbsp;为了克服**enscript**的不足，下面介绍另外一个命令行工具**paps**,它可以读取UTF-8编码的文件，并生成对应的Post Script格式的文件，这样就解决了**enscript**无法转换中文的问题了。在Ubuntu中，你可以使用下面的命令来安装**paps**:     

$ sudo apt-get update && sudo apt-get install paps -y     
现在假设你的当前目录下有一个包含中文的文件example.txt,下面的命令将会生成这个文件对应的Post Script格式的文件example.ps:    

    $ paps example.txt > example.ps
之后，就可以将这个Post Script格式的文件转换为PDF文件了:     

    $ ps2pdf example.ps example.pdf
