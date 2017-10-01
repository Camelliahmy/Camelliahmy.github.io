---
title: TensorFlow 之基于Inception V3的多标签分类 retrain
categories:
 - TensorFlow
tags:
 - TensorFlow
 - python
---
本文参考http://blog.csdn.net/Numeria/article/details/73604339
以及参考开源代码github链接: https://github.com/BartyzalRadek/Multi-label-Inception-net

### 准备训练数据  

**1.下载数据集**
本文采用南京大学开源的数据集(点击下载:http://lamda.nju.edu.cn/files/miml-image-data.rar)
数据集中含有2000张图像，5个类，分别为 desert, mountains, sea, sunset , trees。
下载后包含两个压缩文件包:original.rar 以及 processed.rar
original.rar 中包含了2000张图像数据 

示例如下：
![图像数据示例](http://img.blog.csdn.net/20170808220354577?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjkzNjc2NQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 processed.rar 中包含了图像对应的标签信息。
 解压后为matlab文件格式。
 本文需要的是标签矩阵 target.mat 
 
 示例如下： 
  ![target.mat](http://img.blog.csdn.net/20170808220957581?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjkzNjc2NQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 
 **2.转换标签文件**
 本文需将 target.mat 转换为txt格式的标签文件。
 对于每一张图像 imagename.jpg, 对应生成一个 imagename.jpg.txt 的标签文件，文件中每一行为图像所含标签。
 
 对于下图左边的图像，有标签 desert，mountains，则对应的标签文件内容如右边所示。

![这里写图片描述](http://img.blog.csdn.net/20170808221634510?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjkzNjc2NQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 
 对于 target.mat 文件，转化的matlab脚本如下:
 

``` matlab
 load('miml data.mat');
if ~exist('labeldir') 
    mkdir labeldir;
end
LabelDir='labeldir/';
for i = 1:2000
    stri = num2str(i);
    label_file_name = [LabelDir stri '.jpg.txt'];
    fid = fopen(label_file_name,'w');
    for j = 1:5
        if targets(j,i)==1
            fprintf(fid,'%s\n',class_name{j});
        end
    end
    fclose(fid);
end
```

处理后的文件为:https://pan.baidu.com/s/1pLoeC6R

### 多标签分类
 本文利用已训练好的权重，将最后一层替换为自定义的数据集的类别数，前面权值不变，只训练最后的一层。
 1.将包含所有图像的文件夹放入 Multi-label-Inception-net/images  目录下  
 2.将labeldir目录拷贝到 Multi-label-Inception-net 目录下，改名为image_labels_dir(替换原同名目录)
 3.运行 python 脚本
 
```shell
python retrain.py \
--bottleneck_dir=bottlenecks \
--how_many_training_steps 500 \
--model_dir=model_dir \
--output_graph=retrained_graph.pb \
--output_labels=retrained_labels.txt \
--summaries_dir=retrain_logs \
--image_dir=images
``` 

开始训练
![这里写图片描述](http://img.blog.csdn.net/20170808231141610?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjkzNjc2NQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20170808232115180?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjkzNjc2NQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 训练完成
 ![这里写图片描述](http://img.blog.csdn.net/20170808232513804?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjkzNjc2NQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast) 
 
### 图像预测
利用脚本label_image.py 进行图像预测
调用如下：

```
python label_image.py images/image/4.jpg
```

得到输出结果：

![这里写图片描述](http://img.blog.csdn.net/20170808232841775?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjkzNjc2NQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 

 

 

:heart_eyes::kissing_heart::kissing_closed_eyes::flushed::relieved::satisfied::grin:


