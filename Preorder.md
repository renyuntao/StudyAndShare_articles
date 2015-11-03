# 树:前序遍历(Iterative)
2015-10-18 <br />     
对一棵二叉树进行前序遍历，使用递归的方式来实现的函数十分简洁，如下:   

    void preorder(BiTreeNode *root)
    {
    	if(root != NULL)
    	{
    		cout<<root->data;
    		preorder(root->left);
    		preorder(root->right);
    	}
    }
&nbsp;&nbsp;&nbsp;但是形式的简洁并不代表程序执行效率的高效，相反，递归形式的遍历在执行效率上要输于非递归的形式，随着二叉树层次和节点数目的增加，这种执行效率的差距会越来越明显.本文将重点讨论非递归形式的遍历算法，即所谓的遍历树的**iterative algorithm**.        
## 非递归前序遍历算法
使用非递归遍历树的算法如下:    

- 设置一个空的栈
- 将树的根结点入栈
- 当栈非空时，循环执行如下操作:
	- 出栈取得一个节点，访问该节点
	- 若该节点的右节点非空，则将该节点的右节点入栈
	- 若该节点的左节点非空，则将该节点的左节点入栈
- 当栈空时，整棵树遍历完成

上述算法中，首先将右节点入栈，是为了在访问节点的时候可以首先访问左节点**(LIFO)**，而这也符合了前序遍历树的要求。   
## 算法的实现(C++)
下面是用C++对上面算法的实现:

    void Preorder_ite(BiTreeNode *root)
    {
    	if(root == NULL)
    		return;
    	std::stack<BiTreeNode*> stk;
    	stk.push(root);
    	while(stk.empty() == false)
    	{
    		struct Node *node = stk.top();
    		cout<<node->data<<" ";
    		stk.pop();
    		
    		if(node->rchild)
    			stk.push(node->rchild);
    		if(node->lchild)
    			stk.push(node->lchild);
    	}
    }
[GitHub](https://github.com/renyuntao/binary_tree_traversal/blob/master/Preorder/Preorder_ite.cxx)上有一个前序遍历的完整例子，关于该函数的详细使用可以查看那里.     
<br />    
一如既往，如果你对文章中的内容有任何疑问，或者是发现文章中有任何错误，都可以通过下面的地址发邮件告诉我.    
E-mail: rytubuntulinux@gmail.com <br /><br />   
   
