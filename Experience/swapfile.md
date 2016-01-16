# Vim:Swap file "..." already exists           
2016-01-14  <br /><br />     

&nbsp;&nbsp;&nbsp;今天在 VPS 下使用 vim 编辑一个文件 `example` 时,由于一些意外情况导致 ssh 连接断开，等到重新使用 ssh 连接到 VPS 并重新打开上次正在编辑的那个文件的时候，出现了一些问题，提示如下的信息:            

> (1) Another program may be editing the same file.            
    If this is the case, be careful not to end up with two            
    different instances of the same file when making changes.                    
    Quit, or continue with caution.              
> (2) An edit session for this file crashed.          
    If this is the case, use ":recover" or "vim -r example"   
    to recover the changes (see ":help recovery").   
    If you did this already, delete the swap file ".example.swp"   
    to avoid this message.          
> Swap file ".example.swp" already exists!   
> [O]pen Read-Only, (E)dit anyway, (R)ecover, (D)elete it, (Q)uit, (A)bort:

上述问题一般出现在下面的两种情况下:            

- 使用 ssh 登录到远程主机，并且在使用 vim 编辑一个文件时，ssh 意外断开连接。            
- 当正在使用 vim 编辑一个文件的时候，终端意外关闭,或者是电脑出现问题，以外关闭或重启。

&nbsp;&nbsp;&nbsp;这是因为在出现意外之时，你正在编辑的文件尚未保存，vim 会将你所做的修改保存到一个文件名为原始文件名加 **`.swp`** 后缀的隐藏文件中去。当你再次使用 vim 打开原始文件时，vim 会检测到该原始文件对应的 **Swap file**,即那个后缀为 **`.swp`** 的隐藏文件，此时就会出现上面所说的提示.这个时候通常所要做的就是恢复出现意外之前对文件修改但并未保存的部分，而执行恢复操作，只要根据提示按下 `R` 键即可，之后再在 vim 中的命令行模式下执行下面的命令保存即可:   

    :w
&nbsp;&nbsp;&nbsp;如果你想要知道文件恢复前后有哪些不同，可以在对文件进行恢复操作之前对文件先备份一下，然后再对文件进行恢复操作，之后使用Linux中的 `diff` 工具对比备份文件和恢复后的文件，即可获知文件恢复前后有哪些不同了。            
关于 `diff` 的用法，可以参考这篇文章: [Linux and Unix diff command](http://www.computerhope.com/unix/udiff.htm) 

