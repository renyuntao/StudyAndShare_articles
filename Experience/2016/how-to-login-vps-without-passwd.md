# SSH:如何无需输入密码登录远程主机?           
2016-01-29  <br /><br />           
&nbsp;&nbsp;&nbsp;当经常需要使用 `SSH` 登录到远程主机(如VPS,树莓派)进行一些工作的时候，每次登录都要输入密码则显得有些麻烦，而且对安全性也有一定的影响,而无需输入密码登录远程主机则可以解决这一问题。下面将分享如何无需输入密码即可登录到远程主机的方法，其中涉及到了一些 `公钥/私钥` 的知识，你可以参考 [维基百科](https://en.wikipedia.org/wiki/Public-key_cryptography) 来了解这方面的内容。             
<br />           

具体操作步骤如下:           

1. 在本地主机运行如下命令:               

		$ ssh-keygen          
	之后会有类似下面的提示:             

	> Enter file in which to save the key (/home/username/.ssh/id_rsa):      
	> Enter passphrase (empty for no passphrase):    
	> Enter same passphrase again:     
 
	其实你只要对所有的提示按回车键即可。       
2. 上述命令会在你的主目录下产生一个名为 `.ssh` 的隐藏目录，执行如下命令进入该目录并查看其中的文件:             
		
		$ cd ~/.ssh
		$ ls
		id_rsa  id_rsa.pub  known_hosts
	这三个文件即是 `ssh-keygen` 命令所产生的文件.其中 `id_rsa.pub` 文件即所谓的公钥，而 `id_rsa` 即所谓的私钥.                          
3. 将公钥(即文件 `id_rsa.pub`) 传递给远程主机，即执行如下命令:            

		$ ssh-copy-id -i ~/.ssh/id_rsa.pub username@RemoteHost
	之后会提示你输入 `username` 的密码，密码输入正确后，文件 `id_rsa.pub` 就会传递给远程主机.
	
将上述三个步骤完成之后，再次使用 `ssh` 登录到远程主机:              

	$ ssh -v username@RemoteHost            
或       

	$ ssh -v RemoteHost -l username

就会发现无需输入密码，直接登录到远程主机了.            
之后再使用 `SSH` 登录到远程主机就不用再输入密码了。  

