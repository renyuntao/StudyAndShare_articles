# Linux:如何解压后缀为xz的tarball?     
2015-12-21    <br />          
&nbsp;&nbsp;&nbsp;今天从网上下载一个文件，下载下来后发现是一个tarball。以前以为自己对`tar`的压缩/解压命令已经很熟悉了，然而
这次面对这个tarball，竟然无从下手，原因在于我以前从未见过这种格式的tarball，它的后缀居然是`xz`，通常我所见的tarball要么是以
`gz`结尾，要么是以`bz2`结尾,却从未见过这种以`xz`结尾的tarball.          
&nbsp;&nbsp;&nbsp;于是立刻查询资料更新自己的知识，经过一番查询后，才知道从`tar 1.22`开始，`tar`开始支持一种新的压缩格式,即这
种以`xz`结尾的tarball。之后查看了一下自己的`tar`的版本:          

    $ tar --version
    tar (GNU tar) 1.27.1
    Copyright (C) 2013 Free Software Foundation, Inc.
    License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
    ....
可以看到我的系统中`tar`的版本是1.27,因此是支持这种新的格式的。       
要解压以`xz`结尾的tarball,须使用`-J`参数，假设当前目录下有一个名为`example.tar.xz`的tarball，那么可以通过下面的命令来解压它:        

    $ tar -Jxf example.tar.xz
上述命令中`-J`告诉`tar`该tarball的压缩格式是`xz`，`x`参数告诉`tar`来解压文件，`f`参数指定要解压的文件。            
其实，现在的`tar`已经很智能了，可以自动识别tarball的压缩格式，因此，在解压时，你无需告诉`tar`压缩文件的压缩格式，直接使用下
面的命令即可解压文件:           

    $ tar -xf example.tar.xz
