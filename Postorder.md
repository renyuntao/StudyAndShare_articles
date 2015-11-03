# 树:后序遍历(Iterative)
2015-10-18 <br />     
对一棵二叉树进行后序遍历，使用递归的方式来实现的函数十分简洁，如下:   

    void preorder(BiTreeNode *root)
    {
    	if(root != NULL)
    	{
    		preorder(root->left);
    		preorder(root->right);
    		cout<<root->data;
    	}
    }
&nbsp;&nbsp;&nbsp;但是形式的简洁并不代表程序执行效率的高效，相反，递归形式的遍历在执行效率上要输于非递归的形式，随着二叉树层次和节点数目的增加，这种执行效率的差距会越来越明显.本文将重点讨论非递归形式的遍历算法，即所谓的遍历树的**iterative algorithm**.        
[CodeProject](http://www.codeproject.com/Articles/21194/Iterative-vs-Recursive-Approaches)上有一篇对比**iterative**方法和**recursive**方法的文章，二者性能的差别可以查看那里.   
## 非递归后序遍历算法
这个算法有一些复杂，原本想像[树:中序遍历(Iterative)](http://www.studyandshare.info/)一文中那样把算法的步骤一步一步写出来，但是发现很混乱，感觉效果不好，因此这里改为对这个算法进行下面的描述:   
>这个算法仍然是使用一个栈来完成遍历操作。算法中用一个变量`prev`来跟踪前一个访问的节点，使用另一个变量`curr`来表示当前节点(在这个算法中该节点总是位于栈顶).当`prev`是`curr`的父节点时，表示我们正在向下遍历树，在这种情况下，如果`curr`节点的左孩子节点存在，我们就把将`curr`节点的左孩子节点压入栈中;如果`curr`节点的左孩子节点不存在，我们再看`curr`节点的右孩子节点是否存在，如果存在的话，就将`curr`节点的右孩子节点压入栈中。如果`curr`节点的左右孩子节点都不存在，则表示`curr`节点为叶子节点，我们就访问该节点(输出该节点的值)，然后将它出栈。      
>
>如果`prev`是`curr`的左孩子节点，则表示我们正在从左侧向上遍历树，那么我们就看`curr`节点的右孩子节点是否存在，如果存在的话，就将`curr`节点的右孩子节点压入栈中;如果不存在的话，就访问`curr`节点(输出该节点的值)然后将其弹出栈。     
>
>如果`prev`是`curr`的右孩子节点，则表明我们正在从右侧向上遍历树。在在这种情况下，就访问`curr`节点(输出该节点的值)并将该节点弹出栈.    

## 算法的实现(C++)
下面是用C++对这个算法的实现:    

    void postOrder_ite(BinaryTree *root)
    {
    	if(!root) return;
    	stack<BinaryTree*> s;
    	s.push(root);
    	while(!s.empty())
    	{
    		BinaryTree *curr = s.top();
    		//向下遍历树
    		if(!prev || prev->left == curr || prev->right == curr)
    		{
    			if(curr->left)
    				s.push(curr->left)
    			else if(curr->right)
    				s.push(curr->right)
    			else
    			{
    				cout<<curr->data<<" ";
    				s.pop();
    			}
    		}
    		//从左侧向上遍历树
    		else if(curr->left == prev)
    		{
    			if(curr->right)
    				s.push(curr->right)
    			else
    			{
    				cout<<curr->data<<" ";
    				s.pop();
    			}
    		}
    		//从右侧向上遍历树
    		else if(curr->right == prev)
    		{
    			cout<<curr->data<<" ";
    			s.pop();
    		}
    		prev = curr;    //记录前一个遍历的节点
    	}
    }
[GitHub](https://github.com/renyuntao/binary_tree_traversal/blob/master/Postorder/postorder_ite.cxx)上有一个中序遍历的完整例子，其中的遍历函数与上述函数稍有不同(GitHub上的函数更为简洁)，但是所用算法完全相同，具体的使用可以查看那里.     
<br />     
一如既往，如果你对文章中的内容有任何疑问，或者是发现文章中有任何错误，都可以通过下面的地址发邮件告诉我.    
E-mail: rytubuntulinux@gmail.com <br /><br />    
    
