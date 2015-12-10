# 一个加密密码的小工具
2015-12-10   <br />         
如今我们都有着各种各样的帐号,比如QQ帐号，支付宝帐号，百度帐号，Google帐号,12306帐号.....我给自己数了一下，发现自己的帐号数目
竟然有十几个，并且每个帐号都对应一个密码。要记忆
这么多的帐号密码确实不容易，有些人为了方便记忆，就将所有帐号的密码设置为一样的，然而这样做是很不安全的，但是如果不这样做，记
忆这样帐号密码就很困难。最终只好选择将这些帐号密码都记在一个本子上面，忘记的时候就查看一下，然而这样又带来一个问题，如果以明
文来记录密码，一旦不小心被坏人看到，就很可能给自己带来损失。为了解决这个问题，便写了一个简单的加密密码的小程序。        

## 使用说明
加密的页面如下:       
![encrypt.png](http://i5.tietuku.com/c967decdca3ccade.png)         
在第一栏中输入你想要加密的密码，在第二栏中输入密语(要牢记密语，以后解密时也要用到它, **注意,密语是大小写敏感的** ),之后点
击`加密`按钮即可进行加密了。          
比如我想要加密的密码是 **`Hello World`** ,那么就在第一栏输入 **`Hello World`** ,输入密语时，你可以输入任意可打印的ASCII字
符，这里输入 **`ZhiMaKaiMen`** ,
之后单击加密，就会显示加密后的密文，如下:        

    #NV:QK9Y@RS
解密的页面如下:            
![decrypt.png](http://i5.tietuku.com/3c3c5eb50bb725d6.png)      
在第一栏输入密文，在第二栏输入你加密是所用的密语，之后点击`解密`即可。             
下面来将上面的密文进行解密，在第一栏输入密文 **`#NV:QK9Y@RS`** ,之后在在第二栏输入加密时所用的密语 **`ZhiMaKaiMen`** ( **注
意，密语是大小写敏感的** ) ,之后点击解密，就会显示解密后的结果，如下:           

	Hello World

下面是加密，解密的链接:      
**加密:** [http://www.studyandshare.info/encrypt.html](http://www.studyandshare.info/encrypt.html)          
**解密:** [http://www.studyandshare.info/decrypt.html](http://www.studyandshare.info/decrypt.html)

<br />      
这个程序是用Python写的，我在[GitHub](https://github.com/renyuntao/Vigenere-Cipher)上也放了一个该程序的C++版本，二者实现稍
有不同，但大体思路一致，感兴趣的可以查看。            
