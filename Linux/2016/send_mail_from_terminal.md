# 如何在 Linux 终端发送E-mail?
<!--
2016-12-10
--><br /><br />

我曾在 [VPS:如何定时自动备份数据?][1] 一文中提到过一种借助 [Gmail][2] 来在Linux终端发送 E-mail 的方法，不过由于朝廷害怕自己的无数见不得人的事被大陆人民知道，威胁到自己的执政地位，于是就把 Google 封锁了，这也就导致大多数的大陆人民无法使用 Gmail, 因此，在这篇文章中，将介绍一种借助国内的邮箱来在 Linux 终端发送 E-mail 的方法。在继续下面的内容之前，这里先作一个说明，这篇文章中所介绍的方法，是在 Ubuntu 14.04 和 Ubuntu 16.04 中进行测试的，虽然如此，在其他的 Linux 发行版中，操作的方法都是类似的。     

推荐阅读: [VPS:如何定时自动备份数据?][1]     

## 安装 mail/mailx    

这篇文章所介绍的在Linux终端发送E-mail的方法，需要用到mail/mailx这个工具, 因此，你需要确保你的系统中已经安装了该工具。Ubuntu系统中默认安装了该工具，因此，如果你使用的是Ubuntu系统，可以直接跳过这一步;如果你使用的其它发行版的Linux系统，你可以在终端执行下面的命令来检查你的系统中是否已经安装了 mail/mailx:       

```bash
$ which mail
```
如果上面的命令输出了 mail/mailx 这个工具的路径，则表明你的系统中已经安装了该工具;否则，表明你的系统中并没有安装该工具。       

因为Ubuntu系统中默认已经安装了 mail/mailx 这个工具，所以，未安装 mail/mailx 这个工具的情况可能发生在 RedHat 系统的Linux发行版中，如 RedHat, CentOs。如果你确实是使用的 RedHat 系统的Linux系统，并且系统中并未安装 mail/mailx 这个工具，则可以执行下面的命令来安装 mail/mailx:    

```bash
$ yum install mailx
```

## 确定 mail/mailx 的配置文件   

在最近的Ubuntu版本中，mail/mailx 的配置文件的文件名发生了一些变化，例如在 Ubuntu 14.04 中，mail/mailx 的配置文件是 **/etc/nail.rc**， 而在 Ubuntu 16.04 中，mail/mailx 的配置文件则变成了 **/etc/s-nail.rc**, 如果你不确定你的系统中的 mail/mailx 的配置文件的文件名，可以在终端执行下面的命令来查看:        

```bash
$ strings `which mail` | grep '\.rc'
```

上面的命令会输出 mail/mailx 的配置文件的绝对路径。                
在 Ubuntu 14.04 中，上面命令的输出结果为:    

```bash
/etc/nail.rc
```
在 Ubuntu 16.04 中，上面命令的输出结果是:    

```bash
/etc/s-nail.rc
```

## 修改 mail/mailx 的配置文件     

在确定了 mail/mailx 的配置文件后，接下来就要修改 mail/mailx 的配置文件了，在修改 mail/mailx 配置文件时，你要确定你要借助哪个国内的邮箱来发送邮件，因为在修改 mail/mailx 的配置文件时，会涉及到这方面的信息。作为例子，下面将使用新浪邮箱, 其他国内邮箱都是类似的。      

首先登入 [新浪邮箱][3], 之后依次点击 **设置** --> **客户端pop/imap/smtp**, 你会看到下面的内容:   

![Sina Mail Settings][4]    

**注意:** 要将上面图片所展示页面中的 **服务状态** 选择为 **开启** 状态。   

接下来，用你喜欢的编辑器打开 mail/mailx 的配置文件，在配置文件的末尾添加如下内容:          

set from=你的新浪邮箱地址 smtp=smtp.sina.com
set smtp-auth-user=你的新浪邮箱地址 smtp-auth-password=你的新浪邮箱的密码 smtp-auth=login

你应该用你的新浪邮箱地址及邮箱密码来替换上面的红色字体部分。例如，假设我的新浪邮箱地址为 **TechForGeek@sina.com**, 密码是 **TechForGeek**, 那么我应该在 mail/mailx 的配置文件的末尾添加下面的内容:         

```bash
set from=TechForGeek@sina.com smtp=smtp.sina.com
set smtp-auth-user=TechForGeek@sina.com smtp-auth-password=TechForGeek smtp-auth=login
```
修改完成后，保存文件并退出。     

## 使用mail/mailx来发送邮件     

接下来，你就可以使用 mail/mailx 在终端发送 E-mail 了，其基本用法如下:             

```bash
$ mail -s "Subject" recepient@xxx
```
上面命令中的 **-s** 选项后面跟的参数表示的是要发送的邮件的主题， **recepient@xxx** 表示收件人的邮箱地址。              
输入上面的命令并按下回车键后，你就可以在终端输入要发送的邮件的正文了，输入完正文后，按下 `<Ctrl> + D` 组合键。如果一切都配置正确的话，此时邮件就会发送出去了。     

如果你使用的手机号是中国移动的，则可以将上面命令中的收件人地址改为 **你的手机号@139.com**, 这样，如果邮件正常发送的话，通常情况下，你会在一两分钟内，收到中国移动发给你的短信提醒。    

上面的命令只是 mail/mailx 的最基本用法， mail/mailx 工具还有很多其他的功能，例如在邮件中添加附件, 关于 mail/mailx 的更多功能你可以通过 **man mail** 来查看。     

## 结束语    

在这篇文章中，介绍了一种在Linux终端中借助国内邮箱来发送E-mail的方法。能够在Linux终端发送E-mail是一个很有用的功能，例如在我之前写的 [VPS:如何定时自动备份数据?][1] 一文中，就是借助在Linux终端发送邮件的功能来实现自动定时备份VPS上的数据的。     

接下来，我打算写一篇关于利用在Linux终端发送E-mail的功能，来实现监控自己的笔记本电脑电量的文章，同时希望你可以持续关注 **[TechForGeek][5]**.         

[1]: how_to_auto_backup_on_vps.html    
[2]: https://en.wikipedia.org/wiki/Gmail
[3]: mail.sina.com
[4]: https://c7.staticflickr.com/1/669/31404376982_9b81384d5a_z.jpg      
[5]: https://www.techforgeek.info   


<!--
Reference: 
http://www.linuxidc.com/Linux/2013-06/85298.htm     
https://www.quppa.net/blog/2016/04/27/ubuntu-16-04-heirloom-mailx-is-replaced-by-s-nail
-->
