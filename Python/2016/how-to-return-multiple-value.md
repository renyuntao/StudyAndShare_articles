# 如何从函数中返回多个值?          
2016-02-10  <br />             
有时候，我们可能需要从函数中返回多个值，下面分享几种从函数中返回多个值的方法.                 

## 方法 #1:使用类/结构体             
可以创建一个类/结构体，将要返回的多个值包含在这个类/结构体中，之后在函数中返回类对象或结构体。示例如下:         

### Python           

```python
# 创建一个类，其中包含要返回的变量 str_ 和 x
class Test:
	def __init__(self):
		self.str_ = 'Hello World'
		self.x = 10

# 在函数中返回类对象
def func():
	return Test()

result = func()

# 输出变量 `str_`,`x` 的值
print(result.str_)        # 输出:Hello World
print(result.x)           # 输出:10
```

### C++       

```cpp
// 创建一个结构体，其中包含要返回的变量 str_ 和 x
struct Test
{
	// 就地初始化 (C++11)
	std::string str = "Hello World";
	int x = 10;
};

// 从函数返回结构体
struct Test func()
{
	return Test();
}

int main()
{
	struct Test result = func();
	
	// 输出返回值
	std::cout<<result.str<<"   "<<result.x<<"\n";
	return 0;
}
```

## 方法 #2: 使用元组
可以在函数中，将要返回的多个值包含在一个元组中，之后将这个元组返回.示例如下:             

### Python

```python
def func():
	str_ = 'Hello World'
	x = 20
	# 将变量 str_ 和 x 包含在一个元组中返回
	return (str_,x)

a,b = func()

# 输出从函数中返回的值
print(a,b)
```

### C++11           
元组(tuple)是C++11新增的特性，要在C++中使用 **tuple** 首先要确保你的编译器支持 C++11.代码片段如下:                                 

```cpp          
std::tuple<std::string,int> func()
{
	// 返回一个tuple,其中包含一个字符串和一个整型数值
	return std::make_tuple("Hello World",20);
}

int main()
{
	std::string str;
	int val;

	// 将从函数中返回的值保存在变量 str 和 val中
	std::tie(str, val) = func();

	std::cout<<str<<"  "<<val<<"\n";
	return 0;
}
```

## 方法 #3:使用列表(Python)
Python中有一种数据结构称为列表(List),列表和元组十分相似，不同的是列表是可变的，而元组是不可变的.代码片段如下:               

```python
def func():
	str_ = 'Hello World'
	x = 20
	# 将变量 str_ 和 x 包含在一个列表中返回
	return [str_,x]

a,b = func()

# 输出从函数中返回的值
print(a,b)
```

## 方法 #4:使用字典(或map)
Python中有一种数据结构称为 **字典**, 在C++中类似的数据结构被称为 **Map**,通过将要返回的多个值放到字典或Map中，再返回这个字典或Map，即可实现返回多个值.示例如下:          

### Python         

```python
def func():
	# 创建一个字典
	d = dict()

	# 为字典赋值
	d['str'] = 'Hello World'
	d['x'] = 20

	# 返回字典
	return d

d = func()

# 输出字典中的值
print(d['str'])
print(d['x'])
```

### C++
在C++中，`Map` 不如Python中的字典灵活和功能强大，`Map` 中键(key)和值(value)的数据类型是固定的.因此，如果函数返回的多个值的数据类型相同，则可以使用 `Map` 或 `Set`(类似于`Map`)。代码片段如下:               

```cpp
// 返回两个int类型的值
std::map<std::string, int> func()
{
	int x = 10;
	int y = 20;
	std::map<std::string, int> dic;
	dic["x"] = x;
	dic["y"] = y;

	// 返回Map
	return dic;
}

int main()
{
	auto result = func();

	// 输出字典中的值
	for(auto e:result) 
		std::cout<<e.second<<" ";
	std::cout<<"\n";
	return 0;
}
```
