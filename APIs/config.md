# 配置文件

`EvoKit`通过可读的`prototxt`配置文件来配置训练算法、优化器和更新机制等。

主要配置的说明和相关链接如下：

- `EvoKitConfig`
  - `solver`
    - type：更新算法，可选[<a href="../algorithms/ES.md">BASIC_ES</a>][<a href="../algorithms/CMA-ES.md">CMA_ES</a>][<a href="../algorithms/GA.md">BASIC_GA</a>]
    - sampling：采样配置，和选择更新算法相关；
    - optimizer：优化器，可选[<a href="../algorithms/SGD.md">SGD</a>][<a href="../algorithms/Momentum.md">MOMENTUM</a>][<a href="../algorithms/Adam.md">ADAM</a>]
    - reward_normalizer：具体参考[<a href="../algorithms/reward_normalizer.md">奖励归一化器</a>]
    - cma_es：CMA-ES算法相关配置
    - basic_ga
  - `async_es`：进化计算异步更新机制配置，具体参考[<a href="../algorithms/async_update.md">异步更新机制</a>]
  - `is_ga`：声明是否使用遗传算法
