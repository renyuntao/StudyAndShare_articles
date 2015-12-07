# 如何在Linux中安装Shadowsocks?      
2015-12-07    <br />       
安装Shadowsocks，首先要确保你的系统中已经装了Python，如果没有安装的话需要先安装Python.在Ubuntu中，默认就安装有Python。        
解决了Python的问题后，接下来就是安装pip(这是一个Python的包管理器),Ubuntu中可以执行如下命令安装pip:         
 
    $ sudo apt-get install python-pip
最后就是使用pip来安装shadowsocks了,如下:          

    $ sudo pip install shadowsocks           
安装完毕后，你的系统中就会多出来两个工具 —— `ssserver`和`sslocal`,前者是用于服务器端，后者用于客户端.通过执行如下命令就可以
查看他们的使用方法了:   

    $ ssserver -h
    $ sslocal -h
