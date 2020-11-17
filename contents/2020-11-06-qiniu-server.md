---
title: 图床使用七牛云图片处理
abstract: 图床使用, 七牛云图片处理、费用、接口使用
tags:
  - hexo
abbrlink: 图床使用
date: 2020-11-06 11:54:23
---
## 七牛图片存储服务

<p align='center'>
<img src='https://www-static.qbox.me/_next/static/media/qiniu_logo.2e53bd892f3e58652902d56baa38624b.svg'>
</p>

### 智能多媒体服务(图片处理)

[七牛文档介绍](https://www.qiniu.com/prices/dora?source=dora&ref=developer.qiniu.com)

| 多媒体处理服务/月 | 部分图片处理服务/月 | CDN/月                    |
| ----------------- | ------------------- | ------------------------- |
| 20 RMB 额度       | 20 TB 额度          | 10 G 额度 HTTPS不计入免费 |

20 TB/月 免费服务内容包括:

1. [imageView2（图片基本处理）](https://developer.qiniu.com/dora/api/1279/basic-processing-images-imageview2) imageView2 提供简单快捷的图片格式转换、缩略、剪裁功能。只需要填写几个参数，即可对图片进行缩略操作，生成各种缩略图。`imageView2`接口可支持处理的原图片格式有`psd`、`jpeg`、`png`、`gif`、`webp`、`tiff`、`bmp`。（webp不支持动图）
2. [imageMogr2（图片高级处理）](https://developer.qiniu.com/dora/api/1270/the-advanced-treatment-of-images-imagemogr2)imageMogr2 提供一系列高级图片处理功能，包括格式转换、缩放、裁剪、旋转等。imageMogr2 接口可支持处理的原图片格式有 `psd`、`jpeg`、`png`、`gif`、`webp`、`tiff`、`bmp`。（webp不支持动图）
3. imageInfo（图片基本信息）: 图片基本信息包括图片格式、图片大小、色彩模型。
   在图片下载URL后附加`imageInfo`指示符（区分大小写），即可获取JSON格式的图片基本信息。
4. exif（图片 EXIF 信息）: 专门为数码相机的照片设定的可交换图像文件格式，通过在图片下载URL后附加`exif`指示符（区分大小写）获取。
5. [watermark（图片水印处理）](https://developer.qiniu.com/dora/api/1316/image-watermarking-processing-watermark) 提供四种水印接口：[图片水印](https://developer.qiniu.com/dora/api/1316/image-watermarking-processing-watermark#pic-watermark)、[文字水印](https://developer.qiniu.com/dora/api/1316/image-watermarking-processing-watermark#text-watermark)，[文字平铺水印](https://developer.qiniu.com/dora/api/1316/image-watermarking-processing-watermark#text-Tile-watermark)、[混合水印](https://developer.qiniu.com/dora/api/1316/image-watermarking-processing-watermark#multi-watermark)。
6. roundPic（图片圆角）roundPic 将图片生成圆角图片，并且可以指定图片的圆角大小。这个接口支持的原图片格式有`png`、`jpg`，处理后的图片格式为`png`。

20 RMB/月 付费内容:

1. [imageslim（图片瘦身）](https://developer.qiniu.com/dora/api/1271/image-thin-body-imageslim) 0.1 元/千次 20W次/月
2. [盲水印](https://developer.qiniu.com/dora/api/5915/blind-watermarking-processing) 盲水印-添加-提取 1 元/千次 2W次/月
3. imageAve（图片主色调）: 在图片下载URL后附加`imageAve`指示符, imageAve接口用于计算一幅图片的平均色调，并以`0xRRGGBB`形式返回。0.1 元/千次 20W次/月

- [ ] 使用图片瘦身接口, 替换[TinyPNG4Mac](https://github.com/kyleduo/TinyPNG4Mac) 的接口
