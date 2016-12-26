# 如何在Linux平台下清理Kindle的目录?
<!--
2016-12-25
--><br /><br />

![Kindle][3]<br />

[Amazon Kindle][1] 是由Amazon设计和销售的电子书阅读器，其采用了电子墨水显示方式，最大程度地做到纸张阅读的效果，相比在手机和电脑上看电子书而言，在Kindle上即使长时间阅读也不易眼疲劳。对于喜欢阅读的人们来说，Kindle是一款很好的电子书阅读器。      

在 Kindle 中的每一本电子书，都会有一个对应的 \*.sdr 目录，该目录下记录了你在这本书中所做的笔记及阅读进度等信息。在某些情况下，即使你在 Kindle 中删除了一本书，这本书所对应的 \*.sdr 目录也不会被删除掉，这样，在使用了一段时间的 Kindle 之后，你可能会发现你的 Kindle 目录下有很多无用的 \*.sdr 子目录，这些子目录白白地浪费这你的 Kindle 的磁盘空间，而且使得你的 Kindle 目录下的内容显得十分的杂乱。    

我在使用了 Kindle 一段时间以后，就遇到了上面所描述的问题，为此，我写了一个 shell 脚本，可以用于清理 Kindle 目录下无用 \*.sdr 子目录，释放 Kindle 的磁盘空间，同时使得 Kindle 目录下的文件显得更加整洁。我已经将这个程序放到了 [GitHub][2] 上，你可以点击下面的按钮来从 [GitHub][2] 上下载该程序:    

											按钮 https://github.com/renyuntao/kindle-cleaner/archive/v1.0.tar.gz

下载完成后，你会得到一个名为 **kindle-cleaner-1.0.tar.gz** 的压缩文件，你可以通过执行下面的命令来解压该文件:   

```bash
$ tar -xvf kindle-cleaner-1.0.tar.gz
```
解压完成后，会得到一个名为 **kindle-cleaner-1.0** 的目录，进入该目录并运行 **install.sh** 脚本来安装 **kindle-cleaner**:    

```bash
$ cd kindle-cleaner-1.0
$ ./install.sh
```

安装完成后，使用 USB 线将 Kindle 与电脑连接，之后在终端执行下面的命令即可对 Kindle 下的目录进行清理:     

```bash
$ kindle-cleaner
```

关于 **kindle-cleaner** 的更多用法，你可以通过 **kindle-cleaner** 的 man 手册来查看:   

```bash
$ man kindle-cleaner
```


**注意:** 该程序我已经在 Ubuntu 16.04 上测试通过了，虽然是在 Ubuntu 中进行的测试，但是在大多数的 Linux 系统中，该程序同样是可以正常使用的，如果该程序无法在你的 Linux 系统中使用，你可以发邮件(rytubuntulinux@gmail.com)告诉我。   


[1]: https://en.wikipedia.org/wiki/Amazon_Kindle   
[2]: https://github.com/renyuntao/kindle-cleaner
[3]: https://c6.staticflickr.com/1/377/31056635093_60f057d167_z.jpg   
