![header.png](https://z3.ax1x.com/2021/07/02/R6Ih28.jpg)

# 带带弟弟OCR通用验证码识别SDK免费开源版
# asdjjfhjfurhcjr
# 今天ddddocr又更新啦！
  当前版本为1.3.1

  想必很多做验证码的新手，一定头疼碰到点选类型的图像，做样本费时费力，神经网络不会写，训练设备太昂贵，模型效果又不好。

  市场上常见的点选类验证码图片如下图所示


 ![Test](https://cdn.wenanzhe.com/img/0446fe794381489f90719d5e0506f2da.jpg) 

 ![Test](https://cdn.wenanzhe.com/img/6175e944c1dc408a89aabe4f7fc07fca.jpg) 

 ![Test](https://cdn.wenanzhe.com/img/20211226135747.png) 

  ![Test](https://cdn.wenanzhe.com/img/f34390d4911c45ce9058dc2e7e9d847a.jpg) 

  那么今天，他来了，ddddocr带着重磅更新大摇大摆的走来了。
# 简介
  ddddocr是由sml2h3开发的专为验证码厂商进行对自家新版本验证码难易强度进行验证的一个python库，其由作者与kerlomz共同合作完成，通过大批量生成随机数据后进行深度网络训练，本身并非针对任何一家验证码厂商而制作，本库使用效果完全靠玄学，可能可以识别，可能不能识别。

  ddddocr奉行着开箱即用、最简依赖的理念，尽量减少用户的配置和使用成本，希望给每一位测试者带来舒适的体验

项目地址： [点我传送](https://github.com/sml2h3/ddddocr) 

# 更新说明

  本次更新其实分为两部分，其中有一部分是在1.2.0版本就已经更新了，但是在这里还是有必要提一下的。

## 第一部分 OCR识别部分

  在1.2.0开始，ddddocr的识别部分进行了一次beta更新，主要更新在于网络结构主体的升级，其训练数据并没有发生过多的改变，所以理论上在识别结果上，原先可能识别效果的很好的图形在1.2.0上有一小部分概率会有一定程度的下降，也有可能原本识别不好的图形在1.2.0之后效果却变得特别好。
  测试代码：
   

```python
import ddddocr

ocr = ddddocr.DdddOcr()

with open("test.jpg", 'rb') as f:
    image = f.read()

res = ocr.classification(image)
print(res)
``` 
由于事实上确实在一些图片上老版本的模型识别效果比新模型好，特地这次更新把老模型也加入进去了，通过在初始化ddddocr的时候使用old参数即可快速切换老模型

```python
import ddddocr

ocr = ddddocr.DdddOcr(old=True)

with open("test.jpg", 'rb') as f:
    image = f.read()

res = ocr.classification(image)
print(res)
``` 

  OCR部分应该已经有很多人做了测试，在这里就放一部分网友的测试图片。

   ![Test](https://cdn.wenanzhe.com/img/20210715211733855.png) 
   ![Test](https://cdn.wenanzhe.com/img/78b7f57d-371d-4b65-afb2-d19608ae1892.png) 
  ![Test](https://cdn.wenanzhe.com/img/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20211226142305.png) 
   ![Test](https://cdn.wenanzhe.com/img/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20211226142325.png) 
   ![Test](https://cdn.wenanzhe.com/img/2AMLyA_fd83e1f1800e829033417ae6dd0e0ae0.png) 
   ![Test](https://cdn.wenanzhe.com/img/aabd_181ae81dd5526b8b89f987d1179266ce.jpg) 
   ![Test](https://cdn.wenanzhe.com/img/2bghz_b504e9f9de1ed7070102d21c6481e0cf.png) 
   ![Test](https://cdn.wenanzhe.com/img/0000_z4ecc2p65rxc610x.jpg) 
   ![Test](https://cdn.wenanzhe.com/img/2acd_0586b6b36858a4e8a9939db8a7ec07b7.jpg) 
  ![Test](https://cdn.wenanzhe.com/img/2a8r_79074e311d573d31e1630978fe04b990.jpg) 
   ![Test](https://cdn.wenanzhe.com/img/aftf_C2vHZlk8540y3qAmCM.bmp) 
   ![Test](https://cdn.wenanzhe.com/img/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20211226144057.png) 
等等更多图片等你测试哟~

## 第二部分 目标检测部分
  在本次1.3.0的更新中，目标检测部分隆重登场！
  目标检测部分同样也是由大量随机合成数据训练而成，对于现在已有的点选验证码图片或者未知的验证码图片都有可能具备一定的识别能力，适用于文字点选和图标点选。
  简单来说，对于点选类的验证码，可以快速的检测出图片上的文字或者图标。
  

```python
import ddddocr
import cv2

det = ddddocr.DdddOcr(det=True)

with open("test.jpg", 'rb') as f:
    image = f.read()

poses = det.detection(image)
print(poses)

im = cv2.imread("test.jpg")

for box in poses:
    x1, y1, x2, y2 = box
    im = cv2.rectangle(im, (x1, y1), (x2, y2), color=(0, 0, 255), thickness=2)

cv2.imwrite("result.jpg", im)

```

举些例子：

 ![Test](https://cdn.wenanzhe.com/img/page1_1.jpg) 
   ![Test](https://cdn.wenanzhe.com/img/page1_2.jpg) 
   ![Test](https://cdn.wenanzhe.com/img/page1_3.jpg) 
   ![Test](https://cdn.wenanzhe.com/img/page1_4.jpg) 
   ![Test](https://cdn.wenanzhe.com/img/result.jpg) 
  ![Test](https://cdn.wenanzhe.com/img/result2.jpg) 
  ![Test](https://cdn.wenanzhe.com/img/result4.jpg) 

以上只是目前我能找到的点选验证码图片，做了一个简单的测试。

# 安装

## 环境支持

`python <= 3.9`

`Windows/Linux/Macos..`

暂时不支持Macbook M1(X)，M1(X)用户需要自己编译onnxruntime才可以使用

## 安装命令

`pip install ddddocr`

以上命令将自动安装符合自己电脑环境的最新ddddocr

# 交流群 （加我好友拉你进群）

 ![Test](https://cdn.wenanzhe.com/img/mmqrcode1640418911274.png!/scale/50) 


   
