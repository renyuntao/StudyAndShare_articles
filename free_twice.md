# 对一个指针调用两次free()的后果      
2015-10-31  <br />         
对一个指针调用两次free()所产生的影响随指针的值不同而不同，C标准对此有明确的规定，下面是引用C标准中的一段话:    

>“The free function causes the space pointed to by its argument to be deallocated, that is, made available for further allocation. If the argument is a null pointer, no action occurs. Otherwise, if the argument does not match a pointer earlier returned by a memory management function, or if the space has been deallocated by a call to free or realloc, the behavior is undefined.”   

&nbsp;&nbsp;&nbsp;由上面的描述可以知道，对一个指针调用两次free(),如果第二次调用free()时指针的值为NULL(也就是说第一次调用free()后，将指针的值赋值为NULL),那么什么也不会发生，这是一种比较好的情况;不过如果在第二次调用free()时，指针的值不为空(即第一次调用free()函数之后，未将指针的值赋值为NULL),则此时结果是未定义的.       
看下面的例子:    

    // Program 1
    int main()
    {
    	int *p = (int*)malloc(sizeof(int));
    	free(p);
    	p = NULL;
    	free(p);    // no action occurs
    }
    
    // Program 2
    int main()
    {
    	int *p = (int*)malloc(sizeof(int));
    	free(p);
    	free(p);     //未定义的行为
    	return 0;
    }
&nbsp;&nbsp;&nbsp;此外，对一个指针调用两次free()的错误，在编译阶段并不能被检测出(至少gcc是这样的)，因此，当我们对一个指针调用free()之后，一定要将指针的值置为NULL，这样才能免除对同一块内存释放两次的错误的困扰。     
