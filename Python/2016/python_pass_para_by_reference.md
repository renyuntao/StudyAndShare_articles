# Python: 如何在调用函数时按引用传递一个参数?            
<!--
2016-10-05    
--><br /><br />

这篇文章的标题实际上有些不太恰当，因为在Python中，在向函数中传递参数时，并没有按引用传递这一说法, 采用这个标题只是为了让大家更容易理解这篇文章中要介绍的内容，在这篇文章中，主要介绍如何在Python中模拟C++语言中的按引用传递参数的方法。          
      
在Python中，向函数中传递参数的时候，是按照 **[passed by assignment][1]** 的方式来进行的，即在传递参数时，只是创建一个指向对象的引用，之后将这个引用赋值给函数参数中的形参，下面举例说明:             

```python
def foo(n):
	n = 10 

x = 1
print(x)
foo(x)
print(x)
```
输出结果:       

```python
1
1
```           
在上面的例子中，**x = 1** 会创建一个引用，这个引用指向 **1** 这个对象，这个引用保存到 **x** 这个变量中去，在将 **x** 传递给函数时，由于是按照 **passed by assignment** 的方式进行的，所以这个过程中又会创建一个指向 **1** 这个对象的引用，并将这个引用保存到函数形参 **n** 中，此时有两个引用(**x** 和 **n**)指向同一个对象 **1**。之后进入到函数 **foo** 中去，语句 **n = 10** 导致创建一个新的指向对象 **10** 的引用，并将这个引用保存到变量 **n** 中。这个过程导致了变量 **n** 的引用进行了更新，即由指向 **1** 的引用更新为了指向 **10** 的引用。接下来退出函数 **foo**, 由于函数外的变量 **x** 仍保存的是指向 **1** 的引用，因此 **print** 函数的输出结果是1, 而 **foo** 函数中的改变并没有影响到函数外的 **x**.               

大致了解了Python中向函数中传递参数的方式之后，下面来介绍几种模拟C++中按引用传递参数的方法。              

## 1. 使用 return 语句        
这种方法就是在函数结束时，将传入函数中的参数使用 return 语句返回，并将函数的返回值再赋值给原来的变量。               

示例如下:        

```python
def foo(n):
	n = 10 
	return n

x = 1
print(x)
x = foo(x)
print(x)
```
输出结果:            

```python
1
10
```

_注：如果返回多个值，可以使用元组_        

--------------------------

## 2. 使用全局变量             

示例如下:        

```python
def foo():
	global x
	x = 10 

x = 1
print(x)
foo(x)
print(x)
```
输出结果:           

```python
1
10
```
虽然达到了预期的目的，但是该方法并不推荐使用，因为在多线程的环境下，这种方法不是一种 **线程安全** 的方法。           

--------------------------

## 3. 传入可变对象        

在文章开头的例子中，函数内对变量值的改变并没有影响到函数外变量的值，这是因为变量 **x** 和 **n** 都是指向的不可变对象，不可变对象不能原地修改，只能创建新的引用来指向新的对象。但是如果指向的对象是一个可变对象(如列表)，那么在函数内对可变对象中的值进行修改，则会影响到函数外的值, 因此，如果你想要向函数中传递一个不可变对象，那么可以首先将这个不可变对象放到一个可变对象之中(如列表)，之后再将这个可变对象作为参数传递给函数。             

下面是一个使用列表的例子:       

```python
def foo(a):
	a[0] = 10

a = [1,]
print(a[0])
func(a)
print(a[0])
```
输出结果:         

```python
1
10
```

同样的道理，由于字典对象也是可变对象，因此也可以将字典作为参数传递给函数:                

```python
def foo(args):
	args['a'] = 10

args = {'a':1}
print(args['a'])
foo(args)
print(args['a'])
```
输出结果:         

```python
1
10
```

-------------------

## 4. 传入类对象        

将类对象作为参数传入函数，也可达到类似的目的， 示例如下:             

```python
class Test:
	def __init__(self, x):
		self.a = x

def foo(args):         
	args.a = 10

test = Test(1)
print(test.a)
foo(test)
print(test.a)
```
输出结果:          

```python
1
10
```

## 结束语       
Python 中并没有类似与C++中的按引用向函数中传递参数的方法，不过，我们可以通过一些小技巧来模拟这样的行为。这篇文章中介绍了4种模拟C++中按引用传递参数的方法, 当我们需要在Python中使用像C++中按引用传递参数的行为时，我们就可以通过这些方法来实现。       

[1]: https://docs.python.org/3/faq/programming.html#how-do-i-write-a-function-with-output-parameters-call-by-reference         
<!--
Reference:    

Python functions call by reference1                
http://stackoverflow.com/questions/13299427/python-functions-call-by-reference         

How do I pass a variable by reference2  
http://stackoverflow.com/questions/986006/how-do-i-pass-a-variable-by-reference         
-->   
