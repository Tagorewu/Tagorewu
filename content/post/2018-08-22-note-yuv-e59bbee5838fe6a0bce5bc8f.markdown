---
author: wuqt
date: 2018-08-22 12:05:34+00:00
draft: false
title: YUV 图像格式
type: post
url: /
categories:
- image process
---

Y：亮度分量 UV：色度分量  

Y与RGB的演算关系为：Y = 0.2126 R + 0.7152 G + 0.0722 B  

  




	YUV4:2:2或4：2：0都是指的Y分量和UV分量在一个像素点中占有的平均比例。






	YUV422:水平方向上的UV分量减半了



YUV420:水平垂直方向都会减半  

  

YUV 4:4:4采样，每一个Y对应一组UV分量。 eg： YUVYUV  

YUV 4:2:2采样，每两个Y共用一组UV分量。 eg： YUYVYUYV  




	YUV 4:2:0采样，每四个Y共用一组UV分量。 eg: YUYVYUYV YYYY






	  







	  






* * *





	  







	 以黑点表示采样该像素点的Y分量，以空心圆圈表示采用该像素点的UV分量。






	  







	![](/wp-content/uploads/2018/08/20180822194835_82210.jpg)
  


