# 如何在CentOS6中使用chkconfig管理自己写的程序?
<!--
2017-03-04
--><br /><br />

我在[如何在CentOS6中管理服务?](manage_service_in_centos.html)一文中，讲解了**chkconfig**的用法，并以**httpd**服务作为例子，介绍了如何使用**chkconfig**来管理**httpd**服务。**chkconfig**在系统中所有管理的服务可以使用 **chkconfig --list** 来查看，这些服务大多是系统自带的一些服务和安装的服务器端服务，有些时候，我们可能想要在开机时自动运行一些自己写的程序，用来管理自己的系统，此时也可以借助**chkconfig**这个工具，在下面的篇幅中，将介绍如何使用让**chkconfig**来管理我们自己写的程序。    

注意: 本文所进行的所有操作都需要使用root权限来执行    

要使用**chkconfig**来管理我们自己的脚本，需要按照下面的步骤来进行:   

1. 在 **/etc/init.d/** 目录下创建一个文件        
   在 **/etc/init.d/** 目录下创建一个文件, 这个文件的名称可以随便起，名称只是辨识不同的服务或程序，不过这个名称最好起的有意义一些，这样，我们看到这个文件名就知道这个服务或程序是用来做什么的。由于只是作为例子演示，所以这里就起一个并没有什么实际意义的名称 —— **myscript**:   

   ```bash
   # touch myscript
   ```
2. 创建好文件后，在该文件中添加下面的内容:

   ```bash
   #!/bin/bash
   # chkconfig: 2345 20 80
   
   start() {
       # 在此处写启动你的程序的命令
   }
   
   stop() {
       # 在此处写杀死你的程序的命令
   }

   restart() {
       # 在此处写重启你的程序的命令
   }

   case "$1" in 
       start)
          start
          ;;
       stop)
          stop
          ;;
       restart)
          stop
          start
          ;;
       *)
          echo "Usage: $0 {start|stop|restart}"
   esac
   
   exit 0 
   ```
   **注意:** 上面代码片段的第二行注释 `# chkconfig: 2345 20 80` 并不是可有可无的，而是必要的，其中2345表示要管理的程序在2,3,4,5[Runlevel](https://en.wikipedia.org/wiki/Runlevel)下开机时自动启动, 你可以修改这个值，来让你的程序在你想要的Runlevel下开机自启动; 20和80分别表示开机启动程序和关机杀死程序的顺序, 由于通常我们写的程序对系统的开关机基本没有影响，因此这个顺序并不十分重要。
3. 给文件添加可执行权限           
   上面的两步完成后，接下来就要给文件添加可执行权限了，由于我在第一步中创建的文件名为**myscript**, 因此，我需要执行的是下面的命令:   

   ```bash
   # cd /etc/init.d/
   # chmod +x myscript
   ```
   你需要将上面命令中的**myscript**改为你在/etc/init.d/目录下所创建的文件名。   
4. 告知chkconfig来管理我们自己写的程序          
   要告知chkconfig来管理我们自己写的程序，只需执行如下格式的命令即可:

   ```bash
   # chkconfig --add [Name]
   ```
   其中**Name**为你在**/etc/init.d/**目录下创建的文件的名称，由于我在第一步中创建的文件名称为**myscript**, 因此我所要执行的命令是下面这个样子的:   

   ```bash
   # chkconfig --add myscript
   ```

完成上面4个步骤之后，**chkconfig**就开始管理我们自己写的程序了，此时, 通过执行 **chkconfig --list** 命令，你会发现你的程序已经在**chkconfig**的管理列表中了。

## 结束语

**chkconfig** 这个工具不仅可以管理系统自带服务的开机自启动，也可以管理我们自己写的程序。这篇文章中介绍了如何将自己写的程序交由**chkconfig**管理，通过**chkconfig**的管理，我们可以在开机时自动运行一些我们自己写的程序，从而更好地管理我们的系统。

<!--
[UNIX & Linux](http://unix.stackexchange.com/questions/20357/how-can-i-make-a-script-in-etc-init-d-start-at-boot)
[Stack Overflow](http://stackoverflow.com/questions/5133552/service-doesnt-support-chkconfig)
-->
