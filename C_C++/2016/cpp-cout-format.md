# C++:使用cout进行格式化总结               
2016-02-26   <br />              
              
## 使用标准控制符进行格式化                
标准控制符都是位于命名空间 `std` 中，因此在使用前应添加形如 `using std::xxx` 的语句.                   

> boolalpha  ======> 使用true,false来显示bool值             
> noboolalpha  ======> 取消上述功能                
> showbase ======>  对于输出，使用C++基数前缀            
> noshowbase ======> 取消上述的功能               
> showpoint ======>  打印末尾的0和小数点                 
> noshowpoint ======> 取消上述功能              
> showpos ======> 在正数前面加上'+'符号            
> noshowpos ======>  取消上述功能                             
> uppercase   ======> 对于16进制输出，使用大写字母表示                  
> nouppercase ======> 取消上述功能                         
> iternal ======> 符号或基数前缀左对齐，值右对齐              
> left ======> 左对齐         
> right ======> 右对齐             
> dec ======> 以十进制输出            
> hex ======> 以十六进制输出              
> oct ======>  以八进制输出            
> fixed ======> 使用定点计数法               
> scientific ======> 使用科学计数法                

由于 **ostream** 类重载了 `<<` 操作符，这使得语句             

```cpp
boolalpha(cout);  
```
等价于     

```cpp
cout<<boolalpha;
```
从而使用起来更加方便。               
另外，使用上述操作符，实际上是在调用 `setf`，`unsetf` 函数，如 `boolalpha` 即是调用            

```cpp
setf(ios_base::boolalpha)           
```
而 `noboolalpha` 即是调用           

```cpp
unsetf(ios_base::noboolalpha)
```

## 使用ostream类成员函数来进行输出格式化               
ostream类中用来进行输出格式化的成员函数有 `width()`,`fill()` 和 `precision()`，它们分别用来设置字段宽度，填充字符和设置精度。由于它们是成员函数，因此在调用的时候要使用成员操作符 —— `.`,如:            

```cpp
cout.width(12);
```
**注意:`width()` 成员函数设置一次后便失效，即它只针对紧随其后的输出有效.**               

## 其他的一些操作符            
位于 `iomanip` 头文件中的 `setprecision()`, `setfill()`, `setw()` 操作符，分别用于设置精度，填充字符和字段宽度。由于它们同样是操作符，因此它们可以与 `cout` 语句连用，如             

```cpp
cout<<"#"<<setw(10)<<"apple"<<"#"<<endl;
```
**同样地，`setw()` 也是设置一次后便失效，它也是只针对紧随其后的输出有效。**               
