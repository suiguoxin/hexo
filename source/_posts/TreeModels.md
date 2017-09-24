---
title: 树模型
tags: [Tree Models,GBDT]
---

## 决策树 (Decision tree):
一种逼近离散值目标函数的方法，核心任务是把样例分类到各可能的离散值对应的类别里，因此经常被称为分类问题。 例子：根据天气情况分类星期六上午是否打网球。

适用于以下特征的问题：

- ...etc
- 训练数据可以包含缺少属性值的实例。

### 决策树基本的学习算法
#### ID3:
自顶向下构造决策树。贪婪搜索(greedy search)：算法不断回溯重新考虑以前的选择。

属性选择：用熵来刻画样例集的纯度，熵越大，说明样例中信息量大。一个属性的信息增益就是由于适用这个属性分割样例而导致的期望熵降低。ID3算法计算每一个候选属性的信息增益，然后选择信息增益最高的一个。

#### C4.5
* Algorithm C4.5 is similar and more advanced to ID3
* Splitting the node according to information gain ratio other than gain

#### CART(Classification and Regression Tree) Algorithm

* Regression Tree : Output the predicted value

  Algorithm: Repeat until stop condition satisfied:
  * Find the optimal splitting to minimize the loss function
  * Calculate the prediction value of the new region R1, R2

* Classification Tree : Output the predicted class

  Algorithm: Repeat until stop condition satisfied:
  * Find the optimal splitting to minimize the Gini Impurity
  * Calculate the prediction value of the new region R1, R2

* Stop conditions :
  * Node instance number is small
  * Gini impurity is small
  * No more feature

### 决策树学习的常见问题
#### over-fitting
原因：
- 训练数据含有随机错误或噪声
- 少量的样例被关联到叶子节点, etc

避免过度拟合：
- 及早停止树增长：难以精确估计何时停止数增长
- 后修剪法(post-prune)：

#### missing
例：医学领域部分患者缺少验血结果

通常需要根据此属性已知的其他实例来估计这个缺少的属性：
- 最常见值
 - 所有样例中最常见值
 - 当前类别中最常见值
- 为该属性的所有可能值赋予一个概率

#### 其他问题

## 随机森林（Random Forest）:
用随机的方式建立一个森林，森林里面有很多的决策树组成，随机森林的每一棵决策树之间是没有关联的。在得到森林之后，当有一个新的输入样本进入的时候，就让森林中的每一棵决策树分别进行一下判断，看看这个样本应该属于哪一类（对于分类算法），然后看看哪一类被选择最多，就预测这个样本为那一类。

* 行采样：为每棵树选择样本
* 列采样：为每棵树选择features

## GBDT
### 原始的Boost算法：
步骤：
* 每一个样本有一个权重值
* 每一步结束后，增加分错的点的权重，减少分对的点的权重
* N次迭代（由用户指定），将会得到N个简单的分类器（basic learner），然后我们将它们组合起来（比如说可以对它们进行加权、或者让它们进行投票等），得到一个最终的模型。


### Gradient Boost：每一次的计算是为了减少上一次的残差
典型步骤：
* 得到梯度：用当前模型得到估计结果，经过Logistic变换得到损失函数，求导得到梯度。梯度越接近0表示越准确
* 减小梯度：
问题：如何根据当前每一个样本的梯度的情况，建立一棵决策树？

陈天奇
Deep Forest
Ensemble


参考：
- "Machine Learning" Tom M.Mitchell 曾华军 张银奎 等译
- [SJTU CS420: Machine Learning](http://wnzhang.net/teaching/cs420/index.html)
- [机器学习中的算法(1)-决策树模型组合之随机森林与GBDT](http://www.cnblogs.com/LeftNotEasy/archive/2011/03/07/random-forest-and-gbdt.html)
