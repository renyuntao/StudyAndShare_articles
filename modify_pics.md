# Linux:如何在命令行界面下修改图片?
2015-10-26 <br />     
&nbsp;&nbsp;&nbsp;`ImageMagick`是一组可以修改、设置图片的命令行工具的集合，通过在终端`man imagemagick`就可以查看该集合中各个工具的功能，本文将讨论`ImageMagick`中的`convert`工具，该工具也是`ImageMagick`中最常用的的一个工具.      
## 安装ImageMagick
在Ubuntu中安装`ImageMagick`十分简单，使用如下命令即可:    

    $ sudo apt-get update && sudo apt-get install imagemagick

## 转换图片的格式
假如你当前所在目录下有一个文件名为`test.jpg`,你想改变该图片的格式为`PNG`格式，可以使用如下命令:      

    $ convert test.jpg test.png
转换后，原始图片保持不变，而你当前所在目录下会多出来一个名为`test.png`的图片文件，这就是转换得到的`PNG`图片。类似地，其他图片格式间的转换只要对上述命令稍作修改即可.       
## 改变图片的大小
`convert`命令可以快速的修改一张图片的大小，下面的命令将把图片的大小改变为200像素宽，100像素高:   

    $ convert test.png -resize 200x100 test.png
<hr />
**注意:**上述命令将会使修改后的图片覆盖原始图片，如果想要保留原始图片，可以将上述命令中的第二个`test.png`改为其他名字.   
<hr />
&nbsp;&nbsp;&nbsp;`convert`命令会尽量保持图片的宽高比例，因此，有时候上述命令并不会使图片修改为确切的200像素宽，100像素高，不过`convert`命令会使图片能够容纳在200x100的区域内,如果你想强制图片修改为指定的大小，只要在上述命令中加上感叹号即可，如下所示:   

    $ convert test.png -resize 200x100! test.png
除此之外，你也可以在修改图片大小的时候只指定宽度或者高度，`convert`命令会依据图片的宽高比例来修改图片的大小,如:   

    $ convert test.png -resize 200 test.png
或者是    

    $ convert test.png -resize x100 test.png
## 旋转图片    
`convert`命令还可以根据指定的角度来旋转图片，下面的命令将对图片test.png旋转90度，并将旋转后的图片保存为test-rotateed.png:    

    $ convert test.png -rotate 90 test-rotated.png
同样，如果你将旋转后的图片保存为与原始图片同名的文件，即test.png,则原始文件将会被旋转后的图片覆盖.      
<br /><br />      
一如既往，如果你对文章中的内容有任何疑问，或者是发现文章中有任何错误，都可以通过下面的地址发邮件告诉我.     
E-mail: rytubuntulinux@gmail.com <br /><br />    
  
