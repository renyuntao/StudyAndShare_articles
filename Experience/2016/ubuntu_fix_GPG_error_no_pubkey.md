# Ubuntu中修复错误:'W: GPG error: NO PUBKEY'
<!--
2016-06-02
--><br /><br />           
          
在 Ubuntu 中，有时候执行完 `sudo apt-get update` 命令之后，会得到类似面的错误:           
         
> W: GPG error: https://get.docker.io docker Release: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY D8576A8BA88D21E9

遇到这种情况，可以通过运行下面的命令来解决:         
         
```bash
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys <PUBKEY>
```
其中，`<PUBKEY>` 为你得到的错误信息中显示的一串16进制数，以上面的错误信息为例，`<PUBKEY>` 即为 `D8576A8BA88D21E9`.          
上面的命令执行完毕后，接着使用下面命令更新:             
       
```bash
$ sudo apt-get update
```
这次，你就不会再得到错误信息了。               

<!--
Reference: 
AskUbuntu: http://askubuntu.com/questions/13065/how-do-i-fix-the-gpg-error-no-pubkey          
-->
