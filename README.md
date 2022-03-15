# Atlas200DK_YOLO_V5_CPP


华为Atlas200DK部署运行YOLO_V5_CPP

一、环境部署

环境部署分为开发环境和运行环境(此文档为C++版说明)，开发环境为PC端的

ubuntu(16.04验证没有问题)系统环境，运行环境为在Atlas200DK上的环境，具体的环境搭

建可以参考官方说明和网上的相关教程，部分链接地址如下供参考：

https://gitee.com/ascend/samples/tree/v0.3.0/cplusplus/level2_simple_inference/2_object_detection/YOLOV3_coco_detection_picture

https://blog.csdn.net/weixin_44580987/article/details/115704590

过程中遇到的具体问题可以到华为的官方论坛查找或者提问，地址如下：

https://bbs.huaweicloud.com/forum/forum-726-1.html

二、模型转换

1、模型步骤
	
模型转换可参考如下地址的说明：

https://github.com/lenLRX/Atlas_ACL_E2E_Demo/blob/master/yolov5_model_cvt.md

本人选择的是YOLOV5_v5.0(有Focus层)后的一个版本，所以按照说明可以成功转换模型，

而且.om模型已经去除Focus层(这个开源的推理代码我没有编绎成功，代码有一点复杂)。

作者对应的中文说明链接如下：

https://zhuanlan.zhihu.com/p/406816839

三、编绎运行代码
	
1、代码链接

本人选择参考的是如下地址的开源代码：

https://github.com/Hanawh/Atlas_200DK_yolov5_cpp

他的中文说明地址如下：

https://blog.csdn.net/qq_36530992/article/details/110264364


2、代码说明

作者把FOCUS层的操作放到了前处理里面，这样就和前面模型转换的保持一致，后处理

参考华为官方的Samples里的yolov3，然后基于YOLOV5的结果数据解析，推理结果正确。
	
作者的模型转换有点复杂，本人操作也没有成功。


三、测试结果

图像大小：1280*720(720P)

推理网络：yolov5

运行耗时：150ms(前处理140ms)

注：C++的opencv前处理在卡上较慢，需要优化。




说明：代码和工具不是本人源创，都是如下两个开源库作者的贡献
https://github.com/Hanawh/Atlas_200DK_yolov5_cpp
https://github.com/lenLRX/Atlas_ACL_E2E_Demo

再次感谢！！！







