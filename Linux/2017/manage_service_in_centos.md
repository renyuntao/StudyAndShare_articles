# 如何在CentOS 6中管理服务?
<!--
2017-02-11
--><br /><br />

基于Red Hat的Linux发行版(如CentOS)中有两个可以管理服务的工具: **service**和**chkconfig**。 

**service**可以在系统运行的过程中控制服务的启动、停止，以及获取服务的当前状态。   

**chkconfig**可以控制在不同的[Runlevel](https://en.wikipedia.org/wiki/Runlevel)下，管理服务是否在操作系统启动的时候自动启动, 以及获取服务的当前状态。

每当你在**Red Hat**, **Fedora**或**CentOS**中安装一项服务(如**nginx**,**vsftpd**等)，安装完成后，这些服务都不会自动启动，同样，在系统重启时，这些服务仍然不会自动启动，想要在系统运行过程中启动、停止一项服务，你需要使用**service**, 而如果想要在系统启动时自动启动一项服务，你需要使用**chkconfig**, 下面将介绍这两个工具的用法。    

注意: 本文中的命令都在**CentOS 6**中执行并测试通过, 并且命令的执行都需要root权限。    

## service

**service**可以在系统运行的过程中控制服务的启动、停止，以及获取服务的当前状态, 其基本用法如下: 

```bash
service [ServiceName] [start|stop|status|...]
```
其中，**ServiceName** 表示要操作的服务的名称, **start**, **stop**, **status** 表示要对指定服务进行的具体操作。    


下面将使用 **vsftpd** 服务作为例子来展示 **service** 的用法。    

1. **获取指定服务所有可进行的操作**

   通常，对一项服务可进行的操作都包含 **start**, **stop**, **status** 这三个操作，一些服务还提供了一些其它的操作，要获取所有可用的操作，可以使用如下命令:   

   ```bash
   # service [ServiceName]
   ```
   例如要获取 **vsftpd** 服务所有可用的操作，可以执行如下命令:   

   ```bash
   # service vsftpd
   ```

2. **启动服务**  
   启动服务可以执行下面的命令:  

   ```bash
   # service [ServiceName] start
   ```
   启动 **vsftpd** 服务的命令如下: 

   ```bash
   # service vsftpd start
   ```
3. **停止服务**   
   停止服务需要执行下面的命令:   

   ```bash
   # service [ServiceName] stop
   ```
   停止 **vsftpd** 服务的命令如下:  

   ```bash
   # service vsftpd stop
   ```
4. **获取服务的当前状态**    
   要获取一项服务的当前状态，需要执行如下命令:

   ```bash
   # service [ServiceName] status
   ```
   获取 **vsftpd** 服务的当前状态的命令如下:   

   ```bash
   # service vsftpd status
   ```

你可以在终端执行 **man service** 命令来获取更多关于 **service** 用法的信息。    

## chkconfig

虽然你可以使用 **service** 来启动一项服务，但是系统在重启之后，你之前启动的服务仍然不会启动，因此，在每次系统重启后，你都需要重新使用 **service** 命令来启动你想要启动的服务，如果你不想要做这样重复的工作，这时候就是 **chkconfig** 的用武之地了, **chkconfig**可以控制在不同的[Runlevel](https://en.wikipedia.org/wiki/Runlevel)下，管理服务是否在操作系统启动的时候自动启动。   

首先，你可以执行如下命令来查看 **chkconfig** 所管理的所有服务及其状态:   

```bash
# chkconfig
```
或   

```bash
# chkconfig --list
```
执行上面的命令后，会输出类似下面的内容:   

```
crond           0:关闭  1:关闭  2:关闭  3:关闭  4:关闭  5:关闭  6:关闭
htcacheclean    0:关闭  1:关闭  2:关闭  3:关闭  4:关闭  5:关闭  6:关闭
httpd           0:关闭  1:关闭  2:关闭  3:关闭  4:关闭  5:关闭  6:关闭
sshd            0:关闭  1:关闭  2:启用  3:启用  4:启用  5:启用  6:关闭
...
```
输出结果中，第一列表示 **chkconfig** 所管理的服务名称，之后的7列中的数字表示不同的[Runlevel](https://en.wikipedia.org/wiki/Runlevel), **Runlevel** 后面跟的 **关闭** 和 **启用** 表示对应服务在该 **Runlevel** 下，是否在系统启动时自动启动，**关闭** 表示不自动启动服务，**启用** 表示自动启动服务。    

以上面输出中的 **sshd** 服务为例，该服务在 2,3,4,5 Runlevel下，在系统启动时自动启动服务，而在其它Runlevel下，在系统启动时不自动启动服务。   

以 **httpd** 服务为例，假如现在想要在 3,5 Runlevel下，在系统启动时自动启动 **httpd** 服务，则可以执行如下命令:   

```bash
# chkconfig --level 35 httpd on
```
而如果想要在 4,5 Runlevel下，在系统启动时不自动启动 **sshd** 服务，则可以执行下面的命令:   

```bash
# chkconfig --level 45 sshd off
```

关于 **chkconfig** 的其它用法，你可以在终端执行 **man chkconfig** 来查看。    

## 结束语

本文介绍了在CentOS 6中管理服务的两个工具 —— **service** 和 **chkconfig**。**service** 可以在系统运行过程中对服务进行管理，而 **chkconfig** 则管理服务在系统启动阶段是否自启动，恰当地使用这两个工具，可以使我们能够更好地掌控系统中的服务，并减少重复性的工作。

<!--
Reference:
[rpm-based](http://www.rpm-based.org/how-to-manage-services-with-chkconfig-and-service)
[UNIX & Linux](http://unix.stackexchange.com/questions/20357/how-can-i-make-a-script-in-etc-init-d-start-at-boot)
[Stack Overflow](http://stackoverflow.com/questions/5133552/service-doesnt-support-chkconfig)
[ServerFault](http://serverfault.com/questions/176055/how-to-change-linux-services-startup-boot-order)
-->
