# 如何改变ls输出的文件名的颜色?
<!-- 2017-07-22 --><br /><br />

## 为什么要改变ls输出的文件名的颜色?

如果你使用过一段时间的Linux系统, 可能会遇到这样一种情况, 即某些ls输出的文件名的背景颜色是绿色的，示例图如下所示:  

![不清楚的文件名](https://farm5.staticflickr.com/4319/35948942611_e485bf86d9_o.png)    

ls的这种输出样式使得文件名看起来十分模糊，不易看清楚文件名，这是我，也可能是大多数人想要改变ls输出的文件名的颜色的原因。另外，一些人由于个人对于颜色的偏好，可能也想要改变ls输出的文件名的颜色。接下来我将介绍一种在Linux中改变ls输出的文件名的颜色的方法。    

注: 下面的操作我是在Ubuntu 16.04系统中测试通过, 但是对于大多数的Linux系统，本文所介绍的方法同样适用。   

## 改变ls输出的文件名的颜色的方法   

本文所介绍方法是借助 **dircolors** 这个命令行工具来改变ls输出的文件名的颜色的，该工具在多数Linux系统中默认已安装, 如果你使用的是当前比较常见的Linux发行版，那么你的系统中很可能已经安装了该工具。   
下面详细介绍如何使用该工具来改变改变ls输出的文件名的颜色:   

1. 输出dircolors的默认配置并保存到 **~/.dircolors** 文件中:   
   
   ```bash
   $ dircolors -p > ~/.dircolors
   ```
2. **~/.dircolors** 文件格式简介   
   打开 **~/.dircolors** 文件，你会看到类似下面的内容: 

   > ...   
   > .m4a 00;36   
   > .mid 00;36   
   > .midi 00;36   
   > .mka 00;36   
   > .mp3 00;36   
   > .mpc 00;36   
   > .ogg 00;36    
   > .ra 00;36   
   > .wav 00;36   
   > ...

   其通用格式为:   

   ```
   [TARGET] [TEXT_STYLE];[FOREGROUND_COLOR];[BACKGROUND_COLOR]
   ```

   - **TARGET** 表示文件类型, 如 **.mp3** 就表示所有以
 **.mp3** 后缀结尾的文件    
   - **TEXT\_STYLE** 表示字体样式:  

	 ```
     00 = none
	 01 = bold
	 04 = underscore
	 05 = blink
	 07 = reverse,
	 08 = concealed
	 ```
   - **FOREGROUND_COLOR**表示字体颜色:   

     ```
	 30 = black
	 31 = red
	 32 = green
	 33 = yellow
	 34 = blue,
	 35 = magenta
	 36 = cyan
	 37 = white
	 ```
   - **BACKGROUND_COLOR** 表示背景颜色:  

     ```
	 40 = black
	 41 = red
	 42 = green
	 43 = yellow
	 44 = blue,
	 45 = magenta
	 46 = cyan
	 47 = white
	 ```
3. 修改 **~/.dircolors** 文件  
   在理解了 **~/.dircolors** 中为不同类型的文件设置颜色的方法之后，接下来我们就可以修改该文件了。在本文的开头，我提到了有时候ls会输出背景为绿色的文件名，导致文件名看不清楚; 当一个目录的others可写(即o+w)时，ls输出该目录的目录名时，就会将输出这种背景为绿色的文件名, 作为例子，下面将修改ls的这种行为。   
   首先在 **~/.dircolors** 文件中找到如下一行:   
   
   ```
   OTHER_WRITABLE 34;42    
   ```
   该行设置了所有others具有写权限的目录，在ls输出中所显示的样式。    
   经过我的测试，我发现黄色字体，绿色背景的的文件名看起来较为清楚，因此在 **~/.dircolors** 文件中将上面的那行修改为如下:  

   ```
   OTHER_WRITABLE 33;42
   ```
   修改完成后，保存文件并退出。 
4. 使修改生效     
   配置文件修改完后，需要执行下面的命令来使修改生效:   

   ```
   $ eval $(dircolors ~/.dircolors)
   ```

完成上面的操作后, 在使用ls输出others具有写权限的目录时, 就会发现目录名变为黄色字体、绿色背景的了，示例图如下:   

![清楚的文件名](https://farm5.staticflickr.com/4312/35948942411_1a63d5b8d2_o.png)    
   

<!--
Reference: 
[Fix LS Colors for directories with 777 permission?](https://unix.stackexchange.com/questions/241726/fix-ls-colors-for-directories-with-777-permission)
[What causes this green background in ls output?](https://unix.stackexchange.com/questions/94498/what-causes-this-green-background-in-ls-output)
-->   
