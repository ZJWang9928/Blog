---
title: "OpenCV 与 PIL.Image 之间的图像通道（channel）转换"
date: 2021-05-14T19:46:19+08:00
categories: ["Alchemy Of CV"]
tags: ["cv", "notes", "deep learning", "nvidia", "PIL", "python"]
draft: false
---

今天有学弟提到了一个打开图像并显示时图像色调变蓝的问题，经历一番周折后最终解决，在此记录。  

问题的根本原因在于 OpenCV 默认是以 `BGR` 的通道顺序打开和显示图像的，而 PIL.Image 采用的通道顺序是 `RGB`，故此若将两个库的打开与显示混用就有可能导致上述问题的出现。  

为了解决这一问题，可以考虑转换通道，具体方法如下。  

## OpenCV 转 PIL.Image 

```
cv2_img = cv2.imread("test.jpg")
cv2.imshow("cv2_img", cv2_img)
cv2.waitKey(0)
pil_img = Image.fromarray(cv2.cvtColor(cv2_img, cv2.COLOR_BGR2RGB))
pil_img.show()
```

## PIL.Image 转 OpenCV

```
pil_img = Image.open("test.jpg")
pil_img.show()
cv2_img = cv2.cvtColor(np.asarray(pil_img), cv2.COLOR_RGB2BGR)
cv2.imshow("cv2_img", cv2_img)
cv2.waitKey(0)
```
