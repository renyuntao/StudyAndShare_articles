# Tail Recursion & Tail Call Elimination            
<!--
2016-03-30 
--><br /><br />            
          
## 什么是 Tail Recursion?               
**Tail Recursion** 是在一个递归函数中，递归调用是该递归函数所要做的最后一件事情。下面的递归函数即是一个 **Tail Recursion**:     
        
```cpp
// 下面的函数打印 0~n 之间的值
void print(int n)
{
	if(n < 0)
		return;
	cout<<n<<" ";
	print(n-1);
}
```
注意，下面的递归函数不是一个 **Tail Recursion**:              
         
```cpp
// 下面这个函数计算 1+2+...+n 的和
int foo(int n)
{
	if(n == 1)
		return 1;
	return n+sum(n-1);
}
```
上面的递归函数不是 **Tail Recursion** 的原因在于递归调用不是该递归函数要做的最后一件事情，`sum(n-1)` 返回的值还要为 `sum(n)` 所用。             


## Tail Recursion 的优点
- **Tail Recursion** 的递归函数可以避免对每次递归调用分配栈空间，从而消除了递归函数可能导致 **Stack Overflow** 的隐患，关于这一点，[Stack Overflow](http://stackoverflow.com/questions/310974/what-is-tail-call-optimization) 上有一个非常好的解释，你可以查看那里。           
- 通常而言，**Tail Recursion** 被认为比普通的递归函数要好，因为 **Tail Recursion** 可以被编译器进行优化，这就是下面所要说的 **Tail Call Elimination** 了.           
       
## Tail Call Elimination                    
**[Tail Call Elimination](https://en.wikipedia.org/wiki/Tail_call)** 是对 **Tail Recursion** 递归函数的一种优化，采用的优化方式一般为将 **Tail Recursion** 递归函数的最后一句递归调用使用 **goto** 语句来替换掉。下面是对上述的 `print()` 函数进行 **Tail Call Elimination** 的一个例子:     
       
```cpp
// 下面的函数打印 0~n 之间的值            
void print(int n)
{
start:
	if(n < 0)
		return;
	cout<<n<<" ";

	// 下面两条语句替换了之前的递归调用
	--n;
	goto start;
}
```
另外，快速排序也是应用了 **Tail Recursion**,而归并排序并不是 **Tail Recursion**, 这也是快速排序表现更好的一个原因.                
