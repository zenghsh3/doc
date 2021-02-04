# AsyncESAgent

`AsyncESAgent`提供了`EvoKit`进行**异步**更新的主要接口，包括配置加载、模型加载、模型扰动、模型预测和模型更新等功能。

路径: `paddle/include/evo_kit/async_es_agent.h`

函数说明如下：

## load_config
- 功能

  加载和解析用户提供的配置文件。

- 参数
  -  `config_path`：(const std::string&) 配置文件的路径

- 返回值
  - `bool`：是否加载和解析配置成功。


## clone

- 功能

  克隆一个新的agent用于采样，会从原始agent中拷贝所有属性（包括模型参数），但`is_sampling_agnet`属性设为`true`（原始agent该属性为`false`）。
注意：
  - `clone`函数返回的agent不能调用`clone`/`update`/`load`/`load_inference_model`/`load_solver`/`save`/`save_inference_model`/`save_solver`函数；
  - 只有`clone`函数返回的agent能调用`add_noise`函数。

- 参数
  - 无

- 返回值
  - `std::shared_ptr`：返回克隆后的新agent（如果调用此函数的agent为clone出来的，返回`nullprt`）。

## update

- 功能

  更新模型参数，根据`SamplingInfo`还原采样噪声，然后根据噪声和`rewards`来计算模型的更新梯度。（注意：异步更新过程中会从`SamplingInfo`中获取模型id，根据当前模型和采样模型的参数偏差来修正噪声偏移，具体参考[<a href="../algorithms/async_update.md">异步更新机制</a>]）。

- 参数
  - `sampling_infos`：(std::vector<SamplingInfo>&) 采样模型的相关信息（噪声key、模型id），具体参考[<a href="SamplingInfo.md">SamplingInfo</a>]。
  - `rewards`：(std::vector<float>&) 采样模型和环境交互过程中的评估奖励。

- 返回值
  - `bool`：是否更新模型成功（如果调用次函数的agent是`clone`出来的，返回`false`）。

## add_noise

- 功能

  根据配置文件设置的`sampling`方法对模型进行随机噪声扰动，并保存还原噪声的相关信息，并且会记录当前模型id用于后面异步更新。（注意：只有`clone`函数返回的agent可以调用此函数。）


- 参数

  - `sampling_info`：(SamplingInfo&) 用于存储可以还原噪声的key和当前模型id。
  - `gen_key`： (bool) 是否需要随机生成噪声的key，如果为`true`的话，需要`sampling_info`已经存储有key，并基于这个key生成噪声；如果为`false`的话，则agent会随机生成产生噪声的key，并保存到`sampling_info`。

- 返回值

  - `bool`：是否成功对模型进行噪声扰动。
    - 如果调用此函数的agent为`clone`出来,返回`false`
    - 如果`gen_key`为false并且`sampling_info`中不包含key，返回`false`

## get_predictor

- 功能

  获取PaddleLite框架的predictor，可以后续用于模型推理。

- 参数

  - 无


返回值:

  - `std::shared_ptr<PaddlePredictor>`：可以进行模型推理（inference）的predictor指针。

## load

- 功能

  通过调用`load_inference_model`以及`load_solver`函数来从`model_dir`处加载模型（inference_model）以及算法（solver）配置。


- 参数

  - `model_dir`：(const std::string&) 用来加载inference_model和solver的路径


- 返回值
  - `bool`：是否加载成功。


## load_inference_model

- 功能

  从`model_dir`处加载模型（inference_model）


- 参数

  - `model_dir`：(const std::string&) 用于加载inference_model的路径

- 返回值
  - `bool`：是否加载成功。


## load_solver
- 功能

  从`model_dir`处加载算法（solver）配置。（注意：`load_solver`需要在调用`load_inference_model`函数后调用）。


- 参数

  - `model_dir`：(const std::string&) 用于加载solver的路径

- 返回值
  - `bool`：是否加载成功。

## save

- 功能

  通过调用`save_inference_model`以及`save_solver`函数把模型（inference_model）以及算法（solver）配置保存到`model_dir`。


- 参数

  - `model_dir`：(const std::string&) 用来保存inference_model和solver的路径


- 返回值
  - `bool`：是否保存成功。

## save_inference_model

- 功能

  把模型（inference_model）保存到`model_dir`。


- 参数

  - `model_dir`：(const std::string&) 用来保存inference_model的路径


- 返回值
  - `bool`：是否保存成功。

## save_solver

- 功能

  把算法（solver）配置保存到`model_dir`。


- 参数

  - `model_dir`：(const std::string&) 用来保存solver的路径


- 返回值
  - `bool`：是否保存成功。


## init_solver

- 功能

  根据加载的配置文件来初始化算法（solver），（注意：`init_solver`需要在调用`load_inference_model`函数后调用）。

- 参数

  - 无

- 返回值
  - `bool`：是否初始化solver成功。
