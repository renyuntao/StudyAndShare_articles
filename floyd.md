#Floyd Warshall
2015-10-07 <br />  
这里只是谈一下个人对[Floyd Warshall](https://en.wikipedia.org/wiki/Floyd%E2%80%93Warshall_algorithm)算法的理解，关于该算法的具体内容[维基百科](https://en.wikipedia.org/wiki/Floyd%E2%80%93Warshall_algorithm)上已经有很详细的说明了,可以参看那里.     
**Floyd Warshall**算法是关于求图中任意两点之间的最短路径的算法，例如假如要计算点**i**到点**j**之间的最短路径，该算法是通过取如下两种情况中的值较小的那一种作为点**i**到点**j**的最短路径:

- 直接计算点**i**到点**j**的路径长度  
- 计算点**i**到点**k**的距离，然后加上点**k**到点**j**的距离

在**Floyd Warshall**算法中，上述**k**的取值是**{1..N}**,其中**N**为图中顶点的个数,在具体实现**Floyd Warshall**算法的时候，按照下面的规则进行:

> 让图中的每个都经过1号顶点，并使**`graph[i][j]`**取值为**`graph[i][j]`**和**`graph[i][1]+graph[1][j]`**中的较小者   
> 让图中的每个都经过2号顶点，并使**`graph[i][j]`**取值为**`graph[i][j]`**和**`graph[i][2]+graph[2][j]`**中的较小者   
> 让图中的每个都经过1号顶点，并使**`graph[i][j]`**取值为**`graph[i][j]`**和**`graph[i][3]+graph[3][j]`**中的较小者   
> ...  
> 让图中的每个都经过1号顶点，并使**`graph[i][j]`**取值为**`graph[i][j]`**和**`graph[i][N]+graph[N][j]`**中的较小者   

下面是我用Python对**Floyd Warshall**算法的具体实现: 

    import itertools
    def floyd_warshall(graph,vertex_num):  #vertex_num为图中顶点的个数
    	for k in range(vertex_num):
    		for i,j in itertools.product(range(vertex_num),range(vertex_num)):
    			if graph[i][j] > graph[i][k] + graph[k][j]:
    				graph[i][j] = graph[i][k] + graph[k][j]
<br />  
一如既往，如果你对文章中的内容有任何疑问，或者是发现文章中有任何错误，都可以通过下面的地址发邮件告诉我.  
E-mail: rytubuntulinux@gmail.com <br />   

