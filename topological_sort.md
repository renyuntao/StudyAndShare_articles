# 拓扑排序
2015-10-20 <br />
## 拓扑排序的简单介绍
&nbsp;&nbsp;&nbsp;有时候对一些事物的排序是根据需求来排序的，比如你想要在使用Django来搭建一个网站，首先需要了解一些Python和HTML的知识，而要想学习Python，你首先要在你的电脑上安装Python,如果使用拓扑排序，上述过程就是首先安装Python，之后学习Python和HTML，之后再学习Django，最后使用Django来搭建网站。这就是拓扑排序。
## 拓扑排序的算法
拓扑排序的算法不止一种，这里只是介绍**Kahn's algorithm**,关于拓扑排序的其他算法可以参见[维基百科](https://en.wikipedia.org/wiki/Topological_sorting).**Kahn's algorithm**过程如下:

- 创建一个空列表`L`，用它来存储已序的元素
- 常见一个集合`S`，并将图中没有传入边的节点放入该集合中
- 当`S`不为空时，循环执行如下操作:
	- 从`S`中移除一个结点`n`
	- 将上述节点`n`添加到列表`L`的尾部
	- 对于每一个`n -> m`(即节点`n`到节点`m`有边)，移除节点`n`到节点`m`之间的边
	- 如果上述`m`再无传入边，则将`m`节点加入到`S`集合中
- 如果图中仍含有边，则返回错误(图中包含环);否则，返回列表`L`(含有拓扑排序了的节点)

## 拓扑排序示例
以下图为例，如果使用拓扑排序的方法对下图中的元素排序，则其中的一种拓扑排序(拓扑排序的结果可能有很多种)为:4 1 6 5 3 2<br /><br />
![picture](http://i13.tietuku.com/e03d6a73a248d36a.png)<br /><br />
## 拓扑排序的实现
下面是我用Python对上述算法的实现:   

    def topologicalsort(graph):
    	S,L = [],[]
    	length = len(graph)
    	for col in range(length):    #initial S
    		have_incoming_edge = False
    		for row in range(length):
    			if graph[row][col] == 1:    #1 denote have edge 
    				have_incoming_edge = True
    				break
    		if not have_incoming_edge:
    			S.append(col)
    	test_node = []
    	while S:
    		test_node.clear()
    		tmp = S.pop()
    		L.append(tmp+1)
    		for col in range(length):
    			if graph[tmp][col] == 1:
    				graph[tmp][col] = 0   #0 denote don't have edge
    				test_node.append(col)
    		for t_node in test_node:
    			have_incoming_edge = False
    			for row in range(length):
    				if graph[row][t_node] == 1:
    					have_incoming_edge = True
    					break
    			if not have_incoming_edge:
    				S.append(t_node)
		for row,col in itertools.product(range(length),range(length)):
			if graph[row][col] == 1:
				print('Error:have cycle')
				return
    	print(*L)
[GitHub](https://github.com/renyuntao/sort/tree/master/TopologicalSort)上有一个完整的例子，关于该函数的详细使用可以查看那里.     
<br />    
一如既往，如果你对文章中的内容有任何疑问，或者是发现文章中有任何错误，都可以通过下面的地址发邮件告诉我.     
E-mail: rytubuntulinux@gmail.com <br /><br />     
    
