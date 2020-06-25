# Food-Classification---CNN
## 1.实验数据集
数据集为食物照片，共有11类 Bread, Dairy product, Dessert, Egg, Fried food, Meat, Noodles/Pasta, Rice, Seafood, Soup, and Vegetable/Fruit，其中Training set有9866张照片，Validation set有3430张照片，Testing set有3347张照片。
## 2.作业概述
输入一张食物的图片，然后输出图片中食物所属的类别（0、1、2、……、10），选用的模型是卷积神经网络CNN。
## 3.运行环境
GPU：2X 1080TI
Windows10
Python=3.6 and Pytorch=1.2.0
## 4.模型结构
首先将图片导入，用opencv（cv2）读取图片并存放在numpy中，每张图片都是128*128*3，然后通过随机旋转、水平翻转图片等方法对图片进行数据增强，最后建立模型。
我们首先建立了一个原始模型，利用nn.Conv2d，nn.BatchNorm2d，nn.ReLU，nn.MaxPool2d这4个函数来构建一个5层的CNN，然后进入到一个3层全连接层
然后我们对模型进行了几次变形，包括增加卷积层（每层均有卷积和池化）、增加卷积层（中间有仅有卷积无池化的层）、减少卷积层。接下来是训练参数，使用训练集training set进行训练，并使用验证集validation set来选择最好的参数，具体参数见代码文件，同时我们也将training set和validation set合并到一起共同训练，最后各模型结果如表1所示。

表1 各模型结果
<table>
   <tr>
      <td>模型</td>
      <td>训练集训练模型+验证集验证</td>
      <td></td>
      <td></td>
      <td></td>
      <td>训练集+验证集训练模型</td>
      <td></td>
   </tr>
   <tr>
      <td></td>
      <td>训练集Accuracy</td>
      <td>训练集Loss</td>
      <td>验证集Accuracy</td>
      <td>验证集Loss</td>
      <td>Accuracy</td>
      <td>Loss</td>
   </tr>
   <tr>
      <td>原始模型</td>
      <td>0.863572</td>
      <td>0.00309</td>
      <td>0.601458</td>
      <td>0.01255</td>
      <td>0.927798</td>
      <td>0.001628</td>
   </tr>
   <tr>
      <td>增加卷积层（每层均有卷积和池化）</td>
      <td>0.886783</td>
      <td>0.002609</td>
      <td>0.677843</td>
      <td>0.010301</td>
      <td>0.934943</td>
      <td>0.00145</td>
   </tr>
   <tr>
      <td>增加卷积层（中间有仅有卷积无池化的层）</td>
      <td>0.755321</td>
      <td>0.005596</td>
      <td>0.641983</td>
      <td>0.00868</td>
      <td>0.837696</td>
      <td>0.003616</td>
   </tr>
   <tr>
      <td>减少卷积层</td>
      <td>0.797993</td>
      <td>0.004717</td>
      <td>0.574636</td>
      <td>0.013245</td>
      <td>0.879738</td>
      <td>0.002646</td>
   </tr>
   <tr>
      <td></td>
   </tr>
</table>

## 5.模型改进思路
根据第四部分和第五部分的模型结果，可以发现验证集的准确率较低，同时还存在一定程度上的过拟合问题，因此我们有了如下的改进思路：
（1）引入残差神经网络，即ResNet，它允许网络尽可能的加深，可以避免随着网络的加深，出现了训练集准确率下降的现象。
（2）引入dropout，能够在CNN中防止过拟合。
表3 改进模型结果
<table>
   <tr>
      <td>模型</td>
      <td>训练集训练模型+验证集验证</td>
      <td></td>
      <td></td>
      <td>训练集+验证集训练模型</td>
      <td></td>
   </tr>
   <tr>
      <td>训练集Accuracy</td>
      <td>训练集Loss</td>
      <td>验证集Accuracy</td>
      <td>验证集Loss</td>
      <td>Accuracy</td>
      <td>Loss</td>
   </tr>
   <tr>
      <td>0.885465</td>
      <td>0.002643</td>
      <td>0.741399</td>
      <td>0.007159</td>
      <td>0.895382</td>
      <td>0.00237</td>
   </tr>
</table>
