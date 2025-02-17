English|[中文](README_CN.md)

**This sample provides reference for you to learn the Ascend AI Software Stack and cannot be used for commercial purposes.**

**This README file provides only guidance for running the sample in command line (CLI) mode. For details about how to run the sample in MindStudio, see [Running Image Samples in MindStudio](https://github.com/Ascend/samples/wikis/Mindstudio%E8%BF%90%E8%A1%8C%E5%9B%BE%E7%89%87%E6%A0%B7%E4%BE%8B?sort_id=3164874).**

## dehaze_picture Sample   
Function: Use deploy_vel model to dehaze picture. 
Input: jpg image. 
Output: image dehazed.  

### Prerequisites
Check whether the following requirements are met. If not, perform operations according to the remarks. If the CANN version is upgraded, check whether the third-party dependencies need to be reinstalled. (The third-party dependencies for 5.0.4 and later versions are different from those for earlier versions.)
| Item                     | Requirement                                                  | Remarks                                                      |
| ------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Hardware                 | Atlas200 DK/Atlas300 ([AI1s](https://support.huaweicloud.com/en-us/productdesc-ecs/ecs_01_0047.html#ecs_01_0047__section78423209366))/Atlas200I DK A2 | Currently, the Atlas200 DK, Atlas300 and Atlas200I DK A2 have passed the test. For details about the product description, see [Hardware Platform](https://ascend.huawei.com/en/#/hardware/product). For other products, adaptation may be required. |
| CANN version             | Atlas200 DK/Atlas300(AI1s): 6.3.RC1 Atlas200I DK A2: 6.2.RC1 | Install the CANN by referring to [Sample Deployment](https://github.com/Ascend/samples#安装) in the About Ascend Samples Repository. If the CANN version is earlier than the required version, switch to the sample repository specific to the CANN version. See [Release Notes](https://github.com/Ascend/samples/blob/master/README.md). |
| Third-party dependencies | opencv, ffmpeg+acllite                                       | For details, see [Third-Party Dependency Installation Guide (C++ Sample)](../../../environment). |

### Sample Preparation

1. Obtain the source package.

   You can download the source code in either of the following ways:  
    - Command line (The download takes a long time, but the procedure is simple.)
       ```    
       # In the development environment, run the following commands as a non-root user to download the source repository:   
       cd ${HOME}     
       git clone https://github.com/Ascend/samples.git
       ```
       **Note: To switch to another tag (for example, v0.5.0), run the following command:**
       ```
       git checkout v0.5.0
       ```
    - Compressed package (The download takes a short time, but the procedure is complex.)  
       **Note: If you want to download the code of another version, switch the branch of the samples repository according to the prerequisites.**  
       ``` 
        # 1. Click Clone or Download in the upper right corner of the samples repository and click Download ZIP.   
        # 2. Upload the .zip package to the home directory of a common user in the development environment, for example, ${HOME}/ascend-samples-master.zip.    
        # 3. In the development environment, run the following commands to unzip the package:    
        cd ${HOME}    
        unzip ascend-samples-master.zip
       ```

2. Obtain the source network model required by the application.
    | **Model**      | **Description**                 | **How to Obtain**                                            |
    | ------------------ | ----------------------------- | ------------------------------------------------------------ |
    | deploy_vel | Picture dehazing process based on tensorflow. | Please refer to https://github.com/Ascend/ModelZoo-TensorFlow/tree/master/TensorFlow/contrib/cv/dehaze/ATC_deploy_vel_tf_AE original model section for downloading the original model. |
    To facilitate download, the commands for downloading the original model and converting the model are provided here. You can directly copy and run the commands. You can also refer to the above table to download the model from ModelZoo and manually convert it. 
    
    Downloading the original model.
    
    ```
    cd ${HOME}/samples/python/contrib/dehaze_picture/model     
    wget https://obs-9be7.obs.cn-east-2.myhuaweicloud.com/003_Atc_Models/AE/ATC%20Model/SingleImageDehaze/output_graph.pb 
    ```

    Converting the model.
    
    - If you are using Atlas200 DK or Atlas300, please run the following command:
    
      ```
      atc --model=output_graph.pb --framework=3 --input_shape="t_image_input_to_DHGAN_generator:1,512,512,3" --output=deploy_vel --soc_version=Ascend310 --input_fp16_nodes="t_image_input_to_DHGAN_generator" --output_type=FP32
      ```
    
    - If you are using Atlas200I DK A2, please run the following command:
    
      ```
      atc --model=output_graph.pb --framework=3 --input_shape="t_image_input_to_DHGAN_generator:1,512,512,3" --output=deploy_vel --soc_version=Ascend310B1 --input_fp16_nodes="t_image_input_to_DHGAN_generator" --output_type=FP32
      ```
    
3. Obtain the test images required by the sample.
    ```
    cd $HOME/samples/python/contrib/dehaze_picture/data
    wget https://obs-9be7.obs.cn-east-2.myhuaweicloud.com/models/SingleImageDehaze/test_image/10992_04_0.8209.png 
    ```

### Sample Running

**Note: If the development environment and operating environment are set up on the same server, skip step 1 and go to [step 2](#step_2) directly.**  

1. Run the following commands to upload the **dehaze_picture** directory in the development environment to any directory in the operating environment, for example, **/home/HwHiAiUser**, and log in to the operating environment (host) as the running user (**HwHiAiUser**):
    ```
    # In the following information, xxx.xxx.xxx.xxx is the IP address of the operating environment. The IP address of Atlas200 DK or Atlas200I DK A2 is 192.168.1.2 when it is connected over the USB port, and that of Atlas 300 (AI1s) is the corresponding public IP address.
    scp -r $HOME/samples/python/contrib/dehaze_picture HwHiAiUser@xxx.xxx.xxx.xxx:/home/HwHiAiUser
    ssh HwHiAiUser@xxx.xxx.xxx.xxx   
    ```

2. <a name="step_2"></a>Run the sample.

   - If **step 1** has been performed, please run the following command

     ```
     cd ${HOME}/dehaze_picture/src
     python3.6 main.py ../data/
     ```

   - If not, please run the following command

     ```
     cd $HOME/samples/python/contrib/dehaze_picture/src
     python3.6 main.py ../data/
     ```

### Result Viewing

After the execution is complete, an inferred image is generated in the **out** directory of the sample project.

### Common Errors
For details about how to rectify the errors, see [Troubleshooting](https://github.com/Ascend/samples/wikis/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E5%AE%9A%E4%BD%8D/%E4%BB%8B%E7%BB%8D). If an error is not included in Wiki, submit an issue to the **samples** repository.
