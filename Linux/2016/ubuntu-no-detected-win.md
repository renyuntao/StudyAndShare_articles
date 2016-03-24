# 安装Ubuntu时无法检测到Windows系统解决方案               
2016-03-24  <br /> <br />                
                 
如果你的电脑上原本安装了 **Windows 8** 或者是 **Windows 10** 系统，那么你在安装 Ubuntu 系统时会遇到下面的情况:                   
<br />           
<a data-flickr-embed="true"  href="https://www.flickr.com/photos/137980533@N03/25393991254/in/dateposted/" title="NoDetectedWin"><img src="https://farm2.staticflickr.com/1686/25393991254_4dd78a32b3_z.jpg" width="640" height="268" alt="NoDetectedWin"></a><script async src="//embedr.flickr.com/assets/client-code.js" charset="utf-8"></script>                         
<br />                
即 Ubuntu 系统无法检测到 Windows 系统的存在，这对于想要安装双系统的人来说是一个很大的问题。出现这种问题的原因在于 Windows,而不在于 Ubuntu,并且这种情况目前只会出现在原本电脑上安装了 Windows 8 或 Windows 10 系统的情况下，如果原本电脑上安装的是 Windwos 7 以及更早的Windows版本，则不会出现这种问题.因为 Windows 8 和 Windows 10 为了加快开机的速度，增加了一个被称为 **Fast Startup** 的特性，这个特性使得你在关机后，系统并未真正停止运行，这样在下次开机时开机速度就会加快；但是这样做也会带来一些问题，例如在安装Ubuntu时，无法检测到Windows系统的存在.                      
关于Windows中 **Fast Startup** 特性的详细信息，可以参阅 **[这里](http://www.howtogeek.com/243901/the-pros-and-cons-of-windows-10s-fast-startup-mode/)**.                    
              
因此，在安装 Ubuntu 系统之前，首先要做的就是将Windows的这个 **Fast Startup** 特性关闭掉，下面是关闭掉该特性的方法:              
           
1. 首先按下 **Windows+X** 快捷键，在弹出的窗体里点击 **控制面板**.      
2. 在 **控制面板** 里选择 **硬件和声音**             
3. 在随后的窗体里选择 **电源选项**       
4. 之后点击 **选择电源按钮**      
5. 接下来点击 **更改当前不可用的设置**            
6. 然后取消掉 **关机设置** 里面的 **fast startup** 选项并保存         
7. 最后 **重启电脑** 使设置生效, 注意不要关闭电脑                
            
完成上述操作并重启电脑之后，**Fast Startup** 特性就被关闭掉了，此时，你再来安装Ubuntu系统就可以正常检测到Windows系统了.                  

<!-- https://sites.google.com/site/easylinuxtipsproject/windows -->
