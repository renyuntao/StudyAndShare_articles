# 斐波纳契数的一个应用 —— Fibonacci Search         
2015-12-13    <br />           
斐波纳契数中存在有如下规律:            

> F(n-1) &approx; (2/3)*F(n)        
> F(n-2) &approx; (1/3)*F(n)        

&nbsp;&nbsp;&nbsp;利用斐波纳契数的这个规律，可以实现一个类似于二叉搜索的算法。与二叉搜索不同的是，二叉搜索每次都是一个较大
的范围分成两个两个较小的且范围相同的子范围，而利用斐波纳契数实现的查找算法则是将一个较大的范围分成两个不等的子范围，一个子
范围占原来范围的1/3,另一个子范围占原来范围的2/3,这样的搜索算法被称为 **Fibonacci Search**            
假定现在要在一个长度为n已序数组arr中寻找一个数n，使用 **Fibonacci Search** 进行搜索的算法如下:          

1. 找出第一个大于等于n的斐波纳契数fibM(第M个斐波纳契数),并将fibM的前一个斐波纳契数(第(M-1)个斐波纳契数)定义为fib1，将fibM)的前两个斐波纳契数(第(M-2)个斐波纳契数)定义为fib2.数组被fib2分为了两个不等的部分，fib2之前的部分占整个数组的1/3,fib2之后的部分占整个数组的2/3.                       
2. 当fibM大于1时，循环执行如下操作:             
	- 比较x与占1/3范围的那个部分的最后一个元素t的大小关系       
	- 如果二者相等，则表示查找成功，返回索引值
	- 如果x小于t,则表明要查找的x位于占1/3范围的那个部分中，因此，将fibM,fib1,fib2都向前移动两位(即fibM=fib2,fib1=fib1-fib2,
	fib2=fibM-fib1)      
	- 如果x大于t，则表明要查找的x位于占2/3范围的那个范围中，因此，将fibM,fib1,fib2都向前移动一位(即fibM=fib1,fib1=fib2,fib2
	=fibM-fib1)      
3. 当fibM等于1时，表明只剩下一个元素可作比较，此时将x与该元素做比较，如果相等，则查找成功，返回索引值;否则查找失败。     

关于 **Fibonacci Search** 算法,你可以参阅 [维基百科](https://en.wikipedia.org/wiki/Fibonacci_search_technique) 来获取更详
细的说明.           
下面对该算法的实现(C++):       

    int fibonacciSearch(int arr[],int len,int x)
    {
    	int fib2 = 0;
    	int fib1 = 1;
    	int fibm = 1;
    
		// 获取第一个大于等于数组长度的斐波纳契数
    	while(fibm < len)
    	{
    		fib2 = fib1;
    		fib1 = fibm;
    		fibm = fib2 + fib1;
    	}
    
    	int offset = -1;
    
    	while(fibm > 1)
    	{
    		int i = offset+fib2 <= len-1 ? offset+fib2 : len-1;
    
    		if(arr[i] < x)
    		{
    			fibm = fib1;
    			fib1 = fib2;
    			fib2 = fibm - fib1;
    			offset = i;
    		}
    		else if(arr[i] > x)
    		{
    			fibm = fib2;
    			fib1 = fib1 - fib2;
    			fib2 = fibm - fib1;
    		}
    		else
    			return i;
    	}
		// arr[offset+1]总是表示前1/3范围的那部分的第一个元素
		if(fib1 && arr[offset+1] == x) return offset+1;
		return -1;
	}
**时间复杂度:** O(log<sup>n</sup>)        
**空间复杂度:** O(1)         
&nbsp;&nbsp;&nbsp;一些资料上说 **Fibonacci Search** 的平均查找效率要比二叉搜索算法效果好，但我做了一个测试，在一千万的已序序列中查找数据，发现二者的差别很小，几乎可以忽略，可能还是测试的数据量比较小吧,在一些数据量更大的场所，二者的区别可能会变得明显一些.           
