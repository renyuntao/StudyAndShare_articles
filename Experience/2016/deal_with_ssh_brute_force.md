# 应对SSH Brute Force的7种方法
2016-04-24  <br /><br />             
             
攻击者不断地向被攻击主机发送SSH连接请求，并以 **username/password** 对来尝试，旨在登录到被攻击主机，这就是所谓的 **SSH Brute Force**,本文将介绍应对 **SSH Brute Force** 的7种方法。          
## 方法 #1:改变SSH服务的监听端口            
&nbsp;&nbsp;&nbsp;SSH服务的默认监听端口号为 22,很多攻击者可能会批量扫描主机的的特定端口，比如查看SSH服务的22号端口是否开放，如果发现某些主机的22号端口开放，攻击者可能就会对这些主机采取攻击行为。因此，改变SSH服务的监听端口号可以在一定程度上减小主机受到攻击的可能性。               
改变SSH服务的监听端口号，可以在远程主机上使用vim打开配置文件 `/etc/ssh/sshd_config`:              
          
```bash
# vim /etc/ssh/sshd_config
```
之后在文件中找到下面这行:           
      
> Port 22

将22号端口改为其它的端口号，这里以端口号495为例,如下:            
         
> Port 495

保存文件并退出，执行如下命令重启SSH服务使上面的修改生效:             
          
```bash 
# service ssh restart
```
接下来，你可以在本地再次尝试使用SSH来登录到远程主机，不过由于我们改变了SSH服务的默认监听端口号，因此需要使用 `ssh` 的 `-p` 选项来指定要使用的端口号:            
         
```
# ssh -v username@IP -p Port
```

## 方法 #2:不输密码登录远程主机            
&nbsp;&nbsp;&nbsp;在使用 `ssh` 登录远程主机时，你需要输入相应用户的密码，ssh会对你的密码进行加密后发往远程主机。虽然密码经过加密，但仍然有可能被黑客拦截并解密，这样就会导致你的密码的泄漏。一个更加安全的方法是在本地使用 `ssh-keygen` 来产生一对密钥(公钥与私钥)，然后将公钥放到远程主机上。之后当你在本地使用 `ssh` 连接远程主机时，就可以无需输入密码而直接登录了。                 
关于这种方法的具体实施步骤，可以参见下面的链接:               
**[SSH:如何无需输入密码登录远程主机?](http://www.studyandshare.info/how_to_login_remote_without_passwd.html)**             
           
## 方法 #3:禁止root登录   
&nbsp;&nbsp;&nbsp;在默认情况下，SSH服务允许任何用户登录，当然也包括root。然而很多攻击者在发现你的主机的22号端口开放时，就会不断的尝试你的root用户的密码(**SSH Brute Force**)，如果你查看你的远程主机SSH服务的日志文件(Ubuntu中可以查看 `/etc/log/auth.log`),你可能会发现很多类似下面的记录:             

> ... Failed password for root ...

&nbsp;&nbsp;&nbsp;这就是有攻击者在不断的尝试你的 root 用户的密码。因此禁用 root 用户的登录可以提高你的远程主机的安全性，当你需要使用root用户时，可以从普通用户通过 `su` 来转变为 root 用户。                
要禁止 root 用户登录，首先使用你喜欢的编辑器打开SSH服务的配置文件 `/etc/ssh/sshd_config`:

```bash
# vim /etc/ssh/sshd_config
```
之后在文件中找到下面一行:            
 
> PermitRootLogin yes

将该行中的 `yes` 改为 `no`:            
       
> PermitRootLogin no

保存文件并退出，执行如下命令重启SSH服务使上面的修改生效:               
         
```bash
# service ssh restart
```
此后，如果你再次尝试使用root登录，就会发现不能够登录了。                 

## 方法 #4:禁止不必要的用户登录  
&nbsp;&nbsp;&nbsp;在 **方法 #3** 中介绍了如何禁止root用户登录，其实，当你的远程主机中有多个用户的时候，应该只允许必要的用户有登录远程主机的权限，而禁止不必要的用户登录远程主机，这样也可以增强你的远程主机的安全性。            
要禁止非root用户登录到远程主机，首先使用你喜欢的编辑器打开SSH服务的配置文件:          
         
```bash
# vim /etc/ssh/sshd_config
```
之后在文件的末尾添加允许登录远程主机的用户(**AllowUsers**)列表,作为例子，这里允许 `user1` 和 `user2` 这两个用户登录到远程主机:            

> AllowUsers  user1 user2

未在 **AllowUsers** 列表中的用户将被禁止登录到远程主机。            
之后重启SSH服务使修改生效.

## 方法 #5:添加防火墙
&nbsp;&nbsp;&nbsp;通过查看SSH服务的日志文件(Ubuntu中可以查看文件`/etc/log/auth.log`),可以找到那些不断登录你的远程主机的IP，你可以将这些IP加入到防火墙的屏蔽规则中，从而屏蔽掉来自这些IP的SSH连接请求。                     
**[UFW](https://help.ubuntu.com/community/UFW)** 是一个简单易用的防火墙管理工具，你可以使用它来屏蔽掉来自某些特定IP的数据包，例如，你想要屏蔽到来自 **56.104.78.120** 的数据包，可以使用下面的命令:               
            
```bash
# ufw deny from 56.104.78.120
```
关于 **UFW** 的详细用法，你可以参考下面的链接:                    
[https://help.ubuntu.com/community/UFW](https://help.ubuntu.com/community/UFW)                    
                        
&nbsp;&nbsp;&nbsp;此外，攻击者常常会使用很多个IP来对你的远程主机发起攻击，这样手动地添加防火墙规则太过于繁琐，因此，你可以写一个Shell脚本，通过分析SSH服务的日志文件(Ubuntu中为 `/var/log/auth.log`)来筛选出那些恶意IP,之后将它们自动添加到防火墙的屏蔽列表中。               
这里以Ubuntu系统为例，下面是一个实现上述功能的Shell脚本示例:

```bash
#!/bin/bash

tail -n 100 /var/log/auth.log | grep -E '(Failed password|Invalid user)' | 
grep -Eo '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' | 
sort | uniq -c | sort -nr | awk '{if($1>3)print $2}' | 
xargs -n 1 ufw insert 2 deny from
```
利用Linux系统中的 `CRON` 服务每隔一定时间将上述脚本自动运行一次，即可定时检测是否存在恶意IP的攻击，如果存在，则将它们加入到防火墙的屏蔽列表中去.

## 方法 #6:借助入侵检测/预防工具              
**DenyHosts** 和 **Fail2Ban** 是两个很流行的入侵检测/预防工具，它们可以通过分析相应的日志文件来检测出恶意IP，进而屏蔽掉相应的IP.          
**参考资料:**              

[Install DenyHosts to Block SSH Server Attacks in RHEL / CentOS / Fedora](http://www.tecmint.com/block-ssh-server-attacks-brute-force-attacks-using-denyhosts/)              
[Install Fail2ban to Prevent SSH Server Attacks in RHEL / CentOS / Fedora](http://www.tecmint.com/install-fail2ban-on-rhel-centos-fedora/)      

## 方法 #7:显示警告信息            
这个方法对于提升远程主机并没有实质性的作用，只是对入侵者的一个警告作用。                     
警告信息在SSH登录前以及登录后都可以显示。在SSH登录前显示的信息使用的是文件 `/etc/issue.net`,而在SSH登录后显示的信息则是使用的文件 `/etc/motd`.另外，警告信息最好设置为英文，因为攻击者很可能来自国外。               
### 设置SSH登录前的显示信息            
要设置SSH登录前的显示信息，首先用你喜爱的编辑器打开文件 `/etc/issue.net`:            
       
```bash
# vim /etc/issue.net
```
之后在该文件里面编辑你想要在SSH登录前显示的警告信息。下面是一个警告信息的示例:           

``` 
#################################################################
#                          ALERT!                               #
#  You are entering into a secured area! Your IP, Login Time,   #
#  Username has been noted and has been sent to the server      #
#  administrator!                                               #
#  This service is restricted to authorized users only. All     #
#  activities on this system are logged.                        #
#  Unauthorized access will be fully investigated and reported  #
#  to the appropriate law enforcement agencies.                 #
#                                                               #
#################################################################
```
保存文件并退出，之后打开SSH服务的配置文件 `/etc/ssh/sshd_config`:

```bash
# vim /etc/ssh/sshd_config
```
在该文件中找到 **Banner** 这个单词,如下:              

> \# Banner /some/path

去掉该行的注释，然后将 `/some/path` 替换为 `/etc/issue.net`,修改完毕后，应该是下面这个样子:                
     
> Banner /etc/issue.net

保存文件并退出，执行如下命令重启SSH服务是上述修改生效:               
       
```bash
# service ssh restart
```
在你下次登录SSH之前就会看到终端上显示文件 `/etc/issue.net` 中的内容.                 
### 设置SSH登录后的显示信息               
要设置SSH登录后显示的内容，首先用你喜爱的编辑器打开文件 `/etc/motd`:               
          
```bash
# vim /etc/motd
```
之后，在该文件中编辑你想要在SSH登录后显示的警告信息。下面是一个警告信息的示例:             
            
```bash
#############################################################
#              Welcome to StudyAndShare.info                #
#       All connections are monitored and recorded          #
# Disconnect IMMEDIATELY if you are not an authorized user! #
#############################################################
```
保存文件并退出，在你下次登录到远程主机之后，就会看到上面显示的内容了。                  


<!-- 
http://www.tecmint.com/disable-or-enable-ssh-root-login-and-limit-ssh-access-in-linux/#
http://www.tecmint.com/protect-ssh-logins-with-ssh-motd-banner-messages/#
http://www.tecmint.com/5-best-practices-to-secure-and-protect-ssh-server/
-->
