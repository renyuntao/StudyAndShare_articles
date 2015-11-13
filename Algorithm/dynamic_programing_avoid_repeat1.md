# 动态规划:解决重复计算问题 (一)
2015-11-11   <br />      
动态规划(**dynamic programming**)适合于解决具有如下性质的问题:     

- **最优子结构**  如果将一个问题分解为多个子问题，而综合这多个子问题的最优解所得到的解即为这个问题的最优解，则该问题具有最优子结构性质      
- **重复计算问题**  即将一个问题分解为多个子问题后，这些子问题有重叠，计算时会导致重复计算   

如果一个问题具有如上两个性质，则可以考虑使用动态规划来解决问题.       
下面将讨论如何使用动态规划来避免重复计算问题，从而提高程序的执行效率.        
使用递归方法计算斐波纳契(**Fibonaaci**)数列的第n项是一个典型的重复计算的例子:      

    int nthFibonaaci(int n)  
    {
    	assert(n > 0);
    	if(n == 1 || n == 2)
    		return 1;
    	return nthFibonaaci(n-1) + nthFibonaaci(n-2);
    }
以计算第5项斐波纳契数为例:      

        		 F(5)
        	   /      \
        	F(4)       F(3)
           /  \         /       \
        F(3) F(2)    F(2)    F(1)
       /  \
    F(2) F(1)
&nbsp;&nbsp;&nbsp;由上可以清楚的看到，计算第5项斐波纳契数时，进行了很多重复计算，因此使用递归的方法计算第n项斐波纳契数的效率并不高.动态规划(**dynamic programming**)中，解决重复计算问题的一个方法就是使用辅助空间 —— 通常是一个数组，来保存计算的结果，当再次用到这个结果时，直接从辅助空间获取，而不再进行重复计算，下面是用动态规划的方法来计算第n个斐波纳契数的程序(**C++**):        

    int nthFibonaaci(int n)
    {
    	assert(n > 0);
    	int fib[n+1];
    	fib[0] = 0;
    	fib[1] = 1;
    	for(int i = 2; i <= n; ++i)
    		fib[n] = fib[n-1] + fib[n-2];
    	return fib[n];
    }
**对上面方法的空间优化:**因为在计算第k(k>2)个斐波纳契数时，只需要知道第(k-2)和(k-1)个斐波纳契数就足够了，因此只要存储第(k-1),(k-2)个斐波纳契数就足够了。这样一个更好的方法是使用两个变量而不是一个数组来存储值，这样就对上面的方法进行了空间优化。程序如下:      

    int nthFibonaaci(int n)
    {
    	assert(n > 0);
    	int a = 0, b = 1;
    	int c;
    	for(int i = 2; i <= n; ++i)
    	{
    		c = a + b;
    		a = b;
    		b = c;
    	}
    	return b;
    }