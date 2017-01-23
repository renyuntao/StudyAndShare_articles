# 如何在Ubuntu中创建Wifi热点?    
<!--
2017-01-22
--><br /><br />

有时候，我们只能通过网线来在电脑上上网，如果我们想要让手机、Kindle等无线网络设备也能够共享有线网络来上网，可能会想到使用[无线路由器](http://baike.baidu.com/link?url=eusfnPd7z8OvDaSIgVNNdXhk2WlBAjPR3bLSQCXl1HB3i0NJXIZ12x6Uwoa_e2aRmqwSuIbfxcyJKPROXk5MFqBN7WgSBkClgHn9_qU1wd1dG4y3Qu2fMxiYJqfkZwOvqsSOLQVPUFieCTle_NqHzK)(如智能手机、Kindle等)来将有线网络以无线信号的形式分享给附近的无线设备。然而，如果你是在一台笔记本电脑上使用的Ubuntu系统，你不必花钱去买无线路由器，只需要简单的几步操作，你就可以在Ubuntu系统中创建一个Wifi热点，通过这个Wifi热点，附近的无线设备即可共享你电脑上的网络了。     

在继续介绍如何操作之前，这里先提及一下，本文所介绍的操作均已在Ubuntu 16.04(64bit)系统和Dell Inspiron 14 7000笔记本上测试通过了。  

## 操作步骤

在Ubuntu中创建Wifi热点的具体操作步骤如下:           

1. 将网线连接到你的笔记本电脑上，并禁用无线网卡。不同笔记本电脑禁用无线网卡的按键是不同的，在我使用的笔记本电脑上，禁用无线网卡的按键是 **Fn**+**PrtScr**,你可以根据你所使用的电脑来按下对应的按键。
2. 点击桌面右上角的网络连接按钮，在弹出的窗口中点击**编辑连接**:   

   ![pic1](https://c1.staticflickr.com/1/587/32308262502_a019be64dd_z.jpg)

3. 之后会出现如下窗口，在这个窗口中点击**增加**按钮： 
 
   ![pic2](https://c1.staticflickr.com/1/721/32308262002_5ceed2f23c.jpg)

    在弹出的新窗口中选择 **Wi-Fi**, 并点击 **新建** 按钮: 

   ![pic3](https://c1.staticflickr.com/1/494/32308261782_562c919532_z.jpg)

4. 在随后的窗口中编辑 **连接名称**, **SSID**, **模式** 选项，其中，**模式** 一栏应当选择 **热点** 选项, 而 **连接名称** 和 **SSID** 只要是一个合法的名字即可，作为例子，这里的 **连接名称** 和 **SSID** 都是 **TechForGeek**:   

   ![pic4](https://c1.staticflickr.com/1/427/32308261692_e26fa2af33_z.jpg)

5. 点击 **Wi-Fi安全性** 标签，在其中，**安全** 一栏选择 **WPA及WPA2个人**, 并设置 Wifi 密码，如下图:  

   ![pic5](https://c1.staticflickr.com/1/594/32419513666_20c7099030_z.jpg)

6. 点击 **IPv4设置** 标签，在其中的 **方法** 一栏，选择 **与其它计算机共享** 选项，如下图:
   
   ![pic6](https://c1.staticflickr.com/1/558/32308261462_89b6f1433b_z.jpg)

7. 点击 **保存** 按钮     
8. 接下来启用无线网卡, 启用无线网卡的按键通常与禁用无线网卡的按键是相同的。
9. 随后点击桌面右上角的网络连接按钮，并点击 **连接到隐藏的Wi-Fi网络**:  

   ![pic7](https://c1.staticflickr.com/1/507/32338202171_0ab9f68aa8_z.jpg)

10. 在弹出窗口的 **连接** 一栏中，选择你刚刚创建的Wi-Fi网络名称，在这个例子中为 **TechForGeek**, 之后点击 **连接按钮**:

    ![pic8](https://c1.staticflickr.com/1/720/32308261062_c458059c61.jpg)

完成上面的操作后，你就可以拿出智能手机、Kindle等无线设备，搜索刚刚创建的Wifi热点(在这个例子中，为**TechForGeek**)并连接来共享有线网络了。   

## 结束语  

多数人在使用手机流量来上网时，或多或少都会有些顾忌，担心流量用超，本文介绍了如何在Ubuntu系统中创建Wifi热点，通过Wifi热点，我们就可将有线网络以无线信号的形式共享给附近的手机之类的无线设备，这样我们就可以在手机上随心所欲的上网，而不必担心手机流量用超了。  
   


<!--
Reference:
[AskUbuntu](http://askubuntu.com/questions/762846/how-to-creat-wifi-hotspot-in-ubuntu-16-04-since-ap-hotspot-is-no-more-working)  
-->
