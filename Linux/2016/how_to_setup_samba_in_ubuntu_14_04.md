# 如何在Ubuntu 14.04 LTS中搭建Samba服务器?             
<!--
2016-08-07
--> <br /><br />

## 什么是 Samba?
下面是摘自维基百科中关于Samba的一段描述:             

> Samba，是种用来让UNIX系列的操作系统与微软Windows操作系统的SMB/CIFS（Server Message Block/Common Internet File System）网络协议做链接的自由软件。第三版不仅可访问及分享SMB的文件夹及打印机，本身还可以集成入Windows Server的域，扮演为域控制站（Domain Controller）以及加入Active Directory成员。简而言之，此软件在Windows与UNIX系列OS之间搭起一座桥梁，让两者的资源可互通有无。

&nbsp;&nbsp;&nbsp;&nbsp;Samba 的一个应用场景是，有两台主机，一台Windows主机，一台Linux主机，当这两台主机位于同一个局域网之下时，可以在Linux主机上搭建一个Samba服务器，之后，在Windows主机上连接到Linux主机，二者就可以共享各自主机的文件了，而不必再通过额外的介质(例如U盘)来在两台主机之间传递文件了。                 

## 安装 Samba
Ubuntu 14.04 LTS 中默认没有安装 Samba, 因此你需要自己安装它。安装方法很简单，只需要在终端输入如下两条命令即可:             
           
```bash
$ sudo apt-get update
$ sudo apt-get install samba
```

## 创建一个 Anonymous share
&nbsp;&nbsp;&nbsp;&nbsp;创建一个 **Anonymous share**, 顾名思义，即在Linux主机上创建一个可以匿名访问的目录，之后，任何可以连接到该Linux主机的Windows主机，都可以访问到这个目录下的文件，而无需输入用户名和密码。           
                           
**Anonymos share** 共享目录的创建步骤如下:

1. 打开Samba的配置文件 /etc/samba/smb.conf               

	```bash
	$ sudo vim /etc/samba/smb.conf
	```
2. 在文件末尾添加如下内容:            

	```
	[share] 
		comment = Ubuntu File Server 
		path = /srv/samba/share 
		browsable = yes 
		guest ok = yes 
		read only = no
	```
	上述配置的解释如下:                  
	**comment** 是关于该目录的简要描述             
	**path** 参数的共享目录的位置             
	**browsable** 表示是否在 Window Explorer中显示该目录            
	**guest ok** 表示是否允许匿名访问该共享目录            
	**read only** 表示是否是只读            
3. 创建 share 目录(默认权限为 777 - $(umask))

	```bash
	$ sudo mkdir -p /srv/samba/share
	```
4. 重启Samba服务使配置生效

	```bash
	$ sudo restart smbd
	$ sudo restart nmbd
	```

至此，Samba服务器配置完成，现在，可以在Windows Explorer 的地址栏中输入Samba服务器所在主机的主机名:                 

```
\\hostname
```
或这是Samba服务器所在主机的IP:             

```
\\IP
```
之后就可以在 Windows Exploer 中看到一个名为 share 的共享文件夹（Windows中称目录为文件夹）了。

现在，你在Windows中对该文件夹中的文件只具有只读权限，要想能够在该文件夹下删除、添加或者修改文件，需要在Linux中对该目录的Others权限添加w(写)权限，具体做法如下:               

```bash
$ sudo chmod o+w /srv/samba/share
```

&nbsp;&nbsp;&nbsp;&nbsp;创建一个 **Anonymous share** 目录的优点是访问比较方便，只要在 Windows Exploer 中找到相应的Linux主机后，就可以直接访问，而无需输入用户名和密码，这种情况比较适用于一个安全的环境中(例如家庭中)， 但是如果在一个不安全的环境中(例如公共场合)， **Anonymous share** 就不太适用了。                

## 创建一个 Secured share
&nbsp;&nbsp;&nbsp;&nbsp;**Secured share** 与 **Anonymous share** 不同，一个 **Secured share** 共享目录，当客户端访问时需要输入用户名和密码，因此，相比 **Anonymous share**, **Secured share** 共享目录的安全性更高。当你在一个公共场合搭建Samba服务器时，应该创建 **Secured share** 共享目录。                   
               
**Secured share** 共享目录的创建方法如下:                   

1. 创建一个 secured share 共享目录           

	```bash
	$ mkdir -p /srv/samba/secret
	```
2. 创建一个用于访问这个 secured share 的用户，下面创建一个名为sambauser1的用户              

	```bash
	$ sudo useradd sambauser1 -s /usr/sbin/nologin
	```
3. 修改共享目录的User 为 sambauser1                

	```bash
	$ sudo chown sambauser1:sambauser1 /srv/samba/secret
	```
4. 修改 Samba 配置文件          
	首先打开 Samba 的配置文件 `/etc/samba/smb.conf`
	$ sudo vim /etc/samba/smb.conf
	在文件的末尾添加如下内容:
	```
	[secret]
		comment = Secret File
		path = /srv/samba/secret
		valid user = sambauser1
		guest ok = no
		writable = yes
		browsable = yes
	```
5. 将用户 sambauser1 加入到本地的 smbpasswd file:
	
	```bash
	$ sudo smbpasswd -a sambauser1
	```
6. 重启 Samba 服务使配置生效

	```bash
	$ sudo restart smbd
	$ sudo restart nmbd
	```

至此，Samba服务器配置完成，现在，可以在Windows Explorer 的地址栏中输入Samba服务器所在主机的主机名:

```
\\hostname
```
或这是Samba服务器所在主机的IP:           

```
\\IP
```
就可以看到一个名为 secret 的共享文件夹（Windows中称目录为文件夹）了, 在访问该文件夹时，会提示输入用户名和密码，此时，输入刚刚创建的用户的用户名和密码，就可以访问该文件夹中的内容了。

## 总结
&nbsp;&nbsp;&nbsp;&nbsp;Samba 服务器可以方便的让我们在Linux主机和Windows主机之间共享文件，这篇文章解释了两种在Ubuntu 14.04 LTS中创建一个Samba共享目录的方法 —— **Anonumous share** 和 **Secured share**。**Anoymous share** 使用起来更加方便，但是缺乏安全性，比较适用于在一个安全的环境中进行文件共享;而 **Secured share** 使用起来相对麻烦一些，但其安全性更高，适用于在一个不安全的环境中使用。你应该根据自己的实际情况来选取合适的方法。                          

<!--
Reference
Setup File Server on Ubuntu 14.04 LTS(Samba)
http://www.krizna.com/ubuntu/setup-file-server-ubuntu-14-04-samba/

《Ubuntu 14.04 LTS Server Guide》
-->
