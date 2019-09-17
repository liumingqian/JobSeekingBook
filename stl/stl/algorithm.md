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

//find_if:pred是一个判断iterator是否在范围内的函数
template<class InputIterator, class UnaryPredicate>
  InputIterator find_if (InputIterator first, InputIterator last, UnaryPredicate pred)
{
  while (first!=last) {
    if (pred(*first)) return first;
    ++first;
  }
  return last;
}
```



