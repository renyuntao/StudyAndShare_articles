# 将Bing主页的背景图片设置为Ubuntu的桌面壁纸               
<!--
2016-09-24      
--> <br /><br />

**[Bing][1]** 虽然不是最好用的搜索引擎，但是对于 Bing 有一点我特别喜欢，那就是 Bing 每天都会在其主页展示一张高质量的、优美的图片。对于喜欢 Bing 主页的优美图片的人来说，可能会想要将 Bing 主页的背景图片设置为自己的桌面壁纸（例如我就是这样），这需要你手动下载 Bing 主页的背景图片，并将下载的图片手动设置为桌面的壁纸。           

然而，对于 Linux 用户而言，你不必每天都进行这样重复的操作。我为 Ubuntu 用户写了一个小程序 —— **[bingWallpaper][2]**, 这是一个可以运行在 Ubuntu 中的命令行工具，每天你只要执行一次该命令，就可以将当天 Bing 主页的背景图片设置为的你的桌面壁纸了。             

<center>        
![bingWallpaper][3]             
</center>          

你可以点击下面的按钮来从 [GitHub][2] 上下载该程序:              

<center><a class="button" href="https://github.com/renyuntao/bingWallpaper/archive/v1.0.tar.gz">点 击 下 载 bingWallpaper</a></center>          

下载完成之后，你会得到一个名为 **bingWallpaper-1.0.tar.gz** 的压缩包，执行如下命令对其解压:             

```bash
$ tar -xzvf bingWallpaper-1.0.tar.gz
```
解压完成之后，会得到一个名为 **bingWallpaper-1.0** 的目录，进入该目录，并执行 **make** 命令进行安装操作:         

```bash
$ cd bingWallpaper-1.0           
$ make
```
做完上面的操作并确保网络可用之后，就可以在终端执行如下命令来将你的 Ubuntu 桌面壁纸设置为 Bing 的背景图片了:             

```bash
$ bingWallpaper        
```

关于该命令行工具的更多信息，你可以查看[GitHub 中的相应页面][2]。              

_注: 我是在 Ubuntu 14.04 中测试的这个程序，但是对于最近的 Ubuntu 系统（如 Ubuntu 14.10, Ubuntu 16.04）应该都是适用的，如果你发现这个程序在你的 Ubuntu 系统中不能正常使用，可以发邮件告诉我。_           


[1]: http://www.bing.com         
[2]: https://github.com/renyuntao/bingWallpaper
[3]: https://c2.staticflickr.com/6/5018/29811077961_f4d2b983b3_z.jpg
