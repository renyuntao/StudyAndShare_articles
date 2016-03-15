# Python:不要给Lambda表达式赋予一个名称               
2016-03-11  <br />               
              
当想要使用一个具有名称的函数时，应该使用 **def** 来定义一个函数，而不应该将一个 **Lambda** 表达式赋予一个变量.                       
             
**Yes:**               
          
```python         
def func(x):
	return x+1                 
```                 
**No:**                   
          
```python
func = lambda x: x+1
```
这样做的原因在于，**Lambda** 表达式的优势在于它可以直接嵌入到一个更大的表达式之中，使用 **Lambda** 表达式应该专用于这个目的，在其它情况下，应该使用 **def** 来定义个函数。        
                
例如，现在有两个列表，现在要求这两个列表中对应元素的差值的绝对值，并将结果保存到第三个列表中:                   
            
**Yes:**              
        
```python
ls1 = [1,2,3,4]
ls2 = [7,2,7,6]
ls3 = [(lambda x,y: x-y if x > y else y-x)(x,y) for x,y in zip(ls1,ls2)]
print(ls3)
```           
**No:**                
         
```python
ls1 = [1,2,3,4]
ls2 = [7,2,7,6]    
func = lambda x,y: x-y if x > y else y-x
ls3 = [func(x,y) for x,y in zip(ls1,ls2)]
```
           
            
**参考:** [PEP 8](https://www.python.org/dev/peps/pep-0008/)
