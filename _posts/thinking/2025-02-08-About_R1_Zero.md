---
title: About DeepSeek R1 & Zero
author: LYK
date: 2025-02-07 00:00:00 +0800
categories: [Thinking, DeepSeek, R1, Zero]
tags: [DeepSeek, R1, Zero]
pin: false
description: About DeepSeek R1 & Zero
---


## Conclusion
1. DeepSeek-R1是完全的新产品吗？  
    - 不是，DeepSeek-R1是公开了OpenAI-O1的创新路径，并且完全开源；  
    - 在中美对立的背景下，DeepSeek-R1低成本打破了OpenAI、Claude的垄断，导致巨大反响  

2. 为什么说DeepSeek-R1-Zero比R1更值得关注？
    - 因为DeepSeek-R1-Zero是完全不需要专家CoT打标的，纯Pre-train + RL, 更适合Scaling
    - 这就证明了更多算力=金钱，可以确定性带来更多智能

3. 智能和CoT长度是完全正相关的吗？
    - 不是，通常智能表现为：知识、经验、泛化智能密度
    - 知识和经验与模型规模呈正相关
    - 泛化智能密度可通过压缩CoT长度来提升， Andrej Karpathy曾提出AGI级别的智能模型可能只需要10B参数，结合大量子模型和Agent实现AGI。

3. 对 DeepSeek 和智能下半场的几条判断
    - 推理时训练更加重要（当然基模也很重要）
    - 压缩CoT长度，可以带来更多泛化智能
    - Scaling up & down, 会同时出现，智能提升（大基模）且尺寸下降（蒸馏小模型）
    - 合并通用模型和推理模型，例如：O1 + GPT4o --> GPT 5, 类似Claude的路径
    - 企业对Agent落地的信心会大大提升，AI公司会加大投入Agent.


## Reference
- [为什么说DeepSeek的R1-Zero比R1更值得关注？]( )
- [对 DeepSeek 和智能下半场的几条判断](https://mp.weixin.qq.com/s/NMW2p8xalh9c8lTLN9B5gg)
- [华人研究团队揭秘：DeepSeek-R1-Zero或许并不存在「顿悟时刻」](https://mp.weixin.qq.com/s/_VK7fm8p3mpfhPh_zBdagA)
- [OpenAI o3-mini 被曝大量使用中文推理，有什么意义？](https://www.zhihu.com/question/11319415340/answer/94111685009)




