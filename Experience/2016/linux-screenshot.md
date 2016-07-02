# Linux中用于截图的两个工具                 
<!-- 2016-03-19 -->  <br /><br />            
             
&nbsp;&nbsp;&nbsp;&nbsp;在日常的工作或学习工程中，截图是一个很常用的操作，Linux 中有很多截图工具，包括命令行工具和图形界面的工具，二者各有优缺点。命令行工具在使用熟练后操作效率更高，并且可以写入 Shell 脚本中，而图形界面工具则操作简单，更适用于刚从 Windows 阵营转移到 Linux 阵营中的新手。下面就来介绍两个 Linux 平台下的截图工具(包含命令行工具和图形界面工具)。            

## 使用 gnome-screenshot              
`gnome-screenshot` 是Ubuntu中默认的截图工具，该工具有三种使用方法，即                     
          
- 通过快捷键            
- 通过 `gnome-screenshot` 命令             
- 通过 `gnome-screenshot` GUI            
      
下面介绍各自的使用方法。　　　　　              
           
### 通过快捷键截图                  
`gnome-screenshot` 可以通过如下快捷键来进行截图:            
           
- **shift+printscreen(Prt scr)**  该快捷键可以对指定的区域进行截图       
- **Alt+printscreen(Prt scr)**  该快捷键可以对当前的窗口进行截图           
- **Fn+printscreen(Prt scr)**  该快捷键可以对桌面进行截图
             
### 通过命令来截图
下面来介绍如何使用 `gnome-screenshot` 命令来截图:            
           
**1. 要对桌面进行截图，可以直接在终端执行如下命令:**            
          
```bash
$ gnome-screenshot
```
**2. 要对当前窗体进行截图，可以在终端执行如下命令:**            
         
```bash
$ gnome-screenshot -w
```          
**3. 执行如下命令，可以对指定区域进行截图:**             
         
```bash
$ gnome-screenshot -a
```
**4. 要延迟3秒后对桌面进行截图,可以使用 `gnome-screenshot` 命令的 `-d` 选项:**                 
          
```bash
$ gnome-screenshot -d 3
```
我很喜欢 `gnome-screenshot` 的这个功能.              
           
命令行工具 `gnome-screenshot` 的功能很强大，更多的功能可以查看它的帮助:             
        
```bash
$ gnome-screenshot -h
```
### 通过GUI来截图              
如果你不喜欢使用命令来截图的话，可以使用GUI工具来截图,你只要在终端执行如下命令即可使用 `gnome-screenshot` 的GUI工具:               
                
```bash
$ gnome-screenshot -i        
``` 

## 使用import命令来截图               
命令行工具 `import` 是工具集 `ImageMagick` 的一个成员，因此，要使用 `import`, 就要首先安装 `ImageMagick`,安装方法如下:             
          
```bash
$ sudo apt-get update
$ sudo apt-get install imagemagick
```       
安装完毕后，就可以使用 `import` 这个工具了.               
`import` 这个工具的优点是截图后会直接将图片保存到指定的文件中，而不会弹出窗口提示.下面几个使用的例子:             
         
- 对桌面进行截图，并将图片保存到文件 `example.png` 中:         
	
	```bash
	$ import -window root example.png
	```
- 对指定窗口进行截图             

	```bash
	$ import -screen example.png
	```
	执行上述命令口，用鼠标点击想要截图的窗口即可。截图后图片会保存在文件 `example.png` 中。                 
- 对指定区域进行截图 
	
	```bash
	$ import example.png
	```
	执行上述命令后，用鼠标选择相应的截图区域即可。同样，截图会保存在文件 `example.png` 中。
