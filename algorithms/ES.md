# ES

Evolution Strategies (ES) 进化策略算法是一种无梯度随机优化算法。从抽象层面来看, ES所做的便是从某些随机参数开始, 不断重复: (1) 稍稍随机改动这些参数；(2)让参数向着能获得更好reward的改动方向移动.这两个步骤。

具体来讲,在每一次迭代,我们以一个参数向量$w$为基础,通过对$w$加上一些高斯噪声来产生一定数量的和$w$稍稍不同的参数向量:$w_1,w_2,.......w_n$. 随后我们便会根据reward function计算出这$n$个参数向量对应的reward值, 并且把这些reward加起来. 而新的$w_{new}$便是根据每个参数向量$w_i$乘以由$reward_i$占全部reward的比例加权平均而来. 随后再对$w_{new}$进行下一次迭代.

## 如何使用
在配置文件的solver中声明`type`字段为`BASIC_ES`，并且`sampling`字段中的`type`设为`GAUSSIAN_SAMPLING`，如下面`config.prototxt`所示：
```
solver {
    type: BASIC_ES
    sampling {
        type: GAUSSIAN_SAMPLING
        gaussian_sampling {
            std: 0.5
            cached: true
            seed: 1024
            cache_size: 100000
        }
    }
    optimizer {
        ...
    }
    reward_normalizer {
        ...
    }
}
```

### gaussian_sampling参数
- `std`: 高斯分布的标准差，默认值为1.0，通过调整`std`的大小可以调整每次迭代的权重搜索空间的大小。std越大，搜索空间越大。
- `cached`: 是否提前创建noise缓存，使用缓存可以提前建立noise表，提升采样效率，默认值为false。
- `seed`: 随机创建noise表使用的随机种子。
- `cache_size`: 缓存的大小，默认值为100000。

## 参考
论文：[Evolution Strategies as a Scalable Alternative to Reinforcement Learning](https://arxiv.org/abs/1703.03864)

博客：[Evolution Strategies as a Scalable Alternative to Reinforcement Learning](https://openai.com/blog/evolution-strategies/)
