# 如何在 MySQL 中创建/删除用户?        
2016-06-02   <br /><br />                 
            
&nbsp;&nbsp;&nbsp;&nbsp;在安装完 MySQL 数据库后，要使用数据库，往往是创建一个普通用户，然后用这个普通用户来对数据库进行操作，而不是直接使用 root 用户；在不需要某个用户的时候，也要及时删除掉这个用户，从而保持数据库的整洁。下面将会介绍几种在 MySQL 中创建用户和删除用户的方法。                
          
## 创建用户
可以使用两种方法来创建一个新用户。                 
### 方法 #1: 使用 INSERT 语句创建用户
&nbsp;&nbsp;&nbsp;&nbsp;使用 MySQL 语句创建用户，就是使用 root 用户对 `mysql` 数据库下的 `user` 表进行操作，`user` 表中保存了当前 MySQL 数据库的所有用户的信息，因此，可以使用 `INSERT` 语句向该表中插入一条记录，从而创建一个新用户。           
假如现在要创建一个名为 **example** 的新用户，并给予这个新用户 **SELECT**, **INSERT**, **UPDATE** 的权限，则可以使用 root 用户执行下面的 MySQL 语句:     
   
```sql
mysql> INSERT INTO user
	-> (host, user, password,
	-> select_priv, insert_priv, update_priv)
	-> VALUES ('localhost', 'example',
	-> PASSWORD('example123'), 'Y', 'Y', 'Y');
```
其中，`PASSWORD()` 函数是 MySQL 提供的加密函数，用来加密密码，这样插入到 `user` 表中的密码就不会以明文显示了。             
接下来执行下面的语句进行更新:            
     
```sql
mysql> FLUSH PRIVILEGES;
```

### 方法 #2: 使用 GRANT 语句创建用户
这种方法是在 MySQL Shell 中, 执行 `GRANT` 语句来创建用户。下面的例子将会在 `exam_db` 数据库下创建一个名为 `foo` 的用户，其密码为 `123`:      
      
```sql
mysql> GRANT SELECT, INSERT, UPDATE, DELETE
	-> ON exam_db.*
	-> TO 'foo'@'localhost'
	-> IDENTIFIED BY '123';
```
上面的语句创建的 `foo` 用户在 `exam_db` 数据库下具有 **SELECT**, **INSERT**, **UPDATE** 和 **DELETE** 的权限。如果要给予 `foo` 用户所有的权限，则可以使用 `ALL` 来代替上面语句中的 `SELECT, INSERT, UPDATE, DELETE`.           

## 删除用户
删除一个用户的方法也有两种。                 
### 方法 #1: 使用 DELETE 语句                
&nbsp;&nbsp;&nbsp;&nbsp;上面提到过，**mysql** 数据库下的 `user` 表中保存了当前 MySQL 数据库中的所有用户的信息，所以可以通过使用 DELETE 语句从 `user` 表中删除相应的一条记录，来达到删除一个用户的目的。下面的语句从 MySQL 数据库中删除掉用户 `example`:               
            
```sql
mysql> DELETE FROM user
	-> where user = 'example';
```

### 方法 #2: 使用 DROP USER 语句
DROP USER 语句也可以用来删除一个用户，该语句的用法如下:           
      
> DROP USER [IF EXISTS] _user_  [, _user_] ...

假如现在要删除一个名为 `foo` 的用户，可以执行如下语句:             
         
```sql
mysql> DROP USER foo
```

