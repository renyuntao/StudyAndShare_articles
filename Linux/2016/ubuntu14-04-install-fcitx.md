# Ubuntu 14.04:如何使用Fcitx输入法?             
2016-03-04     <br />                 
       
Ubuntu 14.04(**注意是Ubuntu 14.04,而不是Ubuntu Kylin 14.04**)中默认没有安装有Fcitx输入法(又名小企鹅输入法),有一个 **Sun Pinyin** 中文输入法，但是个人感觉这个中文输入法不如Fcitx输入法好用。对于喜欢 Fcitx 输入法的人说，可以自己来安装这个输入法。下面介绍如何在 Ubuntu 14.04 中安装 Fcitx.     
        
## 安装 Fcitx
### 使用Ubuntu软件中心来安装                 
打开 Ubuntu软件中心，选择 **所有软件**,之后搜索 **fcitx** ,在搜索结果中，安装所有标有 **fcitx** 的软件。　　　　　　　　　　　
　　　　
### 使用apt-get来安装        
你也可用使用命令行来安装 Fcitx, 在终端执行如下命令即可:            
          
```bash
$ sudo apt-get update
$ sudo apt-get install fcitx-table-wbpy
```
   
## 设置Fcitx为系统输入法
安装好Fcitx后，你需要设置Fcitx为系统默认的输入法，设置方法如下:     
   
1. 打开终端，并执行如下命令:      

	```bash
	$ im-config
	```
2. 接下来会弹出几个窗口，你只要点击 **确定** 就可以了，在最后一个窗口中选择 **fcitx** 并点击 **确定**                 
3. 这样就配置好了，要使配置生效，你可以注销当前用户然后在重新登录，或者是重启计算机　　　     
       
　　　　　
配置生效后，你会在桌面的右上角看到一个键盘图标，这个就是 Fcitx 的图标,之后你就可以使用Fcitx输入法了。　　　　　

