## 深度学习在医学图像处理中的应用

### 简介

本文参考了三篇深度学习在医学图像处理中的三篇综述性 [ ^doc1][^doc2][^doc3] 的文章，旨在对于深度学习和医学图像相结合的现有情况做一个小总结，并探讨一下未来的一些发展趋势和自身的一些思考

### 医学影像深度学习工具

#### 深度学习模型

![](ss)[^doc1]

在医学影像处理中使用的到的深度学习的模型框架主要有：

#####SAE(stack auto-encoder) 

无监督学习方案，逐层训练，得到特征描述为主

#####RBM(restricted Boltzmann machine)

无监督学习方案，与SAE 类似

#####CNN(convolutional neural network)

卷积神经网络，使用最为广泛，可以用来提取图片特征或者直接完成分类检测等任务
#####RNN(recurrent neural network)
循环神经网络，用来获取时序上的信息，在CT等逐行扫描图像中使用
#####U-net (with a single downsampling stage)
类似于带short-cut的全卷机网络，用来融合不同尺度的图像的特征
#####FCNN(fully convolutional neural network)
全卷机网络，可以获取与原图相同分辨率的图片，常用于分割等任务
######FRCNN(Faster Region-proposal based neural network)
一种快速的深度学习检测网络框架，分为rpn 和 rcnn 两层，用于检测图像中的多种物体
#### 深度学习框架
常用的框架包括：
* caffe
* tensorflow
* torch
* Theano 
#### 暂时没有使用到的深度学习技术
* VAE
* GAN

###State of Arts

||数据集|目标|Top1 方法|Top 1 结果|
|---|---|---|---|---|
|脑部图像|[BRATS](http://braintumorsegmentation.org)|检测分割脑肿瘤|DL CNN + U-Net + Original CT + CNN[^doc6]|CT = 0.87 Core =0.81 Enhance =0.72|
|眼部图像|[diabetic retinopathy detection competition](https://www.kaggle.com/c/diabetic-retinopathy-detection/leaderboard)|检测糖尿病视网膜病变|使用fractional max-pooling|score=0.84958|
|胸片|[LUNA16](https://luna16.grand-challenge.org)|检测胸片中的结节|ZNET[^zfnet]|CPM=0.811|
|病理学和显微图片处理|[CAMELYON16](https://camelyon16.grand-challenge.org/results/)|判断病理切片是否是肿瘤|patch normalize + Inceptionv3 network + tumor heatmap + 联通域方法 + RF 分类[^doc5]|AUC = 0.9935|
|乳房X光片处理|[DREAM challenge](https://www.synapse.org/#!Synapse:syn4224222/wiki/434546)|通过X光片判断乳腺癌|modifyed VGG + data augment + random crop|AUC = 0.8735|
|心脏图像处理|[Second Annual Data Science Bowl](https://www.kaggle.com/c/second-annual-data-science-bowl/leaderboard)|计算心脏容量|U-Net + deep learning|score = 0.003959|
|肝脏分割|[sliver07](http://www.sliver07.org)|CT 图像上得到肝脏的位置|基于血管的半自动分割算法 |score = 85.7|
|前列腺分割|[PROMISE12](https://promise12.grand-challenge.org/results/)|CT 图像上得到前列腺的位置|CNN + U-NET + ResBlock[^doc4]|score = 86.85|

[^zfnet]: 具体算法为 CNN+U-NET + 联通域提取 +  基于patch的预测做reduction 使用的技巧包括 data augment Leak RELU + ADAM optimization


### 深度学习在医学图像领域的一些限制
* 缺少高质量的标注的训练样本，因此训练出来的模型可能是过拟合的或者说推广性不好，因此需要将的到的模型放在各种情况下测试推广性[^doc3]
* 深度学习得到的模型是一个黑盒子，无法解释其有效性，在一些特殊的情况下会出现非常奇怪无法解释的问题，因此在医疗行业中的接受度也是一个问题[^doc3]
* 在商业系统中使用临床上的图片资料会存在法律和伦理上的问题而不使用这样的样本无法进一步的提高深度学习模型的水平[^doc3]

### 一些自己的思考
#### 2D VS 3D
从文献综述来看，大部分的工作都是基于2D图像的，其实在医学图像中，CT 和 MRI都是3D的数据2D化的结果，在医疗图像处理的算法中3D重建等等也是非常重要的一大类算法，但是现有的基于3D的算法一来耗时比较高，二来并没有比基于2D的算法提高很多，使用2D还是3D是一个值得思考的问题。
#### Feature vs Result
从文献综述中来看，稍微久远一些的算法就是把CNN当作是一个特征提取的算子获得图像的描述特征而最新的一些方法直接将CNN的结果就作为最终的输出结果来使用， 这里喔感觉直接使用CNN的输出作为结果，会涉及到文献中所说的黑盒子的限制，可解释型一般是比较差的，而作为特征来使用解释性可能会好一些，因为后续的一些后处理中可以增加的规则类的比较多，解释性会更佳
#### 过滤 vs 诊断
一直以来作者觉得在医疗行业中，计算机能做的最大的贡献就是帮助医生做大量医学影像的过滤工作，至于使用诊断上最多也只是一个辅助的诊断工具，而机器学习到达了深度学习的时代，有些本来以为不太可能的任务都被深度学习算法一个一个的攻克了，在未来的工作做，计算机深度学习是不是可能独立的进行本属于医生独享的诊断工作我还是不得而知，然后我们可以知道的是，技术的发展使得过滤的正确率大大的提高，极大的提高生产的效率，这一方面是肯定有助于医疗行业的，相应深度学习在医疗领域的前景还是很广阔的。









[^doc1]: A Survey on Deep Learning in Medical Image Analysis 
[^doc2]: Deep Learning in Medical Image Analysis  
[^doc3]: Deep Learning in Medical Imaging: General Overview 
[^doc4]: Volumetric ConvNets with Mixed Residual Connections for Automated Prostate Segmentation from 3D MR Images 
[^doc5]: DEEP LEARNING BASED CANCER METASTASES DETECTION 
[^doc6]: Proceedings of MICCAI-BRATS 2016 

￼ 


 



