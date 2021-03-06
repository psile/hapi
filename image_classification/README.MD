# 高级api图像分类

## 数据集准备
在开始训练前，请确保已经下载解压好[ImageNet数据集](http://image-net.org/download)，并放在合适的目录下，准备好的数据集的目录结构如下所示：

```bash
/path/to/imagenet
    train
         n01440764
              xxx.jpg
              ...
         n01443537
              xxx.jpg
              ...
         ...
    val
         n01440764
              xxx.jpg
              ...
         n01443537
              xxx.jpg
              ...
         ...
```


## 训练
### 单卡训练
执行如下命令进行训练
```bash
python -u main.py --arch resnet50 /path/to/imagenet
```
加-s 可以切换成静态图模式训练，默认为动态图模式。

### 多卡训练
执行如下命令进行训练
```bash
CUDA_VISIBLE_DEVICES=0,1,2,3 python -m paddle.distributed.launch main.py --arch resnet50 /path/to/imagenet
```

## 预测

### 单卡预测
执行如下命令进行预测
```bash
python -u main.py --arch resnet50 --eval-only /path/to/imagenet 
```

### 多卡预测
执行如下命令进行多卡预测
```bash
CUDA_VISIBLE_DEVICES=0,1,2,3 python -m paddle.distributed.launch main.py --arch resnet50 --eval-only /path/to/imagenet
```


## 参数说明


* **arch**: 要训练或预测的模型名称
* **device**: 训练使用的设备，'gpu'或'cpu'，默认值：'gpu'
* **static**: 是否使用静态图模式训练
* **epoch**: 训练的轮数，默认值：120
* **learning-rate**: 学习率，默认值：0.1
* **batch-size**: 每张卡的batch size，默认值：64
* **output-dir**: 模型文件保存的文件夹，默认值：'output'
* **num-workers**: dataloader的进程数，默认值：4
* **resume**: 恢复训练的模型路径，默认值：None
* **eval-only**: 是否仅仅进行预测
* **lr-scheduler**: 学习率衰减策略，默认值：piecewise
* **milestones**: piecewise学习率衰减策略的边界，默认值：[30, 60, 80]
* **weight-decay**: 模型权重正则化系数，默认值：1e-4
* **momentum**: SGD优化器的动量，默认值：0.9

注意：使用```--resume```恢复训练时，假如你的模型路径为```./output/118.pdparams```，你输入的路径不需要带后缀，即```--resume ./output/118```即可。

## 模型

| 模型 | top1 acc | top5 acc |
| --- | --- | --- |
| [ResNet18](https://paddle-hapi.bj.bcebos.com/models/resnet18.pdparams) | 71.72 | 90.60 |
| [ResNet34](https://paddle-hapi.bj.bcebos.com/models/resnet34.pdparams) | 75.02 | 92.31 |
| [ResNet50](https://paddle-hapi.bj.bcebos.com/models/resnet50.pdparams) | 76.27 | 93.03 |
| [ResNet101](https://paddle-hapi.bj.bcebos.com/models/resnet101.pdparams) | 78.33 | 94.04 |
| [ResNet152](https://paddle-hapi.bj.bcebos.com/models/resnet152.pdparams) | 78.78 | 94.40 |
| [vgg16](https://paddle-hapi.bj.bcebos.com/models/vgg16.pdparams) | 71.92 | 90.65 | 
| [mobilenet_v1](https://paddle-hapi.bj.bcebos.com/models/mobilenet_v1_x1.0.pdparams) | 71.16 | 89.89 | 
| [mobilenet_v2](https://paddle-hapi.bj.bcebos.com/models/mobilenet_v2_x1.0.pdparams) | 72.30 | 90.74 | 
| [darknet53](https://paddle-hapi.bj.bcebos.com/models/darknet53.pdparams) | 78.43 | 94.24 | 

上述部分模型的复现参数请参考scripts下的脚本。需要注意的是darknet要使用image size为256的输入来预测, 即```--image-size 256```


## 参考文献
- ResNet: [Deep Residual Learning for Image Recognitio](https://arxiv.org/abs/1512.03385), Kaiming He, Xiangyu Zhang, Shaoqing Ren, Jian Sun
- MobileNetV1: [MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Applications](https://arxiv.org/abs/1704.04861), Andrew G. Howard, Menglong Zhu, Bo Chen, Dmitry Kalenichenko, Weijun Wang, Tobias Weyand, Marco Andreetto, Hartwig Adam
- MobileNetV2: [MobileNetV2: Inverted Residuals and Linear Bottlenecks](https://arxiv.org/pdf/1801.04381v4.pdf), Mark Sandler, Andrew Howard, Menglong Zhu, Andrey Zhmoginov, Liang-Chieh Chen
- VGG: [Very Deep Convolutional Networks for Large-scale Image Recognition](https://arxiv.org/pdf/1409.1556), Karen Simonyan, Andrew Zisserman

