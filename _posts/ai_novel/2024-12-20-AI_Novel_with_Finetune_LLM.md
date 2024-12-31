---
title: Processing AI Novel with Finetune LLM
author: LYK
date: 2024-12-26 19:00:00 +0800
categories: [AIA, AI Novel]
tags: [AI Novel, Thinking, Planning, Finetune, LLM, Prompt Engineering]
pin: false
published: true
description: 技术方案 - 如何使用AI打造一套内容生产系统（网文）
---

**项目名**：内容生产系统（网文）  
**目标**：基于训练LLM，打造一套完整且自动化的内容生产系统（网文）  


## 参考论文
## Summary - Papers Worth a Deep Read
共31篇论文，从中筛选13篇值得精读的论文。
### Outline Generation
- DOC：详细大纲控制
- RECURRENTGPT: 递归生成长文本
- Improving Pacing: 改进节奏

### Plot Generation
- MoPS: 结构故事设定，创造情节
- SWAG: 故事情节搜索
- WHAT-IF: 探索分支叙事，互动小说，元提示
- Suspenseful Stories: 悬疑故事

### Long-form Story Generation
- DOME: 长故事生成，叙事框架结合知识图谱
- Dramatron: 分层故事生成, 人工介入

### Training
- LongWriter: 长故事生成, 训练GLM-9B
- Weaver: 写作专属模型

### Prompt Tricks
- Reflections & Resonance: 多Agent，反思与共鸣
- Meta-Prompt: 元提示, Fresh-eye(子任务只能看到有限提示)


## 任务梳理
### 现有任务梳理

#### Phase Ⅰ
1. 抽取章纲: 原文 ---> 章纲
2. 抽取全局大纲: 章纲 ---> 全局大纲
3. 抽取角色列表: 章纲-出场人物 ---> 角色列表
4. 抽取人物小传：章纲 + 角色列表 ---> 人物小传
5. 抽取角色关系: 章纲 + 出场人物 ---> 角色关系
#### Phase Ⅱ
6. 推导改写大纲：用户创意 + 全局大纲 ---> 改写方案
7. 生成改写大纲：改写方案 + 全局大纲 ---> 改写大纲
8. 生成人物小传：改写大纲 + 角色列表 ---> 人物小传
9. 生成角色关系：改写大纲 + 角色列表 ---> 角色关系
#### Phase Ⅲ
10. 大纲改写正文：改写大纲 + 人物小传 + 原文 ---> 正文
11. 推导替代情节：章纲 + 正文 ---> 替代情节方案
12. 单章替换改写正文：替代情节方案 + 正文 ---> 正文
#### Phase Ⅳ
13. 推导多章改写方案：用户创意 + 改写大纲 + 正文 ---> 多章改写方案
14. 生成改写章纲：多章改写方案 + 正文 ---> 章纲
15. 生成改写正文：改写章纲 + 正文 ---> 正文
#### Phase Ⅴ
16. 书名生成：大纲 + 市场信息 ---> 书名、封面、Slogan

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
3. 生成故事内容



## Reference
- 
