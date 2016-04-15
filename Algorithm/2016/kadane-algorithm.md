# Kadane's Algorithm                
2016-04-11   <br /><br />                
         
**Kadane's Algorithm** 用来解决这样一个问题:             
        
> 在一个一维数组中，寻找连续子数组的最大和                   
           
举例来说，假如有一个一维数组为 **{-2,-3,4,-1,-2,1,5,-3}**, 在这个数组中连续子数组最大的和为 **7**, 所对应的连续子数组为 **{4,-1,-2,1,5}**.     
      
**Kadane's Algorithm** 的算法如下:               
           
1. 初始化两个变量 `max_so_far` 和 `max_ending_here`,令它们的值都为0.     
2. 遍历一维数组 `arr` 中的每一个元素:             
	- max_ending_here += arr[i]
	- if(max_ending_here < 0)   
	  &nbsp;&nbsp;&nbsp;&nbsp;max_ending_here = 0
	- if(max_ending_here > max_so_far)   
	   &nbsp;&nbsp;&nbsp;&nbsp;max_so_far = max_ending_here
3. 返回变量 `max_so_far` 的值，该值即为所求的最大和            
          
下面是算法的具体实现(**C++**):                
          
```cpp
int kadane(int arr[], int len)
{
	int max_so_far = 0;
	int max_ending_here = 0;

	for(int i = 0; i < len; ++i)
	{
		max_ending_here += arr[i];
		if(max_ending_here < 0)
			max_ending_here = 0;
		else if(max_ending_here > max_so_far)
			max_so_far = max_ending_here;
	}

	return max_so_far;
}
```
关于 **Kadane's Algorithm** 更为详细的介绍可以参阅 **[维基百科](https://en.wikipedia.org/wiki/Maximum_subarray_problem#Kadane.27s_algorithm)**,那里还有一个以 **Python** 来实现的 **Kadane's Algorithm**. 
