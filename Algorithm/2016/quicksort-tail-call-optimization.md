# 快速排序优化:降低最差情况的空间复杂度             
2016-04-01    <br /><br />                

在学《数据结构》的时候，书本上的通常给出如下的快速排序的算法:              
              
```cpp
void quicksort(int arr[], int low, int high)
{
	if(low < high)
	{
		int pi = partition(arr, low, high);
		
		quicksort(arr, low, pi-1);
		quicksort(arr, pi+1, high);
	}
}
```
这种方法实现的快速排序方法在最坏情况下的空间复杂度是 **O(n)**, 最坏的情况发生在所要排序的数组是已序的的情况下,这样每次划分数组为两部分，一部分中总是有 **0** 个元素，而另一部分中总是有 **n-1** 个元素。借用 [维基百科](https://en.wikipedia.org/wiki/Quicksort) 中的一句话，这种实现快速排序的方法是 **naive** 的。                       
之前在文章 [Tail Recursion & Tail Call Elimination]() 一文中说过，快速排序也是一种 **Tail Recursion**, 因此我们消除上述程序片段中的最后一个递归调用来对其进行优化:                
            
```cpp
void quicksort(int arr[], int low, int high)
{
	while(low < high)
	{
		int pi = partition(arr, low, high);
		
		quicksort(arr, low, pi-1);
		low = pi + 1;
	}
}
```
虽然上述程序减少了一个递归调用，但是在最坏的情况下，空间复杂度仍然可能为 **O(n)**, 因为每次划分数组时，划分的第一部分可能总是有 **n-1** 个元素。因此，我们可以继续对上述代码进行优化，使得每次都对包含数组元素较少的那一个划分部分进行递归调用，如下:               
             
```cpp
void quicksort(int arr[], int low, int high)
{
	while(low < high)
	{
		int pi = partition(arr, low, high);
		
		if(pi - low < high - pi)
		{
			quicksort(arr, low, pi-1);
			low = pi + 1;
		}
		else
		{
			quicksort(arr, pi+1, high);
			high = pi - 1;
		}
	}
}
```
这样，在最坏情况下，快速排序的时间复杂度就会降低到 **O(log<sup>n</sup>)**.               
