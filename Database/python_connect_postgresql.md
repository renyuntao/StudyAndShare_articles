# Python3.x:编写基于 PostgreSQL 的程序        
2016-01-02   <br />           

## 安装 Psycopg2
&nbsp;&nbsp;&nbsp;要编写基于 PostgreSQL 的程序，首先要使 Python 能够连接到 PostgreSQL,这就需要用到 `Psycopg2`,它是 Python 中 PostgreSQL 的驱动器.因此，要连接到 PostgreSQL,首先需要安装 `Psycopg2`,在Ubuntu中，可以通过如下命令来安装:           

    $ sudo apt-get update
    $ sudo apt-get install libpq-dev
    $ sudo apt-get install python3-Psycopg2
## 连接到 PostgreSQL
使用 `Psycopg2` 模块中的 `connect` 函数连接到 PostgreSQL 数据库,如下:      

    >>> import psycopg2
    >>> conn = psycopg2.connect(host='your_hostname',dbname='your_db_name',
		   user='your_user_name',password='your_passwd')
## 执行 SQL 的查询语句    
要执行 SQL 语句，首先要获得 `cursor` 对象，只有该类对象才能够执行 SQL 语句。获得 `cursor` 对象的例子如下:         

    >>> cur = conn.cursor()           
获得 `cursor` 对象后，就可以使用该对象的 `execute` 方法来执行 SQL 语句了，示例如下:           

    >>> cur.execute('SELECT * FROM your_tablename')
通过下面的语句可以输出查询结果:         

    >>> for res in cur:   
    ...  print(res)
    ...
## 执行 SQL 的更新语句         
&nbsp;&nbsp;&nbsp;执行 SQL 的更新语句与执行 SQL 的查询语句类似，不同的是在执行完更新语句后，你需要调用连接对象的 `commit` 函数来确认更新，否则更新操作不会生效。示例如下:     

    >>> cur.execute('INSERT INTO your_tbname VALUES (...)')
    >>> conn.commit()       # 使修改生效
## 断开连接
要断开与数据库的连接，需要调用 `close` 函数，如下：         

    >>> cur.close()
    >>> conn.close()
