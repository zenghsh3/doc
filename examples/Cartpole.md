# Cartpole示例

> CartPole又叫倒立摆。小车上放了一根杆，杆会因重力而倒下。为了不让杆倒下，我们要通过移动小车，来保持其是直立的。如下图所示。 在每一个时间步，模型的输入是一个4维的向量（小车位置、小车速度、杆子夹角及角变化率），表示当前小车和杆的状态，模型输出的信号用于控制小车往左或者右移动。当杆没有倒下的时候，每个时间步，环境会给1分的奖励；当杆倒下后，环境不会给任何的奖励，游戏结束。


本示例主要是演示如何基于`EvoKit`的ES算法来解决Cartpole控制问题，以及ESAgent相关接口的使用。

示例路径："./demo/cartpole/"

## 如何运行
1. 下载代码
    - 在icode上clone代码，仓库路径： http://icode.baidu.com/repos/baidu/nlp/evokit/tree/stable

2. 编译demo
    - 通过bcloud的云端集群编译即可，命令为：
    ```
    bcloud build
    ```

3. 运行demo
    - 编译完成后，我们需要增加动态库查找路径：
    ```
    export LD_LIBRARY_PATH=./output/so/:$LD_LIBRARY_PATH
    ```
    - 运行demo： 
    ```
    ./output/bin/cartpole/train
    ```
    【注意: 运行时需确保当前的路径为项目根路径】

## 运行结果
运行200 epochs后可以达到201最高分。
