# Python:vars(),locals()和globals()的区别
2015-11-04 <br />       
在Python中有三个内置函数，分别为`vars()`,`locals()`和`globals()`,它们三者之间有着一些相似的地方，因此很容易弄混。下面将讨论Python中这三个内置函数的相似之处和不同之处。       
`vars()`,`globals()`和`locals()`三者的功能如下:    

- **globals()** 返回一个_模块命名空间_的字典     
- **locals()** 返回一个_当前命名空间_的字典       
- **vars()** 在不接受参数的情况下，其与locals()完全相同;此外vars()还可以接收一个参数，如模块参数，类参数等，此时它可以返回一个_相应参数的命名空间_的字典       

下面举例来说明以上几点:     

    >>> a = 100
    >>> def foo():
    ...  b = 200
    ...  c = 300
    ...  print('globals():',globals())
    ...	 print('locals():',locals())
    ...  print('vars():',vars())
    ...
    >>> foo()
    globals(): {'__loader__': <class '_frozen_importlib.BuiltinImporter'>, '__name__': '__main__', '__builtins__': <module 'builtins' (built-in)>, 'a': 100, 'foo': <function foo at 0xb7271584>, '__package__': None, '__spec__': None, '__doc__': None}
    locals(): {'c': 300, 'b': 200}
    vars(): {'c': 300, 'b': 200}
有程序的输出可见，globals()函数输出了模块命名空间的字典;而vars()和locals()函数的输出则相同，这两个函数输出了当前命名空间（即foo()函数命名空间的字典）。       
Python中的这一技术可以方便地使用**基于字典的字符串格式化**,示例如下:     

    >>> name = 'Tom'
    >>> age = 20
    >>> introduction = """
    Hello,my name is %(name)s and I'm %(age)d years old.
    """
    >>>print(introduction % locals())
    Hello,my name is Tom and I'm 20 years old.
此外，vars()函数还可以接受一些参数，比如模块，类等，此时vars()可以返回相应参数的命名空间的字典，示例如下:

    >>> class Foo():
    ...   a = 100
    ...   def __init__(self):
    ...    b = 200
    ...
    >>> vars(Foo)
    mappingproxy({'__init__': <function Foo.__init__ at 0xb7216584>, '__weakref__': <attribute '__weakref__' of 'Foo' objects>, '__dict__': <attribute '__dict__' of 'Foo' objects>, 'a': 100, '__doc__': None, '__module__': '__main__'})
从输出结果中可以看出，vars()函数返回了Foo类的命名空间的字典.       

