# CMA-ES

CMA-ES (Covariance Matrix Adaptation Evolution Strategy) 协方差矩阵自适应进化算法是进化策略算法中比较流行的一个变种。
算法可以根据每一代的结果，适应性调整下一代的搜索空间，例如在距离最优解较远时，增大搜索空间，在距离最优解较近时，减小搜索空间。在每一代中，CMA-ES算法会提供一个多变量的正态分布的参数，用于下一代的采样。

## 如何使用
在EvoKit中使用CMA-ES算法，需要在配置文件的`solver`中声明`type`字段为`CMA_ES`，并且`sampling`字段中的`type`设为`CMA_SAMPLING`，如下面`config.prototxt`所示：
```
solver {
    type: CMA_ES
    sampling {
        type: CMA_SAMPLING
        cma_sampling {
            sigma: 1.0
            gaussian_sampling {
                std: 0.5
                cached: true
                seed: 1024
                cache_size : 100000
            }
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

### cma_sampling参数
- `sigma`：步长，默认值为1.0 
- `gaussian_sampling`：
  - `std`：高斯分布的标准差，默认值为1.0，通过调整`std`的大小可以调整采样噪声的数值大小；
  - `cached`：是否提前创建噪声缓存，使用缓存可以提前建立噪声表，提升采样效率，默认值为false；
  - `seed`：创建噪声表使用的随机种子；
  - `cache_size`：缓存的大小，默认值为100000。

## 参考
[CMA-ES - Wikipedia](https://en.wikipedia.org/wiki/CMA-ES)
[The CMA Evolution Strategy:A Tutorial](https://arxiv.org/abs/1604.00772?context=cs)
