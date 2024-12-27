---
title: Intro Text2Image models which use MLLM (Flux_QWEN2VL)
author: LYK
date: 2024-12-04 19:00:00 +0800
categories: [Reading, Paper]
tags: [MLLM, Text2Image, Qwen, Qwen2VL, Qwen, Flux]
pin: false
published: true
description: So so excited to see the progress of Text2Image models which use MLLM. Expecting since 2023!!!
---

## Intro

非常高兴看到类似这样文章，LLM + DiT.  
2023年 ~ 2024年Q2，一直找类似的模型，因为LLM已经很成功了；但是SD、SDXL、SD3、SD3.5、Flux仍然抱着CLIP、T5不放。
这些文生图模型最难的问题不是生图不好看，而是生图和用户想要的不一致。  
不知道为什么没有人做这样的尝试？成本太高？不好训？
很高兴目前看到了希望：  
- 202409 Playground V3: [Playground v3: Improving Text-to-Image Alignment with Deep-Fusion Large Language Models](https://arxiv.org/abs/2409.10695)
- 202411 Qwen2VL-Flux: [https://github.com/erwold/qwen2vl-flux/blob/main/technical-report.pdf](https://github.com/erwold/qwen2vl-flux/blob/main/technical-report.pdf)

> Playground V3 已单独成篇，值得一看！[跳转链接](https://lyk0014.github.io/posts/Playground_v3/)

## 要解决什么问题？
文生图模型从2022年开始已经非常成功了，从娱乐向逐渐转向了生产向。  
但一直面临着**两个重大挑战**： 
1. 在整合参考图像时保持语义一致性和风格保真度
2. 生成过程提供细粒度控制  

**主要原因**：文本编码器的多模态理解能力有限，它们难以有效地弥合文本描述和视觉特征之间的语义差距

为了解决这个问题，有**两个成熟的方向**：
1. 专注于提高文本编码器的能力，像Stable Diffusion和FLUX.1这样的模型结合了T5-XXL和CLIP以获得更好的文本理解
2. 探索基于参考的生成，通过各种控制机制，如空间条件或注意力操作   
这两个方向存在一些缺点：
1. 需要对额外的控制模块进行大量的训练
2. 在保持风格一致性的同时无法实现精确的语义迁移   

本文提出一个新的方向 - 利用**视觉-语言模型来替换传统的文本编码器**，有两个出发点：
1. VLMs在统一的语义空间中理解文本和视觉，可以增强模型理解和生成图像的能力
2. VLMs的注意力及时可以用于对生成过程进行细粒度控制，实现对风格和语义元素的精确控制

## 如何实现的？

### Unified Multi-modal Architecture
统一的多模态架构，主要是三大组件：
- Image&Text Encoder: Qwen2VL - 7B
- Connector: 链接VLM 和 图像生成之间的语义差距
- Image Generator：Flux

![qwen2vl_flux.jpg](https://cloudflare-imgbed-4vb.pages.dev/file/1733471501425_qwen2vl_flux.jpg)


### Zero-shot Style Variation
统一多模态架构的一个关键优势：可以在没有文本提示的情况下执行零样本风格迁移。  

大致**流程**：
1. 提供参考图像
2. 通过Qwen2VL编码器处理图像以提取与风格相关的特征。
3. 使用连接网络将这些特征投影到FLUX变换器的空间中。
4. 在通过不同的噪声种子变化内容的同时，使用提取的风格信息指导生成过程。

这允许模型生成多样化的变化，保留参考图像的关键风格元素，如色彩调色板、光照和艺术技巧，同时探索不同的构图可能性。

**Question：**
- 训练数据是怎么构建的呢？
- 没有风格提示词？输入的提示语是怎样的？

### GridDot Panel for Semantic-Aware Generation
继承了ControlNet的功能：
- depth-anything-v2
- mistoline
- segment-anything-v2
  
**实现用户操控注意力权重，实现精准控制？【没理解】**

## 实验配置




### 模型配置

```

模型基于FLUX.1-dev作为基础架构，将其T5-XXL文本编码器替换为Qwen2VL-7B。连接网络由一个线性投影层组成，该层将Qwen2VL的3584维隐藏空间映射到FLUX的4096维空间。对于不同组件采用分阶段训练策略：

1. Qwen2VL：Qwen2VL的最后一层（k=1）被设置为可训练，以微调高级视觉理解，同时保持大部分预训练权重不变。
2. FLUX变换器：对于FLUX变换器块采用三阶段渐进式训练方法：
   - 第一阶段 - Flux变换器：可训练层：[1, 4, 7, 12, 16, 19, 24, 28, 32, 37, 40, 44, 48, 52, 56]
   - 第二阶段 - Flux变换器：可训练层：[0, 3, 6, 9, 14, 16, 21, 26, 30, 34, 39, 42, 46, 50, 54, 56]
   - 第三阶段 - Flux变换器：可训练层：[2, 5, 8, 11, 15, 18, 23, 25, 29, 33, 36, 41, 45, 49, 53, 55]
3. 连接网络：在整个训练阶段中，连接网络保持可训练，以维持有效的特征空间桥接。

```

### 训练程序

```

数据处理：遵循FLUX的做法，训练期间支持多种宽高比，包括16:9、2:1、2.35:1、1:1和1:2。图像在保持其宽高比的同时自动调整大小，最大尺寸为1024像素。在每个训练步骤中，一批参考图像通过Qwen2VL和FLUX管道处理。

优化：模型使用AdamW优化器进行训练，使用以下超参数：
- 学习率：FLUX层、连接网络和Qwen2VL层均为1e-5
- 批次大小：128（分布在8个GPU上）
- 梯度累积步骤：2
- 权重衰减：0.1
- Adam β1, β2 = (0.9, 0.95)

学习率遵循一个梯形计划，包括一个短暂的热身期和线性衰减。

训练目标：保持基于修正流匹配的原始FLUX训练目标。
```

### 训练设备

```
模型在8个NVIDIA A100 GPU上进行训练，每个GPU具有80GB内存。为了优化内存使用，采用了以下技术：

- 混合精度训练与bfloat16
- 在Qwen2VL和FLUX中使用梯度检查点

在作者策划的300万高质量图像数据集上，训练运行100K次更新，大约需要1个月的时间在硬件配置上完成。

```



## Reference



