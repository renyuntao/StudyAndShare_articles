# 对比两种快速排序的执行效率
2015-10-24 <br />    
快速排序有两种实现方法，这两种方法的区别在于划分操作上，一种快速排序的划分方法如下:    

    int partition(int arr[],int low,int high)
    {
    	int i = low + 1;
    	int j = high;
    	int tmp = arr[low];
    	while(1)
    	{
    		while(j != low && arr[j] > tmp) --j;   //from right to left
    		while(i != high && arr[i] <= tmp) ++i;  //from left to right
    		if(i < j)
    		{
    			int t = arr[j];
    			arr[j] = arr[i];
    			arr[i] = t;
    		}
    		else
    			break;
    	}
    	return j;
    }
另一种快速排序的划分操作如下:    

    int partition(int arr[],int low,int high)
    {
    	int tmp = arr[low];
    	int i = low;
    	int j = high;
    	while(i < j)
    	{
    		while(i < j && arr[j] > tmp) --j;    //from right to left
    		if(i < j)
    		{
    			arr[i] = arr[j];
    			++i;
    		}
    		while(i < j && arr[i] <= tmp) ++i;    //from left to right
    		if(i < j)
    		{
    			arr[j] = arr[i];
    			--j;
    		}
    	}
    	return j;
    }
&nbsp;&nbsp;&nbsp;一直好奇于这两种方法的执行效率哪个更好一些，于是今天便用两种划分操作实现了快速排序，然后分别用这两种快速排序的方法对相同的`500万`随机数据进行排序，并进行计时。这两种快速排序方法各被我执行了3次，对执行时间取平均值后，结果如下:    

>第一种划分操作实现的快速排序方法: 5.234547s    
>第二种划分操作实现的快速排序方法: 5.206447s   

从结果可以看出，对`500万`数据进行排序，两种实现方法的时间差距不到`0.03s`,那么对较小量的数据进行排序时，这两种方法的执行效率可以视为相等了.       
<br />
我将这两种快速排序的方法放到了[GitHub](https://github.com/renyuntao/sort/tree/master/Comp_Quicksort)上，你可以从那里获取源代码，然后在你的电脑上运行试试看哪种方法更快一些。
<br /><br />    
一如既往，如果你对文章中的内容有任何疑问，或者是发现文章中有任何错误，都可以通过下面的地址发邮件告诉我.    
E-mail: rytubuntulinux@gmail.com   <br /><br />    

