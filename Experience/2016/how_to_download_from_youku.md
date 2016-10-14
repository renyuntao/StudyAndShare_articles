# 如何在Linux中下载优酷视频？             
<!--
2016-10-14
--><br /><br />
有时候，我们不能保证随时都能很方便地连接到网络，因此，对于一些喜欢的视频，我们可能希望在有网络连接时下载下来，之后在没有网络连接时，也可以在本地观看。在Windows平台下，可以通过优酷的客户端来下载视频; 由于没有Linux版的优酷客户端，因此，在Linux平台下，不能通过这种方法来下载视频。在这篇文章中，将介绍如何在Linux平台下使用 [youtube-dl][2] 来从优酷上下载视频。         

## 什么是 youtube-dl?       

youtube-dl 是一个托管在 GitHub 上的开源项目，正如其名字所暗示的那样(**YouTube downloader**), youtube-dl 的主要功能是从 YouTube 中下载视频，不过它也可以从其他的一些视频网站下载视频。       

## 如何使用 youtube-dl?

由于朝廷的封锁, 大多数的大陆人民是无法观看 YouTube 的，因此，很多大陆人民都是使用像优酷这样的国内的视频网站来观看视频，幸运的是，**youtube-dl** 同样支持下载优酷中的视频，下面将介绍如何使用 **youtube-dl** 在Linux平台中下载优酷中的视频。          

1. **下载 youtube-dl**      
 
	你可以在终端执行如下命令来下载安装 **youtube-dl**:        

	```bash
	$ sudo curl -L https://yt-dl.org/downloads/latest/youtube-dl -o /usr/local/bin/youtube-dl
	$ sudo chmod a+rx /usr/local/bin/youtube-dl
	```
2. **获取优酷视频的URL**          
   
	在优酷中播放一个你想要下载的视频，用鼠标右击视频页面，在弹出的菜单中选择 **复制视频地址**， 示例如下:             
	<br />
	![download video from youku][1]   
	<br />

3. **下载视频**             

	在终端执行如下命令即开始下载视频:             

	```bash
	$ youtube-dl <URL>          
	```
	上面命令中的 **\<URL>** 即为你在第2步中所获取的视频URL。              

## 结束语       

**youtube-dl** 的功能不仅仅是下载视频，它还有其它的很多功能，例如选择不同格式的视频下载，只下载音频文件等，这些功能你都可以通过查看 **youtube-dl** 的帮助来获取详细的使用方法:       

```bash
$ youtube-dl -h
```
或者是 [在 GitHub 中查看 youtube-dl 的具体用法][2]。          


[1]: https://c3.staticflickr.com/6/5785/30277096066_19abf4f492_z.jpg   
[2]: https://github.com/rg3/youtube-dl             
