# Python:如何获取所有本地可用模块的名称?
2015-12-11   <br />       
## 在 Python Shell 中查看

**1)** 直接在Python Shell中输入`help()`,之后在执行`modules`语句,如下:     

    >>> help()
    >>> modules
之后就会在Python Shell中输出本地所有可用的模块了.          

----------
**2)** 直接在Python Shell执行如下语句:       

    >>> help('modules')
上述语句同样可以输出本地所有可用的模块.      
# 使用命令 pydoc
`pydoc`是一个可以在本地查看Python文档的命令行工具，使用该工具，你不用进入Python Shell,只需在终端下执行如下命令即可:       

    $ pydoc modules
上述命令可以直接在Shell中输出本地所有可用的模块.       
## 在浏览器中查看        
命令行工具`pydoc`不仅提供了在Shell中查看Python文档的功能，还提供了在浏览器中查看Python文章的功能，要在浏览其中查看Python
文档，需要指定一个端口，如下:  

    $ pydoc -p port
加入要使用端口5678,则可以执行如下命令:      

    $ pydoc -p 5678
之后就可以打开浏览器，在地址栏输入:       

    http://localhost:5678
或者

    http://127.0.0.1:5678       
按回车就可以查看Python文档了，在其中很容易就能找到所有本地可用的模块.  
