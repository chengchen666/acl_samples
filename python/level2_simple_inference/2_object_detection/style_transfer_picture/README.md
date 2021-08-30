**本样例为大家学习昇腾软件栈提供参考，非商业目的！**

**本样例适配3.2.0及以上版本，支持产品为Atlas200DK、Atlas300([ai1s](https://support.huaweicloud.com/productdesc-ecs/ecs_01_0047.html#ecs_01_0047__section78423209366))。**

**本README只提供命令行方式运行样例的指导，如需在Mindstudio下运行样例，请参考[Mindstudio运行图片样例wiki](https://github.com/Ascend/samples/wikis/Mindstudio%E8%BF%90%E8%A1%8C%E5%9B%BE%E7%89%87%E6%A0%B7%E4%BE%8B?sort_id=3164874)。**

## style_transfer_picture样例


功能：使用风格迁移模型对输入图片进行推理，并生成风格化后的图片。

样例输入：原始图片jpg文件。

样例输出：带推理结果的jpg文件。

### 前提条件

部署此Sample前，需要准备好以下环境：

- 请确认已按照[环境准备和依赖安装](../../../environment)准备好环境。

- 已完成对应产品的开发环境和运行环境安装。

### 软件准备

1. 获取源码包。

   可以使用以下两种方式下载，请选择其中一种进行源码准备。

    - 命令行方式下载（下载时间较长，但步骤简单）。

        开发环境，非root用户命令行中执行以下命令下载源码仓。

       **cd $HOME**

       **git clone https://github.com/Ascend/samples.git**

    - 压缩包方式下载（下载时间较短，但步骤稍微复杂）。

        1. samples仓右上角选择 **克隆/下载** 下拉框并选择 **下载ZIP**。

        2. 将ZIP包上传到开发环境中的普通用户家目录中，例如 **$HOME/ascend-samples-master.zip**。

        3. 开发环境中，执行以下命令，解压zip包。

            **cd $HOME**

            **unzip ascend-samples-master.zip**

2. 获取此应用中所需要的原始网络模型。

    参考下表获取此应用中所用到的模型，并将其存放到开发环境普通用户下的工程目录：   
 **cd $HOME/samples/python/level2_simple_inference/2_object_detection/style_transfer_picture/model** 
    
    |  **模型名称**  |  **模型说明**  |  **模型下载路径**  |
    |---|---|---|
    |  yolov3| 基于GAN网络的模型。  |  请参考[https://github.com/Ascend/modelzoo/tree/master/contrib/TensorFlow/Research/cv/style_transfer](https://github.com/Ascend/modelzoo/tree/master/contrib/TensorFlow/Research/cv/style_transfer)目录中README.md下载原始模型章节下载模型和权重文件。 |

3. 将原始模型转换为Davinci模型。
    
    **注：请确认环境变量已经在[环境准备和依赖安装](../../../environment)中配置完成**

    1. 设置LD_LIBRARY_PATH环境变量。

        由于LD_LIBRARY_PATH环境变量在转使用atc工具和运行样例时会产生冲突，所以需要在命令行单独设置此环境变量，方便修改。

        **export install_path=\\$HOME/Ascend/ascend-toolkit/latest**

        **export LD_LIBRARY_PATH=\\${install_path}/atc/lib64**  

    2. 执行以下atc命令进行模型转换。

        **atc --model=./xingkong1.pb --framework=3 --output=xingkong1_fp32_nchw_no_aipp --soc_version=Ascend310**

        **atc --model=./bijiasuo.pb --framework=3 --output=bijiasuo_fp32_nchw_no_aipp --soc_version=Ascend310**

        **atc --model=./tangguo.pb --framework=3 --output=tangguo_fp32_nchw_no_aipp --soc_version=Ascend310**

        **atc --model=./work_soldiers.pb --framework=3 --output=work_soldiers_fp32_nchw_no_aipp --soc_version=Ascend310**

    3. 将转换好的4个om模型放到工程目录下的model文件夹中。

4. 获取样例需要的测试图片。

    执行以下命令，进入样例的data文件夹中，下载对应的测试图片。

    **cd $HOME/samples/python/level2_simple_inference/2_object_detection/style_transfer/data**

    **wget https://c7xcode.obs.myhuaweicloud.com/models/style_transfer_picture/data/test.jpg**


### 样例运行

**注：开发环境与运行环境合一部署，请跳过步骤1，直接执行[步骤2](#step_2)即可。**   

1. 执行以下命令,将开发环境的 **samples** 目录上传到运行环境中，例如 **/home/HwHiAiUser**，并以HwHiAiUser（运行用户）登录运行环境（Host）。

    **scp -r $HOME/samples HwHiAiUser@xxx.xxx.xxx.xxx:/home/HwHiAiUser**

    **ssh HwHiAiUser@xxx.xxx.xxx.xxx**    

    ![](https://images.gitee.com/uploads/images/2020/1106/160652_6146f6a4_5395865.gif "icon-note.gif") **说明：**  
    > - **xxx.xxx.xxx.xxx**为运行环境ip，200DK在USB连接时一般为192.168.1.2，300（ai1s）为对应的公网ip。

2. <a name="step_2"></a>运行可执行文件。

    - 如果是开发环境与运行环境合一部署，执行以下命令，设置运行环境变量，并切换目录。

      **export LD_LIBRARY_PATH=**

      **source ~/.bashrc**
        
      **cd $HOME/samples/python/level2_simple_inference/2_object_detection/style_transfer/src**    

      （1）星空风格
      **python3.6 main.py ../data xingkong**
      （2）糖果风格
      **python3.6 main.py ../data tangguo**
      （3）毕加索风格
      **python3.6 main.py ../data bijiasuo**
      （4）工农兵风格
      **python3.6 main.py ../data worksoldiers**

    - 如果是开发环境与运行环境分离部署，执行以下命令切换目录。
    
      **cd $HOME/samples/python/level2_simple_inference/2_object_detection/style_transfer/src**    

      切换目录后，执行以下命令运行样例。

      （1）星空风格
      **python3.6 main.py ../data xingkong**
      （2）糖果风格
      **python3.6 main.py ../data tangguo**
      （3）毕加索风格
      **python3.6 main.py ../data bijiasuo**
      （4）工农兵风格
      **python3.6 main.py ../data worksoldiers**

### 查看结果

运行完成后，会在运行环境的工程目录的outputs文件夹下保存推理后的图片。