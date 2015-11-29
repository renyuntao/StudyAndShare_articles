# 如何在Linux平台下生成随机文本?            
2015-11-28    <br />          
&nbsp;&nbsp;&nbsp;有时候我们会需要一些随机的文本作为测试数据，比如写了一个排序的程序，想要测试这个程序对文本的排序能力。我们
可以自己来写一个程序来产生随机文本，但是如果是在Linux平台下，有一个更加方便快捷的方法来产生随机文本，下面将分享这种方法.       
&nbsp;&nbsp;&nbsp;这里所用的方法是使用Linux中的特殊文件`/dev/random`或者是`/dev/urandom`,这两个特殊文件都可以用来产生随机数，
但是稍有区别，你可以参考[维基百科](https://en.wikipedia.org/wiki//dev/random)来了解二者的区别，这里只是介绍如何使用它们来产生
随机数.         
你可以直接执行如下命令来产生随机数:          

    $ cat /dev/urandom | head -c 1000 > example
&nbsp;&nbsp;&nbsp;这样文件example中就包含了由`/dev/urandom`特殊文件所产生的1000个字节的随机数了。不过，当你打开example文件时，
会发现都是乱码。这是怎么回事呢？来执行下面的命令查看:           

    $ file example
    example: data
&nbsp;&nbsp;&nbsp;`file`是一个查看文件类型的命令，由上面的输出可以看到，example文件的类型是`data`,而`data`通常就是表示文件是二
进制文件或者表示文件内容不可打印，因此，直接有`/dev/urandom`产生的随机数是不可使用的，因此我们需要对产生的随机数再做进一步的处
理，此时就要用到另外一个工具了 —— `base64`,它同样是一个命令行工具，功能是对数据进行编码/解码，而无论数据是什么，只要经过
`base64`进行编码过后，都可以变成人类可读懂的文本串，所以我们可以通过`base64`来对`/dev/urandom`产生的随机数来进行编码，从而产
生可读懂的文本串，具体的方法如下:        

    $ base64 /dev/urandom | head -c 1000 > example
	$ file example
	example: ASCII text
此时example文件的类型就变成ASCII text了，这时再打开example文件就是普通的文本内容了,这样就成功生成了随机文本.           
