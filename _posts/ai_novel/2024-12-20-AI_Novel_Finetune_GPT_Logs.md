---
title: Experiment - Finetune GPT for AI Novel
author: LYK
date: 2025-01-06 19:00:00 +0800
categories: [AIA, AI Novel]
tags: [AI Novel, Thinking, Planning, Finetune, LLM, GPT]
pin: false
published: true
description: 实验记录：Finetune GPT 实现小说改写相关任务
---

**项目名**：内容生产系统（网文）  
**目标**：基于训练LLM，打造一套完整且自动化的内容生产系统（网文）  


## Gen Chapter Outline
### Experiment 1
- BaseModel: gpt-4o-mini-2024-07-18
- BatchSize: 1
- Epoch: 3
- LR-Multiplier: 1.8
- Seed: 42
- FinalModel: ft:gpt-4o-mini-2024-07-18:crater-pte-ltd:content2outline:Ak9R4oDd:ckpt-step-900
- Experience: 效果可用，速度明显提升
  - 两个Epoch刚好，三个Epoch开始过拟合，可以尝试降低LR或减少Epoch
  - 数据集的多样性有待提升，可以增加更多英文数据，以及人工数据校准
  - 可以进一步优化输出格式（使用SOTA LLM来清洗数据格式）

### Experiment 2
- Date: 2025-01-09
- BaseModel: ft:gpt-4o-mini-2024-07-18:crater-pte-ltd:content2outline:Ak9R4oDd:ckpt-step-900
- BatchSize: 4
- Epoch: 1
- LR-Multiplier: 0.01
- Seed: 1120173883
- FinalModel: -
- Optimizer Schema:
  - 清洗数据：清除XML格式不正确样本，train: 450 --> 446, test: 50 --> 45
- Experience: 


## Gen Rewrite Chapter Outline
### Experiment 1
- version: rewrite-step1-gen-outline-v1
- BaseModel: gpt-4o-mini-2024-07-18
- Epoch: 3
- BatchSize: 2
- LR-Multiplier: 1.8
- Dataset: 
  - train: 48
  - test: 6
  - val: 6
- BestModel: 
  - (过拟合) ft:gpt-4o-mini-2024-07-18:crater-pte-ltd:rewrite-step1-gen-outline-v1:Amylq85M
  - (欠拟合) ft:gpt-4o-mini-2024-07-18:crater-pte-ltd:rewrite-step1-gen-outline-v1:AmylqkI1:ckpt-step-48
- Elapsed time: 59s
- Conclusion: 效果可用，达到预期85%
- Imperfection
  - 3个Epoch的模型过拟合，无法再接受其他指令，但对当前指令的执行效果很好
  - 2个Epoch的模型欠拟合，能够达到预期，也能接受其他指令；但最终格式还有一点瑕疵
- Next Step
  - 减少过拟合：增加更多数据，可增加多样性
  - 减少欠拟合：batch_size可以适当增加 --> 3/4, 使训练更稳定
  - 提升格式稳定性：在最终输出后，增加Ending text，更符合模型惯性
  - 注意：如果数据增加较多，可考虑适度降低Epoch --> 2, batch_size --> 4


## Gen Rewrite Chapter Outline & Content
### Experiment 1
- version: rewrite-step2-gen-content-v1
- BaseModel: gpt-4o-mini-2024-07-18
- Epoch: 3
- BatchSize: 2
- LR-Multiplier: 1.8
- Dataset: 
  - train: 96
  - test: 12
  - val: 12
- BestModel: 
  - (过拟合) ft:rewrite-step1-gen-outline-v1
  - (欠拟合) ft:rewrite-step1-gen-outline-v1:ckpt-step-48
- Elapsed time: Step1: 50s, Step2: 60s
- Conclusion: 效果可用，并且能够实现两个阶段任务，能够在第一轮停止，并接受第二轮指令，超出V1预期；最终效果85%
- Imperfection
  - 章纲的格式还有一点瑕疵，需要分析是存在脏数据，还是欠拟合
  - 内容输出格式不正确，总是缺少</final_content>
- Next Step
  - 批量校验数据格式是否存在异常，特别是章纲的格式
  - 尝试增加一本新书20章的数据，总数据量由120 --> 140
  - batch_size可以适当增加 --> 3/4, 使训练更稳定
  - 提升格式稳定性：在最终输出后，增加Ending text，更符合模型惯性
  - 混合任务的效果优于单任务，可以增加对比实验确认
