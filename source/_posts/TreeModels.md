---
title: 树模型
date: 2017-09-24
tags: [Tree Models,GBDT]
categories: Machine Learning
---

记录一下树模型的相关概念。
<!-- more -->

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

## Ensemble Methods
Consider a set of predictors f1, ..., fL, the idea is to construct a predictor F(x) that combines the individual decisions of f1, ..., fL.

Combining Predictor:
- Averaging
- Weighted Averaging
- Gating
- Stacking : The general formulation.
- Multi-Layer : Use neural networks as the ensemble model
- Tree Models (Question : A predictor contains only one layer ???)

Encourage to
- involve different types of predictors
- vary the training sets
- vary the feature sets

Cause of the Mistake | Diversification Strategy
---- | ---
Pattern was difficult | Try different models (????? what are pattern and models)
over-fitting |  Vary the training sets
Some features are noisy |  Vary the set of input features

## Bagging (Bootstrap Aggregating)
Bootstrap replication:Given n training samples Z, construct a new training set Z* by sampling n instances with replacement.

Steps:
- Create bootstrap replicates of training set
- Train a predictor for each replicate
- Validate the predictor using out-of-bootstrap data
- Average output of all predictors

Problem: the models trained from bootstrap samples are probably positively correlated

Bagging是减少variance，而boosting是减少bias。
- Bagging是再取样 (Bootstrap) 然后在每个样本上训练出来的模型取平均，所以是降低模型的 variance. Bagging 比如 Random Forest 这种先天并行的算法都有这个效果。
- Boosting则是迭代算法，每一次迭代都根据上一次迭代的预测结果对样本进行加权，所以随着迭代不断进行，误差会越来越小，所以模型的 bias 会不断降低。这种算法无法并行，例子比如 Adaptive Boosting.



## 随机森林（Random Forest）:
用随机的方式建立一个森林，森林里面有很多的决策树组成，随机森林的每一棵决策树之间是没有关联的。在得到森林之后，当有一个新的输入样本进入的时候，就让森林中的每一棵决策树分别进行一下判断，看看这个样本应该属于哪一类（对于分类算法），然后看看哪一类被选择最多，就预测这个样本为那一类。

Random forest is a substantial modification of bagging that builds a large collection of de-correlated trees, and then average them.

* 行采样：为每棵树选择样本
* 列采样：为每棵树选择features

To make a prediction at a new point x:
* Regression: prediction average
* Classification: majority voting

 Bagging vs. Random Forest vs. Boosting
* Bagging (bootstrap aggregating) simply treats each predictor trained on a bootstrap set with the same weight
* Random forest tries to de-correlate the bootstrap- trained predictors (decision trees) by sampling features
* Boosting strategically learns and combines the next predictor based on previous predictors

## AdaBoost

### Additive Models
加性模型中是利用一个平滑函数将Yi和Xi连接起来， nonparametric method.
* Additive Regression Models
* Additive Classification Models

### 过程
参考(https://zhuanlan.zhihu.com/p/27126737)

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

### XGBoost : The most effective and efficient toolkit for GBDT

## 扩展
可以通过查看路径来获得对决策树和随机森林的更加深入的理解。通过决策树计算路径中每一次分支时一个feature的contribution，可以得到当前页节点每一个节点的contribution。通过将许多决策树组成森林并为一个变量取所有树的平均贡献，这个确定特征的贡献的过程可以自然地扩展成随机森林。
参考:[机器之心：如何解读决策树和随机森林的内部工作机制？](http://mp.weixin.qq.com/s/DTDH2m21Gz1UQ2tW64kPZg)

To learn:
- Tianqi Chen 52cs
- Deep Forest
- DNN

参考：
- "Machine Learning" Tom M.Mitchell 曾华军 张银奎 等译
- [SJTU CS420: Machine Learning](http://wnzhang.net/teaching/cs420/index.html)
- [机器学习中的算法(1)-决策树模型组合之随机森林与GBDT](http://www.cnblogs.com/LeftNotEasy/archive/2011/03/07/random-forest-and-gbdt.html)
- [XGBoost 与 Boosted Tree](http://www.52cs.org/?p=429)
