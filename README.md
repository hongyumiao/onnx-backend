## onnx-backend

>  通用神经网络模型 onnx 在 RT-Thread 上的后端

如果能在 RT-Thread 上解析并运行 onnx 的模型，那么就可以在 RT-Thread 上运行几乎所有主流机器学习框架了，例如 Tensorflow, Keras, Pytorch, Caffe2, mxnet ...

由于 onnx 的模型是 Google Protobuf v3 的格式，所以这个后端依赖于2个软件包：

- protobuf-c
- onnx-parser



#### 手写体例程

当前只有一个手写体识别的例程：利用 Keras 训练一个卷积神经网络模型，保存为 onnx 模型，再在 RT-Thread 上解析出模型里的参数进行 inference，当前以及在 STM32F407 上测试通过。

```
_________________________________________________________________
Layer (type)                 Output Shape              Param #   
=================================================================
conv2d_5 (Conv2D)            (None, 28, 28, 2)         20        
_________________________________________________________________
max_pooling2d_5 (MaxPooling2 (None, 14, 14, 2)         0         
_________________________________________________________________
dropout_5 (Dropout)          (None, 14, 14, 2)         0         
_________________________________________________________________
conv2d_6 (Conv2D)            (None, 14, 14, 2)         38        
_________________________________________________________________
max_pooling2d_6 (MaxPooling2 (None, 7, 7, 2)           0         
_________________________________________________________________
dropout_6 (Dropout)          (None, 7, 7, 2)           0         
_________________________________________________________________
flatten_3 (Flatten)          (None, 98)                0         
_________________________________________________________________
dense_5 (Dense)              (None, 4)                 396       
_________________________________________________________________
dense_6 (Dense)              (None, 10)                50        
=================================================================
Total params: 504
Trainable params: 504
Non-trainable params: 0
_________________________________________________________________

```



```
msh />onnx_mnist 1
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@        @@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@              @@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@                    @@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@          @@@@@@@@    @@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@        @@@@@@@@@@    @@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@  @@@@@@    @@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@  @@@@      @@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@          @@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@              @@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@            @@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@          @@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@      @@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@    @@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@    @@@@@@@@@@@@@@@@@@  @@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@    @@@@@@@@@@@@@@@@@@    @@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@  @@@@@@@@@@@@@@@@@@      @@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@    @@@@@@@@@@@@        @@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@                      @@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@                  @@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

Predictions:
0.007383 0.000000 0.057510 0.570970 0.000000 0.105505 0.000000 0.000039 0.257576 0.001016

The number is 3

```



```
msh />onnx_mnist 0
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@                          @@@@@@@@@@@@@@@@@@@@
@@@@@@                                @@@@@@@@@@@@@@@@@@
@@@@              @@@@@@@@@@@@@@        @@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@      @@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@      @@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@      @@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@        @@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@                  @@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@                    @@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@                @@@@      @@@@@@@@@@@@@@
@@@@@@@@@@@@@@        @@@@@@@@@@@@@@      @@@@@@@@@@@@@@
@@@@@@@@@@@@      @@@@@@@@@@@@@@@@@@      @@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@      @@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@    @@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@        @@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@      @@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@    @@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@      @@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

Predictions:
0.000498 0.000027 0.017220 0.028220 0.000643 0.002182 0.000000 0.753116 0.026616

The number is 7

```



## Todo List

- 模型量化
- 解析更加复杂的模型，生成计算图，
- 针对不同算子进行硬件加速。



## 联系方式

- 维护：Wu Han
- 主页：http://wuhanstudio.cc
- 邮箱：wuhanstudio@qq.com

