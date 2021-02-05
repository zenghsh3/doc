# GA 

GA (Genetic Algorithm) 遗传算法是模拟达尔文生物进化论的自然选择和遗传学机理的生物进化过程的计算模型，是一种通过模拟自然进化过程搜索最优解的方法。遗传算法以一种群体中的所有个体为对象，并利用随机化技术指导对一个被编码的参数空间进行高效搜索。其中，选择、交叉和变异构成了遗传算法的遗传操作；参数编码、初始群体的设定、适应度函数的设计、遗传操作设计、控制参数设定五个要素组成了遗传算法的核心内容。

## 如何使用

在`EvoKit`中使用GA算法，需要在配置文件的`solver`中声明`type`字段为`BASIC_GA`，`sampling`字段中的`type`设为`GA_SAMPLING`，在`basic_ga`字段中配置遗传算法相关设置，并且将`is_ga`字段设为`true`，如下面`config.prototxt`所示：

```
solver {
    type: BASIC_GA
    sampling {
        type: GA_SAMPLING
        ga_sampling {
            model_population {
                pop_size : 10
                init_std: 0.5
            }
        }
    }
    basic_ga {
        elite_num: 5
        crossover_type: CROSSOVER_ELEMENTWISE
        mutation_std: 0.5
    }
    optimizer {
        ...
    }
    reward_normalizer {
        ...
    }
}
is_ga: true
```

### ga_sampling参数
- `model_population`
  - `pop_size`：种群的数量，默认值为8；
  - `init_std`：初始化可能解时的标准差 默认值为0.05。

### basic_ga参数
- `elite_num`：每一代中被选择成为“父母”代的数量，假设每代共有`pop_size`个体，则拥有前`elite_num`名reward的解可以作为“父母”代，剩下的`pop_size` - `elite_num`个解将经过“父母”代间通过交叉和变异产生；
- `crossover_type`： 产生新的子代可能解的方式，有`CROSSOVER_ELEMENTWISE`和`CROSSOVER_LAYERWISE`两种方式。`CROSSOVER_ELEMENTWISE`: 以最小元素（每个模型参数）为粒度，效率较低，但随机性强；`CROSSOVER_LAYERWISE`：以layer（网络层）为粒度，子代随机从父母中继承整个layer的参数，效率较高，并且因为粒度较大，效果更加可控。默认值为`CROSSOVER_LAYERWISE`；
- `mutation_std`：每一代产生新的解之后变异率的标准差，默认值为0.05。

## 参考
[Genetic Algorithm - Wikipedia](https://en.wikipedia.org/wiki/Genetic_algorithm)

[Genetic Algorithm - tutorialspoint](https://www.tutorialspoint.com/genetic_algorithms/genetic_algorithms_introduction.htm)
