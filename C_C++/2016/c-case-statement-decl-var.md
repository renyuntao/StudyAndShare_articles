# C: 在case语句中声明变量             
2016-04-17  <br /><br />            
        
先来看下面的C程序片段:                  
            
```cpp
int var;
scanf("%d",&var);
switch(var)
{
	case 1:
		int a = 1;
		...
		break;
	...
}
```
使用 **GCC** 编译上面的程序，会产生下面的错误:       

> error:a label can only be part of a statement and a declaration is not a statement

产生上述错误的原因在于, 在C中，case语句是一个标签(**label**), 而在C中，标签下不允许使用声明语句.             

## 三种解决方法 　　　      
### 方法 #1: 在switch语句外使用声明语句              
既然不允许在case语句中进行语句声明，可以将声明语句放到switch语句外面,如下:            
        
```cpp
int var;
int a = 1;
scanf("%d",&var);
switch(var)
{
	case 1:
		...
		break;
	...
}
```

### 方法 #2: 使用花括号             
使用花括号将case语句和break语句之间的部分括起来可以解决这个问题:             

```cpp
int var;
scanf("%d",&var);
switch(var)
{
	case 1:
		{
		int a = 1;
		...
		}
		break;
	...
}
```
或

```cpp
int var;
scanf("%d",&var);
switch(var)
{
	case 1:
	{
		int a = 1;
		...
		break;
	}
	...
}
```
### 方法 #3: 使用假case语句              
这是一个小技巧，即把要执行的语句不放在case语句下,但是仍在switch语句中:              

```cpp
int var;
scanf("%d",&var);
switch(var)
{
	case 1:;
		int a = 1;
		...
		break;
	...
}
```
## C++中的情况          
与C不同，C++中，在case语句下使用声明语句是被允许的。
<!-- Refer: http://shareprogrammingtips.com/why-variables-can-not-be-declared-in-a-switch-statement-just-after-labels/  -->
