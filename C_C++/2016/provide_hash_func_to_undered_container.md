# C++11中为无序容器提供你自己的Hash函数           
2016-05-25  <br /><br />

&nbsp;&nbsp;&nbsp;&nbsp;在 C++11 中引入了无序容器(**Unordered Container**), 包括 unordered set, unordered multiset, unordered map 和 unordered multimap, 无序容器是以 hash table 为基础的容器，因此，相比与以平衡二叉树实现的关联容器(set, multiset, map, multimap)而言，无序容器在插入元素和取元素的平均效率都更高(关于关联式容器和无序容器的效率对比，可以查看 **[这里](http://supercomputingblog.com/windows/ordered-map-vs-unordered-map-a-performance-study/)** 和 **[这里](https://tinodidriksen.com/2010/04/02/cpp-set-performance/)**)。因此，在无序容器和关联式容器之间作选择时，除非你要求容器内的元素必须是有序的，否则，应该尽量使用无序容器。                 
           
&nbsp;&nbsp;&nbsp;&nbsp;由于无序容器是以 hash table 为基础的，因此，在构造无序容器的时候，需要传入一个 Hash 函数，但是，通常情况下，这个函数不需要我们自己传入，例如创建一个保存整型的数的 set,只需传入要存储的元素的数据类型即可:

```cpp
std::unordered_set<int> uset;
```
&nbsp;&nbsp;&nbsp;&nbsp;这是因为存在一个默认的 Hash 函数，在构造无序容器时，如果没有显式传入一个 Hash 函数，就会使用这个默认的 Hash 函数，这个默认的 Hash 函数是由 `<functional>` 提供的 `hash<>`, 这个 Hash 函数可以应付常见的数据类型，如整型数，浮点数类型，string, 指针等(完整的数据类型可以查看 **[这里](http://www.cplusplus.com/reference/functional/hash/)**), 但是对于其它的类型，如 Pair, 这个默认的 Hash 函数就不能够应付了:                 
      
```cpp
std::unordered_set<std::pair<int,int>> uset;
```
上面的语句会在编译时报错。　                  
此时，我们就需要自己构造一个 Hash 函数 `hash_func`，然后这个 Hash 函数作为第二参数传给无序容器的构造函数, 如下:         
           
```cpp
std::unordered_set<std::pair<int,int>, hash_func>
```
在构造我们自己的 Hash 函数的时候，应该构造一个函数对象(Function Object), 并且应该遵循如下的规则:           
	
- Hash 函数的参数应该是 `const` 类型       
- Hash 函数应该是一个 `const` 函数

&nbsp;&nbsp;&nbsp;&nbsp;假设现在要构造一个元素类型为 `Pair` 的 unordered set, `Pair` 内元素的类型都为 `int`, 那么一个简单的构造 Hash 函数的方法就是，对 `Pair` 内的每一个元素调用默认的Hash函数 `hash<>`，然后返回它们累加的值，如下:             
          
```cpp
class hash_func
{
	public:
		std::size_t operator()(const std::pair<int,int> &myPair) const
		{
			return std::hash<int>()(myPair.first) + std::hash<int>()(myPair.second);
		}
};
```
之后在构造 unordered set 的时候，将这个 Hash 函数传给 unordered set 的构造函数:            
       
```cpp
std::unordered_set<std::pair<int,int>, hash_func> uset;
```

下面是一个完整的例子:             
        
```cpp
#include<functional>
#include<iostream>
#include<utility>
#include<unordered_set>

class hash_func
{
	public:
		std::size_t operator()(const std::pair<int,int> &myPair) const
		{
			return std::hash<int>()(myPair.first) + 
				   std::hash<int>()(myPair.second);
		}
};

int main()
{
	std::unordered_set<std::pair<int,int>, hash_func> uset {{1,2},{3,4},{5,6}};
	for(auto e : uset) std::cout<<e.first<<"  "<<e.second<<"\n";
	return 0;
}
```
输出:          

```
5  6
1  2
3  4
```
