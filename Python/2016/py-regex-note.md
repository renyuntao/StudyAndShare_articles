# Python:使用正则表达式               
2016-03-20  <br /> <br />       
              
## #1: 导入相关模块               
在Python中，要使用正则表达式，就要用到 `re` 模块，因此在使用正则表达式之前需要首先导入该模块.              
        
```python
>>> import re
``` 
## #2: 创建正则表达式对象               
如果要在程序中多次使用同一个正则表达式的话，可以创建一个相应的正则表达式对象，这样可以提高使用效率.创建正则表达式对象，需要用到 `re` 模块中的 `compile()` 函数:               
                
reobj = re.compile("_regex pattern_")                 
                 
## #3:设置正则表达式的选项            
在Python中，可以通过下列常量来设置正则表达式的选项:              
            
- **宽松排列:** re.VERBOSE 或 re.X             
- **不区分大小写:** re.IGNORECASE 或 re.I           
- **点号匹配换行符:** re.DOTALL 或 re.S               
- **脱字符和美元符号匹配换行处:** re.MULTILINE 或 re.M 
 
你可以将上述常量放到 `re.compile()` 函数的第二个可选参数处来设置，如果要同时设置多个选项，可以使用 **|** 符号来分隔多个选项:             
           
```python
import re
reobj = re.compile("regex pattern", 
				re.VERBOSE | re.IGNORECASE |
				re.DOTALL | re.MULTILINE)
```
## #4: 检查是否可以在目标字符串中找到匹配               
对于一次性的快速测试，可以直接使用全局函数 `re.search()`:              
         
```python
import re
# string 为目标字符串
if re.search("regex pattern", string)
	# Successful match
else:
	# Match attempt failed
```
如果要在程序中多次使用同一个正则表达式，可以首先创建一个相应的正则表达式对象,之后调用该对象的 `search()` 函数:              
           
```python
import re
reobj = re.compile("regex pattern")
# string 为目标字符串
if reobj.search(string):
	# Successful match
else:
	# Match attempt failed
```
## #5: 测试正则表达式能否完整匹配目标字符串              
想要检查一个字符串是否整体符合某个特定模式，也就是说，想要检查包含该模式的正则表达式是否可以从头到尾匹配该字符串，此时就要用的 `match()` 函数了.  
对于一次性的快速测试，可以直接使用全局函数 `re.match()`:              
       
```python
import re
# string 表示目标字符串
if re.match(r"regex pattern\Z", string):
	# Successful match
else:
	# Match attempt failed
```
如果要在程序中多次使用同一个正则表达式，可以首先创建一个相应的正则表达式对象,之后调用该对象的 `match()` 函数:              

```python
import re
# string 表示目标字符串
reobj = re.compile(r"regex pattern\Z", string)
if reobj.match(string):
	# Successful match
else:
	# Match attempt failed
```             
上面的程序中 `\Z` 匹配字符串的末尾，更多转义字符的含义可以查看 **[Python文档](https://docs.python.org/3/library/re.html)**.                 
## #6: 获取匹配文本              
假设有一个可以匹配目标字符串一部分的正则表达式，现在想要把匹配的文本提取出来，可以使用 `group()` 函数.                    
对于一次性的快速测试，可以直接像下面这样做:                
              
```python
import re
# string 表示目标字符串
matchobj = re.search("regex pattern", string)
if matchobj:
	result = matchobj.group()
else:
	result = ''
```
如果要重复使用同一个正则表达式，最好使用编译好的对象:             
            
```python
import re
reobj = re.compile("regex pattern")
# string 表示目标字符串
matchobj = reobj.search(string)
if matchobj:
	result = matchobj.group()
else:
	result = ''
```
## #7: 确定匹配的位置和长度              
上面的例子是提取匹配的字串，如果想要确定该匹配的开始位置和长度，就要用到 `start()` 和 `end()` 函数了.                
对于一次性的快速测试，可以直接像下面这样做:                

```python
import re
# string 表示目标字符串
matchobj = re.search("regex pattern", string)
if matchobj:
	matchstart = matchobj.start()       
	matchlength = matchobj.end() - matchstart
```               
如果要重复使用同一个正则表达式，最好使用编译好的对象:             

```python
import re
reobj = re.compile("regex pattern")
# string 表示目标字符串
matchobj = reobj.search(string)         
if matchobj:
	matchstart = matchobj.start()
	matchlength = matchobj.end() - matchstart
```
另外，可以通过给 `start()` 和 `end()` 函数传递一个参数来获取正则表达式中的某个捕获分组匹配到的文本范围。调用 `start(1)` 可以得到第一个捕获分组的开始位置，而调用 `end(2)` 则会得到第二个分组的结束位置，以此类推。(第0个分组是完整的正则表达式匹配)              

## #8: 获取匹配文本的一部分              
在第6个例子中，我们使用了无参的 `group()` 函数来提取匹配的文本，但是如果我们想要的只是匹配文本中的一部分该怎么办呢？此时就要用到捕获分组了，使用捕获分组将想要的部分分离出来.例如 `https://([0-9a-z.]+)` 会匹配字符串 "Click https://www.google.com" 中的 "https://www.google.com"。而捕获分组中匹配的是 "www.google.com", 你想要提取这部分内容。这时仍然要用到 `group()` 函数，不同的是，这次要使用的是包含有参数的 `group()`.               
对于一次性的快速测试，可以直接像下面这样做:                

```python
import re
string = 'Click https://www.google.com'
matchobj = re.search("https://(0-9a-z.)+", string)
if matchobj:
	result = matchobj.group(1)
else:
	result = ''
```            
如果要重复使用同一个正则表达式，最好使用编译好的对象:             

```python
import re
string = 'Click https://www.google.com'
reobj = re.compile("https://(a-z0-9.)+")
matchobj = reobj.search(string)
if matchobj:
	result = matchobj.group(1)
else:
	result = ''
```
上面的程序中将参数 **1** 传递给 `group()` 函数，表示获取第一个分组的内容，如果传递 **2**, 则表示获取第二个分组的内容，以此类推。Python最多支持99个捕获分组.                 

## #9: 获取所有的匹配子串             
通过 `search()` 函数只能匹配第一个匹配的子串，而要想获取所有匹配的子串，可以使用 `findall()`函数，该函数匹配目标字符串中与正则式匹配的所有子串，并返回一个包含所有匹配子串的列表.               
对于一次性的快速测试，可以直接像下面这样做:                

```python
import re
# string 为目标字符串
result = re.findall("regex pattern", string)
```            
如果要重复使用同一个正则表达式，最好使用一个编译好的对象,之后调用该对象的 `findall()` 函数:               
               
```python
import re
reobj = re.compile("regex pattern")
# string 为目标子串
result = reobj.findall(string)
```

## #10: 遍历所有匹配            
要遍历所有的匹配，可以使用 `finditer()` 函数，该函数会返回一个迭代器，通过该迭代器可以找到正则表达式的所有匹配。              
如果只打算使用同一个正则表达式处理少量字符串，可以使用全局函数:              
            
```python
import re
# string 为目标字符串
for matchobj in re.finditer("regex pattern", string):
	# do something
```
如果要重复使用同一个正则表达式，就需要使用一个编译好的对象:                     
           
```python
import re
reobj = re.compile("regex pattern")
# string 为目标字符串
for matchobj in reobj.finditer(string):
	# do something
```

## #11: 获取符合某些条件的匹配结果            
假如现在有一个字符串 "1,2,3,4,5",现在要获取其中的偶数数字，就需要在获取这些数字之前进行一些验证.示例如下:                  
如果只打算使用同一个正则表达式处理少量字符串，可以使用全局函数:              

```python
import re
ls = []
string = '1,2,3,4,5'
for matchobj in re.finditer(r'\d+', string):
	if int(matchobj.group()) % 2 == 0:
		ls.append(matchobj.group())
```
如果要重复使用同一个正则表达式，就需要使用一个编译好的对象:                     

```python
import re
ls = []
string = '1,2,3,4,5'
reobj = re.compile(r"\d+")
for matchobj in reobj.finditer(r'\d+', string):
	if int(matchobj.group()) % 2 == 0:
		ls.append(matchobj.group())
```
         
## #12: 在一个匹配子串中查找另一个匹配
假设有一个HTML文件，其中有些段落使用`<b>`标签标记为了粗体。现在想要找到所有标记为粗体的数字，此时就要用到 **在一个匹配子串中查找另一个匹配** 了,即在所有标记为粗体的子串中查找数字。                 
            
```python
import re
ls = []
string = "<b>2</b>3 4 <b>5 6 7</b>"
inner = re.compile(r"\d+")
for outer in re.finditer("<b>(.*?)</b>", string):
	ls.extend(inner.findall(outer.group(1)))
```
        
## #13: 替换所有匹配             
要将所有匹配的子串替换为另外一些字符，可以使用 `sub()` 函数，该函数接受三个参数，全局的 `sub()` 函数可以接受三个参数，第一个参数是正则表达式，用于匹配子串，第二个参数是要将匹配的子串替换为哪些字符，第三个参数是目标字符串.                
如果只是处理少量字符串，可以使用全局函数:           
       
```python
import re
# string 为目标字符串，new_string 为要替换为的字符串
result = re.sub('regex pattern', 'new_string', string)
```           
如果要在程序中反复使用同一个正则表达式，就需要使用编译好的对象:               
        
```python
import re
reobj = re.compile('regex pattern')
result = reobj.sub('new_string', string)
```
此外，`sub()` 函数还可以接受一个可选参数，允许你使用它来限制替换执行的次数.                  

## #14: 使用匹配的子串来替换匹配              
假如你想要匹配由等号分隔的单词对，然后把等号两边的单词进行交换，可以使用 **反向引用** 来完成.                 
如果只打算使用同一个正则表达式处理少量字符串，可以使用如下全局函数:            
           
```python
import re
string = 'Hello=World'
result = re.sub(r"(\w+)=(\w+)", r"\2=\1", string)
```
要重复使用同一个正则表达式，就需要使用一个编译好的对象:                 
         
```python
import re
string = 'Hello=World'
reobj = re.compile(r"(\w+)={\w+}")
result = reobj.sub(r"\2=\1", string)
```
### 命名捕获        
我们也可以为捕获分组命名，之后在替换字符串中使用其名称来引用它们。             
如果只打算使用同一个正则表达式处理少量字符串，可以使用如下全局函数:            

```python
import re
string = 'Hello=World'
result = re.sub(r"(?P<left>\w+)=(?P<right>\w+)",
				r"\g<right>=\g<left",
				string)
```

要重复使用同一个正则表达式，就需要使用一个编译好的对象:                 

```python
import re
string = 'Hello=World'
reobj = re.compile(r'(?P<left>\w+)=(?P<right>\w+)')
result = reobj.sub(r"\g<right>=\g<left>", string)
```

## #15: 向 sub 函数中传递函数参数             
通常情况下，`sub()` 函数的第二个参数是要替换为的字符串，但是也可以向第二个参数传递一个函数参数，这个可以在一个函数中对正则式匹配内容做一些处理之后在进行替换。例如，要把一个字符串中所有数字都替换为该数字的2倍。                   
如果只打算使用同一个正则表达式处理少量字符串，可以使用如下全局函数:            

```python
import re
# string 为目标字符串，func 为传递的函数参数
result = re.sub.(r"\d+", func, string)
```
要重复使用同一个正则表达式，就需要使用一个编译好的对象:                 

```python
import re
reobj = re.compile(r"\d+")
result = reobj.sub(func, string)
```            
上面的两个代码片段中都调用了函数 `func`,这个函数需要在把它传递给 `sub()` 函数之前进行声明:                
          
```python
def func(matchobj):
	return str(int(matchobj.group())*2)
```            

## #16: 替换一个正则式匹配内的所有匹配              
假设有一个字符串为 "<b>one 1 2 3 two 4 5 three</b>four 6 7",你想要将 `<b>..</b>` 之间的数字都替换为字符 `!`,即替换一个正则匹配中的所有匹配，可以按照下面的方式来进行(**类似于第12个例子**).                
           
```python
import re
string = '<b>one 1 2 3 two 4 5 three</b>four 6 7'
inner = re.compile(r'\d+')
def replacewithin(matchobj):
	return inner.sub(matchobj, matchobj.group())
result = re.sub("<b>.*?</b>", replacewithin, string)
```
      
## #17: 拆分字符串到列表              
假设现在有一个字符串为 "1,2,3,4,5", 在这个字符串中，以逗号为分隔符，分割各个元素到一个列表之中，所得列表为 [1,2,3,4,5].此时，就要用到 `re` 模块中的 `split()` 函数了。              
如果只需要处理少量字符串，可以使用全局函数:               
            
```python
import re
string = '1,2,3,4,5'
result = re.split(r',', string)
```
要重复使用同一个正则表达式，就需要使用一个编译好的对象:                 

```python
import re
reobj = re.compile(r',')
result = reobj.split(string)
```
**使用正则表达式拆分字符串实质上就是要产生与示9相反的结果。所得到的不是所有正则匹配的列表，而是位于匹配之间的文本列表，其中也包含了第一个匹配之前和最后一个匹配之后的文本。**                  
另外，`split()` 函数还可接受一个可选的参数，用来指定字符串进行拆分的最大次数.               

## #18: 拆分字符串到列表，并保留分隔符               
假设现在有一个字符串为 "1*2*3*4*5", 在这个字符串中，以逗号为分隔符，分割各个元素到一个列表之中，同时保留分隔符，所得列表为 [1,*,2,*,3,*,4,*,5].此时，就要用到 `re` 模块中的 `split()` 函数了,与上例不同的是，这次还要使用捕获分组，使用捕获分组后，正则匹配也会添加到结果列表中。              
如果只需要处理少量字符串，可以使用全局函数:               

```python
import re
string = '1*2*3*4*5'
result = re.split(r'(\*)', string)
```
要重复使用同一个正则表达式，就需要使用一个编译好的对象:                 

```python
import re
string = '1*2*3*4*5'
reobj = re.compile(r'(\*)')
result = reobj.split(string)
```
另外，`split()` 函数还可接受一个可选的参数，用来指定字符串进行拆分的最大次数.               

## #19: 逐行查找               
有一个多行的字符串，现在想要每次将正则表达式用在这个多行字符串的一行之上，即进行类似于 `grep` 的逐行查找，可以首先把它拆分成一个字符串列表，该列表中每个字符串都包含一行文本。      
          
```python
import re
# 拆分多行字符串 MultilineString
lines = re.split(r"\r?\n", MultilineString)
```          
之后遍历这个列表 `lines`, 并将正则表达式应用到每一行之上:             
            
```python
reobj = re.compile("regex pattern")
for line in lines:
	if reobj.search(line):
		# Match
	else:	
		# Do not match
```
## #20: 获取捕获分组的位置               
要获取捕获分组在字符串中的位置，可以使用 `re` 模块中的 `span()` 函数，示例如下:             
           
```python
import re
m = re.search('[0-9]*([a-zA-Z]*)[0-9]*', string)
print(m.group(1))   # 输出 Hello
print(m.span(1))    # 输出 (3,8),即捕获分组的位置信息                
```
