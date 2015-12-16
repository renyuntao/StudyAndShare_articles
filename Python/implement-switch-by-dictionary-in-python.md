# Python:通过字典来模拟Switch语句             
2015-12-15    <br />               
&nbsp;&nbsp;&nbsp;和C/C++不同，在Python中并没有`Switch`语句，因此Python通常使用`if...elif...else`语句来完成具有多个选择的问题，但是在我们可以通过使用字典来自己模拟一个Python中的`switch`语句，下面将分享一种在Python中模拟`switch`语句的方法.           
下面是用C++写的一个简单的含有`switch`语句的程序:           

    int a = 10, b = 5;
    int result = 0;
    int flag;
    cin>>flag;
    switch(flag):
    {
    	case 1:
    		result = a + b;
    		break;
    	case 2:
    		result = a - b;
    		break;
    	case 3:
    		result = a * b;
    		break;
    	case 4:
    		result = a / b;
    		break;
    }
    cout<<"result:"<<result<<endl;
在Python中，我们可以使用字典来模拟上面的`switch`语句，方法如下:           

    a,b = 10,5
    flag = input()
    result = {'1':a+b,'2':a-b,'3':a*b,'4':a/b}.get(flag,0)
    print('result:',result)
&nbsp;&nbsp;&nbsp;上面的程序中使用`get()`函数来获取字典中的值，这样当`flag`所表示的键不在字典中时，就会返回一个0值，而不是报错而导致程序终止，从而提到了程序的鲁棒性.           
