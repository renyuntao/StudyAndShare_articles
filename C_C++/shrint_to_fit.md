# STL:如何去除vector中多余容量?
2015-11-03 <br />     
&nbsp;&nbsp;&nbsp;对于一个矢量，当你向其中加入元素的时候，如果矢量的空间不足，它会自动扩充容量，这是矢量的一个很好的优点;不过，当你从矢量中删除元素的时候，一个矢量却不会因为元素的减少而相应的自动释放空间，假如初始时你创建了一个矢量，并在其中存放了1000个元素，后来由于某种原因而删除了其中的大部分元素，最后只剩下10个元素，但此时矢量却仍然占有着初始创建矢量时可容纳1000个元素的空间，为了避免这种空间的浪费，下面将讨论如何将这些多余的空间释放掉，即**shrink-to-fit**.
## 使用swap成员函数
一个用来去除矢量中多余元素的办法是使用矢量容器的swap成员函数，下面先来看一个例子:     

	std::vector<int> v = {1,2,3,4,3,5,3};
	cout<<"v.capacity():"<<v.capacity()<<endl;      //初始时矢量的容量,输出7
	cout<<"v.size():"<<v.size()<<endl;              //初始时矢量中元素的个数,输出7
	v.erase(remove(v.begin(),v.end(),3),v.end());
	cout<<"after erase...\n";
	cout<<"v.capacity():"<<v.capacity()<<endl;   //仍然输出7
	cout<<"v.size():"<<v.size()<<endl;           //删除了部分元素，输出4
	cout<<"after shrink-to-fit...\n";
	std::vector<int>(v).swap(v);        //这里是关键点 
	cout<<"v.capacity():"<<v.capacity()<<endl;     //输出4,表明成功缩小了容量
	cout<<"v.size():"<<v.size()<<endl;			   //输出4
上面程序的关键点在于这一条语句:     

    std::vector<int>(v).swap(v);
该条语句中`std::vector<int>(v)`创建了矢量v的一个副本，这由矢量的复制构造函数来完成，而矢量的复制构造函数只为新创建的矢量分配所需要的内存，因此这个新创建的临时矢量(它是一个匿名矢量)并不包含矢量v中的多余容量，之后，这个新建的矢量调用swap成员函数，将自身与矢量v进行交换，交换完成之后，矢量v就具有了新创建的临时矢量的容量，而临时矢量就具有了原来矢量v所具有的多余容量，当语句`std::vector<int>(v).swap(v);`执行完毕后，临时矢量就被析构，从而释放了先前矢量v所具有的内存。这样就完成了去除vector中多余容量的任务.     
**补充:**这个小技巧对于string来说同样适用.     
*****
## 使用shrink_to_fit()成员函数(C++11)
shrink_to_fit()是C++11新增的特性，用来去除容器中多余的容量，使得Container::capacity()的大小与Container::size()的大小相同，下面是一个使用shrink_to_fit()成员函数来去除多余容量的例子:      

	std::vector<int> v = {1,2,3,4,3,5,3};
	cout<<"v.capacity():"<<v.capacity()<<endl;     //初始时矢量的容量，输出7
	cout<<"v.size():"<<v.size()<<endl;             //初始时矢量中元素的个数，输出7
	v.erase(remove(v.begin(),v.end(),3),v.end());
	cout<<"after erase...\n";
	cout<<"v.capacity():"<<v.capacity()<<endl;     //仍然输出7
	cout<<"v.size():"<<v.size()<<endl;             //删除了部分元素，输出4
	cout<<"after shrink-to-fit...\n";
	v.shrink_to_fit();                 //调用shrink_to_fit成员函数，使得v.capacity()等于v.size()
	cout<<"v.capacity():"<<v.capacity()<<endl;   //输出4
	cout<<"v.size():"<<v.size()<<endl;           //输出4
**注意:**shrink_to_fit只使用于vector、string和deque.    
