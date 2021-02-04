# ESAgent

`ESAgent`提供了`EvoKit`进行同步更新的主要接口，包括模型和配置加载、模型扰动、模型预测和模型更新等功能。
路径: `paddle/include/evo_kit/es_agent.h`
函数说明如下：

## load_config
- 功能

加载和解析用户提供的配置文件 （注意：load_config需要在调用`load_inference_model`函数后调用）。

- 参数
  -  `config_path`：(const std::string&) 配置文件的路径

- 返回值
  - `bool`：是否加载和解析配置成功。


## clone

- 功能
克隆一个新的agent用于采样，会从原始agent中拷贝所有属性（包括模型参数），但`is_sampling_agnet`属性设为`true`（原始agent该属性为`false`）。
注意：
  - `clone`函数返回的agent不能再调用`clone`函数
  - 只有`clone`函数返回的agent能调用`add_noise`函数


- 参数
  - 无

- 返回值
  - `std::shared_ptr`：返回克隆后的新agent（如果调用此函数的agent为clone出来的，返回`nullprt`）。

#### update

参数:

* `std::vector<SamplingInfo>& sampling_infos`
* `std::vector<float>& rewards`

功能:

* 通过调用`_config`中对应的solver类型的`update`函数来实现更新效果
  * 如果`_config->is_ga()`为`true`,则会调用`_ga_solver->update`
  * 否则会调用`_es_solver->sync_update`

返回值:`bool`

* 如果调用此函数的agent为clone出来,返回`false`
* 如果`_config`中对应的`solver->update`的返回值为false,返回`false`
* 不是以上情况,返回`true`

#### add_noise

参数:

* `SamplingInfo& sampling_info`
* `bool gen_key`

功能:     

* 通过调用`config`中对应的solver类型的`sampling`函数来实现添加噪声效果

返回值:

* 如果调用此函数的agent为clone出来,返回`false`
* 如果`gen_key`为false并且`sampling_info`中不包含key,返回`false`
* 如果`_config`中对应的`solver->sampling`的返回值为false,返回`false`
* 不是以上情况,返回`true`

#### get_predictor​​

参数:

* 无

功能:

* 获取predictor, predictor内定义了神经网络,可以用来计算对应输入经过神经网络处理之后的输出.

返回值:

* `std::shared_ptr<PaddlePredictor> _sampling_predictor`

#### load

参数:

* `const std::string& model_dir` 用来加载model的路径

功能:

* 通过调用`load_inference_model`以及`load_solver`函数来从`model_dir`处加载inference_model以及solver

返回值:`bool`

* 如果`load_inference_model`以及`load_solver`函数均返回`true`则返回`true`
* 否则返回`false`

#### load_inference_model



参数:

* `const std::string& model_dir`用于加载inference_model的路径

功能:

* 从`model_dir`处加载inference_model

返回值:`bool`

* 如果调用此函数的agent为clone出来的,则无法加载,返回`false`
* 否则返回`true`

#### load_solver

参数:

* `const std::string& model_dir` 用于加载solver的路径

功能:

* 根据`_config`的值从`model_dir`处加载`ga_solver`或`es_solver`
* 注意:在加载solver前需要首先加载inference_model,否则会函数会返回`flase`

返回值:`bool`

* 如果调用此函数的agent为clone出来的,则无法加载,返回`false`

* 根据`solver->load()`的返回值返回`true`或`false`

#### save

参数:

* `const std::string& model_dir`保存model以及solver的路径

功能:

* 通过调用`save_inference_model(model_dir)` 以及`save_solver(model_dir)`来保存inference_model以及solver

返回值: `bool`

* 均保存成功返回`true`
* 否则返回`false`

#### save_iniference_model

参数:

* `const std::string& model_dir`保存model的路径

功能:

* 通过调用`_predictor->SaveOptimizeModel`保存model

返回值:`bool`

* 如果调用此函数的agent为clone出来的,则无法保存,返回`false`
* 如果调用此函数的agent为sampling_agent,则在保存model后返回`true`

#### save_solver

参数:

* `const std::string& model_dir`保存model的路径

通过调用`es_solver->save`或`_ga_solver->save`来保存solver

返回值:`bool`

返回`solver->save`的返回值

#### init_solver

参数:

* 无

功能:

* 根据`_config`的值初始化一个ga_sovler或es_solver

返回值:`bool`

* 如果在调用`init_solver`之前没有调用`load_inference_model`,则会返回`false`

* 否则便会一句`config`中的配置返回`_es_solver->init()`或`_ga_solver->init()`的结果
