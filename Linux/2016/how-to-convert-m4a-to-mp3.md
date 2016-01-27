# Linux:如何将m4a文件转换为mp3文件?
2016-01-27  <br /><br />            

&nbsp;&nbsp;&nbsp;寒假回家后，空闲时间就多了起来。由于比较喜欢听小说，于是就下载了一些有声小说，打算在闲暇之时听听。然而下载下来后，发现这些音频文件并非常见的 `mp3` 格式，而是 `m4a` 格式的音频文件.我在Ubuntu中常使用 `mpg123` 来播放一些音频文件，当我使用 `mpg123` 播放这些音频文件时，才发现 `mpg123` 并不支持 `m4a` 格式的音频文件,所以我只好将这些 `m4a` 格式的文件转换为常见的 `mp3` 格式文件,下面是我转换这些文件的方法。           

我是使用 `avconv` 来转换这些文件的格式的，Ubuntu中默认没有安装这个工具，你可以通过执行如下命令来安装它:             

    $ sudo apt-get update
    $ sudo apt-get install libav-tools

安装完毕后，就可以使用它来转换音频文件的格式了。我所下载的 `m4a` 格式的音频文件如下格式:              

> GhostBlow1.m4a    
> GhostBlow2.m4a    
> GhostBlow3.m4a         
> ...          
> GhostBlow57.m4a            

假如现在要将 `GhostBlow1.m4a` 转换为 `mp3` 格式,则可以执行如下命令:             

    $ avconv -i GhostBlow1.m4a GhostBlow1.mp3

&nbsp;&nbsp;&nbsp;上述命令中 `-i` 参数表示输入文件，其后接要转换的文件，本例中即为 `GhostBlow1.m4a`; 末尾的 `GhostBlow1.mp3` 表示的是输出文件名,即将转换完毕的文件保存在 `GhostBlow1.mp3` 文件中。                
&nbsp;&nbsp;&nbsp;虽然转换成功了，但是我一共有57个这样的 `m4a` 文件，如果手工对这些文件一一转换，就要输入57次类似上述的命令，这样实在是太麻烦了。于是就写了一个shell脚本，来代替我完成重复而又无趣的工作,脚本内容如下:            

	#!/usr/bin/env bash

	for inputFile in *m4a
	do
		avconv -i $inputFile ${inputFile%.m4a}.mp3
	done

这个shell脚本保存在文件 `convert.sh` 中，将该脚本与 `m4a` 文件放到同一目录下，之后执行如下命令即可对所有的 `m4a` 文件执行转换操作:            

	$ chmod u+x ./convert.sh
	$ ./convert.sh &> /dev/null &

&nbsp;&nbsp;&nbsp;对这57个 `m4a` 格式的文件进行转换操作需要花费一定的时间，为了避免影响我在终端做其他事情，我将上述脚本放到了后台去运行,并且将输出到屏幕的信息重定向到了 `/dev/null` 中了,这样就避免了输出到屏幕的信息干扰前台的工作了。              
&nbsp;&nbsp;&nbsp;然而这样做仍然有一个问题，因为 `avconv` 这个工具十分耗费CPU，当使用 `avconv` 来转换文件格式的时候，`avconv` 的CPU使用率经常会达到 `100%`,这使得系统的负载常常很高。我解决这个问题的办法就是使用 `cpulimit` 来限制 `avconv` 的CPU使用率。当执行了上述的两条命令后，紧接着可以执行下面的命令来限制 `avconv` 的CPU使用率:   

    $ cpulimit -l 40 -e avconv -b

&nbsp;&nbsp;&nbsp;上述命令中，`cpulimit` 的 `-l` 参数指定要将进程的CPU使用率限制在什么范围内，这里我将 `avconv` 的CPU使用率限制在了40%以内，`-e` 参数指定要限制的进程的名称，`-b` 参数表示将 `cpulimit` 放在后台运行.关于 `cpulimit` 的用法，我之前写过一篇文章 [Linux:如何限制进程的CPU使用率?](http://www.studyandshare.info/how_to_limit_cpu_usage.html) ,你可以查看那篇文章来了解更多关于 `cpulimit` 的信息.             

