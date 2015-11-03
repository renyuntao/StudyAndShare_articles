# 树:中序遍历(Iterative)
2015-10-18 <br />     
对一棵二叉树进行中序遍历，使用递归的方式来实现的函数十分简洁，如下:   

    void preorder(BiTreeNode *root)
    {
    	if(root != NULL)
    	{
    		preorder(root->left);
    		cout<<root->data;
    		preorder(root->right);
    	}
    }
&nbsp;&nbsp;&nbsp;但是形式的简洁并不代表程序执行效率的高效，相反，递归形式的遍历在执行效率上要输于非递归的形式，随着二叉树层次和节点数目的增加，这种执行效率的差距会越来越明显.本文将重点讨论非递归形式的遍历算法，即所谓的遍历树的**iterative algorithm**.        
[CodeProject](http://www.codeproject.com/Articles/21194/Iterative-vs-Recursive-Approacheshttp://www.codeproject.com/Articles/21194/Iterative-vs-Recursive-Approaches)上有一篇对比**iterative**方法和**recursive**方法的文章，二者性能的差别可以查看那里.   
## 非递归中序遍历算法
使用非递归遍历树的算法如下:

1. 设置一个空栈
2. 设置一个`current`指针，用它来指向当前节点
3. 将`current`指针指向根结点
4. 如果栈非空或`current`指针非空,则执行下一步;否则，*goto step 8*
5. **判断`current`指针是否为空，如果`current`指针非空，则执行下面的第6步;否则，执行下面的第7步**
6. **将`current`指针指向的节点入栈，并将`current`指针指向当前所指节点的左节点,_goto step 4_**
7. **出栈获取一个节点，访问该节点，并将`current`指针指向该节点的右节点，_goto step 4_**
8. 遍历结束

**注:上述算法的黑体部分即是该算法的循环体部分**   
## 算法的实现(C++)
下面是用C++对上面算法的实现:    

    void inOrder_ite(BiTreeNode *root)          
    {
		if(!root) return;
    	std::stack<BiTreeNode*> stk;
    	Node *current = root;
    	while(!stk.empty() || current)
    	{
    		if(current)
    		{
    			stk.push(current);
    			current = current->lchild;
    		}
    		else
    		{
    			current = stk.top();
    			stk.pop();
    			cout<<current->data<<" ";
    			current = current->rchild;
    		}
    	}
    }
[GitHub](https://github.com/renyuntao/binary_tree_traversal/blob/master/inOrder/iterative_tra.cxx)上有一个中序遍历的完整例子，关于该函数的详细使用可以查看那里.    
<br />     
一如既往，如果你对文章中的内容有任何疑问，或者是发现文章中有任何疑问，都可以通过下面的地址发邮件告诉我.    
E-mail: rytubuntulinux@gmail.com <br /><br />   
     
