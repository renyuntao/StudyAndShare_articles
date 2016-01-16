# Tkinter:Python2.X与Python3.X中的区别
2016-01-16  <br /><br />
&nbsp;&nbsp;&nbsp;前一段时间由于需要，学了一些 Tkinter 的知识。在查找 Tkinter 相关资料的过程中，发现都是基于 Python2.X 来讲解 Tkinter 的，并未找到基于 Python3.X 的 Tkinter 的相关资料。虽然基于 Python2.X 的 Tkinter 与基于 Python3.X 的 Tkinter 的绝大多数内容都是相同的，只是在细微之处有些不同，然而对于初次接触 Tkinter ，并且使用 Python3.X 进行编程的人而言，这些细微之处的差别仍然会造成很大的困惑。              
&nbsp;&nbsp;&nbsp;我在学习 Tkinter 的过程中，也遇到了一些因 Python 的版本不同而造成的 Tkinter 部分的代码不能正确运行的问题，对此我做了一些总结，现在分享如下.     

## 区别 \#1: Tkinter 模块名的改变
在 Python2.X 中，要导入 Tkinter 模块只要执行如下语句即可:       

    >>> import Tkinter
然而在 Python3.X 中，执行上述语句，会提示如下错误:         

> ImportError: No module named 'Tkinter'

这是因为，在 Python3.X 中，Tkinter的模块名由 `Tkinter` 更改为了 `tkinter`,因此在 Python3.X 中，要导入 Tkinter 模块,应该执行如下语句:      

    >>> import tkinter

## 区别 \#2: Tkinter 中弹出窗体的模块发生改变
&nbsp;&nbsp;&nbsp;在 Python2.X 中，要弹出一个窗体来显示信息，可以使用 `tkMessageBox` 模块。下面是弹出一个显示 "Hello World" 的标题为 "Say Hello" 的窗体的例子:    

    >>> import Tkinter
    >>> import tkMessageBox
    >>> tkMessageBox.showinfo("Say Hello","Hello World")      
&nbsp;&nbsp;&nbsp;然而在 Python3.X 中，`tkMessageBox` 模块被更名为 `messagebox`,并且被包含在了 `tkinter` 模块中。你可以执行如下语句来找到包含在 `tkinter` 模块中的 `messagebox`:           

    >>> import tkinter
    >>> help(tkinter)
要在 Python3.X 中使用弹出窗体来显示信息，应该像下面这样做:            

    >>> import tkinter
    >>> import tkinter.messagebox
    >>> tkinter.messagebox.showinfo("Say Hello","Hello World")
