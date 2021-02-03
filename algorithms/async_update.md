# 异步更新机制
由于线上项目一般在产品线中，线上无法实时拿到用户日志，所以通常是通过保存用户日志（例如推荐系统的点击/时长），在线下根据用户数据更新模型，然后再推送到线上，完成模型的迭代更新。
但由于线上环境的复杂，日志生成和处理可能需要较长时间，如果线上采样和线下更新是串行执行的话，可能模型迭代频率较低，影响模型优化效率，所以`EvoKit`对`ES`和`CMA-ES`两种进化算法
支持了异步更新机制，如下图所示，线上采样和线下更新可以同时进行。

<p align="center">
<img src=".images/async_update.png" width=300/>
</p>

同时，在异步更新机制中，`EvoKit`会自动帮助用户进行模型仓库的管理，并且会在模型更新时，自动修正采样模型和更新模型不相同带来的噪声偏移问题，下图演示了`EvoKit`是
如何在异步更新的进化算法中计算模型梯度：

<p align="center">
<img src=".images/update_mechanism.png" width=500/>
</p>


## 如何使用
使用`EvoKit`进行异步更新时，需要使用`AsyncESAgent`类创建agent，主要包括下面三个步骤：
- 初始化solver：保存solver相关信息到初始模型文件夹中（注意只需要运行一次）；
- 在线采样：创建`AsyncESAgent`实例，加载指定模型和配置，在线上进行模型的噪声扰动（`add_noise`），然后和线上环境进行交互和评估，并保存日志；
- 离线更新：创建`AsyncESAgent`实例，加载指定模型和配置，根据日志文件还原模型噪声和对应评估奖励，更新模型参数，并产出下一轮迭代模型。


线上完整模拟示例可以参考：<a href="examples/async_online_example.md">异步更新示例</a>


### 配置文件
注意只有进化算法（`BASIC_ES`和`CMA_ES`）支持异步更新机制，因此需要在solver中的type需要声明为`BASIC_ES`或`CMA_ES`，另外`async_es`字段中可以配置异步更新相关参数，如下面`config.prototxt`所示：
```
solver {
    type: BASIC_ES
    sampling {
        ...
    }
    optimizer {
        ...
    }
    reward_normalizer {
        ...
    }
}

async_es {
    model_warehouse: "./model_warehouse"
    max_to_keep: 5
}
```

#### async_es参数
- `model_warehouse`：模型仓库保存地址，默认为"./model_warehouse"；
- `max_to_keep`：模型仓库最多保存模型数量，对于超过这个数量的过期模型将会被自动移除，并且过期模型采样的数据将不会用来更新。
