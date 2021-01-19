# GA 

GA (Genetic Algorithm) 遗传算法是一种基于基因和自然选择理论的优化技巧,被用来寻找最优或近似最优解. 在遗传算法中,我们拥有针对特定问题**一定数量**(`population size`)的可能解(可能解最初使随机产生的).这些解随即会像自然界中的基因一样进行重组与变异, 产生新的子代,子代会被视为新的可能解,然后不断重复这一过程. 每一个个体(或可能解)都会根据目标函数而被赋予契合值(fitness value), 拥有更高契合值的个体会有更高的概率产生子代. 我们以这种方式来不断使个体发生进化,直到达到了停止标准.
进化算法具有随机性,但因为它利用到了过往解的信息,所以远远要比随机本地搜索要更优秀.


## 参考
[Genetic Algorithm - Wikipedia](https://en.wikipedia.org/wiki/Genetic_algorithm)
[Genetic Algorithm - tutorialspoint](https://www.tutorialspoint.com/genetic_algorithms/genetic_algorithms_introduction.htm)
