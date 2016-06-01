# 如何在 Ubuntu 14.04中 enable/disable 一个 service?
2016-05-29  <br /><br />              
          
&nbsp;&nbsp;&nbsp;&nbsp;目前，在 Ubuntu 中有三种方法控制一个 Service 是否在开机时自动启动 —— **[SysV](https://en.wikipedia.org/wiki/Init#SysV-style)**, **[Upstart](https://en.wikipedia.org/wiki/Upstart)** 和 **[Systemd](https://en.wikipedia.org/wiki/Systemd)**。其中 **Systemd** 是在 Ubuntu 15.04 才引入 Ubuntu 中，因此在本文中对此不做介绍，下面将介绍如何使用 **SysV** 和 **Upstart** 来控制一个 Service 在开机或重启时是否自动启动。　                 
              
## SysV
&nbsp;&nbsp;&nbsp;&nbsp;使用 **SysV** 来控制的 Service 的脚本都是放置在 `/etc/init.d/` 目录下的，之后使用 `update-rc.d` 这个工具来控制相应 Service 的 enable 与 disable.  
下面来查看一下 `/etc/init.d/` 目录下的内容:              
         
```bash
$ ls /etc/init.d/
lightdm     procps      README      saned              stunnel4  umountnfs.sh
networking  pulseaudio  reboot      sendsigs           sudo      umountroot
ondemand    rc          resolvconf  single             thermald  unattended-upgrades
openvpn     rc.local    rsync       skeleton           udev      urandom
pppd-dns    rcS         rsyslog     speech-dispatcher  umountfs  x11-common
```
上面这些都是可以使用 `update-rc.d` 来控制 enable/disable 的 Service, 下面介绍如何使用 `update-rc.d` 来控制这些 Service。           
### Disable 一个 service     
Disable 一个 service, 即是在开机或者重启机器时不要启动这个 service, 要 disable 一个 service, 应该按照如下的格式使用 `update-rc.d`:           
         
```bash
$ sudo update-rc.d -f service_name remove
```
其中, `service_name` 为要 disable 的 service 的名称，例如, 要 disable 掉 stunnel4 这个 service, 应该使用如下的命令:            
        
```bash
$ sudo update-rc.d -f stunnel4 remove
```
**缺点:** 使用这种方法 disable 掉一个 service，当使用 apt-get 对这个 service 的软件包进行更新后，上面的操作就会失效，即在下次开机或者重启机器时，这个 service 又会自动启动。               
**解决方法:** 在最近的 `update-rc.d` 版本中，`update-rc.d` 新增了两个参数 —— **enable** 和 **disable**, 使用 **disable** 参数即可 disable 一个 service, 并且即使这个 service 的软件包在之后进行了更新，其状态也不会变为 enable:           
       
```bash
$ sudo update-rc.d stunnel4 disable
```

### Enable 一个 service
Enable 一个 service, 既是在开机或者重启机器的时候，自动启动这个 service。要 enable 一个 service, 应该按照如下的格式使用 `update-rc.d`:     
      
```bash
$ sudo update-rc.d service_name defaults
```
其中，`service_name` 为要 enable 的 service 的名称；另外，关键字 `defaults` 表示在 **[runlevel](https://en.wikipedia.org/wiki/Runlevel)** 为 **2,3,4,5** 时 enable 这个 service, 而在 **runlevel** 为 **0,1,6** 时 disable 这个 service, 因为我们通常使用的 **runlevel** 都是在 **2-5** 之间，所以使用 `defaults` 关键字就可以 enable 我们想要的 service 了。下面举例说明，假如要 enable stunnel4 这个 service, 应该使用如下的命令:           
          
```bash
$ sudo update-rc.d stunnel4 defaults
```
如果执行上述命令时得到如下错误:            
      
```bash
System start/stop links for /etc/init.d/stunnel4 already exist.
```
则改用如下命令即可:              
           
```bash
sudo update-rc.d stunnel4 enable
```
**update-rc.d** 还有很多功能，例如，它可以精确地控制一个 service 在何种 **runlevel** 下 enable 或 disable, 甚至还可以控制 service 启动的优先级，如果你对此感兴趣的话，可以通过 `man update-rc.d` 来查看 `update-rc.d` 的更多功能。               

## Upstart
&nbsp;&nbsp;&nbsp;&nbsp;使用 **Upstart** 来控制的 service 都在 `/etc/init/` 目录下放置有相应的 `.conf` 配置文件。由于在 Ubuntu 14.04 中使用 **Upstart** 来控制的 service 比较少，因此，对于一个 service, 如果你不确定其是否有 **Upstart** 来管控，可以使用下面格式的命令来进行查看:            
        
```bash
$ initctl status service_name
```
或      

```bash
$ status service_name
```
如果一个 service 不是由 **Upstart** 来管控，将会提示如下的错误:             
            
```bash
status: Unknown job: service_name
```
其中，`service_name` 为要查看的 service 的名称。      
在确定了某个 service 是由 **Upstart** 来管控之后，就可以使用下面的方法来 enable/disable 这个 service 了.           

### Disable 一个 service
要 Disable 一个有 **Upstart** 管控的 service 十分简单，只要按照如下格式执行命令即可:                
         
```bash
echo "manual" | sudo tee /etc/init/service_name.override
```
其中，`service_name` 为要 disable 掉的 service 的名称。　　　　　              
上面的命令只是在 `/etc/init/` 目录下添加一个 `service_name.override` 文件，并向该文件中输入内容 `manual`。这样就可以 disable 掉 service_name 这个 service 了(**助记:英文 manual 表示手工，暗示了该 service 为手工启动，而不是在开机或重启机器时自动启动**).       
下面举个例子，假如要 disable 掉 squid3 这个 service, 可以执行如下命令:               
             
```bash
$ echo "manual" | sudo tee /etc/init/squid3.override
```

### Enable 一个 service
&nbsp;&nbsp;&nbsp;&nbsp;要 Enable 一个 service 更加简单，只要将 `/etc/init/` 目录下的 `.override` 文件删除掉即可，上面的例子中，为了 disable 掉 squid3 这个 service, 在 `/etc/` 目录下新建了一个文件 `squid3.override`, 现在想要 enable squid3 这个 service, 直接将文件 `squid3.override` 删除掉就可以了:           
          
```bash
$ sudo rm /etc/init/squid3.override
```

## 临时 enable/disable 一个 service 
&nbsp;&nbsp;&nbsp;&nbsp;上面所介绍的方法都是永久性的 enable/disable 一个 service, 如果只是想临时 enable/disable 一个 service, 就要使用 `service` 这个命令了。`service` 这个工具的一个优点是，无论是对于 **SysV** 的脚本，还是对于 **Upstart** 的 job,都可以使用 `service` 来统一的管理。                
使用 `service` 来 enable 一个 service:            
         
```bash
$ sudo service service_name start
```
使用 `service` 来 disable 一个 service:              
      
```bash
$ sudo service service_name stop
```
其中 `service_name` 为要 enable 或 disable 掉的 service 的名称。              
另外，如果你想要 enable/disable 的 service 由 **Upstart** 管控，你还可以使用 **Upstart** 风格的管理方法:             
            
- **start** - Enable service: `sudo initctl start service_name` 或 `$ sudo start service_name`
- **stop** - Disable service: `sudo initctl stop servcie_name` 或 `sudo stop service_name`

其中，`service_name` 为要 enable/disable 掉的 service 的名称。

参考:         
AskUbuntu: **[How to enable or disable services?](http://askubuntu.com/questions/19320/how-to-enable-or-disable-services)**        
AskUbuntu: **[How can I configure a service to run at startup](http://askubuntu.com/questions/9382/how-can-i-configure-a-service-to-run-at-startup)**  
Debuntu: **[How-To: Managing Services With Update-Rc.D](https://www.debuntu.org/how-to-managing-services-with-update-rc-d/)**        
DigitalOcean: **[The Upstart Event System: What It Is And How To Use It](https://www.digitalocean.com/community/tutorials/the-upstart-event-system-what-it-is-and-how-to-use-it)**            
