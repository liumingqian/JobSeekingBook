# \*搜索

深搜和宽搜

* 复杂度

  * bfs:2\*n
  * dfs:n

* 问题特点：
  * 求问题的所有解
* 题型
  * bfs:有最小，最近等特性，可以解决最短路，[最小步数](https://www.acwing.com/solution/LeetCode/content/425/)等问题
    * 矩阵型：[连通区域个数](https://leetcode.com/problems/number-of-islands/description/)，泛滥填充等
    * 图：[检测环](https://leetcode.com/problems/course-schedule/description/)
  * dfs:

    * 树形：求[最小深度](https://leetcode.com/problems/minimum-depth-of-binary-tree/description/)、[二叉树直径](https://leetcode.com/problems/diameter-of-binary-tree/description/)等
    * 字符串：[全排列](https://leetcode.com/problems/permutations/)\(回溯）、[去重全排列](https://www.cnblogs.com/zhizhizhiyuan/p/3821442.html)
* 注意：
  * dfs:递归层数太多有爆栈的风险
* 模板

```cpp
 void dfs()
    {
        if(expression1)//终止条件
            do something
        for(int i=0;i<N;i++)
        {
            if(expression2)//剪枝条件
                do something
            dfs();
        }
    }
```

