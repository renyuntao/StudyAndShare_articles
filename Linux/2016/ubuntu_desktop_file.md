# Ubuntu: 创建桌面快捷方式       
<!--      
2016-10-04
--><br /><br />

一名Linux老手会偏爱于使用命令的方式来完成他想要做的操作，而Windows用户则偏爱于使用鼠标点击的方式来完成相应的任务, 这一点通过二者的桌面就可以看出来。           

**Windows 用户的桌面:**                
        
![Windows Desktop][1]       

**Linux 用户的桌面:**                

![Linux Desktop][2]   

Windows 用户的桌面常常布满了快捷方式，而Linux用户的桌面通常而言则简洁很多。             
          
现在，越来越多的人从Windows阵营转移到Linux阵营下，而对于刚刚从Windows阵营转移到Linux阵营的新手而言，可能暂时无法摆脱在Windows平台下进行操作的习惯，仍然想要使用鼠标点击的方式来完成一些操作。               
         
这篇文章就是为那些刚刚投入Ubuntu怀抱之中，但又希望使用鼠标点击的方式来完成一些操作的Linux新手而写的。在这篇文章中，将会介绍如何在Ubuntu平台下，创建类似于Windows平台下的桌面快捷方式，通过点击这些快捷方式，就可以像在Windows平台下那样，执行相应的程序。    

## 在Ubuntu中创建桌面快捷方式的方法               

要在Ubuntu中创建桌面快捷方式，可以按照下面的步骤进行操作:              

1. 在Ubuntu桌面上创建一个普通文件，文件名任意，但是文件名的后缀需要为 **.desktop**                       
2. 使用你熟悉的编辑器打开上面创建的文件，在文件中写入类似于下面的内容:              

	```bash         
	[Desktop Entry]
	Version=x.y
	Name=ProgramName
	Comment=This is my comment
	Exec=/home/alex/Documents/exec.sh
	Icon=/home/alex/Pictures/icon.png
	Terminal=false
	Type=Application
	Categories=Utility;Application;
	```
	上面内容的解释如下:              

	- **Version** - 你想要执行的程序的版本信息，如果不知道的话可以随便写一个，也可以直接忽略该项           
	- **Name** - 你想要执行的程序的名称，这也是快捷方式创建完成后的名称    
	- **Comment** - 注释，说明这个程序是干什么的, 不想写的话也可忽略该项   
	- **Exec** - 这一项最为重要，表示你想要执行命令行程序的绝对路径    
	- **Icon** - 快捷方式的图标的图片位置, 图片位置应该以绝对路径表示   
	- **Terminal** - 表示执行程序时是否需要终端             
	- **Type** - 类型信息，因为这篇文章中要讲的是为一个可执行程序创建桌面快捷方式，因此这里需要填写 _Application_           
	- **Categories** - 应用程序的类别，该项会影响快捷方式在Dash中的位置，通常，该项填写 _Utility;Application;_ 即可                     

   	**在书写 \*.desktop 文件时的几点注意事项:**             

	1. **Icon=...** 一行的行尾不能有空白字符(如空格)的存在，否则，快捷方式的图标将不能正常显示                  
	2. **Terminal=...** 一行的行尾不能有空白字符(如空格)的存在，否则，双击快捷方式会不起作用            
	3. **Type=...** 一行的行尾不能有空白字符(如空格)的存在，否则，会显示下面的错误:        
    
		![exec_program_error][3]            

3. 填写完成后，保存文件并退出。               

如果操作正确的话，此时，你会在桌面上看到一个你创建的快捷方式，双击该图标，就会像在Windows平台下那样，执行相应的程序。     

## 结束语              

这是一篇写给刚刚从Windows阵营转入Ubuntu Linux阵营，但暂时又无法摆脱Windows平台下操作习惯的Linux新手而写的，通过一些简单的操作，就可以在Ubuntu桌面上创建类似与Windows平台下的桌面快捷方式。但我相信，随着Linux新手们对Linux系统越来越熟悉，他们会发现, 相比于使用鼠标点击，命令行才是效率更高的操作方式，最终他们会热衷于使用命令行的方式来执行操作、解决问题。               

参考:                 
[UnityLaunchersAndDesktopFiles][4]        

[1]: https://c3.staticflickr.com/8/7521/29987097682_e26449c347_z.jpg             
[2]: https://c1.staticflickr.com/9/8727/29813857480_c0d9332156_z.jpg        
[3]: https://c2.staticflickr.com/6/5795/30109400585_70e93556e1_m.jpg     
[4]: https://help.ubuntu.com/community/UnityLaunchersAndDesktopFiles         
