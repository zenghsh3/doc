# MovieLens示例

本示例主要是基于MovieLens数据集来模拟线上的交互环境，演示如何基于`EvoKit`只对部分模型参数进行更新。（在示例中user和item的embedding参数不会被更新）

MovieLens 是一个推荐系统和虚拟社区网站，于1997年建立。其主要功能为应用协同过滤技术和用户对电影的喜好，向用户推荐电影。

示例路径：./examples/movielens/

主要代码模块说明：
- `config.prototxt`：参数配置文件（算法、优化器、奖励归一化器等参数配置），如下面所示，配置声明了只对`fc_0.w_0`、`fc_0.b_0`、`fc_1.w_0`和`fc_1.b_0`四层参数进行更新。
  ```
  solver {
    type: BASIC_ES
    optimizer {
        type: MOMENTUM
        base_lr: 5.0
        base_reg: 0.0001
        momentum {
            momentum: 0.0
        }
    }
    sampling {
        type: GAUSSIAN_SAMPLING
        gaussian_sampling {
            std: 0.05
            cached: true
            seed: 1024
            cache_size : 10000000
        }
    }
    reward_normalizer {
        type: CENTERED_RANK_NORMALIZER
    }
  }
  weight_name: "fc_0.w_0"
  weight_name: "fc_0.b_0"
  weight_name: "fc_1.w_0"
  weight_name: "fc_1.b_0"
  ```
- `train.cc`：训练主要文件，包括演示了下面接口的使用：
  - 模拟线上采样
    ```C++
    auto sampling_agent = agent.clone(); // 克隆一个采样agent
    bool success = sampling_agent.add_noise(sampling_info); // 给采样agent的模型增加噪声，并把噪声key保存到SamplingInfo
    sampling_agent.predict(feature);  // 带噪声的采样agent进行推理，和环境进行交互
    ```
  - 模拟线上反馈，定义了`evaluate`函数来模拟线上的评估奖励，这里基于模型拟合样本的loss取反后作为奖励。
  - 模拟线下更新
    ```C++
    agent->update(sampling_infos, rewards) // 根据采样agent的噪声和奖励计算模型更新梯度。
    ```
  

## 如何运行
1. 新建仓库 && 拷贝代码
    - 在icode上新建仓库项目并下载到本地，例如： http://icode.baidu.com/repos/baidu/personal-code/yourdemo
    - 下载[evokit项目](https://console.cloud.baidu-int.com/devops/icode/repos/baidu/nlp/evokit/tree/stable)(stable分支)，将`./examples/movielens/`目录下的代码拷贝到你的仓库项目根目录下。

2. 编译demo
    - 进入你的仓库项目根目录，通过bcloud的云端集群编译即可，命令为：
    ```
    cd baidu/personal-code/yourdemo
    bcloud build
    ```

3. 运行demo
    - 编译完成后，我们需要增加动态库查找路径：
    ```
    export LD_LIBRARY_PATH=./output/so/:$LD_LIBRARY_PATH
    ```
    - 运行demo： 
    ```
    ./output/bin/app/train
    ```
    【注意: 运行时需确保当前的路径为项目根路径】

## 运行结果
运行200 epochs后test loss大概可以优化1.0-2.0左右。
