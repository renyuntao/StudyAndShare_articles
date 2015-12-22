# Linux:如何在man页面中搜索时控制是否区分大小写?           
2015-12-05  <br />       
&nbsp;&nbsp;&nbsp;在Linux平台下，`man`是一个很方便的工具，在遇到一些不熟悉的命令或者C函数时，通过`man`可以帮助我们快速了解这
个命令或者C函数，而搜索操作又是在使用`man`时常用的一个操作，更好的掌握一些搜索的技巧可以帮助我们更快的解决问题.下面将分享几种
在man中搜索时控制是否区分大小写的方法.                     
**1)** 在`man`中有如下一些规则:           

> /Apple    =====>      区分大小写      
> /apple    =====>      不区分大小写         
> /APPLE    =====>      区分大小写          

&nbsp;&nbsp;&nbsp;由上面的规则可以看出，如果输入的字符串中包含大写字母，则在进行搜索时就会区分大小写;如果输入的字符串中全部
是小写字母,则在搜索时就会不区分大小写。因此当我们想要不区分大小写地搜索时，只须在输入字符串时全部输入小写字母即可.              

---------
**2)** 在man页面中输入`-i`,`-i`可以切换man的搜索规则，比如要在`ls`命令的man页面中搜索`list`这个单词，可以直接在`ls`的man页面
中输入如下语句:

    /list
&nbsp;&nbsp;&nbsp;这时所有的`list`都会高亮显示，并且是不区分大小写的，即像`List`，`LIST`这样的单词都会高亮显示，但是如果你只
想搜索`list`，即区分大小写的搜索，则可以直接在man页面下输入如下控制命令:        
    -i
这时就会发现只有`list`被高亮显示了，这是因为`-i`切换了man的搜索规则，使得搜索是按照区分大小写进行的.            

---------
**3)** 你同样可以使用环境变量来控制man的搜索规则，环境变量`MANPAGER`决定了由那个工具来显示man页面，默认是采用`less`来显示man
页面的。如果你想要在man中进行搜索时都按照不区分大小写的方式来进行搜索操作，你可以在shell中执行如下语句:          

    export MANPAGER='less -I'
上述语句中的`-I`是`less`的一个参数，它可以指定在进行搜索操作时不区分大小写。             
上述方法有一个缺点，即在关闭当前的shell之后，上述的语句就会失效。通过将上述语句写入`~/.bashrc`文件可以解决这个问题.                
