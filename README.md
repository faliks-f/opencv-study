OpenCV笔记

=================

## ROI提取

```python
cat = img[0:200, 0:200]
```

## 颜色通道  

```python
img.shape
b, g, r = cv2.split(img)
img = cv2.merge((b, g, r))
# 只保留铜套
img[:,:,0] = 0
img[:,:,1] = 0
```

## 边界填充

```python
top_size, bottom_size, left_size, right_size = (50, 50, 50, 50)
replicate = cv2.copyMakeBorder(img, top_size, bottom_size,left_size, righ_size, method)
```

## 图像阈值

ret, dst = cv2.threshold(src, thresh, maxval, type)  

## 滤波

```python
# 均值滤波，算平均值
blur = cv2.blur(img, (3, 3))
# 方框滤波，和均值一样，可以选择归一化，False时，越界变255
box = cv2.boxFilter(img, -1, (3, 3), normalize=True)
# 高斯滤波，卷积核满足高斯分布，更重视中间
gaussian = cv2.Gaussian(img, (5, 5), 1)
# 中值滤波，排序后用中值替代
median = cv2.medianBlur(img, 5)
```

## 形态学，腐蚀膨胀开闭运算

```python
kernel = np.ones(((5, 5), np.uint8)
# 腐蚀
erosion = cv2.erode(img, kernel, iterations=1)
# 膨胀
dilate = cv2.dilate(img, kernel, inerations=1)
# 开运算：先腐蚀，再膨胀
opening = cv2.morphologyEx(img, cv2.MORPH_OPEN, kernel)
# 闭运算，先膨胀，再腐蚀
closing = cv2.morphologyEx(img, cv2.MORPH_CLOSE, kernel)
# 梯度运算，梯度 = 膨胀 - 腐蚀
gradient = cv2.morphologyEx(img, cv2.MORPH_GRADIENT, kernel)
# 礼帽 = 原始输入 - 开运算
tophat = cv2.morthologyEx(img, cv2.MORPH_TOPHAT, kernel)
# 黑帽 = 闭运算 - 原始输入
blackhat = cv2.morthologyEx(img, cv2.MORPH_BLACKHAT, kernel)
```

## Sobel算子

计算梯度  
dst = cv2.Sobel(src, ddepth, dx, dy, ksize)  
> ddepth: 图像深度  
> dx、dy: 分别表示水平和数值方向  
> ksize: Sobel算子大小
```python
# 左白右黑算出来是负数
# 负数会变为0，可以去绝对值
sobelx = cv2.Sobel(img, cv2.CV_64F, 1, 0, ksize=3)
sobelx = cv2.convertScaleAbs(sobelx)
sobely = cv2.Sobel(img, cv2.CV_64F, 0, 1, ksize=3)
sobely = cv2.convertScaleAbs(sobely)
# x,y求和
sobelxy = cv2.addWeighted(sobelx, 0.5, sobely, 0.5)
# 以下不建议
sobelxy = cv2.Sobel(img, cv2.CV_64F, 1, 1, ksize=3)
sobelxy = cv2.convertScaleAbs(sobelxy)
```

## Scharr算子和Laplacian算子
Scharr算子相比Sobel来说卷积核内的参数值更大，更加敏感  
Laplacian算子不单独使用
```python
charrx = cv2.Scharr(img, cv2.CV_64F, 1, 0, ksize=3)
laplacian - cv2,Laplacian(img, cv2.CV_64F)
```
