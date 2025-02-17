English|[中文](README_CN.md)

**This sample provides reference for you to learn the Ascend AI Software Stack and cannot be used for commercial purposes.**

**This README file provides only guidance for running the sample in command line (CLI) mode. For details about how to run the sample in MindStudio, see [Running Video Samples in MindStudio](https://github.com/Ascend/samples/wikis/Mindstudio%E8%BF%90%E8%A1%8C%E8%A7%86%E9%A2%91%E6%A0%B7%E4%BE%8B?sort_id=3170138).**

## Video Colorization Sample
Function: Perform inference on the input black-and-white video by using the colorization model.  

Input: black-and-white MP4 video file.   

Output: inference results displayed on the Presenter Server WebUI.

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
       **To switch to another tag (for example, v0.5.0), run the following command:**
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

2. Obtain the source model required by the application. 
    |  **Model** |  **Description** |  **How to Obtain** |
    |---|---|---|
    |  colorization| Inference model for black-and-white video colorization. It is a colorization model based on Caffe. | Download the model and weight files by referring to the links in **README.md** in the [ATC_colorization_caffe_AE](https://github.com/Ascend/ModelZoo-TensorFlow/tree/master/TensorFlow/contrib/cv/colorization/ATC_colorization_caffe_AE) directory of the ModelZoo repository. |

    To facilitate download, the commands for downloading the original model and converting the model are provided here. You can directly copy and run the commands. You can also refer to the above table to download the model from ModelZoo and manually convert it.
    
    Downloading the original model.
    
    ```
    cd ${HOME}/samples/cplusplus/level2_simple_inference/6_other/colorization_video/model
    wget https://obs-9be7.obs.cn-east-2.myhuaweicloud.com/003_Atc_Models/AE/ATC%20Model/colorization/colorization.caffemodel
    wget https://obs-9be7.obs.cn-east-2.myhuaweicloud.com/003_Atc_Models/AE/ATC%20Model/colorization/colorization.prototxt
    ```
    
    Converting the model.
    
    - If you are using Atlas200 DK or Atlas300, please run the following command:
    
      ```
      atc --input_shape="data_l:1,1,224,224" --weight="./colorization.caffemodel" --input_format=NCHW --output=colorization --soc_version=Ascend310 --framework=0 --model="./colorization.prototxt"
      ```
    
    - If you are using Atlas200I DK A2, please run the following command:
    
      ```
      atc --input_shape="data_l:1,1,224,224" --weight="./colorization.caffemodel" --input_format=NCHW --output=colorization --soc_version=Ascend310B1 --framework=0 --model="./colorization.prototxt"
      ```

### Sample Deployment
Run the following commands to execute the compilation script to start sample compilation:
```
cd ${HOME}/samples/cplusplus/level2_simple_inference/6_other/colorization_video/scripts
bash sample_build.sh
```

If `fatal error: opencv2/opencv.hpp: No such file or directory` occurred, please run the following code:

```
sudo ln -s /usr/include/opencv4/opencv2 /usr/include/
```

### Sample Running
**Note: If the development environment and operating environment are set up on the same server, skip step 1 and go to [step 2](#step_2) directly.**  

1. Run the following commands to upload the **colorization_video** directory in the development environment to any directory in the operating environment, for example, **/home/HwHiAiUser**, and log in to the operating environment (host) as the running user (**HwHiAiUser**):     
    ```
    # In the following information, xxx.xxx.xxx.xxx is the IP address of the operating environment. The IP address of Atlas 200 DK is 192.168.1.2 when it is connected over the USB port, and that of Atlas 300 (AI1s) is the corresponding public IP address.
    scp -r ${HOME}/samples/cplusplus/level2_simple_inference/6_other/colorization_video HwHiAiUser@xxx.xxx.xxx.xxx:/home/HwHiAiUser    
    ssh HwHiAiUser@xxx.xxx.xxx.xxx     
    ```

2. <a name="step_2"></a>Execute the script to run the sample.

    - If **step 1** has been performed, please run the following command

      ```
      cd ${HOME}/colorization_video/scripts
      bash sample_run.sh
      ```

    - If not, please run the following command

      ```
      cd ${HOME}/samples/cplusplus/level2_simple_inference/6_other/colorization_video/scripts
      bash sample_run.sh
      ```
### Result Viewing
1. Open the Presenter Server WebUI.  
   - For Atlas 200 DK   
     
      Open the URL that is displayed when Presenter Server is started.      
      
   - For Atlas 300 AI accelerator card (AI1s cloud inference environment)   
     
      **The following assumes that the intranet IP address of the Atlas 300 AI accelerator card (AI1s) is 192.168.0.194 and the public IP address is 124.70.8.192.**    
      
      The message "Please visit http://192.168.0.194:7009 for display server" is displayed when Presenter Server is started.   
      
      Replace the intranet IP address **192.168.0.194** in the URL with the public IP address **124.70.8.192**. That is, change the URL to http://124.70.8.192:7009.    
      
      Open the URL in the browser.    
2. Wait for Presenter Agent to transmit data to the server and click **Refresh**. When data arrives, the icon in the **Status** column for the corresponding **Channel** changes to green.    
3. Click a link in the **View Name** column to view the result.    

### Common Errors
For details about how to rectify the errors, see [Troubleshooting](https://github.com/Ascend/samples/wikis/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E5%AE%9A%E4%BD%8D/%E4%BB%8B%E7%BB%8D). If an error is not included in Wiki, submit an issue to the **samples** repository.
