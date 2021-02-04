# Cartpole示例

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
