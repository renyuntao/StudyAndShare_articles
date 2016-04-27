# Sieve of Eratosthenes
2015-11-21  <br />       
给出一个大于等于2的正整数n，要求出2到n之间的所有素数，最直接的想法就是从2开始，一直到n，依次判断其中的每一个奇数(2不是奇数，但也包括在内)是否为素数，这是一中效率比较低的方法。这里分享一种效率较高的算法 —— [Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes#Algorithm_complexity) ,这种算法计算2到n之间所有素数的方法如下:       

1. 创建一个从2到n的序列        
2. 设置一个变量p，初始化为2(最小的素数)      
3. 除p自身外，将序列中所有能整除p的数做上标记(即将2p,3p,...做上标记)
4. 寻找第一个大于p且未做标记的数。如果没有找到这样的数，算法结束,序列中所有未做标记的数即为所要求的素数;否则，让p等于这个新的数，_goto step 3_

下面来举一个例子，假如n等于10,即要求2-10之间的所有素数,初始序列如下:       

> 2 3 4 5 6 7 8 9 10

消除序列中能整除2的数字:     

> 2 3 <s>4</s> 5 <s>6</s> 7 <s>8</s> 9 <s>10</s>

消除序列中能整除3的数字:     

> 2 3 <s>4</s> 5 <s>6</s> 7 <s>8</s> <s>9</s> <s>10</s>

消除序列中能整除5的数字:      

> 2 3 <s>4</s> 5 <s>6</s> 7 <s>8</s> <s>9</s> <s>10</s>

消除序列中能整除7的数字:      

> 2 3 <s>4</s> 5 <s>6</s> 7 <s>8</s> <s>9</s> <s>10</s>

最终结果为:   

> 2 3 5 7

下面是我用Python对这个算法的实现:   

    def findAllPrime(arr,mark,n):
    	if n < 2:
    		return 
    	
    	# Initial
    	for i in range(2,n+1):
    		arr[i-2] = i
    		mark[i-2] = False
    	
    	j = -1
    	find = False
    	while True:
    		for i in range(j+1,n-2+1):
    			if not mark[i]:
    				p = arr[i]
    				j = i
    				find = True
    				break
    
    		if not find:
    			break
    
    		final = True
    		for i in range(j+p,n-2+1,p):
    			if not mark[i]:
    				mark[i] = True
    				final = False
    
    		if final:
    			break
    	for i in range(n-2+1):
    		if not mark[i]:
    			print(arr[i],end=' ')
    	print()

----------
**拓展:** 该算法有一个缺点，当n的值很大时，素数的范围也将很大，这样就有可能导致内存容纳不下，解决这个问题的方法是使用一个改进的算法 —— [Segmented Sieve](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes#Segmented_sieve),这个算法将[2,n]范围内的数分为几段，然后依次计算每一段内的素数，你可以参考[维基百科](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes#Segmented_sieve)的相关页面来了解这个算法.          
