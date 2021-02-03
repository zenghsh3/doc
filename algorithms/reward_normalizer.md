# 奖励归一化

`Evokit`提供了不同的奖励归一化工具，用户可以参考下面配置说明来选择不同的奖励归一化器。

### reward_normalizer参数
- `type`：奖励归一化器类型，默认值为`NO_REWARD_NORMALIZER`，可选：
  1. `NO_REWARD_NORMALIZER`，表示不进行奖励归一化；
  2. `CENTERED_RANK_NORMALIZER`，对奖励按照大小进行排序，然后按照排名等分归一化到[-0.5, 0.5]；
  3. `CMA_DEFAULT_NORMALIZER`，CMA-ES算法使用的归一化方法， 取奖励在前`elite_ratio`的数据进行归一化和模型更新。
- `elite_ratio`：CMA-ES算法归一化中被选取用于计算下一代更新的比例，默认值为0.5。
