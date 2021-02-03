# 配置文件

`EvoKit`通过可读的`prototxt`配置文件来配置训练算法、优化器和更新机制等。

配置说明如下：

- `EvoKitConfig`
  - `solver`
    - type：更新算法，可选[<a href="../algorithms/ES.md">BASIC_ES</a>][<a href="../algorithms/CMA-ES.md">CMA_ES</a>][<a href="../algorithms/GA.md">BASIC_GA</a>]
    - sampling
    - optimizer
    - reward_normalizer
    - cma_es
    - basic_ga
  - `async_es`：进化计算异步更新机制配置，具体参考<a href="../algorithms/async_update.md">异步更新机制</a>
  - `is_ga`：声明是否使用遗传算法
