# ES

Evolution Strategies (ES) 进化策略算法是一种无梯度随机优化算法。ES算法主要通过对参数进行随机扰动，并根据扰动后模型的评估表现来更新迭代参数。其算法流程如下图：

<img src=".images/ES_Algorithm.png" width="800"/>

优点：
- 不需要梯度回传，黑盒优化；
- 可以较好解决稀疏奖励或奖励分配问题；
- 易于并行。

## 如何使用
EvoKit实现了OpenAI论文中提到的ES算法，并支持线上进行参数扰动和预测，线下可以通过key还原噪声和进行参数更新。

在EvoKit中使用ES算法，需要在配置文件的`solver`中声明`type`字段为`BASIC_ES`，并且`sampling`字段中的`type`设为`GAUSSIAN_SAMPLING`，如下面`config.prototxt`所示：
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
- `std`： 高斯分布的标准差，默认值为1.0，通过调整`std`的大小可以调整采样噪声的数值大小；
- `cached`：是否提前创建噪声缓存，使用缓存可以提前建立噪声表，提升采样效率，默认值为false；
- `seed`：创建噪声表使用的随机种子；
- `cache_size`：缓存的大小，默认值为100000。

## 参考
论文：[Evolution Strategies as a Scalable Alternative to Reinforcement Learning](https://arxiv.org/abs/1703.03864)

博客：[Evolution Strategies as a Scalable Alternative to Reinforcement Learning](https://openai.com/blog/evolution-strategies/)
