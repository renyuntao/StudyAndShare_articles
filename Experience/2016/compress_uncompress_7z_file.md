# 如何在Linux中解压 7zip 文件?              
2016-05-01  <br /><br />               

在Linux平台下解压 *.7z 文件(7zip 文件)，可以使用命令 `p7zip`。            
首先检查你的系统中是否已经安装有 `p7zip` 这个工具:               
        
```bash
$ which p7zip
```
执行上述命令后，如果输出了 `p7zip` 的绝对路径，则表明你的系统中已经安装了此工具,你可以直接跳过 **安装 p7zip** 这一部分内容；如果上述命令没有输出任何信息，则表明你的系统中没有安装 `p7zip`，那么你首先需要安装该工具。             

## 安装 p7zip
在Ubuntu中，你可以执行下面的命令来安装 `p7zip`:           

```bash
$ sudo apt-get install p7zip
```
对于 Red Hat 系列的Linux系统，则使用下面的命令来安装:          
        
```bash
$ yum install p7zip
```
安装完毕后，就可以继续看下面的内容了

## p7zip 使用示例
### 压缩文件           
压缩文件很简单，直接将想要压缩的文件作为 `p7zip` 的参数即可。如下:            
         
```bash
$ p7zip example.txt
```
执行上述命令后，会产生一个名为 `example.txt.7z` 的压缩文件.                  
### 解压文件           
解压文件和压缩文件的方法很类似，只是多了一个选项 `-d`, 假如现在想要解压上面的 `example.txt.7z` 文件, 可以使用下面的命令:            
        
```bash
$ p7zip -d example.txt.7z
```
关于 `p7zip` 的更多信息，你可以通过 `man p7zip` 来查看.             

