# 360疑似收集用户信息        
<!-- 
2016-07-27
--> <br /><br />

&nbsp;&nbsp;&nbsp;&nbsp;早就听说过360疑似收集用户的信息，当发现用户有翻墙的行为时，就会收集相关的信息，并提供给 **[GFW](https://en.wikipedia.org/wiki/Great_Firewall)** 进行封杀。但一直以来我并不太确定这种说法，因为我作为一名Linux用户，平时是不使用360的;直到前几天，我发现了一些360收集用户信息的证据，才确信了这种说法。                     
            
&nbsp;&nbsp;&nbsp;&nbsp;我自己搭建有一个 Shadowsocks 服务器，平时的几个朋友（都是Windows用户）就用它来翻墙。前些天，我查看了一下我自己搭建的Shadowsocks服务器的日志文件，竟然有了一个吃惊发现，首先来看一张日志文件内容的截图:                       
    
<a data-flickr-embed="true"  href="https://www.flickr.com/photos/137980533@N03/28372718426/in/dateposted/" title="360"><img src="https://c3.staticflickr.com/9/8300/28372718426_dce28ef74b_z.jpg" width="640" height="177" alt="360"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>
             
&nbsp;&nbsp;&nbsp;&nbsp;从日志文件的内容来看，360似乎一直在通过用户的设备，向360的网站(服务器)传送一些信息，虽然并不知道传送的具体内容是什么，但360这种卑鄙的行为已经足以让人感到不安了，并且在整个日志文件中，将近 2/5 的内容都是这种记录!       

## 防范措施

为了阻止 360 的这种卑鄙行为，下面介绍几种防范措施。                                

### 方法一: 卸载360（推荐）
&nbsp;&nbsp;&nbsp;&nbsp;卸载360,使用 **国外** 的杀毒软件。不要以为仅仅是360有收集用户信息的行为，其他所有大陆的杀毒软件都有可能会收集用户的信息。因为这些公司都位于天朝中，**朝廷让你干什么，你就得干什么**,这样就难以保证其他天朝里的公司就不会收集用户的信息。比较保险的方法是使用国外的杀毒软件，虽然国外的杀毒软件也可能会收集用户的信息，但是至少这些信息不大可能会发给朝廷。                       
      
关于国产软件窃取用户信息的新闻有很多，具体可以参见 **[编程随想的博客](https://program-think.blogspot.com/)**(已被朝廷屏蔽) 中的下面这篇文章:    
**[如何隐藏你的踪迹，避免跨省追捕\[2\]：个人软件的防范
](https://program-think.blogspot.com/2010/04/howto-cover-your-tracks-2.html)**                      
               
**注意:** 上述链接已被朝廷屏蔽，因此需要翻墙才能访问，具体的翻墙方法可以参见 **[这里](https://www.studyandshare.info/modify_dns_antigfw.html)**。  

### 方法二: 屏蔽360网站(服务器)
&nbsp;&nbsp;&nbsp;&nbsp;这种方法是写一个 Shell 脚本，在相关的日志文件（比如Shadowsocks日志文件）中找到360将信息发送到哪些网站(服务器)，并将这些地址筛选出来，之后屏蔽掉这些网站（服务器）即可（关于屏蔽网站的方法，可以参见 **[如何在Linux(或Windows)中屏蔽特定的网站？](https://www.studyandshare.info/how_to_block_website.html)**）,同时借助 Linux 系统中的 **crond** 服务，定时执行这个脚本，就可以实现自动屏蔽 360 的相关网站(服务器)了。         

**缺点:** 这种方法通常只适用于Shadowsocks服务器的搭建者，因为Shadowsocks的客户端用户通常接触不到Shadowsocks的日志文件。                

### 方法三: 翻墙前首先退出360
如果你不想或不能采用上述的两种方法，那么一定要使用这种方法，即翻墙前，首先将360退出。                   
               
## 总结
&nbsp;&nbsp;&nbsp;&nbsp;这篇文章写了360收集用户信息的一些证据，以及对 360 的这种卑鄙行为的防范措施, 目的在于提高大家的警惕性，要学会保护好个人的信息。总之，以后在翻墙时，绝不要让360在你的电脑或手机上运行。              

