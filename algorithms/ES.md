# ES

Evolution Strategies (ES) 进化策略算法是一种无梯度随机优化算法。从抽象层面来看, ES所做的便是从某些随机参数开始, 不断重复: (1) 稍稍随机改动这些参数；(2)让参数向着能获得更好reward的改动方向移动.这两个步骤。

具体来讲,在每一次迭代,我们以一个参数向量$w$为基础,通过对$w$加上一些高斯噪声来产生一定数量的和$w$稍稍不同的参数向量:$w_1,w_2,.......w_n$. 随后我们便会根据reward function计算出这$n$个参数向量对应的reward值, 并且把这些reward加起来. 而新的$w_{new}$便是根据每个参数向量$w_i$乘以由$reward_i$占全部reward的比例加权平均而来. 随后再对$w_{new}$进行下一次迭代.


## 参考
Evolution Strategies as a Scalable Alternative to Reinforcement Learning [论文](https://arxiv.org/abs/1703.03864]) [博客](https://openai.com/blog/evolution-strategies/)
