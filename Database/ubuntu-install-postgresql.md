# 如何在Ubuntu 14.04 中安装PostgreSQL
2015-12-26  <br />          
## 为什么使用PostgreSQL
&nbsp;&nbsp;&nbsp;和MySQL一样，PostgreSQL也是一个开源数据库。但是既然有了MySQL，为什么还要使用PostgreSQL呢?原因在于MySQL的功能不够强大，它对很多的SQL语法不能够支持。比如，MySQL不支持`CHECK`约束。而PostgreSQL则是目前功能最强大的开源数据库，可以支持绝大多数的SQL语法。     

----------
## 在Ubuntu 14.04 中安装PostgreSQL
Ubuntu 14.04 的默认软件仓库中包含了PostgreSQL的软件包，因此，可以直接通过`apt-get` 来安装PostgreSQL,如下:          

    $ sudo apt-get update
    $ sudo apt-get install postgresql postgresql-contrib
其中`postgresql-contrib`软件包增加了一些额外的工具和功能.          

-----------
## 开启、停止PostgreSQL
安装完PostgreSQL之后，PostgreSQL会自动开启，如果想要停止PostgreSQL,可以执行如下命令:         

    $ sudo service postgresql stop
如果想要重新开启PostgreSQL,则可以执行如下命令:          

    $ sudo service postgresql start

------------
## 登录到PostgreSQL
安装PostgreSQL之后，默认会在操作系统中创建一个名为`postgres`的用户，你可以使用该用户登录到PostgreSQL.            
首先我们需要先转换到`postgres`:

    $ sudo -i -u postgres
你会被要求输入密码，密码输入正确后之后就转换为`postgres`用户了。        
之后输入 `psql` 命令即可连接到PostgreSQL了, `psql` 会使用系统当前的用户名来尝试连接到 PostgreSQL,此时就不用再输入密码了:         

    $ psql            
`psql` 是PostgreSQL中的一个命令行交互客户端程序。执行上述命令类似与在MySQL中执行 `mysql` 命令.                        

## psql的一些简单用法
在 `psql` 中，所有的命令都是以 `\` 开头的，这一点也与 MySQL 不同，例如要查看所有的数据库，则可以执行如下命令:          

    postgres=# \l
**注意:** `postgres=#` msql的为提示符。
上述语句类似与 MySQL 中的 `show databases;` 语句。         
同样，在 PostgreSQL 中，查看数据表则可以执行 `\d` 命令。         
要查看某一 SQL 语句的用法，例如要查看 `SELECT` 语句的用法，则可以执行如下命令:         

    postgres=# \h SELECT
要断开与 PostgreSQL 的连接可以执行 `\q` 命令。         
你可以通过执行 `\?` 命令来查看所有可用的命令及命令的用法。           

---------
## 执行 SQL 语句
在 psql 的提示符下，就可以直接输入 SQL 语句并执行了.与 MySQL 相同，SQL 语句以 `;` 结尾.         

