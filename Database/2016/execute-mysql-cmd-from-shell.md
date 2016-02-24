# MySQL:如何在Shell Script中执行MySQL命令?     
2016-02-23  <br /><br />         
        
&nbsp;&nbsp;&nbsp;经常对一些重要的数据进行备份是一个很好的习惯。手动备份不仅十分麻烦，而且还常常忘记。因此，写一个自动进行备份数据的 Shell 脚本，并使用 `Cron` 来定时备份数据就显得十分有必要了。由于经常要备份 MySQL 数据库中的数据，这就需要在 Shell Script 中执行 MySQL 命令。下面就分享几种在 Shell Script中执行 MySQL 命令的方法。                 

## 只执行一条MySQL命令
如果在 Shell Script 中只执行一条 MySQL 语句，则可以直接在 Shell Script 中书写如下格式的语句:             

```bash         
mysql -u username -pPassword -D db_name -e "MySQL command"
```
**提示:** 将上述语句中的 `username` 修改为你的系统中MySQL用户名，`Password` 修改为你的系统中相应用户的登录密码，`db_name` 修改为你所要在其中进行操作的数据库名，`MySQL command` 修改为你所要执行的 MySQL 语句。            
**注意上述语句中 `-p` 选项和 `Password` 之间没有空白。**                

## 执行多条MySQL语句           
如果你要在 Shell Script中执行多条语句，上面的方法就不适合了。有两种方法可以在 Shell Script 中执行多条 MySQL 语句,如下:               

### 方法 \#1:将MySQL语句写入文件中          
你可以将要执行的多条 MySQL 语句写入一个文件中，之后可以通过在 Shell Script 中写入如下格式的语句来执行多条 MySQL 语句:            

```bash    
mysql -u username -pPassword -D db_name < example.sql
```
**提示:** 将上述语句中的 `username` 修改为你的系统中MySQL用户名，`Password` 修改为你的系统中相应用户的登录密码，`db_name` 修改为你所要在其中进行操作的数据库名，`example.sql` 修改为你的写有多条 MySQL 语句的文件名。         
**注意写入文件中的 MySQL 语句，每一条语句仍需要以分号 `;` 结尾。**            

### 方法 \#2:使用输入结束符号             
除了将多条语句写入一个文件中执行外，你还可以直接将要执行的多条语句写到 Shell Script 中，只是此时需要使用 `<<` 来标识 MySQL 语句何时结束，示例如下:    

```bash
mysql -u username -pPassword -D db_name << EOF
MySQL command1;
MySQL command2;
MySQL command3;   
...
EOF
```
上述语句中的 `<< EOF` 表示当遇到 `EOF` 时 MySQL 语句执行完毕。                  
**注意直接写入 Shell Script 的多条语句仍需要以分号 `;` 结尾。**
