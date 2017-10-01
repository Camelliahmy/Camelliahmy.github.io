---
title: TensorFlow 快速安装教程(OSX 10.11.6 + Python 2.7)
categories: 
 - TensorFlow 
tags: 
 - TensorFlow
 - Python
---
### 系统配置

 - 操作系统：OSX EI Capitan 10.11.6
 - Python：2.7

### Virtualenv 方式安装
1.打开终端（快捷键 Command + T）
2.安装pip、virtualrenv
```bash
sudo easy_install pip
sudo pip install --upgrade virtualenv  
```
3.新建一个  virtualenv 环境,这里的targetDirectory 使用  ~/tensorflow
```bash
# for Python 2.7
virtualenv --system-site-packages targetDirectory 
```
4.进入tensorflow文件夹，激活  virtualenv 环境
```
MacBook-Pro:~ camellia$ cd tensorflow
MacBook-Pro:tensorflow camellia$ source bin/activate
(tensorflow) MacBook-Pro:tensorflow camellia$
```
5.安装  tensorflow
```
$ pip install --upgrade tensorflow      # for Python 2.7
$ pip3 install --upgrade tensorflow     # for Python 3.n
```
6.测试
```
 $ python
 >>> import tensorflow as tf
 >>>
```
7.使用  tensorflow
```
# 打开虚拟环境
MacBook-Pro:tensorflow camellia$ source bin/activate
# 关闭虚拟环境
MacBook-Pro:tensorflow camellia$ deactivate
```






