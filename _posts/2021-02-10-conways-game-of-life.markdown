---
layout: post
read_time: true
show_date: true
title:  康威生命游戏
date:   2021-02-10 13:32:20 -0600
description: 通过有趣的小项目再次挑战重拾编程，这次轮到康威生命游戏了。
img: posts/20210210/Game_of_Life.jpg
tags: [编程, Python]
author: 阿曼多·梅内斯（Armando Maynez）
github: amaynez/GameOfLife/
---
<p>我最近一直在尝试重新开始编程。自从我8岁时学会为一台坦迪彩色电脑（Tandy Color Computer）编程以来，编程就一直是我生活的一部分，那真是美好的旧时光。</p>

<img src="./assets/img/posts/20210210/300px-TRS-80_Color_Computer_3.jpg" alt="坦迪彩色电脑TRS80 III"/><small>坦迪彩色电脑TRS80 III</small>

<p>我已经用Java、C# 编程过，当然还有BASIC语言，我认为学习Python是个很棒的主意，因为我对数据科学和机器学习非常感兴趣，而这两个领域在Python程序员中似乎有一个活跃的社区。</p>

<p>作为我的入门快速编程任务之一，我决定编写康威生命游戏的代码，这是一个非常简单的细胞自动机，基本上可以自行运行。</p>

<p>这个游戏由一个大小为n的网格组成，在网格的每个方格中，根据以下规则，一个细胞要么是死的，要么是活的：</p>

<ul><li>如果一个细胞的邻居（即相邻的活细胞）少于2个，这个细胞会因孤独而死亡。</li><li>如果一个细胞的邻居多于3个，它会因过度拥挤而死亡。</li><li>如果一个空白方格恰好有3个相邻的活细胞，一个新的细胞将在那个位置诞生。</li><li>如果一个活细胞有2个或3个活邻居，它将继续存活。</li></ul>

<img src="./assets/img/posts/20210210/GameOfLife.gif" alt="康威生命游戏的规则"/><small>康威生命游戏的规则</small>

<p>为了增加一些挑战，我还决定实现一种记录游戏棋盘的“稀疏”方法。这意味着，我不会用典型的二维数组来表示整个棋盘，而是只记录那些存活的细胞。这样做可以节省大量的内存空间和处理时间，同时也为这个挑战增添了一些趣味性。</p>

<p>最棘手的部分是弄清楚如何计算哪些空白方格恰好有3个活邻居，以便在那里诞生一个新细胞。在记录整个网格的情况下，这很简单，因为我们只需遍历整个棋盘，找到网格中所有方格的活邻居即可。但在只保留活细胞的情况下，这确实是一个挑战。</p>

<p>最终，算法如下：</p>

<ol><li>遍历所有活细胞，并获取它们的所有邻居。</li></ol>

```python
def get_neighbors(self, cell):
    neighbors = []

    for x in range(-1, 2, 1):
        for y in range(-1, 2, 1):
            if not (x == 0 and y == 0):
                if (0 <= (cell[0] + x) <= self.size_x) and (0 <= (cell[1] + y) <= self.size_y):
                    neighbors.append((cell[0] + x, cell[1] + y))
    return neighbors
```

<ol start="2"><li>每当遇到一个特定的细胞时，将其所有相邻方格标记为各自增加一个邻居。这样，对于每个相邻的活细胞，特定方格的计数器就会增加，最终它将包含与该方格相邻的活细胞总数。</li></ol>

```python
def next_state(self):
    alive_neighbors = {}

    for cell in self.alive_cells:
        if cell not in alive_neighbors:
            alive_neighbors[cell] = 0

        neighbors = self.get_neighbors(cell)

        for neighbor in neighbors:
            if neighbor not in alive_neighbors:
                alive_neighbors[neighbor] = 1
            else:
                alive_neighbors[neighbor] += 1
```

<p>诀窍在于使用一个字典来记录那些有活邻居的方格，以及在当前状态下存活但没有活邻居（因此将会死亡）的细胞。</p>

<p>有了这个字典，每当遇到一个活细胞的邻居时，就可以很容易地添加细胞并增加它们的邻居计数器。</p>

<p>现在字典中已经填满了所有有活邻居的细胞以及它们各自拥有的邻居数量，接下来只需应用游戏规则即可：</p>


```python
for cell in alive_neighbors:
    if alive_neighbors[cell] < 2 or alive_neighbors[cell] > 3:
        self.alive_cells.discard(cell)
    
    elif alive_neighbors[cell] == 3:
        self.alive_cells.add(cell)
```

<p>请注意，由于我只保留了存活细胞的坐标数组，所以我只需要应用三条规则：因孤独而死亡、因过度拥挤而死亡以及因繁殖而诞生（恰好有3个活邻居），因为那些已经存活且有2个或3个邻居的细胞，在下次迭代中可以继续存活。</p>

<p>我发现以这种方式实现康威生命游戏非常有趣，这是一个相当令人耳目一新的挑战，而且我感觉自己的编程技能又在逐步提升了。</p> 