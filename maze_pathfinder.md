#迷宫中搜索出口路径
2015-10-05 <br />   
在[维基百科](https://en.wikipedia.org/wiki/Maze_solving_algorithm#Recursive_algorithm)上看到一个关于在迷宫中搜索出口路径的算法，感觉很有意思，便想记录下来，下面是关于迷宫的描述:  

>迷宫是以一个二维数组的形式给出，如下所示:   
1111101110111  
0010001000001  
1011111111111  
1000000010100  
1111111110101  
0010000010001  
1111101010111  
1010001000100  
1011111111111     
上述数组中**1**表示路径，**0**表示墙,迷宫的出口在数组的左上角,迷宫的入口在数组的右下角,任务是编写程序找出从迷宫入口到迷宫出口的一条路径.  

&nbsp;&nbsp;&nbsp;解决这个问题的一种算法是采用递归的方式，该算法以迷宫入口出的**X**和**Y**的值开始，如果该位置不是墙，则以该位置的临近位置的**X**和**Y**的值递归调用该算法，直到最终**X**和**Y**的值分别为迷宫出口处的值，此时，该算法会保存这条通往迷宫出口的路径.   
在[维基百科]上有一个以**Java**实现该算法的例子，这里我采用**Python**来实现该算法，如下:  

    def recursiveSolve(x,y):
    	if x == endX and y == endY:
    		return True
    	if maze[x][y] == 0 or wasHere[x][y]:  # 0 = wall and 1 = path.
    		return False
    	wasHere[x][y] = True
    	if x != 0:   #Checks if not on left edge
    		if recursiveSolve(x-1,y):
    			correctPath[x][y] = True
    			return True
    	if x != height - 1:   #Checks if not on right edge
    		if recursiveSolve(x+1,y):
    			correctPath[x][y] = True
    			return True
    	if y != 0:     #Checks if not on top edge
    		if recursiveSolve(x,y-1):  
    			correctPath[x][y] = True
    			return True
    	if y != width-1:    #Checks if not on bottom edge
    		if recursiveSolve(x,y+1):
    			correctPath[x][y] = True
    			return True
    	return False
    width,height = [int(i) for i in input().split()]   #input the weight and height of maze
    wasHere = [[False for j in range(width)] for i in range(height)]  # 'wasHere[x][y]=True' denote already use the x,y before 
    correctPath = [[False for j in range(width)] for i in range(height)]  #Set boolean array to default value,here is False
    endX,endY = 0,0     #Ending X and Y values of maze
	startX,startY = height-1,width-1   # Starting X and Y values of maze
    maze = [[] for i in range(height)]
    for i in range(height):   # initial maze
    	input_ = input()
    	for j in input_:
    		maze[i].append(int(j))
	recursiveSolve(startX,startY)
    for i in range(height):
    	print(correctPath[i])    #print the path
<br />   
一如既往，如果你对文章中的内容有任何疑问，或者是发现文章中有任何错误，都可以通过下面的地址发邮件告诉我.  
E-mail: rytubuntulinux@gmail.com  <br /><br />  

