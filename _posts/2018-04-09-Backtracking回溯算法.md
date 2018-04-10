---    
layout: post    
date:   2018-04-09     
categories: Algorithm backtracking    
desc:    
---    
  


# BackTracking 回溯算法
回溯从本质上来说是一种结构化的穷举算法。
DFS是回溯应用于树和图上的一种具体实现。

以走迷宫为例，我们每次遇到岔路（假设所有岔路都是2分叉）都按照先左再右的顺序走，如果是死路就折回之前的岔路口，选择其他的路径，直到到达出口（如果有的话），这就是一种回溯穷举的思想，在这个过程中要做好走遍所有可能路线而找不到出口的情况。

但是相比起brute-force穷举，回溯主要有两个优点:
1.记忆历史节点，2.删选未来路径
记录历史走过的路径，因此每当遇到死路可就以回头走就近的分支，而不是从头走。
每当遇到分支，可以直接排除一些路径，这样就不会走很多弯路了。


## 解的条件 Goal
以树状的结构来考虑回溯思想（下图）：
假设A是迷宫入口，并且A,B,C都是岔路。
D,E,F,G是路的尽头，但是只有F是迷宫出口。
回溯算法:
首先尝试A-B-D，发现D是死路，折回就近分叉B，发现B的另一条支路没有走过，因此尝试A-B-E，发现E也是死路，折回B，已经知道B的所有支路都是死路，因此B也是死路上的节点，于是返回上层A，尝试A-C-F，得到最终解。

![Screen Shot 2018-04-09 at 20.53.15](http://o8e9d1ezz.bkt.clouddn.com/Screen Shot 2018-04-09 at 20.53.15.png)

这种算法很容易用递归来表示:

```
boolean findpath(treenode):
    if treenode == goal:
        return true;
    if treenode != null and findpath(treenode.left): return true;
    if treenode != null and findpath(treenode.right): return true;
    return false;
```

有没有解和是不是解
只有在到达叶子节点时才能够判断是真解还是假解。

## 剪枝 Children Elimination
上述的走迷宫方式还不够机智，尽管利用了历史信息，但是还必须有先见之明，要知道有些路是可以避免的。
假设如今走到了B需要抉择走D还是E，而E路口前放了一块路障，表示此路绝对不通，那么就根本没有必要走E这条路线了。

### 是不是解和有没有解

通常叶子节点是用来判断是不是解。比如传入递归中的treenode参数，首先就进行判断是否为goal。

剪枝就是预先知道走这条路线的话，最后是没有解还是可能有解。如果没有解，那么就不需要走下去了。如果可能有解，就需要走下去，在下个分叉判断是否剪枝，或者在叶子处判断是不是解。


## 问题分析 Solutions
如果一个问题非要通过穷举来解决，那么就可以考虑用回溯。
一般而言回溯问题的形式有以下几种:
1.寻找一个解:
求问有没有解: 返回True or False
求解的路径
比如

2.寻找所有解:
求解的总个数
求所有解的路径
比如[permutation](https://leetcode.com/problems/permutations/description/), [subsets](https://leetcode.com/problems/subsets/description/)

3.寻找最优解:
求最优解的数值大小
求最优解的路径

前面已经分析了，是不是解，只有在到达叶子节点时才能知道。
但剪枝是在分叉时需要判断的。
因此我们可以有一个大致的算法模式。

## 算法解析 General Algorithms
通用算法: 返回True or False

```
boolean findpath(Node n):
    if n == goal:
        return true;
        else: return false;
    else: 
        for each child c of n:
            if findpath(c) succeeds: return true
        return false
```
返回True or False的函数，大多用来求解是否存在解，1个即可。由于backtracking也是深度优先的，因此上述的算法只要有一条路走通就直接返回True了。而只要所有路都走不通才会返回False（无解）。

可以知道，如果叶子节点本身就能判断是否是解，那么根本不需要保存以前路径的状态。如果叶子节点需要以前的状态来判断是不是解，那么就需要额外传递参数（历史信息）给叶子节点。
比如所有深度超过x的叶子节点才算是解，那么叶子节点自身当然无法知道深度，除非传给他父节点的深度，当然也有x这个值。[permutation](https://leetcode.com/problems/permutations/description/)如果每次只取一个数，那么只有取到最后一个数的时候才算是解，如何判断是不是取到最后一个数，需要历史的信息。


如果要求解的路径，我们肯定需要一种记录路径的方式:
1.在每个节点上记录上一个节点的引用，如果得到解，则反向遍历输出路径，标准DFS就是用了这种方式。
2.使用一个全局List变量记录当前的路径，如果得到解，则直接输出。

两种方法都需要额外存储信息，1需要在每个节点的结构中加入上一个节点的引用，2需要在每次递归中额外传入一个全局List的引用。
当然如果需要保存所有的解，那么还需要传入一个全局列表引用，用来存储所有解。

通常使用方法2，因为只需要传递引用进去就可以了，全局只有一个List用来记录单个解，省空间。但是也有缺点，因为只有单个List，所以算法过程中这个List是动态变化的，如果要求所有解，每次得到解都需要复制一个List，因为继续回溯会改变旧的List
的路径值。


```
#method 1
void findpath(Node n):
    if n == goal:
        while( p = n.pre != null):
            print p;
    else:
        for each child c of n:
            c.pre = n;
            findpath(c, path);

#method 2
path = [];
void findpath(Node n, ArrayList path):
    if n == goal:
        path.add(n);
        print path;
    else:
        for each child c of n:
            findpath(c, path);

#method 2 for getting all solutions     
all = []; path=[];        
void findallpath(Node n, ArrayList path, ArrayList all):
    if n == goal:
        path.add(n);
        all.add(new ArrayList(path));
        print path;
    else:
        for each child c of n:
            findallpath(c, path);           
return all; 
```

同理求所有解的个数N，或者最优解O。我们可以定义全局变量，并将其传入递归，并只有在每次递归满足n == goal条件时，更新这些值。

往往backtracking算法都会需要传入一个参照表，一个全局数据的引用。
比如DFS的函数头void DFS-VISIT(G,u)，G就是整个图数据的引用（比如邻接表），u是当前节点。G的作用就是给剪枝提供了参照，u的邻接节点需要通过G查询。此外递归过程中修改的也是G中节点的数据。因此每当算法得到解，并且需要打印路径时，只需要遍历G就可以了。


## Referrences


[https://www.cis.upenn.edu/~matuszek/cit594-2012/Pages/backtracking.html](https://www.cis.upenn.edu/~matuszek/cit594-2012/Pages/backtracking.html)

[https://segmentfault.com/a/1190000006121957](https://segmentfault.com/a/1190000006121957)

[http://jcnavi.com/code/2016/12/27/backtracking/](http://jcnavi.com/code/2016/12/27/backtracking/)


