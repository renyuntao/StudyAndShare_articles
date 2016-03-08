# 免费SS帐号分享      
2016-03-07   <br />          
         
<a data-flickr-embed="true"  href="https://www.flickr.com/photos/137980533@N03/24621938399/in/dateposted/" title="new_google_logo"><img src="https://farm2.staticflickr.com/1648/24621938399_b9fac00ca1_z.jpg" width="640" height="257" alt="new_google_logo"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>
  
<a data-flickr-embed="true"  href="https://www.flickr.com/photos/137980533@N03/25585432635/in/dateposted/" title="wikipedia"><img src="https://farm2.staticflickr.com/1592/25585432635_555f43d014_z.jpg" width="640" height="427" alt="wikipedia"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script><br /><br />
Google 和维基百科对于学习而言是一个很好的助手，然而这两个网站在中国大陆被全部或部分封锁，导致了学习上的不便。前些天在 **G+** 上受到一点启发，又恰好手中有一些免费的SS资源，再加上从网上搜集的一些资源，打算将其分享出来.                     
        
由于不便公开放到网站上，所以采用Email的方式发放免费的SS帐号。你可以通过下面的链接来提交想要接受SS帐号的邮箱:                 
            
[http://www.studyandshare.info/ss_user_addr_collect.html](http://www.studyandshare.info/ss_user_addr_collect.html)                 
              
在每天的 8:00 ~ 22:00 期间的整点时刻(即8:00, 9:00, 10:00, ...,22:00)会集中通过Email来发放SS帐号。比如你想在 9:00 接收到SS帐号信息，你需要在8:00以后9:00之前提交你的邮箱。      
      
## 一些注意事项

- 每次发放SS帐号完毕后，都会清除之前提交的邮箱，因此，如果你想继续接受SS帐号信息，就必须再重新提交你的邮箱
- 由于是免费资源，这些SS帐号很不稳定，如果使用几个小时后不能再继续使用了，你需要再次提交你的邮箱地址，然后在下一次发放SS帐号信息时接受最新的SS帐号信息 
- 每次发放SS帐号时，至少会发送三个，其中有些可能不能使用，你可以尝试其它的帐号

## 安装SS客户端      
### Ubuntu 用户       
方法一: 使用命令行客户端 

```bash
$ sudo apt-get update
$ sudo apt-get install python-pip
$ sudo pip install shadowsocks
```
之后在当前目录下创建一个名为 config.json 的文件，内容如下:    

```
{
	"server" : "Server_IP",
	"server_port" : Server_Port,
	"local_address" : "127.0.0.1",
	"local_port" : 1080,
	"password" : "SS_Password",
	"timeout" : 600,
	"method" : "aes-256-cfb",
	"fast_open" : false
}
```

将上述内容中的 Server_IP, Server_Port, SS_Password 修改为你从Email中获取的SS信息即可。       
之后运行如下命令启动Shadowsocks:        
    
```bash
$ sudo sslocal -c config.json -d start
```
方法二: 使用GUI客户端
在终端执行如下命令:     
    
```bash
$ sudo add-apt-repository ppa:hzwhuang/ss-qt5
$ sudo apt-get update
$ sudo apt-get install shadowsocks-qt5
```
之后打开安装的软件，按提示填写从Email获取的内容即可，填写完毕后点击启动.
### Windows 用户
点击 [这里](<https://github.com/shadowsocks/shadowsocks-csharp/releases/download/2.5.6/Shadowsocks-win-2.5.6.zip) 下载并安装，打开软件后按照提示填写从Email获取的内容即可.         

## 设置浏览器代理           
设置代理的方法可以查看 [这里](<http://www.360doc.com/content/12/1124/10/2283188_249898912.shtml),找到设置代理的位置后，选择设置 SOCKSv5 代理,之后填写IP为 127.0.0.1, 端口号为 1080。有些浏览器可能没有代理类型(这里是SOCKSv5)的选项，此时，可以在填写IP时添加 socks5 前缀,即填写 socks5://IP      
     
保存设置，并确保shadowsocks客户端已经启动，此时在浏览器地址栏输入 Google 地址:            
          
> https://www.google.com            
           
如果设置成功，则此时会打开 Google 的首页。 
