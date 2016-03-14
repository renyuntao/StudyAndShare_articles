# apt-get:Fail to fetch ... chrome ...              
2016-03-09    <br />             
                
前些天刚刚安装了64位的Ubuntu 14.04系统，今天在运行 **apt-get update** 命令更新源的时候出现了下面的错误:           
      
> W: Failed to fetch http://dl.google.com/linux/chrome/deb/dists/stable/Release  Unable to find expected entry 'main/binary-i386/Packages' in Release file (Wrong sources.list entry or malformed file)
> 
> E: Some index files failed to download. They have been ignored, or old ones used instead.
             
出现这种问题的原因在于 [Google Chrome 已经停止了对32位Linux系统的支持]()
修复这种问题的方法如下:             
       
1. 打开文件 **/etc/apt/sources.list.d/google-chrome.list** ,其中内容如下:   
   
	> \#\#\# THIS FILE IS AUTOMATICALLY CONFIGURED \#\#\#                                 
	> \# You may comment out this entry, but any other modifications may be lost.              
	> deb http://dl.google.com/linux/chrome/deb/ stable main               

2. 将文件最后一行修改为:              
        
	> deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main               
	
	修改完毕后，整个文件的内容如下:              

	> \### THIS FILE IS AUTOMATICALLY CONFIGURED ###                                 
	> \# You may comment out this entry, but any other modifications may be lost.              
	> deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main               
              
3. 保存文件并退出。
4. 执行如下命令进行更新:               

	```bash
	$ sudo apt-get update
	```
之后这个错误就被解决了.                      
