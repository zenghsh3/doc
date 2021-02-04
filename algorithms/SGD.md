# SGD

`EvoKit`目前暂未提供SGD优化器，但用户可以使用<a href="Momentum.md">Momentum</a>优化器，并将动量项设置为0，从而等价于SGD优化器，如下面`config.prototxt`所示：

```
solver {
    type: BASIC_ES
    sampling {
        ...
    }
    optimizer {
        type: MOMENTUM
        base_lr: 0.5
        momentum {
            momentum: 0.0
        }
    }
    reward_normalizer {
        ...
    }
}
```
