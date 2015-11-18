# 如何对数组进行复制操作? 
2015-11-18  <br />      
## C++ 风格的复制操作
### 使用STL中的copy算法
示例如下:        

	int a[] = {1,2,3,4,5};
	int b[5];
	std::copy(std::begin(a),std::end(a),std::begin(b));
	for(auto e:b) cout<<e<<" ";     // 输出 1,2,3,4,5
上述程序中，copy算法将数组a区间中的数复制到以begin(b)开始的区间中去.     

*****
### 使用array容器 (C++11)
示例如下:

	std::array<int,5> arr = {1,2,3,4,5};
	std::array<int,5> copy;
	copy = arr;      // 将arr中的元素复制到copy中
	arr[0] = 100;
	for(auto e:copy) cout<<e<<" ";      //输出 1,2,3,4,5

******
## C 风格的复制操作
### 使用memcpy()
示例如下:

	int arr[] = {1,2,3,4,5};
	int copy[5];
	int len = sizeof(arr) / sizeof(arr[0]);
	memcpy(copy,arr,len*sizeof(int));   // 输出 1,2,3,4,5
	for(auto e:copy) cout<<e<<" ";
**注意:** memcpy()函数的第三个参数表示的是要复制的字节数，而不是要复制的元素数目。至于这样做的原因，可以先来看
memcpy()的原型:      

>void* memcpy(void* destination,const void* source,size_t num);

由`memcpy()`的函数原型可以看到，该函数的前两个参数的类型是`void*`类型，这样做是为了使memcpy()可以作用于任何类型的
指针，但这样做又导致了一个问题，即`memcpy()`不知道传入数组的每个元素用多少字节来表示。也正是因为这个原因，使得
`memcpy()`的第三个参数不能是要复制的元素个数，而是要复制的字节数.    

-----------
### 使用memmove()
该函数与`memcpy()`类似，只是`memmove`允许目的位置和源位置重叠，示例如下:    

	int arr[] = {1,2,3,4,5,6,7,8};
	memmove(arr+3,arr+1,sizeof(int)*5);
	for(auto e:arr) cout<<e<<" ";       // 输出 1,2,3,2,3,4,5,6

-------
**注意:** 上面的程序中，如果将`memmove()`换作`memcpy()`可能也会正常工作，但是这种行为是不可预计的，当目的位置与源位置存在重叠时，应当使用`memmove()`.

------
