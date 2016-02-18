# Linux:普通用户如何获取root权限?        
2016-01-03   <br />        
&nbsp;&nbsp;&nbsp;有时候，普通用户需要有root权限才能够完成任务。在Ubuntu中，一个普通用户可以通过 `sudo` 来获得root权限,但并不是所有的用户都可以使用 `sudo` 的，一个不被允许获取root权限的普通用户在使用 `sudo` 时会的到类似下面的提示:            

> user_name is not in the sudoers file.  This incident will be reported.

要想使一个普通用户能够获取root权限(即能够使用 `sudo` ),需要root用户(或以root身份)来执行一些简单的操作。下面将分享两种可以是普通用户获得root权限的方法.            

## 方法 #1:将普通用户加入sudo组
&nbsp;&nbsp;&nbsp;在Ubuntu中，存在一个名为 `sudo` 的用户组，凡是在该组中的用户都可以使用 `sudo` 来获取root权限。因此可以通过将一个普通用户加入到 `sudo` 组中来使得该用户可以获取root权限。现在假如要将一个名为 `example` 的普通用户加入到 `sudo` 组中，可以执行如下操作(使用root身份或权限):          

    # useradd example sudo
或者是           

    # usermod -a -G sudo example

---------
## 方法 #2:修改文件 /etc/sudoers 
&nbsp;&nbsp;&nbsp;文件 `/etc/sudoers` 中记录了哪些用户可以使用 `sudo` ，因此可以通过修改该文件，将普通用户加入到该文件中，即可使一个普通用户得以执行 `sudo` ，从而获取root权限。但是 **注意不要直接修改文件 /etc/sudoers** ,因为这个文件有一定的语法规则，如果修改不当，会造成严重的后果。修改这个文件应该使用工具 `visudo`,这个工具在你修改完毕后，可以对文件内容进行语法检查。仍以普通用户 `example` 为例，使用 `visudo` 将该用户加入到文件 `/etc/sudoers` 的方法如下:    
      
以root身份或权限执行命令 `visudo`

    # visudo
之后会打开文件 `/etc/sudoers`,在该文件的第20行作用会看到类似下面的内容:     

> \# User privilege specification        
> root	ALL=(ALL:ALL) ALL

在上面内容的下面添加如下内容:          

> example ALL=(ALL:ALL) ALL

保存退出,之后用户 `example` 即可使用 `sudo` 来获取root权限了.     
