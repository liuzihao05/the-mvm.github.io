layout: post
read_time: true
show_date: true
title: 单神经元感知机
date: 2021-01-25 13:32:20 -0600
description: 能够快速学习对元素进行分类的单神经元感知机。
img: assets/img/posts/20210125/Perceptron.jpg
tags: [机器学习，编码，神经网络]
author: 阿曼多・梅内斯（Armando Maynez）
github: amaynez/Perceptron/
mathjax: yes
作为学习 Python 以及踏入机器学习领域的切入点，我决定从零开始编写这个领域的 “Hello World！”—— 单神经元感知机。
什么是感知机？
感知机是神经网络的基本组成单元，可以将它类比为一个神经元。它的概念正是如今广阔的人工智能领域得以发展的导火索。
早在 20 世纪 50 年代后期，年轻的弗兰克・罗森布拉特（Frank Rosenblatt）设计了一个非常简单的算法，作为构建能够学习执行不同任务的机器的基础。
从本质上讲，感知机只不过是一组值以及通过这些值传递信息的规则，但正是在这种简单性中蕴含着它的强大之处。
<center><img src='./assets/img/posts/20210125/Perceptron.png'></center>
想象一下，你有一个 “神经元”，为了 “激活” 它，你传入几个输入信号，每个信号都通过一个突触连接到神经元。一旦信号在感知机中聚合，然后就会传递到一个或多个已定义的输出端。感知机不过就是一个神经元及其一系列突触，用于接收信号并修改信号以便传递出去。
用更数学的术语来说，感知机是一组值（我们称之为权重），以及将这些值应用于输入信号的规则。
例如，一个感知机可能会接收三个不同的输入，就像图片中那样。假设它接收到的信号输入是：
且
，如果它的权重分别是 
且
，那么当接收到信号时，感知机所做的就是将每个输入值乘以其对应的权重，然后将它们相加。
<p style="text-align:center"><span data-type="inline-math" data-value="PGJyPgpcYmVnaW57YWxpZ259ClxiZWdpbntzcGxpdH0KXGxlZnQoeF8xICogd18xXHJpZ2h0KSArIFxsZWZ0KHhfMiAqIHdfMlxyaWdodCkgKyBcbGVmdCh4XzMgKiB3XzNccmlnaHQpClxlbmR7c3BsaXR9ClxlbmR7YWxpZ259Cg=="></span></p>
<p style="text-align:center"><span data-type="inline-math" data-value="PGJyPgpcYmVnaW57YWxpZ259PGJyPgpcYmVnaW57c3BsaXR9PGJyPgpcbGVmdCgwLjUgKiAxXHJpZ2h0KSArIFxsZWZ0KDEgKiAyXHJpZ2h0KSArIFxsZWZ0KC0xICogM1xyaWdodCkgPSAwLjUgKyAyIC0gMyA9IC0wLjUKXGVuZHtzcGxpdH08YnI+ClxlbmR7YWxpZ259PGJyPgo="></span></p>
通常，当得到这个值时，我们需要应用一个 “激活” 函数来平滑输出，但假设我们的激活函数是线性的，也就是说我们保持这个值不变，那么这就是感知机的输出，即 -0.5。
在实际应用中，输出是有意义的。也许我们希望我们的感知机对一组数据进行分类，如果感知机输出一个负数，那么我们就知道数据属于 A 类型，如果是正数，那么它属于 B 类型。
一旦我们理解了这一点，神奇的事情就会通过一个叫做反向传播的过程发生，在这个过程中，我们 “教育” 我们这个小小的单神经元大脑，让它学会如何完成自己的工作。
<tweet>神奇的事情就会通过一个叫做反向传播的过程发生，在这个过程中，我们 “教育” 我们这个小小的单神经元大脑，让它学会如何完成自己的工作。</tweet>
为此，我们需要一组已经分类好的数据，我们称之为训练集。这些数据包含输入以及它们相应的正确输出。所以我们可以在这个小大脑预测错误时告诉它，并且通过这样做，我们也会朝着我们知道感知机犯错的方向稍微调整一下权重，希望经过多次这样的迭代后，权重能够调整到使得大多数预测都是正确的程度。
在模型成功训练之后，我们可以让它对它从未见过的数据进行分类，并且我们有相当高的信心它会正确地进行分类。
感知机这种神奇特性背后的数学原理被称为梯度下降，它只是一些微分学知识，帮助我们将大脑所犯的错误转化为对权重值朝着最优方向的微小调整。3blue1brown 的这个视频系列对此进行了精彩的解释。
我的程序创建了一个单神经元神经网络，经过调整可以猜测一个点是在一条随机生成的直线上方还是下方，并且基于图表生成可视化效果，以便观察神经网络随着时间的推移是如何学习的。
这个神经元有三个输入和权重来计算它的输出：
输入 1 是点的 X 坐标，
输入 2 是点的 Y 坐标，
输入 3 是偏差，并且它始终为 1
输入 3 或偏差对于不经过原点（0,0）的直线是必需的
感知机开始时所有权重都设置为零，并且每次迭代使用 1000 个随机点来学习。
感知机的输出是通过以下激活函数计算的：
如果 x * weight_x + y * weight_y + weight_bias 为正数，则输出 1，否则输出 0
每个点的误差计算为感知机的预期输出减去实际输出，因此只有三种可能的误差值：
预期值	计算值	误差
1	-1	1
1	1	0
-1	-1	0
-1	1	-1
对于学习的每个点，如果误差不为 0，则根据以下公式调整权重：
新权重 = 旧权重 + 误差 * 输入 * 学习率
例如：新权重_x = 旧权重_x + 误差 * x * 学习率
在所有神经网络中，一个非常有用的参数是学习率，它基本上衡量的是我们对权重的调整幅度有多小。
在这种特定情况下，我编写的学习率会随着每次迭代按如下方式减小：
学习率 = 0.01 / (迭代次数 + 1)
这一点很重要，以确保一旦权重接近最优值，每次迭代中的调整随后会更加细微。
<center><img src='./assets/img/posts/20210125/Learning_1000_points_per_iteration.jpg'></center>
最后，感知机总是会收敛到一个解决方案，并非常精确地找到我们正在寻找的直线。
感知机在能够通过学习来求解方程方面是一个相当大的突破，然而它们也非常有限。由于其本质，它们只能求解线性方程，所以它们的问题空间相当狭窄。
如今的神经网络由许多感知机的组合构成，有许多层，并且还有其他类型的 “神经元”，比如卷积神经元、循环神经元等等，这显著地增加了它们能够解决的问题类型。