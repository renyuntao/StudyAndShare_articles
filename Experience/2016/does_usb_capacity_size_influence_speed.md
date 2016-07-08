# U盘的容量大小会影响它的数据传输速度吗？
<!-- 2016-07-07 -->  <br /><br />           

&nbsp;&nbsp;&nbsp;&nbsp;前些天去买U盘，由于特殊的用途，不需要U盘的容量特别大（只需要4G即可），由于知道现在4G的U盘几乎买不到了，于是买U盘时就问店主店里容量最小的U盘是多大，之后店主便拿出了店里容量最小的U盘（一个8G的U盘），可能是因为店主看到我想买小容量的U盘，便给我普及了一下知识，说是 _**U盘的容量越小，它的数据传输速度就越快**_，当时也没问店主原因，但是之后却对店主的说法十分好奇，想要验证那位店主的说法是否正确，于是便借助互联网的优势，在其中搜寻答案。             
           
&nbsp;&nbsp;&nbsp;&nbsp;经过一番 Googling, 以及在 **[Super User](https://superuser.com)** 进行提问，得出了一个比较可靠的结论，可以肯定地说，那位店主的说法是错误的，即U盘的数据传输速度与U盘的容量大小并没有关系。关于影响U盘数据传输速度的因素，首先引用一段相关的描述:       

> Two things determine the speed of a flash drive: the USB port itself and the flash drive’s components.       
>       
> [USB 3.0 is much faster than USB 2.0](http://www.makeuseof.com/tag/usb-30-technology-explained/), but the standard must be supported by both the USB port and the drive itself. If your flash drive is USB 3.0 but your computer’s port is USB 2.0, transfers will happen at USB 2.0 speeds. (Roughly speaking, USB 3.0 transmits data at 100 MB/s while USB 2.0 transmits at 15 MB/s.)       

**引自: [USB Flash Drive Guide: 5 Things to Know When Buying One](http://www.makeuseof.com/tag/usb-flash-drive-guide-5-things-know-buying-one/)**    

由上面的描述可以看出，影响U盘的数据传输速度的因素只有两点:              
      
1. USB 端口
2. U盘组件

&nbsp;&nbsp;&nbsp;&nbsp;对于U盘组件，USB 3.0 要比 USB 2.0 的数据传输速度快，但我们并不能说 USB 3.0 的U盘就比 USB 2.0 的U盘传输速度快，因为U盘的传输速度还受到另外一个影响因素 —— USB端口 —— 的影响，如果你使用的是 USB 3.0 的U盘，但是电脑的 USB 端口是 USB 2.0, 那么你的U盘的数据传输速度依然是 USB 2.0 的传输速度。     

&nbsp;&nbsp;&nbsp;&nbsp;另外一个U盘组件方面的影响因素是，即使同是 USB 3.0（或USB 2.0）的U盘，不同质量的U盘，其传输速度也是不相同的，好的U盘可能会采用 **[SSD](https://en.wikipedia.org/wiki/Solid-state_drive)** 作为其存储器，而普通的U盘则通常使用 **[Flash Memory](https://en.wikipedia.org/wiki/Flash_memory)** 作为其存储器。采用 **SSD** 作为存储器的U盘在同等条件下, 要比采用 **Flash Memory** 作为U盘存储器的U盘的数据传输速度快。         


&nbsp;&nbsp;&nbsp;&nbsp;因此，在拿到一个U盘之后，我们并不能简单地说这个U盘的数据传输速度是快还是慢，而是应该综合考虑这个U盘自身以及其所使用的USB端口的因素之后，才能对其数据传输速度的快慢下一个定论。

<!--
Reference:
https://superuser.com/questions/1097843/dose-the-size-of-a-usb-flash-drives-storage-capacity-influence-its-transmission
-->
