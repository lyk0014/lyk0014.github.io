---
title: AI Novel Blue Print
author: LYK
date: 2024-12-27 19:00:00 +0800
categories: [AIA, AI Novel]
tags: [AI Novel, Thinking, Planning, Finetune, LLM, Prompt Engineering]
pin: false
published: true
description: 技术方案 - 如何使用AI打造一套内容生产系统（网文）
---

# AI小说生成系统蓝图

## 一、AI小说生成关键技术总结

### 1. 故事结构设计
```
1. **MoPS (模块化故事框架)**
- 核心思想：将故事要素模块化分解
- 关键模块：
  * 主题模块：中心思想与主旨
  * 背景模块：时空设定与世界观
  * 人物模块：角色设定与关系网络
  * 情节模块：事件链与冲突设置
- 技术优势：实现故事元素的结构化组合

2. **Dramatron (分层生成框架)**
- 核心思想：自上而下的分层生成
- 技术特点：
  * 多层次分解：从整体到细节
  * 人机协作：关键节点人工干预
  * 迭代优化：层层递进式完善
- 应用价值：平衡自动化与创作质量
```

### 2. 叙事技术创新
```
1. **RecurrentGPT (长文本记忆机制)**
- 技术创新：
  * 模拟LSTM机制的自然语言记忆系统
  * 双层记忆架构：短期+长期记忆
  * 动态信息更新机制
- 解决问题：长篇叙事的连贯性控制

2. **Improving Pacing (节奏控制)**
- 核心技术：
  * 详细度评估器
  * 模糊度优先级排序
  * 动态扩展策略
- 应用效果：优化叙事节奏与细节分配

3. **SWAG & WHAT-IF (情节规划)**
- 创新方法：
  * 故事作为搜索空间
  * 分支叙事设计
  * Action-Space导向
- 技术优势：增强故事的可能性探索
```

### 3. 专项技术突破
```
1. **DOME (知识增强)**
- 技术特点：
  * 知识图谱集成
  * 事实性验证
  * 逻辑关系强化
- 应用价值：提升故事的知识基础

2. **Suspenseful Stories (情感调控)**
- 专项技术：
  * Action-Reason配对
  * 悬疑节点设计
  * 情感张力控制
- 效果提升：增强故事代入感与吸引力
```

### 4. 底层支持技术
```
1. **LongWriter & Weaver (模型训练)**
- 技术重点：
  * 超长文本生成能力
  * 创意写作特化训练
  * 完整训练流程(预训练+SFT+DPO)
- 基础支持：提供模型能力保障

2. **Meta-Prompting & Reflections (提示工程)**
- 技术创新：
  * Fresh-eye机制
  * 多Agent协作
  * 反馈优化循环
- 应用价值：提升生成质量与创新性
```

## 二、整合方案

### 1. 系统架构
```
Input --> 大纲生成 --> 情节规划 --> 文本生成 --> 评估优化 --> Output

核心模块：
1. 大纲生成器(Outline Generator)
2. 情节规划器(Plot Planner) 
3. 文本生成器(Text Generator)
4. 评估优化器(Evaluation Optimizer)
5. 知识增强器(Knowledge Enhancer)
```

### 2. Prompt Engineering方案

#### 2.1 大纲生成
```python
system_prompt = """
你是一个专业的故事大纲规划助手。请基于以下原则生成故事大纲：
1. 使用RecurrentGPT的记忆机制跟踪关键信息
2. 应用Improving Pacing的节奏控制
3. 遵循MoPS的模块化结构(主题/背景/人物/情节)

输出格式：
1. Short-term Memory: [关键信息摘要]
2. Long-term Memory: [相关历史内容]
3. Outline Structure: [分层大纲]
4. Pacing Notes: [节奏控制建议]
"""
```

#### 2.2 情节规划
```python
system_prompt = """
你是一个专业的情节规划助手。请基于以下框架规划故事情节：
1. 使用SWAG的Action Space指导情节发展
2. 应用WHAT-IF的分支叙事设计
3. 遵循MoPS的模块化前提构建
4. 运用Suspenseful Stories的悬疑构建技巧
5. 结合DOME的知识图谱增强情节逻辑性

输出格式：
1. Main Plot: [主要情节线]
2. Branch Options: [分支选项，每个至少3个]
3. Action-Reason Pairs: [动作-原因对]
4. Knowledge References: [相关知识点]
5. Character Development: [角色发展规划]
"""
```

### 3. Finetune方案

#### 3.1 数据准备

```
数据要求：
- SFT/DPO（单任务）：
  - 基于GPT： 最少10条，百条以内
  - 基于开源LLM：最少100条，千条以内
```

##### 1. 特定任务数据构建

```
### 现有任务梳理
#### Phase Ⅰ
1. 抽取章纲: 原文 ---> 章纲 (Claude-20241022)
NewModel: ft:gpt-4o-mini-2024-07-18:crater-pte-ltd:content2outline:Ak9R4oDd:ckpt-step-900

2. 抽取全局大纲: 章纲 ---> 全局大纲 (gpt-4o-2024-11-20)
3. 抽取角色列表: 章纲-出场人物 ---> 角色列表 (Claude-20241022)
4. 抽取人物小传：章纲 + 角色列表 ---> 人物小传 (gpt-4o-2024-11-20)
5. 抽取角色关系: 章纲 + 出场人物 ---> 角色关系 (-)
#### Phase Ⅱ
6. 推导改写大纲：用户创意 + 全局大纲 ---> 改写方案 (Claude-20241022)
7. 生成改写大纲：改写方案 + 全局大纲 ---> 改写大纲 (gpt-4o-2024-11-20)
8. 生成人物小传：改写大纲 + 角色列表 ---> 人物小传 (gpt-4o-2024-11-20)
9. 生成角色关系：改写大纲 + 角色列表 ---> 角色关系 (-)
#### Phase Ⅲ
10. 大纲改写正文：改写大纲 + 人物小传 + 原文 ---> 正文 (Claude-20241022)
11. 推导替代情节：章纲 + 正文 ---> 替代情节方案 (gpt-4o-2024-11-20)
12. 单章替换改写正文：替代情节方案 + 正文 ---> 正文 (Claude-20241022)
13. 生成章纲：正文 ----> 章纲 (Claude-20241022)
#### Phase Ⅳ
13. 推导多章改写方案：用户创意 + 改写大纲 + 正文 ---> 多章改写方案 (gpt-4o-2024-11-20)
14. 生成改写章纲：多章改写方案 + 正文 ---> 章纲 (Claude-20241022)
15. 生成改写正文：改写章纲 + 正文 ---> 正文 (Claude-20241022)
#### Phase Ⅴ
16. 书名生成：大纲 + 市场信息 ---> 书名、封面、Slogan (gpt-4o-2024-11-20)

### Weaver相关任务
1. 生成正文
2. 生成大纲
3. 扩写
4. 润色
5. 精简
6. 改写
7. 风格迁移（仿写）
8. 审校
9. 头脑风暴
10. 起标题
11. 写作相关对话

### SWAG相关任务
1. Action指令生成正文: pre-context + action ---> content
2. Judge & Rank/Chose Action: pre-context + action-sets ---> action
3. 备注：Action限定在一个列表中，30个。示例："add suspense", "add action", "add comedy"...

### Suspenseful相关任务
1. 生成主角可能采取的行动 - Action
2. 对每个行动生成潜在原因导致行动失败 - Reason
3. 利用Action-Reason对生成简短摘要 - Summary
4. 拆分细化摘要为一系列事件 - Event
5. 使用Event生成正文 - Content

### MoPS相关任务
1. 生成故事前提 - Premise
2. 结合Premise生成Storyline - Storyline
3. 情节抽取 - Event
4. 情节分类 - Event-Class

### WHAT-IF相关任务
1. 抽取关键事件 - 三幕式（激励、冲突、结局）
2. 生成元提示？？？
3. 生成新事件序列？？？

### DOC
1. 生成粗略大纲
2. 生成详细大纲
3. 生成故事内容色
5. 精简
6. 改写
7. 风格迁移（仿写）
8. 审校
9. 头脑风暴
10. 起标题
11. 写作相关对话
```

##### 2. 长文本数据集构建
- 参考LongWriter的数据筛选标准
- 重点关注输出长度要求明确的数据
- 保留评分高于80分的优质样本

##### 3. 数据增强
- 使用MoPS方法生成多样化故事前提
- 应用WHAT-IF生成分支情节变体
- 整合多个视角的叙事版本


#### 3.2 训练流程
```
1. **预训练阶段**
- 使用Weaver的200B高质量创意写作数据
- 重点关注小说、短故事等创意文本

2. **SFT阶段**
- 使用筛选后的长文本数据集
- 采用LongWriter的训练参数配置
- 重点优化长文本生成能力

3. **DPO阶段**
- 使用Weaver的Constitutional DPO技术
- 基于写作原则生成正负样本对
- 优化模型与专业作者风格对齐

4. **人机协作优化**
- 设计人工干预接口
- 定义关键决策点
- 建立反馈机制
```

### 4. 评估与优化
```
1. **多维度评估**
- 相关性(Relevance)
- 连贯性(Coherence)
- 共鸣(Empathy)
- 惊喜(Surprise)
- 参与度(Engagement)
- 复杂性(Complexity)

2. **迭代优化**
- 使用Reflections & Resonance的双代理系统
- 一个代理负责生成，另一个负责评估
- 通过反馈循环持续优化输出质量 

3. **Meta-Prompting优化**
- 实施Fresh-eye机制
- 将大任务分解为限定视野的子任务
- 通过任务分解提升生成内容的独特性 
```

## 三、方案实施

### 1. 原文 --> 章纲
- Dataset: train - 450, test - 50
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

### 2. 合并批量改编不走，三步合并为一步
> 原流程：原文 ---> 正文（替换世界观人设） ---> 情节替代方案 ---> 正文（替换情节）  
> 新流程：原文 ---> 正文（替换世界观人设 + 替换情节）  
> 待解决问题:   
> - 经常出现【未完待续】
> - 情节替换不明显或为替换
> - 输出过于精简
> - 输出格式不规范
>

#### V1 - 2024-12-31
- Dataset: train - 23条
- BaseModel: gpt-4o-2024-11-20
- BatchSize: 1
- Epoch: 3
- LR-Multiplier: 1.8
- Seed: 42
- FinalModel: ft:gpt-4o-2024-08-06:crater-pte-ltd:test-gen-content:AkTl2eqB
- Problem: 
  - 输出格式不固定，**可能是Prompt & Output 不配套导致的**
  - 情节替换不明显或为替换，**问题非常明显**
  - 输出过于精简，**有所缓解，但不稳定**
  - 输出格式不规范，**问题非常明显**
- Experience: 效果未达目标，需要优化
  - 重新设计System Prompt、Instruction Prompt、Context、Output Format
  - 增加数据，需要重新校验

#### V2 - 2025-01-02
- Dataset: train - 23条
