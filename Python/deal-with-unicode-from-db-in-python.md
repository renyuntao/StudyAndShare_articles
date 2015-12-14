# Python3.x:如何正确处理从数据库获取的中文字符?          
2015-12-14    <br />            
在Python3.x中，字符串字面值默认就是采用Unicode编码，因此可以正确处理像中文这样的字符,如下:        

    >>> string = '中文'
    >>> print(string)
    中文
但是，python3.x默认并不能正确处理由数据库中获取的字符串，例如我有一个名为example的数据库，其中有一个名为`students`的表，
表中如下一条记录:       

    130120010079	小明
上述字段分别表示学生的学号和姓名，使用pyhon从数据库中获取信息，使用如下语句:         

    >>> import pymysql
    >>> conn = pymysql.connect(db='example',host='localhostl',user='Tom',
    ... passwd='secret')
    ...
    >>> c = conn.cursor()
    >>> c.execute('SELECT * FROM EXAMPLE')
    >>> result = c.fetchall()      # 获取输出结果
    >>> print(result[0])           # 输出结果
    ('130120010079','??')
&nbsp;&nbsp;&nbsp;输出结果中，本应该显示`小明`的位置，由于python不能正确处理来自数据库的中文字符，从而显示了两个问号`??`,
要解决这个问题，需要在与数据库进行连接的时候，告诉Python要处理Unicode字符，因此需要将上述的`connect()`函数修改如下:       

    >>> conn = pymysql.connect(db='example',host='localhostl',user='Tom',
    ... passwd='secret',charset='utf8')
    ...
上述语句增加了`charset='utf8'`,`charset='utf8'`告诉Python采用`utf-8`编码，这样就可以正确处理中文了,之后再执行上面的查询语
句，就会得到正确的结果,如下:        

    ('130120010079','小明')
