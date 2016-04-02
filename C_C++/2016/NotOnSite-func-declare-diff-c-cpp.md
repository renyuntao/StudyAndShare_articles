# C/C++: func() 与 func(void) 的区别
2016-04-02    <br /><br />                   
                      
在C中，下面的两个函数声明是不同的:                
            
```cpp
void func();
void func(void);
```
`void func()` 表示函数 `func` 可以接受任意参数；而 `void func(void)` 则表示函数 `func` 不接收任何参数.                    
与C不同，在C++中，下面的两个函数声明是等价的:             
            
```cpp
void func();
void func(void);
```
