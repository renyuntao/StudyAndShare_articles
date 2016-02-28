# VPS:如何定时自动备份数据?             
2016-02-24  <br /><br />           

&nbsp;&nbsp;&nbsp;在VPS上常常有一些重要的数据，为了数据安全的缘故，经常对VPS上的数据进行备份是一个很好的主意。然而手动进行备份，不仅十分麻烦，而且常常忘记。因此，本文分享一下我个人备份VPS上数据的经验。            

避免每次备份数据都要进行繁琐操作的方法是，将备份操作写入 Shell 脚本，之后运行这个脚本，即可备份VPS上的数据，从而免除了备份数据时的繁琐操作。    

## 备份数据

备份数据时常常会用到 `tar` 这个工具,如何你不熟悉这个工具，可以参考下面的这篇文章来学习其用法:              
[18 Tar Command Examples in Linux](http://www.tecmint.com/18-tar-command-examples-in-linux/)                           
<br />                

此外，在备份数据时，常常还要备份数据库中的数据，此时就要在 Shell Script 中哦该执行一些 SQL 语句。我之前写过一篇文章介绍了如何在 Shell Script 中执行MySQL 语句，你可以点击下面的链接查看:                 
[MySQL:如何在Shell Script中执行MySQL命令?](http://www.studyandshare.info/execute_mysql_cmd_in_shell.html)          

## 定时自动运行备份脚本             

&nbsp;&nbsp;&nbsp;在将所有的备份操作写入 Shell 脚本后，接下来的工作就是运行该脚本来备份数据了。然而运行这个脚本仍需要我们手工来完成，有没有一种可以定时自动运行该脚本的方法呢？这就需要用到 Linux 系统中的 `Cron` 服务了。`Cron` 可以通过查看 `/var/spool/cron/crontabs/` 目录下的相关文件来定时执行一些操作，如果我们想要定时执行一些操作，就要用到 `crontab` 这个工具，通过它，我们可以将想要定时进行的操作加入到 `/var/spool/cron/crontabs/` 目录下的相关文件中，`Cron` 再通过查看这些文件来定时执行我们想要执行的操作。                  
现在假设将备份的操作写入到了一个名为 `backup.sh` 文件中，如果我们想要在每天的 `00:00:00` 进行备份操作，则可以按照如下步骤操作:          

1. 在终端执行如下命令:           

	```bash
	$ chmod u+x backup.sh
	$ crontab -e
	```
2. 之后会打开一个文件，文件内容如下:              

	> \# Edit this file to introduce tasks to be run by cron.          
	> \#              
	> \# Each task to run has to be defined through a single line              
	> \# indicating with different fields when the task will be run                 
	> \# and what command to run for the task                
	> \#                                                                 
	> \# To define the time you can provide concrete values for              
	> \# minute (m), hour (h), day of month (dom), month (mon),               
	> \# and day of week (dow) or use '*' in these fields (for 'any').#             
	> \# Notice that tasks will be started based on the cron's system              
	> \# daemon's notion of time and timezones.                     
	> \#                                                                    
	> \# Output of the crontab jobs (including errors) is sent through                  
	> \# email to the user the crontab file belongs to (unless redirected).                 
	> \#                                                                
	> \# For example, you can run a backup of all your user accounts             
	> \# at 5 a.m every week with:                             
	> \# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/                
	> \#                                                                        
	> \# For more information see the manual pages of crontab(5) and cron(8)                  
	> \#                                    
	> \# m h  dom mon dow   command                       

3. 在文件的末尾添加如下内容:               

	```bash
	0 0 * * * /path/to/backup.sh
	```
	上述语句包含6个域，从左到右每个域表示的含义分别为:           

	> 分 时 日 月 星期 命令

	语句中的星号表示 **每一**

4. 保存文件退出            

之后 `backup.sh` 这个脚本就会在每天的0点0分执行，从而备份了VPS上的数据。                 

关于 `Cron` 的详细用法的可以参考下面的文章:                   
[Linux and Unix crontab command](http://www.computerhope.com/unix/ucrontab.htm)                

## 通过Email发送备份的数据               
&nbsp;&nbsp;&nbsp;虽然上面的操作已经实现了定时备份VPS上的数据，但是备份的数据仍然是放在VPS上的，如果万一VPS遇到什么意外，备份的数据可能会丢失。因此一个保险的方法是将备份好的数据放到别处。此时可以通过在命令行中发送Email，将备份好的数据作为Email的附件发送到你的电子邮箱里。使用命令行来发送Email可以使用 `mail` 这个工具，假设你要将一个名为 `backup.tar.bz2` 的文件发送到你的邮箱里，可以执行如下命令:                

```bash
$ mail -s "Backup data" -a /path/to/backup.tar.bz2 username@domain < MailMessage.txt            
```

上述命令中 `-s` 选项指定Email的主题，`-a` 选项指定要发送的文件， `username@domain` 表示你的邮箱地址，`MailMessage.txt` 文件中包含了Email的内容。  
将上述命令添加到你的备份脚本中，即可在备份数据完毕后，将备份好的数据发送到你的邮箱中去。               
关于 `mail` 的详细用法，你可以参考下面的这篇文章:                
[Send Gmail from the Linux Command Line](http://tuxtweaks.com/2012/10/send-gmail-from-the-linux-command-line/)
