# 动态规划:解决重复计算问题 (二)
2015-11-11   <br />      
动态规划(**dynamic programming**)适合于解决具有如下性质的问题:     

- **最优子结构**  如果将一个问题分解为多个子问题，而综合这多个子问题的最优解所得到的解即为这个问题的最优解，则该问题
具有最优子结构性质      
- **重复计算问题**  即将一个问题分解为多个子问题后，这些子问题有重叠，计算时会导致重复计算   

如果一个问题具有如上两个性质，则可以考虑使用动态规划来解决问题.         
&nbsp;&nbsp;&nbsp;[动态规划:解决重复计算问题(一)](./.)中介绍了如何使用一个一维数组保存计算过的结果，从而避免重复计算。这篇文章将讨论另外一个使用辅助空间保存计算结果进而避免重复计算的例子，只是更为复杂一些.     
&nbsp;&nbsp;&nbsp;下面将求解这样的问题:**将n个对象分割成k个非空集合，求有多少种分割的方法**.例如，将3个对象(这里假设
这三个对象为数字1,2,3)分割为2个集合，则有3中分割的方法:{{1,2},{3}},{{1},{2,3}},{{1,3},{2}}.这样的问题称为
[Stirling numbers of the second kind](https://en.wikipedia.org/wiki/Stirling_numbers_of_the_second_kind),你可以参考
[维基百科](https://en.wikipedia.org/wiki/Stirling_numbers_of_the_second_kind)的相关页面来了解更多相关信息.        
现在假定将n个对象分割为k个非空集合的方法数为S(n,k),则计算S(n,k)有如下关系:   

>S(n,k) = k * S(n-1,k) + S(n-1,k-1)

&nbsp;&nbsp;&nbsp;如果编程来计算S(n,k)的话，很容易想到的是使用递归来解决这个问题，但是类似于使用递归来计算第n项斐波纳契数一样，使用递归来解决这个问题同样存在很多重复的计算,而具有重复计算的问题，则应该考虑使用动态规划(**dynamic programming**)来解决这个问题.这个例子中，同样是使用辅助空间 —— 一个数组来保存计算过的结果，不过这次使用的是一个二维数组dp[][],因为S(n,k)中包含有两个变量，下面是使用动态规划(**dynamic programming**)来解决这个问题的程序(C++):      

    #include<iostream>
    using namespace std;
    
    int countS(int n,int k)
    {
    	int dp[n+1][k+1];
    
    	//初始化dp[][]
    	for(int i = 0; i <= n; ++i)
    		dp[i][0] = 0;
    	for(int i = 0; i <= k; ++i)
    		dp[0][k] = 0;
    	
    	for(int i = 1; i <= n; ++i)
    	{
    		for(int j = 1; j <= i; ++j)
    		{
    			// 将n个对象分割成1个集合和将n个对象
    			// 分割成n个集合的方法数都为1
    			if(j == 1 || i == j)
    				dp[i][j] = 1;
    			else
    				dp[i][j] = j*dp[i-1][j] + dp[i-1][j-1];
    		}
    	}
    	return dp[n][k];
    }
    
    int main()
    {
    	cout<<countS(3,2)<<endl;
    	return 0;
    }
    
