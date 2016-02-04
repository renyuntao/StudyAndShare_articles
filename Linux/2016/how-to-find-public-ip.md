# Linux:如何使用命令行查看所用网络的公用IP?                     
2016-02-04  <br />              

&nbsp;&nbsp;&nbsp;如今，无论是使用有线网络还是无线网络访问因特网，我们的电脑都是被分配了一个 [专用IP](http://whatismyipaddress.com/private-ip),之后通过 [NAT](https://en.wikipedia.org/wiki/Network_address_translation) 技术将专用IP转换为 [公用IP](https://en.wikipedia.org/wiki/IP_address#Public_addresses),之后通过公用IP访问因特网。                  
&nbsp;&nbsp;&nbsp;由于我们的电脑上只是分配了专用IP，因此在我们的电脑上只能查看专用IP(**Linux中使用 `ifconfig` 查看**)，但是应该如何查看我们所用网络的公有IP呢？最简单的方法就是在 Google 或 百度的搜索栏中输入关键字 `IP`，搜索结果的第一条就会显示出你当前所用网络的公用IP。但是有时候我们可能需要使用命令行的方式来获取所用网络的公用IP,比如要在Shell脚本中获取公用IP。这个时候，你就需要使用命令行工具 `curl` 了，下面将分享使用 `curl` 获取公用IP的几种方法(**以Ubuntu系统为例**).                      
首先查看你的系统中是否安装有 `curl`:             

```bash
$ curl --version
```

如果显示类似下面的内容，则表明你的系统中安装有 `curl`:

> curl 7.35.0 (i686-pc-linux-gnu) libcurl/7.35.0 OpenSSL/1.0.1f zlib/1.2.8 libidn/1.28 librtmp/2.3         
> Protocols: dict file ftp ftps gopher http https imap imaps ldap ldaps pop3 pop3s rtmp rtsp smtp smtps telnet tftp           
> Features: AsynchDNS GSS-Negotiate IDN IPv6 Largefile NTLM NTLM_WB SSL libz TLS-SRP              

否则，你应该执行如下命令安装 `curl`:          

```bash
sudo apt-get update
sudo apt-get install curl
```

之后，在下面的列表中任选一条命令执行，即可获取你所用网络的公用IP:        

```bash
$ curl icanhazip.com
$ curl ident.me
$ curl ipecho.net/plain
$ curl whatismyip.akamai.com
$ curl tnx.nl/ip
$ curl myip.dnsomatic.com
$ curl curlmyip.com
$ curl -s checkip.dyndns.org/ | grep -o '\([0-9]\{1,3\}\.\)*[0-9]\{1,3\}' 
$ curl -s checkip.dyndns.org/ | sed 's/.*IP Address: \(\([0-9]\{1,3\}\.\)\{3\}[0-9]\{1,3\}\).*/\1/g'
```
