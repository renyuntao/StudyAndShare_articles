# 如何在 MySQL 中更改 root 用户的密码?       
<!--
2016-05-31 
--><br /><br />                 
            
&nbsp;&nbsp;&nbsp;&nbsp;如果在安装 MySQL 的过程中，没有设置 root 用户的密码，那么在 MySQL 安装完毕后，就要设置 root 用户的密码，或者由于其它的一些原因，你想要更改 root 用户的密码。这篇文章就来分享两种设置/修改 root 用户密码的方法。                 
             
## 方法 #1: 使用 mysqladmin
&nbsp;&nbsp;&nbsp;&nbsp;使用命令行工具 `mysqladmin`，你可以直接在 Linux Shell 中设置/修改 root 用户的密码，而不必登录到 Mysql Shell 中去。如果你还没有设置 root 用户的密码，那么你可以使用下面的命令来为 root 用户设置密码:            
           
```sql
$ mysqladmin -u root password 'NEWPASSWORD'
```
`NEWPASSWORD` 为你给 root 用户设置的密码。             
如果你已经为 root 用户设置了密码，但是现在想要更改密码，则可以执行下面的命令:             
           
```sql
$ mysqladmin -u root -p'oldpassword' password 'newpassword'
```
`oldpassword` 为 root 用户的原来的密码，`newpassword` 是为 root 用户设置的新密码。                   

> **注意:** `-p` 和 `'oldpassword'` 之间没有空格。                 

### 检测密码是否修改成功           
通过上面的方法修改了 root 用户的密码后，可以执行如下命令来检测密码是否修改成功:              
            
```sql
$ mysql -u root -p'password' -e 'show databases;'
```
如果上面的命令输出了包含数据库名称的二维表，则表示密码修改成功了。                      

## 方法 #2: 使用MySQL命令               
另外一种修改 root 用户密码的方法是，首先登录到 MySQL Shell 中去，然后使用 MySQL 命令来修改密码。       
现在假设你已经使用 root 用户登录到了 MySQL Shell, 首先执行如下命令进入到 **mysql** 数据库:               
            
```sql
mysql> use mysql
```
之后输入下面的命令修改 root 用户的密码:            

```sql
mysql> update user set Password=PASSWORD("NEWPASSWORD") where User='root';
```
`NEWPASSWORD` 为你要为 root 用户设置的新密码;`PASSWORD()`函数用来加密密码。                       
为使修改生效，你可以在 MySQL Shell 中执行如下命令:               
           
```sql
mysql> flush privileges
```
或者是首先退出 MySQL Shell:           

```sql
mysql> quit
```
之后，在 Linux Shell 中重启 MySQL Server 使修改生效:           

```sql
$ sudo service mysql stop
$ sudo service mysql start
```

## 修改普通用户的密码
虽然这篇文章的题目是修改 root 用户的密码，但是上面介绍的方法，对于普通用户而言，依然是适用的。                 

