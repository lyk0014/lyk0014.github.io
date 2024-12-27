---
title: Paper Template
author: LYK
date: 2029-01-01 00:00:00 +0800
categories: [Reading, Paper]
tags: [Template]
pin: false
published: false
description: Template
---

# Intro

## Intro  
> 论文：[LLaVA-o1: Let Vision Language Models Reason Step-by-Step](https://arxiv.org/abs/2411.10440)  

LLaVA-o1，介一个新颖的视觉语言模型, 能够进行多阶段的结构化和自主推理。  
推理过程：总结、标题、推理、结论。  
主要贡献：
1. 创建了带有详细推理注释的LLaVA\-o1\-100k数据集，支持<b>系统化</b>、<b>结构化</b>响应的训练。
2. 提出了一种阶段性beam搜索方法（推理时）  

结论：（11B）超越了更大甚至闭源模型的性能（Gemini-1.5、GPT-4o-mini、LLaMA-3.2-90B-Vision-instruct）


## Question List
- [x] Q1: 什么是视觉语言模型（Vision-Language Models, VLMs）?
- [x] Q2：常见的VLMs有哪些？
- [x] Q3：针对VLMs，常见的评测方法有哪些？
- [x] Q4：LLaVA-o1有哪些亮点？解决了什么问题？
- [x] Q5：LLaVA-o1的主要方法是什么？
- [x] Q6：LLaVA-o1-100k数据集的构建目标、标准、方法是什么？怎么评价数据量是否足够？
- [x] Q7：下一步需要解决的问题？或努力的方向？


## Q1：什么是视觉语言模型（Vision-Language Models, VLMs）?
VLM (Vision-Language Model) 指讲视觉和语言信息融合，通过模型学习实现跨模态交互和推理的技术。
主要用于视觉推理或视觉表征。
区别于传统的计算机视觉模型，VLM不受固定类别集或特定任务（如分类或检测）约束，
它们在大量文本和图像标题对的语料上进行预训练，使其能够以自然语言为指示，并泛化至几乎任何类型的视觉任务。
后续延伸到MLLM (Multimodal-Large-Language-Model)，可以支持语音、图像、文本、视频等多模态输入；输出文本、图像、视频等多模态内容。



## References
1. [66個視覺語言模型VLM經典論文](https://tomohiroliu22.medium.com/66%E5%80%8B%E8%A6%96%E8%A6%BA%E8%AA%9E%E8%A8%80%E6%A8%A1%E5%9E%8Bvlm%E7%B6%93%E5%85%B8%E8%AB%96%E6%96%87-f44f280a7f62)





