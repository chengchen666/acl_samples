中文|[English](README.md)

**本样例为大家学习昇腾软件栈提供参考，非商业目的！**

**本README只提供命令行方式运行样例的指导，如需在Mindstudio下运行样例，请参考[Mindstudio运行图片样例wiki](https://github.com/Ascend/samples/wikis/Mindstudio%E8%BF%90%E8%A1%8C%E5%9B%BE%E7%89%87%E6%A0%B7%E4%BE%8B?sort_id=3164874)。**

## DeRain样例
功能：通过读取本地雨天退化图像数据，对场景中的雨线、雨雾进行去除，实现图像增强效果。    
样例输入：原始图片png文件。    
样例输出：带推理结果的png文件。   

### 前置条件
请检查以下条件要求是否满足，如不满足请按照备注进行相应处理。如果CANN版本升级，请同步检查第三方依赖是否需要重新安装（5.0.4及以上版本第三方依赖和5.0.4以下版本有差异，需要重新安装）。
| 条件 | 要求 | 备注 |
|---|---|---|
| CANN版本 | >=5.0.4 | 请参考CANN样例仓介绍中的[安装步骤](https://github.com/Ascend/samples#%E5%AE%89%E8%A3%85)完成CANN安装，如果CANN低于要求版本请根据[版本说明](https://github.com/Ascend/samples/blob/master/README_CN.md#%E7%89%88%E6%9C%AC%E8%AF%B4%E6%98%8E)切换samples仓到对应CANN版本 |
| 硬件要求 | Atlas200DK/Atlas300([ai1s](https://support.huaweicloud.com/productdesc-ecs/ecs_01_0047.html#ecs_01_0047__section78423209366))  | 当前已在Atlas200DK和Atlas300测试通过，产品说明请参考[硬件平台](https://ascend.huawei.com/zh/#/hardware/product) ，其他产品可能需要另做适配|
| 第三方依赖 | opencv | 请参考[第三方依赖安装指导(C++样例)](../../../environment)完成对应安装 |

### 样例准备

1. 获取源码包。

   可以使用以下两种方式下载，请选择其中一种进行源码准备。   
    - 命令行方式下载（下载时间较长，但步骤简单）。
       ```    
       # 开发环境，非root用户命令行中执行以下命令下载源码仓。    
       cd ${HOME}     
       git clone https://github.com/Ascend/samples.git
       ```
       **注：如果需要切换到其它tag版本，以v0.5.0为例，可执行以下命令。**
       ```
       git checkout v0.5.0
       ```
    - 压缩包方式下载（下载时间较短，但步骤稍微复杂）。   
       **注：如果需要下载其它版本代码，请先请根据前置条件说明进行samples仓分支切换。**   
       ``` 
        # 1. samples仓右上角选择 【克隆/下载】 下拉框并选择 【下载ZIP】。    
        # 2. 将ZIP包上传到开发环境中的普通用户家目录中，【例如：${HOME}/ascend-samples-master.zip】。     
        # 3. 开发环境中，执行以下命令，解压zip包。     
        cd ${HOME}    
        unzip ascend-samples-master.zip
       ```
2. 将原始模型转换为Davinci模型。   
   | **模型名称** | **模型说明**                                     | **模型下载路径**                                             |
   | ------------ | ------------------------------------------------ | ------------------------------------------------------------ |
   | DeRain       | 图像增强推理模型。是基于tensorflow的模型。 | 请参考[ATC_DeRain_TensorFlow_AE](https://github.com/Ascend/ModelZoo-TensorFlow/tree/master/TensorFlow/contrib/cv/DeRain/ATC_DeRain_TensorFlow_AE)目录中README.md下载原始模型章节下载模型和权重文件。 |
   ```
   # 为了方便下载，在这里直接给出原始模型下载及模型转换命令,可以直接拷贝执行。也可以参照上表在modelzoo中下载并手工转换，以了解更多细节。     
   cd ${HOME}/samples/cplusplus/level2_simple_inference/6_other/DeRain/model     
   wget https://obs-9be7.obs.cn-east-2.myhuaweicloud.com/003_Atc_Models/AE/ATC%20Model/DeRain/frozen_graph_noDWT_V2.pb
   wget https://obs-9be7.obs.cn-east-2.myhuaweicloud.com/003_Atc_Models/AE/ATC%20Model/DeRain/insert_op.cfg
   atc --model=./frozen_graph_noDWT_V2.pb --input_shape="degradated_image:1,256,256,1" --framework=3 --output=./DeRain  --soc_version=Ascend310  --insert_op_conf=./insert_op.cfg
   ```
   
   **注：如果在310B芯片上进行转换，修改参数--soc_version=Ascend310B1即可。**

### 样例部署

执行以下命令，执行编译脚本，开始样例编译。     
```
cd $HOME/samples/cplusplus/level2_simple_inference/6_other/DeRain/scripts   
bash sample_build.sh
```
**注：若环境中opencv使用的是opencv4版本，则提前创建如下软链接，并在classify_process.cpp中添加constants_c.h头文件。**

```
# 解决编译问题fatal error: opencv2/opencv.hpp: No such file or directory
ln -s /usr/include/opencv4/opencv2/ /usr/include/
# 解决编译问题error: ‘CV_LOAD_IMAGE_COLOR’ was not declared in this scope
vi ../src/classify_process.cpp
# 添加如下内容
#include "opencv2/imgcodecs/legacy/constants_c.h"
```

### 样例运行

**注：开发环境与运行环境合一部署，请跳过步骤1，直接执行[步骤2](#step_2)即可。**   

1. 执行以下命令,将开发环境的 **DeRain** 目录上传到运行环境中，例如 **/home/HwHiAiUser**，并以HwHiAiUser（运行用户）登录运行环境（Host）。
   ```
   # 【xxx.xxx.xxx.xxx】为运行环境ip，200DK在USB连接时一般为192.168.1.2，300（ai1s）为对应的公网ip。
   scp -r ${HOME}/samples/cplusplus/level2_simple_inference/6_other/DeRain HwHiAiUser@xxx.xxx.xxx.xxx:/home/HwHiAiUser
   ssh HwHiAiUser@xxx.xxx.xxx.xxx
   cd ${HOME}/DeRain/scripts   
   ```
2. <a name="step_2"></a>运行可执行文件。
   ```
   bash sample_run.sh
   ```
### 查看结果

运行完成后，会在样例工程的out/output目录下生成推理后的图片，显示对比结果如下所示。      
![输入图片说明](https://images.gitee.com/uploads/images/2021/1109/102709_abdc3a9e_5400693.png "屏幕截图.png")
![输入图片说明](https://images.gitee.com/uploads/images/2021/1109/102738_59ee192b_5400693.png "屏幕截图.png")

### 常见错误
请参考[常见问题定位](https://github.com/Ascend/samples/wikis/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E5%AE%9A%E4%BD%8D/%E4%BB%8B%E7%BB%8D)对遇到的错误进行排查。如果wiki中不包含，请在samples仓提issue反馈。