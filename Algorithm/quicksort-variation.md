# 快速排序变体
2015-11-18  <br />     
一般书本上在讲解快速排序算法的时候，都会介绍类似下面的**partition**操作:      

    int partition(int arr[],int low,int high)
    {
    	int i = low, j = high;
    	int pivot = arr[low];
    
    	while(1)
    	{
    		while(i < high && arr[i] <= pivot) ++i;
    		while(j > low && arr[j] > pivot) --j;
    		if(i < j)
    			std::swap(arr[i],arr[j]);
    		else
    			break;
    	}
    
    	// Swap
    	std::swap(arr[low],arr[j]);
    
    	return j;
    }
&nbsp;&nbsp;&nbsp;这种快速排序算法的平均时间复杂度为**O(nlog<sup>n</sup>)**,但是当要排序的数据集合中有很多的重复元素或者数据集合中很多部分已序时，这种类型的快速排序算法就会变得很慢，它的时间复杂度就会接近**O(n<sup>2</sup>)**。我做了一个测试:**在[0,10000]范围内随机产生`5000000`个数据，此时这`5000000`个数据的数据集合中重复的数据已经比较多了。用上述类型的快速排序算法进行排序，所花费的时间是`5.258s`左右**,这已经是比较慢的速度了，如果再在数据集合中增加重复的数据，比如在[0,1000]范围内产生`5000000`个随机数据，这种类型的快速排序方法会变得难以想象的慢,也就是说数据集合中重复的数据越多，这种快速排序的方法就越慢.             
下面将介绍一种快速排序的变体，对于数据集合中有很多的重复元素或者数据集合中有很多部分已序的情况，使用这种变体将会使效率大大提高，这种快速排序的具体实现如下所示(**C++**):

    void quicksort(int a[],int lo,int hi)
    {
    	int i = lo, j = (lo + hi) / 2, k = hi;
    	int pivot;
    	if(a[k] < a[i])             // Median of 3
    		std::swap(a[k],a[i]);
    	if(a[j] < a[i])
    		std::swap(a[j],a[i]);
    	if(a[k] < a[j])
    		std::swap(a[k],a[j]);
    	pivot = a[j];
    	while(i <= k)               // Partition
    	{
    		while(a[i] < pivot)  ++i;    // Note: no '='
    		while(a[k] > pivot) --k;     // Note: no '='
    		if(i <= k)
    		{
    			std::swap(a[i],a[k]);
    			++i;
    			--k;
    		}
    	}

		// recurse
    	if(lo < k)                
    		quicksort(a,lo,k);
    	if(i < hi)
    		quicksort(a,i,hi);
    }
&nbsp;&nbsp;&nbsp;同样是在[0,10000]范围内产生`5000000`个随机数据，使用上述快速排序的方法进行排序，在我的电脑上所用的时间是`1.107s`左右，速度提高了 **`78.9%`** !而且，对于这种快速排序的方法，数据集合内重复数据越多，这种快速排序的效率也就越高。不过，当数据集合中重复元素较少的时候，该种快速排序的方法相对于文章开头介绍的那种快速排序方法就要差一些了。

-----------
**注意:** 上面的快速排序中如果将`swap()`替换为`int tmp = a; a = b; b = tmp;`这种形式的语句，程序的执行效率将会有所提高，因为函数调用相对于执行这三条语句要花费较多的时间。不过这里只是为了叙述方便而采用了`swap()`.

----------
