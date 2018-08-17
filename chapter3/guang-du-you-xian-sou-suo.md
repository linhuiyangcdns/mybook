# _**广度优先搜索算法（Breadth-First-Search,BFS）&lt;图算法&gt;**_

## **广度优先搜索的概念** {#广度优先搜索的概念}

广度优先搜索（BFS）类似于二叉树的层序遍历算法，它的基本思想是：首先访问起始顶点v，然后由v出发，依次访问v的各个未被访问过的邻接顶点w1,w2,w3….wn，然后再依次访问w1，w2,…,wi的所有未被访问过的邻接顶点，再从这些访问过的顶点出发，再访问它们所有未被访问过的邻接顶点….以此类推，直到途中所有的顶点都被访问过为止。类似的想法还将应用与Dijkstra单源最短路径算法和Prim最小生成树算法

# leetcode的题目

买卖股票的最佳时机

给定一个数组，它的第 _i_个元素是一支给定股票第_i_天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。

注意你不能在买入股票前卖出股票。

**示例 1:**

```
输入:
 [7,1,5,3,6,4]

输出:
 5

解释: 
在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
```

**示例 2:**

```
输入:
 [7,6,4,3,1]

输出:
 0

解释: 
在这种情况下, 没有交易完成, 所以最大利润为 0。
```

---

```
class Solution:
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        tmp_list = [prices[i+1]-prices[i] for i in range(len(prices)-1)]
        imax = 0
        tmp = 0
        for i in tmp_list:
            if i + tmp > 0:
                tmp  += i
            else:
                tmp = 0
            imax = max(imax,tmp)
        return imaxclass Solution:
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        tmp_list = [prices[i+1]-prices[i] for i in range(len(prices)-1)]
        imax = 0
        tmp = 0
        for i in tmp_list:
            if i + tmp > 0:
                tmp  += i
            else:
                tmp = 0
            imax = max(imax,tmp)
        return imax
```



