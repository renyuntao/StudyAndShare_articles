# STL:erase-remove组合    
2015-11-02   <br />      
&nbsp;&nbsp;&nbsp;在STL中，remove算法并不会真正的移除容器中的元素，它只是用未移除的元素来覆盖应该被移除的元素，之后返回一个新的逻辑上的终点。因此调用remove之后，容器中的元素数量并不会改变，下面的例子说明了这一点:   

    vector<in> v = {1,2,3,3,4,3,5};   //v中有7个元素
    cout<<v.size()<<endl;      //输出7
    auto newEnd = remove(v.begin(),v.end(),3);
    cout<<v.size()<<endl;       //仍然输出7
上面的vector容器v调用remove算法后，其中的元素会变成下面这个样子:     

>1 2 4 5 4 3 5

remove返回vector的新的逻辑终点，即newEnd指向上面元素中的第二个4的位置.     
要想真正删除容器中的元素，只能调用容器的成员函数，因为

>“只有容器的成员函数才有能力真正删除容器中的元素”

这个对于序列容器来说通常是erase，要想真正删除容器中的元素，就要用到所谓的**erase-remove**组合了，上面的例子可以修改为下面这个样子:     

    vector<in> v = {1,2,3,3,4,3,5};   //v中有7个元素
    cout<<v.size()<<endl;      //输出7
    v.erase(remove(v.begin(),v.end(),3),v.end());
    cout<<v.size()<<endl;       //相应元素被真正删除，此时输出4
&nbsp;&nbsp;&nbsp;remove算法返回新的逻辑终点，即最后一个不被删除的元素的下一个位置，因此上面的例子中，remove算法的返回值可以做为vector成员函数erase的要删除区间的左端点,而v.end()作为要删除区间的右端点，就恰好可以将想要移除的元素真正从vector中删除掉.      
**下面还有两点需要补充的内容:**      

- 对于list,它有一个结合了**erase-remove**组合的功能的成员函数 —— remove()成员函数，如果你使用的是list，当你想要从list中删除元素的时候，你应该使用这个成员函数，因为它比**erase-remove**组合更加高效.    
- STL中的remove\_if算法与remove算法的行为类似，当你使用remove_if算法，并且想要真正移除元素时，同样需要调用容器的erase成员函数.此外，STL中的unique算法的行为与remove算法的行为也是相似的，你可以使用类似的**unique-erase**组合，不过，对于list，list::unique会真正移除元素，当你操作的容器是list时，并且想要真正移除元素时，你应该使用list::unique,同样是因为它更加高效.    
