---
title: Step-by-Step to Creating A Website Logo With Sketch
date: 2024-08-15 18:30:50
cover_image: /image/cover001.png
cover_image_alt: Create A Logo With Sketch
thumbnail: /image/cover001.png
tags:
    - UI Design
    - ICON
categories:
    - Design
---

## 前言
由于重度拖延症，设计Logo的计划一直没有付诸实践。现在重新开始写博客，我决定以博客的favicon作为第一个设计Logo的尝试。

## 图标网格
### 步骤
1. 创建一个24x24的画布。
2. 使用钢笔工具画出对角线。
3. 绘制一个18x18的正方形和两个16x20的矩形，并将圆角调整为2。
4. 画一个19x19圆，并与矩形对齐。
5. 画两条中线，统一调整宽度为0.05，颜色设为红色。
6. 画一个10x10的小圆。
7. 画两条平行于中线的线，间距为3。
8. 将所有元素编组并重命名为Grid，锁定。

![图标网格](001.png)

## 图标大小
为了保证风格一致，所有图标都在24x24的画布内绘制。如果需要调整大小，不能直接拖动，而是要使用CMD+K按倍数缩放。

## 画图标技巧
1. **对称形状变化**：图标通常是由对称的几何图形变形而来。
2. **锚点调整**：双击或按回车进入锚点模式，可以调整图形的尺寸和位置。
3. **对齐半像素**：在锚点模式中选择对齐半像素。
4. **布尔运算**：使用布尔运算进行图形合并和剪切。

## 实践：画一个文件夹icon
1. 确定图标的视觉重心。
2. 调整矩形的锚点。
3. 将线的宽度调整为1.5。
4. 按住Shift多选锚点并一起拖动。
5. 同时选中两个矩形并按回车，可以批量编辑圆角和调整细节。
6. 拖动一个24x24的矩形，将刚才的两个矩形全选并使用CMD+G编组，确保图标尺寸为24x24。

![文件夹icon](002.png)

### 如何画一个填充色的图标
1. 将刚才的图标复制一份，选中需要填充的区域并添加填充色。

![文件夹填充](003.png)

- **布尔运算的取舍**：根据需要选择合适的布尔运算。
- **图标留白**：图标不要占满整个画布，适当留白。

![图标留白](004.png)

## 练习
### 实践01
#### Tips
- 如果线段无法变成圆角，先选中再选择转化为形状。

![实践01](005.png)

### 实践02
#### Tips
- 使用布尔运算：先用白色填充，然后使用两个矩形相减得到所需形状。
- 如果剪出来的图形有问题，检查复制出来的图形路径是否闭合。

![实践02-无布尔](006.png)
![实践02-有布尔](007.png)

### 实践03：小太阳
编组之后可以按住CMD进行旋转整个编组中的元素
![实践03-小太阳](008.png)
### 实践04：小闪电
两个三角拼起来
![实践04-小闪电](009.png)
### 实践05：聊天icon&Wi-Fi图标
- Wifi图标使用布尔运算：先将三个椭圆进行联集，然后再和矩形进行交集
调出网格编辑
![实践05-小闪电](010.png)
### 实践06：锁&回收站
![实践06](011.png)
## 创建一个网站的favicon
灵感来源于藤原浩的FRAGMENT DESIGN，小闪电图标也很可爱，所以自己画了一个
![favicon](012.png)
## 快捷键
- CMD + OPT + C：复制样式
- CMD + OPT + V：粘贴样式
- CMD + SHIFT + R：旋转
- CMD + K：缩放
- CMD + G: 编组
- CMD + SHIFT + G：取消编组

## 参考资料
- [【图标】Sketch 包教包会的新手画图标教学 - 新像素 UI 设计培训](https://www.bilibili.com/video/BV1V7411N7Dt)
- [阿里巴巴矢量图标库](https://www.iconfont.cn/)
- [Google Material Design](https://m2.material.io/design/iconography/product-icons.html#grid-and-keyline-shapes)
- [Apple Design](https://developer.apple.com/design/human-interface-guidelines/app-icons)