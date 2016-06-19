# Linux:如何将WebP格式的图片转换为PNG格式?              
2016-03-18  <br /> <br />                 
         
&nbsp;&nbsp;&nbsp;&nbsp;**[WebP](https://en.wikipedia.org/wiki/WebP)** 是一种用于网页中的新型的图片格式，目前由 **Google** 开发。在同等 [SSIM](https://en.wikipedia.org/wiki/Structural_similarity) 质量下，**WebP** 格式的图片的大小要比PNG格式的图片小很多,因此 **Google** 正在推广使用这种格式的图片,现在 **Google+** 中的图片都是这种格式.            
                 
&nbsp;&nbsp;&nbsp;&nbsp;这种格式的图片虽然很好，但是有一个问题，就是现在很多图片查看器都不支持这种格式的图片，因此如果你从网页上下载了一个你喜欢的 **WebP** 格式的图片，很可能在本地无法使用图片查看器打开，因此就有必要对图片做一下格式的转换了。下面介绍如何在 **Linux** 平台上将这种新型的 **WebP** 格式的图片转换为 **PNG** 格式的图片.                
                 
1. 下载相关软件包
	首先打开 [这个](https://storage.googleapis.com/downloads.webmproject.org/releases/webp/index.html) 链接，之后会进入到一个下载页面，根据你所使用的系统来选择不同的软件包下载(这里以Linux系统为例)                      
	**注意:** 打开上述链接需要翻墙，一个简单的翻墙方法可以参见下面这篇文章:                 
	[一个简单的访问 Google 的方法](https://www.studyandshare.info/modify_dns_antigfw.html)                 
2. 解压文件
	在64位Linux系统中，下载的软件包为 **libwebp-0.4.1-linux-x86-64.tar.gz**, 执行如下命令解压:                   
             
	```bash
	$ tar -zxvf libwebp-0.4.1-linux-x86-64.tar.gz
	```
	之后得到一个名为 **libwebp-0.4.1-linux-x86-64** 的目录                       
3. 使用解压工具                
	进入到目录 **libwebp-0.4.1-linux-x86-64** 下的 **bin** 目录中，你会看到两个文件: **cwebp** 和 **dwebp**.**cwebp** 用来生成 **WebP** 格式的文件,而 **dwebp** 则用来将 **WebP** 格式的文件转换为 **PNG** 格式的文件。假如现在你有一个名为 **example.webp** 的文件，你想要将该文件转换为PNG格式的文件，并将其命名为 **example.png**, 可以执行如下命令:           
            
	```bash
	$ dwebp /path/to/example.webp -o example.png
	```
          
关于 **cwebp** 和 **dwebp**还有很多用法，你可以通过 **libwebp-0.4.1-linux-x86-64** 目录下的 **README** 文件来了解更多相关内容.              
         
## 在线转换器              
如果你嫌上面的方法麻烦，可以使用 **在线图片转换器**,点击下面的链接使用 **在线图片转换器**:                   
[Online image converter](http://image.online-convert.com/)             
使用在线转换器，你只要上传想要转换的文件，等转换完成后,将转换好的文件下载下来即可。             
该方法的缺点是只有在有网络连接的情况下你才能进行图片格式的转换。                 

