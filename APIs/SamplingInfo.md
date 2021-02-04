# SamplingInfo
`SamplingInfo`主要用来保存可以还原采样模型噪声的key，以及模型迭代id。

在<a href="../examples/sync_online_example.md">同步更新示例</a>和<a href="../examples/async_online_example.md">异步更新示例</a>中演示了如何保存和加载`SamplingInfo`。


### SamplingInfo参数
- `key`：(int32) 可以还原采样模型噪声的key;
- `model_iter_id`：(int32) 模型迭代id（用在异步更新机制中，用来索引采样模型参数）。
