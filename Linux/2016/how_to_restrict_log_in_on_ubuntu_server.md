# 如何在Ubuntu Server中限制用户登录?             
<!--
2016-08-11
--> <br /><br />               
           
## 为什么要限制用户登录?
限制用户登录系统的原因有很多，其中一个很重要的因素就是安全。如果你使用的是 VPS, 那么你的主机就暴露在因特网中，一些恶意人员可能会对你的 VPS 主机进行 **SSH Brute Force** 攻击(几个月之前，我就遭遇了这样的攻击), 而限制不必要的用户，只允许必要的用户登录系统，可以降低 **SSH Brute Force** 的风险。如果你使用的是个人电脑，那么限制一些不必要的用户登录，也可以降低一些恶意人员尝试登录你的系统的风险。               
             
总之，无论你是使用的 VPS, 还是使用的个人电脑，限制不必要的用户登录，都会增加你的系统的安全性。                                
             
你可能会对这篇文章感兴趣: **[应对SSH Brute Force的7种方法](https://www.techforgeek.info/deal_with_ssh_brute_force.html)**                  
              
## 使用 /etc/passwd 限制用户登录
`/etc/passwd` 文件中保存了一些有关用户的UID, GID, 登录shell等内容，下面以 `techforgeek` 这个用户为例，来查看 `/etc/passwd` 文件中的内容格式:     

```bash
$ cat /etc/passwd | grep 'techforgeek'
```
          
```bash
techforgeek:x:1002:1002:,,,:/home/techforgeek:/bin/bash
```
输出结果有7列内容，每一列之间以冒号**:**作为分隔符，其中最后一列的内容表示的是用户的登录Shell, 即登录成功后所要运行的shell或命令，上述例子中最后一列的值为 `/bin/bash`, 该值也是Ubuntu系统中普通用户的默认值，它表示当用户登录成功后，会运行 `/bin/bash` 这个shell.                
             
Linux系统中还有两个命令 —— `/usr/sbin/nologin` 和 `/bin/false`, `/usr/sbin/nologin` 用于礼貌地拒绝用户登录，它会输出一行提示语句;而 `/bin/false` 则是什么都不做，只表示不成功的含义，由于什么都不做，它也会导致用户登录失败。因此，我们可以将 `/etc/passwd` 文件中相应记录的第7列内容(即用户的登录shell)，修改为 `/usr/sbin/nologin` 和 `/bin/false`, 来限制用户的登录。                         

                 
### 使用 usermod 命令修改用户的登录shell
**将用户的登录shell修改为 `/bin/false`(下面以 techforgeek 用户为例):**                
             
```bash
$ sudo usermod -s /bin/false techforgeek
```
**将用户的登录shell修改为 `/usr/sbin/nologin`(以 techforgeek 用户为例):**               
	
```bash
$ sudo usermod -s /usr/sbin/nologin techforgeek
```
### 使用 chsh 命令来修改用户的登录shell
**将用户的登录shell修改为 `/bin/false`(下面以 techforgeek 用户为例):**                

```bash
$ sudo chsh techforgeek
```
之后会提示你输入新的登录shell,此时你可以输入 `/bin/false` 或 `/usr/sbin/nologin` 来将 `techforgeek` 用户的登录shell设置为 /bin/false 或 /usr/sbin/nologin, 输入完毕并按回车后，`techforgeek` 用户的登录shell就会发生改变，该用户也会被限制登录系统。          

**注意:** 你在执行上面的命令时，应该将 `techforgee` 替换为你所要限制的用户的用户名。
       
之后你可以使用下面的命令来做测试用户是否确实已经被限制登录了:               

```bash
$ sudo su - techforgeek
```
同样，将上述命令中的 `techforgeek` 替换为你所限制的用户的用户名。                      
                
### 解除对用户的登录限制
如果你想解除对用户登录的限制，则可以执行如下两条命令中的任意一条:                
              
```bash
$ sudo usermod -s /bin/bash techforgeek
```
或              
           
```bash
$ sudo chsh techforgeek
```
之后输入 `/bin/bash` 即可。             
             
再次强调一下，上面的两条命令是以 `techforgeek` 用户为例的，你应该将上面命令中的 `techforgeek` 替换为你想要解除登录限制的用户的用户名。          

## 使用 /etc/shadow 限制用户登录
上面的方法中，我们通过修改 `/etc/passwd` 文件中用户的登录shell来限制用户登录系统。接下来的要解释的方法与上面的方法类似，不同的是，下面的方法是通过修改 `/etc/shadow` 文件来限制用户登录系统。             
               
文件 `/etc/shadow` 中保存了用户的密码信息，下面的命令中，将会显示 `techforgeek` 用户在 `/etc/shadow` 文件中的相应记录:               
             
```bash
$ sudo cat /etc/shadow | grep 'techforgeek'
```               
                   
```bash
techforgeek:$6$46N1BH5Q$PdHgpK2x5XW1KGMmMUkijh0alt.Hd0xwBWHu.AYHjAB..1/JBcnMKTiuF.p0n1oc1S5cj0xhjSwMe4jUsSzgU0:17024:0:99999:7:::
```
与文件 `/etc/passwd` 中的记录格式相同，`/etc/shadow` 文件中的字段之间也是由冒号**:**作为分隔符。文件 `/etc/shadow` 中每一行包含9个字段，每一行的第2个字段包含的是Hash过的用户的密码。
                             
上面的输出结果中，第二个字段（即以 **$6$46N...** 开头的那个字段）为用户 `techforgeek` 的Hash过的密码。在 Linux 系统中，通过在第二个字段之前添加一个感叹号 '!',即可冻结一个用户的密码，用户的密码被冻结之后，就无法再登录系统。                 
              
有两个工具可以用来冻结一个用户的密码 —— `usermod` 和 `passwd`:                 
**使用 usermod 来冻结一个用户的密码(下面以 techforgeek 用户为例)**          
          
```bash
$ sudo usermod -L techforgeek
```

**使用 passwd 来冻结一个用户的密码(下面以 techforgeek 用户为例)**               
              
```bash
$ sudo passwd -l techforgeek
```
**注意:** 在执行上面的命令时，你应该将 `techforgeek` 替换为你想要冻结的用户的用户名。                 
                
执行上述两条命令中的任意一条后，可以查看 `/etc/shadow` 文件中相应用户记录的变化:            
            
```bash
$ sudo cat /etc/shadow | grep 'techforgeek'
```             
                    
```bash
techforgeek:!$6$46N1BH5Q$PdHgpK2x5XW1KGMmMUkijh0alt.Hd0xwBWHu.AYHjAB..1/JBcnMKTiuF.p0n1oc1S5cj0xhjSwMe4jUsSzgU0:17024:0:99999:7:::
```
可以看到，techforgeek 用户记录的第二个字段前多了一个感叹号 '!'。此时，如果你尝试使用 `$ su - techforgeek` 命令来切换用户，即使你输入的密码正确，也会受到系统的认证失败的提示，因为此时，该用户的密码已经被冻结了。                    

### 解除对用户的登录限制
要想解除对用户的登录限制，只需解冻用户的密码即可, 下面是解冻一个用户的两种方法，你可以选择其中的任意一种方法来解冻一个用户的密码。                 
**使用 usermod 来解冻一个用户的密码(以 techforgeek 用户为例):**              
            
```bash
$ sudo usermod -U techforgeek
```
**使用 passwd 来解冻一个用户的密码(以 techforgeek 用户为例):**              
        
```bash
$ sudo passwd -u techforgeek
```
同样，你在执行上面的命令时，应该将 `techforgeek` 替换为你想要解冻的用户的用户名。                      
              
**注意，这种限制用户登录的方法,对于所有使用密码来登录系统的用户而言是有效的，但是如果用户不通过密码来登录系统(例如SSH公钥认证),那么这种方法是不起作用的!**                         

你可能会对这篇文章感兴趣: **[SSH:如何无需输入密码登录远程主机?](https://www.techforgeek.info/how_to_login_remote_without_passwd.html)**          
               
## 使用 /etc/nologin 文件来限制用户的登录
如果你想要限制除 root 用户外的所有用户登录系统，那么可以在 `/etc/` 目录下创建一个名为 `nologin` 的普通文件。之后，当有用户登录系统时，就会被系统拒绝，并收到一些提示，提示信息为文件 `/etc/nologin` 中的内容，如果该文件的内容为空，则会受到如下提示:            
          
```
Login incorrect
```

### 解除对用户登录的限制               
对于这种方法，要解除对用户登录的限制十分简单，只需将文件 `/etc/nologin` 删除掉即可:              
             
```bash
$ sudo rm /etc/nologin
```

**注意, 这种限制用户登录系统的方法，对于 `$ su - username` 这样的切换用户的命令而言，是不起作用的。**                 

## 结束语            
这篇文章解释了三种用来限制用户登录系统的方法以及相应修改用户信息的工具，即通过 `passwd`, `usermod` 及 `chsh` 工具来修改文件 `/etc/passwd`, `/etc/shadow` 和 `/etc/nologin` 中的相关内容，从而达到限制用户登录的目的。 恰当地使用这些方法，限制不必要的用户登录你的系统，可以大大地提高你的系统的安全性。                 


<!-- 
Reference:
https://www.digitalocean.com/community/tutorials/how-to-restrict-log-in-capabilities-of-users-on-ubuntu
-->
