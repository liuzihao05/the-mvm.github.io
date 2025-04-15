---
layout: post
read_time: true
show_date: true
title:  神经网络优化方法与算法
date:   2021-03-12 13:32:20 -0600
description: 一些神经网络优化算法，主要用于在反向传播过程中实现动量项。
img: posts/20210312/nnet_optimization.jpg
tags: [编程, 机器学习, 优化, 深度神经网络]
author: 阿曼多·梅内斯（Armando Maynez）
github: amaynez/TicTacToe/blob/7bf83b3d5c10adccbeb11bf244fe0af8d9d7b036/entities/Neural_Network.py#L199
mathjax: yes # 留空或删除可防止加载mathjax JavaScript
toc: yes # 留空或删除则无目录
---
在我进行的一个看似不大的项目中，即[创建一个能够自行学习玩井字棋的机器学习神经网络](./deep-q-learning-tic-tac-toe.html)，我发现有必要在反向传播过程中至少实现一种动量算法来优化网络。

由于我关于井字棋项目的原始文章已经相当长了，所以我决定单独发布这些优化方法，以及我是如何在代码中实现它们的。

### 自适应矩估计（Adam）
[来源](https://ruder.io/optimizing-gradient-descent/index.html#adam)

自适应矩估计（Adaptive Moment Estimation，Adam）是一种优化方法，它为每个权重和偏置计算自适应学习率。除了存储过去梯度平方的指数衰减平均值 \(v_t\) 和过去梯度的指数衰减平均值 \(m_t\)（类似于动量项）之外。动量可以看作是一个沿着斜坡滚动的球，而Adam的行为就像一个带有摩擦力的重球，因此它更倾向于误差表面上的平坦极小值点。我们分别按如下方式计算过去梯度和过去梯度平方的衰减平均值 \(m_t\) 和 \(v_t\)：

<p style="text-align:center">\(<br>
\begin{align}<br>
\begin{split}<br>
m_t &amp;= \beta_1 m_{t-1} + (1 - \beta_1) g_t \\<br>
v_t &amp;= \beta_2 v_{t-1} + (1 - \beta_2) g_t^2<br>
\end{split}<br>
\end{align}<br>
\)</p>

\(m_t\) 和 \(v_t\) 分别是梯度的一阶矩（均值）和二阶矩（未中心化的方差）的估计值，这就是该方法名称的由来。由于 \(m_t\) 和 \(v_t\) 初始化为全零向量，Adam方法的作者观察到它们会偏向于零，尤其是在初始的时间步长内，并且当衰减率较小时（即 \(\beta_1\) 和 \(\beta_2\) 接近 1）这种情况更为明显。

他们通过计算偏差校正后的一阶和二阶矩估计值来抵消这些偏差：

<p style="text-align:center">\(<br>
\begin{align}<br>
\begin{split}<br>
\hat{m}_t &amp;= \dfrac{m_t}{1 - \beta^t_1} \\<br>
\hat{v}_t &amp;= \dfrac{v_t}{1 - \beta^t_2} \end{split}<br>
\end{align}<br>
\)</p>

然后我们使用这些值来更新权重和偏置，得到Adam更新规则：

<p style="text-align:center">\(\theta_{t+1} = \theta_{t} - \dfrac{\eta}{\sqrt{\hat{v}_t} + \epsilon} \hat{m}_t\)。</p>

作者建议 \(\beta_1\) 的默认值为 0.9，\(\beta_2\) 的默认值为 0.999，\(\epsilon\) 的默认值为 \(10^{-8}\)。

[在GitHub上查看](https://github.com/amaynez/TicTacToe/blob/b429e5637fe5f61e997f04c01422ad0342565640/entities/Neural_Network.py#L243)

```python
# 过去梯度的衰减平均值
self.v["dW" + str(i)] = ((c.BETA1
                        * self.v["dW" + str(i)])
                        + ((1 - c.BETA1)
                        * np.array(self.gradients[i])
                        ))
self.v["db" + str(i)] = ((c.BETA1
                        * self.v["db" + str(i)])
                        + ((1 - c.BETA1)
                        * np.array(self.bias_gradients[i])
                        ))

# 过去梯度平方的衰减平均值
self.s["dW" + str(i)] = ((c.BETA2
                        * self.s["dW"+str(i)])
                        + ((1 - c.BETA2)
                        * (np.square(np.array(self.gradients[i])))
                         ))
self.s["db" + str(i)] = ((c.BETA2
                        * self.s["db" + str(i)])
                        + ((1 - c.BETA2)
                        * (np.square(np.array(
                                         self.bias_gradients[i])))
                         ))

if c.ADAM_BIAS_Correction:
    # 偏差校正后的一阶和二阶矩估计值
    self.v["dW" + str(i)] = self.v["dW" + str(i)]
                          / (1 - (c.BETA1 ** true_epoch))
    self.v["db" + str(i)] = self.v["db" + str(i)]
                          / (1 - (c.BETA1 ** true_epoch))
    self.s["dW" + str(i)] = self.s["dW" + str(i)]
                          / (1 - (c.BETA2 ** true_epoch))
    self.s["db" + str(i)] = self.s["db" + str(i)]
                          / (1 - (c.BETA2 ** true_epoch))

# 应用于权重和偏置
weight_col -= ((eta * (self.v["dW" + str(i)]
                      / (np.sqrt(self.s["dW" + str(i)])
                      + c.EPSILON))))
self.bias[i] -= ((eta * (self.v["db" + str(i)]
                        / (np.sqrt(self.s["db" + str(i)])
                        + c.EPSILON))))
```

### 随机梯度下降法带动量项（SGD Momentum）
[来源](https://ruder.io/optimizing-gradient-descent/index.html#momentum)

传统的随机梯度下降法（Vanilla SGD）在穿越峡谷区域时会遇到困难。峡谷区域是指在某一维度上表面的曲率比另一维度上陡峭得多的区域，这种情况在局部最优解附近很常见。在这些情况下，随机梯度下降法会在峡谷的斜坡上来回振荡，而在朝着局部最优解的底部方向上只能缓慢地前进。

动量法是一种有助于在相关方向上加速随机梯度下降法并抑制振荡的方法。它通过将过去时间步长的更新向量的一部分 \(\gamma\) 添加到当前更新向量中来实现这一点：

<p style="text-align:center">\(<br>
\begin{align}<br>
\begin{split}<br>
v_t &amp;= \beta_1 v_{t-1} + \eta \nabla_\theta J( \theta) \\<br>
\theta &amp;= \theta - v_t<br>
\end{split}<br>
\end{align}<br>
\)</p>

动量项 \(\beta_1\) 通常设置为 0.9 或类似的值。

本质上，当使用动量法时，我们就像是将一个球推下山坡。球在向下滚动的过程中会积累动量，速度越来越快（直到它达到终端速度，如果存在空气阻力，即 \(\beta_1 < 1\)）。我们的权重和偏置更新也会发生同样的情况：对于梯度方向相同的维度，动量项会增加；对于梯度方向改变的维度，更新会减少。结果是，我们实现了更快的收敛速度并减少了振荡。

[在GitHub上查看](https://github.com/amaynez/TicTacToe/blob/b429e5637fe5f61e997f04c01422ad0342565640/entities/Neural_Network.py#L210)

```python
self.v["dW"+str(i)] = ((c.BETA1*self.v["dW" + str(i)])
                       +(eta*np.array(self.gradients[i])
                       ))
self.v["db"+str(i)] = ((c.BETA1*self.v["db" + str(i)])
                       +(eta*np.array(self.bias_gradients[i])
                       ))

weight_col -= self.v["dW" + str(i)]
self.bias[i] -= self.v["db" + str(i)]
```

### 涅斯捷罗夫加速梯度法（Nesterov accelerated gradient，NAG）
[来源](https://ruder.io/optimizing-gradient-descent/index.html#nesterovacceleratedgradient)

然而，一个盲目沿着斜坡向下滚动的球是非常不理想的。我们希望有一个更智能的球，一个能够知道自己前进方向的球，这样它就能在山坡再次向上倾斜之前减速。

涅斯捷罗夫加速梯度法（Nesterov accelerated gradient，NAG）是一种赋予我们的动量项这种前瞻性的方法。我们知道，我们将使用动量项 \(\beta_1 v_{t-1}\) 来更新权重和偏置 \(\theta\)。因此，计算 \( \theta - \beta_1 v_{t-1} \) 可以让我们近似得到权重和偏置的下一个位置（完整的更新还缺少梯度项），也就是对我们的权重和偏置将要到达的位置有一个大致的概念。现在，我们可以通过计算相对于权重和偏置的近似未来位置的梯度，而不是相对于当前权重和偏置 \(\theta\) 的梯度，来有效地提前预判：

<p style="text-align:center">\(<br>
\begin{align}<br>
\begin{split}<br>
v_t &amp;= \beta_1 v_{t-1} + \eta \nabla_\theta J( \theta - \beta_1 v_{t-1} ) \\<br>
\theta &amp;= \theta - v_t<br>
\end{split}<br>
\end{align}<br>
\)</p>

同样，我们将动量项 \(\beta_1\) 设置为大约 0.9 的值。动量法首先计算当前梯度，然后沿着更新后的累积梯度方向迈出一大步；而涅斯捷罗夫加速梯度法首先沿着先前累积梯度的方向迈出一大步，测量梯度，然后进行修正，这就形成了完整的涅斯捷罗夫加速梯度法更新。这种前瞻性的更新可以防止我们走得太快，并提高了响应速度，这在许多任务中显著提高了神经网络的性能。

现在，我们能够根据误差函数的斜率来调整更新，并依次加速随机梯度下降法，我们还希望根据每个权重和偏置的重要性，对它们进行更大或更小的更新。

[在GitHub上查看](https://github.com/amaynez/TicTacToe/blob/b429e5637fe5f61e997f04c01422ad0342565640/entities/Neural_Network.py#L219)

```python
v_prev = {"dW" + str(i): self.v["dW" + str(i)],
          "db" + str(i): self.v["db" + str(i)]}

self.v["dW" + str(i)] =
            (c.NAG_COEFF * self.v["dW" + str(i)]
           - eta * np.array(self.gradients[i]))
self.v["db" + str(i)] =
            (c.NAG_COEFF * self.v["db" + str(i)]
           - eta * np.array(self.bias_gradients[i]))

weight_col += ((-1 * c.BETA1 * v_prev["dW" + str(i)])
               + (1 + c.BETA1) * self.v["dW" + str(i)])
self.bias[i] += ((-1 * c.BETA1 * v_prev["db" + str(i)])
               + (1 + c.BETA1) * self.v["db" + str(i)])
```

### 均方根传播法（RMSprop）
[来源](https://ruder.io/optimizing-gradient-descent/index.html#rmsprop)

均方根传播法（RMSprop）是杰夫·辛顿（Geoff Hinton）在他的[Coursera课程的第6e讲](http://www.cs.toronto.edu/~tijmen/csc321/slides/lecture_slides_lec6.pdf)中提出的一种未发表的自适应学习率方法。

均方根传播法的提出是为了解决其他方法中学习率急剧下降的问题。

<p style="text-align:center">\(<br>
\begin{align}<br>
\begin{split}<br>
E[\theta^2]_t &amp;= \beta_1 E[\theta^2]_{t-1} + (1-\beta_1) \theta^2_t \\<br>
\theta_{t+1} &amp;= \theta_{t} - \dfrac{\eta}{\sqrt{E[\theta^2]_t + \epsilon}} \theta_{t}<br>
\end{split}<br>
\end{align}<br>
\)</p>

均方根传播法将学习率除以梯度平方的指数衰减平均值。辛顿建议将 \(\beta_1\) 设置为 0.9，而学习率 \(\eta\) 的一个不错的默认值是 0.001。

[在GitHub上查看](https://github.com/amaynez/TicTacToe/blob/b429e5637fe5f61e997f04c01422ad0342565640/entities/Neural_Network.py#L232)

```python
self.s["dW" + str(i)] = ((c.BETA1
                      * self.s["dW" + str(i)])
                      + ((1-c.BETA1)
                      * (np.square(np.array(self.gradients[i])))
                        ))
self.s["db" + str(i)] = ((c.BETA1
                      * self.s["db" + str(i)])
                      + ((1-c.BETA1)
                      * (np.square(np.array(self.bias_gradients[i])))
                        ))

weight_col -= (eta * (np.array(self.gradients[i])
              / (np.sqrt(self.s["dW"+str(i)]+c.EPSILON)))
              )
self.bias[i] -= (eta * (np.array(self.bias_gradients[i])
               / (np.sqrt(self.s["db"+str(i)]+c.EPSILON)))
                )
```

### 完整代码
总而言之，代码最终如下：
[在GitHub上查看](https://github.com/amaynez/TicTacToe/blob/b429e5637fe5f61e997f04c01422ad0342565640/entities/Neural_Network.py#L1)

```python
@staticmethod
def cyclic_learning_rate(learning_rate, epoch):
    max_lr = learning_rate * c.MAX_LR_FACTOR
    cycle = np.floor(1 + (epoch / (2
                    * c.LR_STEP_SIZE))
                    )
    x = np.abs((epoch / c.LR_STEP_SIZE)
        - (2 * cycle) + 1)
    return learning_rate
        + (max_lr - learning_rate)
        * np.maximum(0, (1 - x))

def apply_gradients(self, epoch):
    true_epoch = epoch - c.BATCH_SIZE
    eta = self.learning_rate
            * (1 / (1 + c.DECAY_RATE * true_epoch))

    if c.CLR_ON:
        eta = self.cyclic_learning_rate(eta, true_epoch)

    for i, weight_col in enumerate(self.weights):

        if c.OPTIMIZATION == 'vanilla':
            weight_col -= eta
                        * np.array(self.gradients[i])
                        / c.BATCH_SIZE
            self.bias[i] -= eta
                        * np.array(self.bias_gradients[i])
                        / c.BATCH_SIZE

        elif c.OPTIMIZATION == 'SGD_momentum':
            self.v["dW"+str(i)] = ((c.BETA1
                                   *self.v["dW" + str(i)])
                                   +(eta
                                   *np.array(self.gradients[i])
                                   ))
            self.v["db"+str(i)] = ((c.BETA1
                                   *self.v["db" + str(i)])
                                   +(eta
                                   *np.array(self.bias_gradients[i])
                                   ))

            weight_col -= self.v["dW" + str(i)]
            self.bias[i] -= self.v["db" + str(i)]

        elif c.OPTIMIZATION == 'NAG':
            v_prev = {"dW" + str(i): self.v["dW" + str(i)],
                      "db" + str(i): self.v["db" + str(i)]}

            self.v["dW