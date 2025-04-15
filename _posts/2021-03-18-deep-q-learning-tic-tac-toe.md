---
layout: post
read_time: true
show_date: true
title:  井字棋的深度Q学习
date:   2021-03-18 15:14:20 -0600
description: "受深度思维（Deep Mind）的惊人成就启发，他们的Alpha Go、Alpha Zero和Alpha Star程序学会了（并且表现出色）围棋、国际象棋、雅达利游戏，以及最近的《星际争霸》；我给自己设定了一个任务，编写一个神经网络，让它能够自己学会玩古老的井字棋游戏。这能有多难呢？"
img: posts/20210318/TicTacToeSml.jpg
tags: [机器学习, 人工智能, 强化学习, 编程, Python]
author: 阿曼多·梅内斯（Armando Maynez）
github: amaynez/TicTacToe/
toc: yes # 留空或删除则无目录
---
<center><img style="float: left;margin-right: 1em;" src='./assets/img/posts/20210318/Game_Screen.png' width="310" height="300"></center>

## 背景
在从事了17年与计算机科学相背离的企业职业生涯后，我现在决定学习机器学习，并在此过程中重新开始编程（这是我一直热爱的事情！）。

为了全面掌握机器学习的本质，我决定从[自己编写一个机器学习库开始](./ML-Library-from-scratch.html)，这样我就能完全理解随机梯度下降中涉及的内部原理、线性代数和微积分知识。并且顺便学习Python（20年前我曾用C++ 编程）。

我构建了一个通用的基础机器学习库，它可以创建一个神经网络（只有全连接层），将权重保存到文件中以及从文件加载权重，使用随机梯度下降法进行前向传播和训练（优化权重和偏置）。我用异或（XOR）问题测试了这个机器学习库，以确保它能正常工作。你可以[在这里](./ML-Library-from-scratch.html)阅读关于它的博客文章。

接下来的挑战，我对强化学习非常感兴趣，灵感来自深度思维的惊人成就，他们的Alpha Go、Alpha Zero和Alpha Star程序学会了（并且表现出色）围棋、国际象棋、雅达利游戏，以及最近的《星际争霸》；我给自己设定了一个任务，编写一个神经网络，让它能够自己学会玩古老的井字棋游戏（也叫noughts and crosses）。

这能有多难呢？

当然，首先要做的是编写游戏本身的代码，所以我选择了Python，因为我正在学习它，这给了我一个很好的练习机会，并且使用PyGame来制作界面。
编写游戏代码相当直接明了，尽管这是我第一个PyGame程序，也几乎是我第一个Python程序，遇到了一些小麻烦。
我非常开放地创建了这个游戏，使得它可以由两个人类玩家对玩，或者一个人类玩家与一个算法人工智能对玩，以及一个人类玩家与神经网络对玩。当然，神经网络也可以与三种人工智能引擎中的一种对玩：随机的、[极小化极大算法](https://en.wikipedia.org/wiki/Minimax)的，或者硬编码的（这是我很久以前就想做的一个练习）。

在训练时，可以禁用游戏的可视化界面，以使训练速度更快。
现在，有趣的部分来了，训练网络，我遵循了深度思维自己的深度Q网络（DQN）建议：

<ul><li>这个网络将是Q值函数或贝尔曼方程的近似，这意味着网络将被训练来预测在给定游戏状态下每个可用动作的“价值”。</li><li>实现了一个经验回放记忆。这意味着神经网络不会在每一步动作之后都进行训练。每一步动作都会与棋盘状态以及采取该动作（步骤）所获得的奖励一起记录在一个特殊的“记忆”中。</li><li>在记忆足够大之后，从经验回放记忆中随机采样的一批经验将用于每一轮训练。</li><li>使用一个辅助神经网络（与主网络相同）来计算Q值函数（贝尔曼方程）的一部分，特别是未来的Q值。然后，每进行<em>n</em>场游戏后，它会用主网络的权重进行更新。这样做是为了避免我们追逐一个不断变化的目标。</li></ul>


## 设计神经网络

<center><img src='./assets/img/posts/20210318/Neural_Network_Topology.png' width="540"></center><br>

所选择的神经网络接受9个输入（游戏的当前状态），并为游戏棋盘上的9个方格（可能的动作）中的每一个输出9个Q值。显然，有些方格是非法走法，因此在训练过程中，对于非法走法会给予负奖励，希望模型能够学会在给定位置不进行非法走法。

我从两个各有36个神经元的隐藏层开始，所有神经元都完全连接，并通过ReLU函数激活。输出层最初使用sigmoid函数激活，以确保我们得到一个介于0和1之间的良好值，该值表示给定状态-动作对的Q值。

## 众多模型...
### 模型1 - 首次尝试

起初，模型是通过与一个“完美”的人工智能对玩来训练的，这意味着一个[硬编码算法](https://github.com/amaynez/TicTacToe/blob/b429e5637fe5f61e997f04c01422ad0342565640/entities/Game.py#L43)，它永远不会输，并且如果有机会就会赢。在进行了数千轮训练后，我注意到神经网络学得并不多；所以我转而与一个完全随机的玩家对玩来进行训练，这样它也能学会如何获胜。在与随机玩家对玩训练后，神经网络似乎取得了一些进展，并且随着时间的推移，损失函数在稳步减小。

<center><img src='./assets/img/posts/20210318/Loss_function_across_all_episodes.png' width="540"></center><br>

然而，该模型仍然产生了许多非法走法，所以我决定修改强化学习算法，对非法走法进行更严厉的惩罚。修改内容包括在训练网络的目标值中，将给定位置的所有相应非法走法填充为零。这对于减少非法走法似乎效果非常好：

<center><img src='./assets/img/posts/20210318/Loss_function_and_Illegal_moves.png' width="540"></center><br>

尽管如此，该模型的表现仍然相当差，与完全随机的玩家对玩时，胜率仅约为50%（我期望它能在超过90%的时间里获胜）。这是在仅训练了10万场游戏之后的结果，所以我决定继续训练并观察结果：

<center><img src='./assets/img/posts/20210318/Loss_function_and_Illegal_moves2.png' width="540">
<small>胜率：65.46% 负率：30.32% 平局率：4.23%</small></center>

请注意，当重新开始训练时，在训练轮次的开始阶段，损失和非法走法仍然较高，这是由ε-贪婪策略导致的，该策略在探索（完全随机的走法）和利用之间更倾向于探索，这种偏好会随着时间的推移而减弱。

在又进行了10万场游戏的训练后，我可以看到损失函数实际上开始减小了，胜率最终达到了65%，所以我抱着一丝希望决定再进行一轮10万场游戏的训练（在i7的MacBook Pro上大约需要2小时）：

<center><img src='./assets/img/posts/20210318/Loss_function_and_Illegal_moves3.png' width="540">
<small>胜率：46.40% 负率：41.33% 平局率：12.27%</small></center>

正如你在图表中看到的，计算出的损失甚至没有趋于平稳，而是随着时间似乎有所增加，这告诉我模型不再学习了。这一点也被胜率所证实，与上一轮相比，胜率下降到了可怜的46.4%，看起来并不比随机玩家好多少。

### 模型2 - 输出层使用线性激活函数

在没有得到我想要的结果后，我决定将输出激活函数改为线性函数，因为输出应该是一个Q值，而不是一个动作的概率。

<center><img src='./assets/img/posts/20210318/Loss_function_and_Illegal_moves4.png' width="540"><br>
<small>胜率：47.60% 负率：39% 平局率：13.4%</small></center><br>

起初，我只用1000场游戏进行测试，看看新的激活函数是否有效，损失函数似乎在减小，然而它在大约1的值附近达到了平稳状态，因此仍然没有像预期的那样学习。我遇到了[布拉德·肯斯特勒（Brad Kenstler）、卡尔·托梅（Carl Thome）和杰里米·乔丹（Jeremy Jordan）提出的一种技术](https://github.com/bckenstler/CLR)，叫做循环学习率（Cyclical Learning Rate），它似乎可以解决这种类型网络中一些损失函数停滞的情况。所以我使用他们的三角形1模型进行了尝试。

使用了循环学习率后，在快速进行了1000场游戏的训练轮次后仍然没有成功；所以我决定在此基础上实现一个衰减学习率，按照以下公式：

<center><img src='./assets/img/posts/20210318/lr_formula.jpeg' width="280"></center>

结合每轮循环和衰减的最终学习率如下：
<center><img src='./assets/img/posts/20210318/LR_cycle_decay.png' width="480">
<small>学习率 = 0.1，衰减率 = 0.0001，循环周期 = 2048轮，<br>
        最大学习率因子 = 10倍</small></center>

```python
true_epoch = epoch - c.BATCH_SIZE
learning_rate = self.learning_rate*(1/(1+c.DECAY_RATE*true_epoch))
if c.CLR_ON: learning_rate = self.cyclic_learning_rate(learning_rate,true_epoch)
```
```python
@staticmethod
def cyclic_learning_rate(learning_rate, epoch):
    max_lr = learning_rate*c.MAX_LR_FACTOR
    cycle = np.floor(1+(epoch/(2*c.LR_STEP_SIZE)))
    x = np.abs((epoch/c.LR_STEP_SIZE)-(2*cycle)+1)
    return learning_rate+(max_lr-learning_rate)*np.maximum(0,(1-x))
```
```python
c.DECAY_RATE = 学习率衰减率
c.MAX_LR_FACTOR = 确定最大学习率的乘数
c.LR_STEP_SIZE = 每个循环持续的轮数
```
<br>进行了这么多更改后，我决定用一组新的随机权重和偏置重新开始，并尝试训练更多（更多得多）的游戏。

<center><img src='./assets/img/posts/20210318/Loss_function_and_Illegal_moves6.png' width="540">
<small>100万轮游戏，750万轮训练周期，每批64步走法<br>
胜率：52.66% 负率：36.02% 平局率：11.32%</small></center>

在经过**24小时**后，我的电脑能够运行100万轮游戏（进行的游戏场次），这代表着750万轮每批64步走法的训练周期（学习了4.8亿步走法），学习率确实下降了（一点点），但显然仍然处于平稳状态；有趣的是，损失函数图的下限似乎继续下降，而上限和移动平均值保持不变。这让我相信我可能陷入了局部最小值。
<a name='Model3'></a>
### 模型3 - 新的网络拓扑结构

在经历了所有失败后，我认为我必须重新思考网络的拓扑结构，并尝试不同网络和学习率的组合。

<center><img src='./assets/img/posts/20210318/Loss_function_and_Illegal_moves7.png' width="540">
<small>10万轮游戏，63.5万轮训练周期，每批64步走法<br>
<b>胜率：76.83%</b> 负率：17.35% 平局率：5.82%</small></center>

我将每个隐藏层的神经元数量增加到了200个。尽管有了很大的改进，但损失函数仍然在大约0.1（均方误差）处趋于平稳。尽管与之前相比已经大大降低了，但与随机玩家对玩时，胜率仍然只有77%，这个网络玩井字棋就像个小孩子一样！

<center><img src='./assets/img/posts/20210318/Game_Screen2.png' width="240" height="240">
<small>*大多数时候我仍然能打败这个网络！（我用红色的X下棋）*</small></center>

<center><img src='./assets/img/posts/20210318/Loss_function_and_Illegal_moves10.png' width="540">
<small>又进行了10万轮游戏，62万轮训练周期，每批64步走法<br>
<b>胜率：82.25%</b> 负率：13.28% 平局率：4.46%</small></center>

**最终我们突破了80%的胜率大关！** 这是相当大的成就，似乎网络拓扑结构的改变是有效的，尽管损失函数看起来也在大约0.15处停滞了。

在进行了更多轮训练以及对学习率和其他参数进行了一些实验后，我无法将胜率提高到超过82.25%。

到目前为止的结果如下：

<center><img src='./assets/img/posts/20210318/Models1to3.png' width="540"></center>
<br>

了解神经网络模型的许多参数（大多数作者称之为超参数）如何影响其训练性能是非常有趣的，我尝试了：
- 学习率
- 网络拓扑结构和激活函数
- 循环和衰减学习率参数
- 批量大小
- 目标更新周期（当目标网络用策略网络的权重进行更新时）
- 奖励策略
- ε-贪婪策略
- 是与随机玩家还是“智能”人工智能进行训练。

到目前为止，最有效的改变是网络拓扑结构，但离我与随机玩家对玩达到90%胜率的目标还差一点，我仍然会尝试进一步优化。

<tweet>网络拓扑结构似乎对神经网络的学习能力影响最大。</tweet>

<a name='Model4'></a>
### 模型4 - 实现动量项

我[向Reddit社区求助](https://www.reddit.com/r/MachineLearning/comments/lzvrwp/p_help_with_a_reinforcement_learning_project/)，一位好心人指出，也许我需要在优化算法中应用动量项。所以我做了一些研究，最终决定实现各种优化方法来进行实验：

- 带动量项的随机梯度下降法
- RMSProp：均方根普通动量法
- NAG：涅斯捷罗夫加速动量法
- Adam：自适应矩估计法
- 并且保留我原来的传统梯度下降法（vGD）☺

<a name='optimization'></a>[点击这里查看所有实现的优化算法的详细解释和代码。](https://the-mvm.github.io/neural-network-optimization-methods/)

到目前为止，我还没有能够用模型4得到更好的结果，我尝试了所有的动量优化算法，但几乎没有成功。
<a name='Model5'></a>
### 模型5 - 实现独热编码并再次更改拓扑结构
我在Github上看到了一个[有趣的项目](https://github.com/AxiomaticUncertainty/Deep-Q-Learning-for-Tic-Tac-Toe/blob/master/tic_tac_toe.py)，它正好涉及深度Q学习，我注意到他对输入使用了“独热”编码，而不是直接将玩家的值输入到9个输入槽中。所以我决定试一试，同时更改我的拓扑结构以匹配他的：

<center><img src='./assets/img/posts/20210318/Neural_Network_Topology3.png' width="540"></center>

所以，“独热”编码基本上是将井字棋棋盘上单个方格的输入更改为三个数字，这样每个状态都用不同的输入表示，因此网络可以清楚地区分这三种状态。正如原作者所说，我之前的编码方式，用0表示空，1表示X，2表示O，网络不容易分辨出，例如，O和X都表示已占据的状态，因为一个与0的距离是另一个的两倍。使用新的编码，空状态将是3个输入：(1,0,0)，X将是(0,1,0)，O是(0,0,1)，如图所示。

即便如此，模型5仍然没有成功，所以我开始认为我的代码可能有一个错误。

为了验证这个假设，我决定使用TensorFlow / Keras实现相同的模型。

<a name='Model6'></a>
### 模型6 - TensorFlow / Keras
<center><img src='https://www.kubeflow.org/docs/images/logos/TensorFlow.png' width="100" height="100"></center>
