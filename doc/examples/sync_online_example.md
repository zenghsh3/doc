# 同步更新示例

本示例主要是基于Cartpole环境来模拟线上的交互环境，演示如何基于`EvoKit`的
<a href="../algorithms/sync_update.md">同步更新机制</a>，在线上环境进行部署和训练。

CartPole又叫倒立摆。小车上放了一根杆，杆会因重力而倒下。为了不让杆倒下，我们要通过移动小车，来保持其是直立的，如下图所示。 在每一个时间步，模型的输入是一个4维的向量
（小车位置、小车速度、杆子夹角及角变化率），表示当前小车和杆的状态，模型输出的信号用于控制小车往左或者右移动。当杆没有倒下的时候，每个时间步，环境会给1分的奖励；当
杆倒下后，环境不会给任何的奖励，游戏结束。

<p align="center">
<img src=".images/Cartpole.gif" width=500/>
</p>



示例路径：./demo/sync_online_example/

主要代码模块说明：
- `config.prototxt`：参数配置文件（算法、优化器、奖励归一化器等参数配置）；
- `init_solver.cc`：加载配置和模型文件，初始化solver并保存solver配置到模型文件夹中；（只需要执行一次）
- `online_sampling.cc`：模拟线上采样过程：1) 加载配置和模型文件； 2) 模型进行噪声扰动，和环境进行交互；3) 保存SamplingInfo和rewards到日志文件。
- `offline_update.cc`：模拟线下更新过程：1) 加载配置和模型文件；2) 读取日志文件；3) 更新模型。
- `train.sh`：进行多轮模型迭代的脚本。
  

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
    sh output/demo/sync_online_example/train.sh
    ```
    【注意: 运行时需确保当前的路径为项目根路径】

## 运行结果
运行200 epochs后可以达到201最高分。
