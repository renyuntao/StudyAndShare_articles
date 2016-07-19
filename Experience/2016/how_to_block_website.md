# 如何在Linux(或Windows)中屏蔽特定的网站？            
<!-- 
2016-07-19
-->
<br /><br />

## 为什么要屏蔽网站？    
&nbsp;&nbsp;&nbsp;&nbsp;说起屏蔽网站，自然会想起大名鼎鼎的 **GFW(the Great Firewall)**, GFW 屏蔽特定网站的主要目的, 就是为了阻止大多数大陆人民知道一些中共不想要大陆人民知道的事情。而我们个人在使用电脑的时候为什么要屏蔽一些特定的网站呢？这个原因就比较简单了，可能是家中有小孩，小孩也经常使用你的电脑上网，你想要让小孩远离成人网站，或者是其他的一些原因，你想要屏蔽掉一些特定的网站。             

## 屏蔽网站的方法
下面介绍一些屏蔽网站的方法，这些方法各有优缺点，你可以根据自己的情况，来选择合适的方法。

### 方法 #1: 使用 hosts 文件
hosts 文件中存储了 IP 地址和域名的映射关系，当你在浏览器通过域名访问某个网站时，hosts 文件会首先被查询，如果在 hosts 文件中存在该域名的记录，那么浏览器就会使用 hosts 文件中相应的 IP 地址来访问网站，而不会再去向DNS服务器查询。     
利用上述特性，我们可以使用 hosts 文件来屏蔽一些网站，比如你想要屏蔽百度，可以打开 hosts 文件(Linux系统中，hosts文件的位置是 `/etc/hosts`), 并在 hosts 文件尾部添加如下一行语句:

```
127.0.0.1 www.baidu.com
```
保存并退出 hosts 文件，之后打开浏览器，在地址栏输入百度的网址来访问百度首页，就会发现百度被屏蔽掉了。      
在 Chrome 浏览器中，会显示如下页面:      

<a data-flickr-embed="true"  href="https://www.flickr.com/photos/137980533@N03/28327137871/in/dateposted/" title="blocksite"><img src="https://c8.staticflickr.com/9/8595/28327137871_c860b26b8e_z.jpg" width="640" height="346" alt="blocksite"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>

                 
**注意:** 该方法在Windows 8 及以后的Windows 系统中不能直接使用，具体的原因及解决方法可以查看下面的链接:                  
**[How to Block Websites in Windows 8’s Hosts File](http://www.howtogeek.com/122404/how-to-block-websites-in-windows-8s-hosts-file/)**       

### 方法 #2: 修改 DNS 服务
如果你想要屏蔽的网站数量不多，那么可以使用修改 hosts 文件的方法，但是如果你想要的屏蔽的网站数量很多（例如想要屏蔽成人网站），那么修改 hosts 文件的方法就不太适合了, 此时可以尝试使用修改 DNS 服务的方法。             
一些 DNS 服务不仅仅提供域名解析服务，还提供了一些其他的额外特性，例如 OpenDNS Family Shield 就提供了屏蔽成人网站的特性。      
OpenDBS Family Shield 的DNS服务器地址为:            

```
208.67.222.123
208.67.220.123
```
#### 修改DNS服务的方法
在 Linux 系统中，修改DNS服务的方法可以参见 **[这里](https://www.studyandshare.info/modify_dns_antigfw.html)**.              
在 Window 系统中，修改DNS服务的方法可以参见 **[这里](https://support.microsoft.com/zh-cn/help/15089/windows-change-tcp-ip-settings#1TC=windows-7)**.           

### 方法 #3: 使用屏蔽软件
关于使用软件来屏蔽网站的方法，你可以查看下面的链接，该链接中介绍了 10 个免费的用于屏蔽成人网站的软件:       
**[10 Free Tools to Filter and Block Porn on the Internet](https://www.raymond.cc/blog/block-pornographic-pictures-by-pixelating-images/)** 

<!--
Reference:
5 DNS Services to Block Porn Sites without Installing Software
https://www.raymond.cc/blog/how-to-block-pornographic-websites-without-spending-money-on-software/

7 Reasons to Use a Third-Party DNS Service
http://www.howtogeek.com/167239/7-reasons-to-use-a-third-party-dns-service/
-->
