# algorithm

```cpp
//Search:从test序列中找t2序列
std::search(test.begin(),test.end(),t2.begin(),t2.end());  

//std::search_n:找特定元素，返回首个位置或end 
//如：寻找首次连续出现2次大于7的位置 
it=search_n(test.begin(),test.end(),2,7,[](int i,int j){return i>j;}); 

//找容器中最小/最大元素min_element/max_element
max_element(first,end,cmp);

//对容器内容进行翻转
void reverse (BidirectionalIterator first, BidirectionalIterator last);

//遍历容器
for_each(iv.begin(), iv.end(), [=](int &x)->int{return x * (a + b);});
```



