---
title: Overview of LLMs (most of 2024)
author: LYK
date: 2025-01-06 00:00:00 +0800
categories: [Reading, Paper]
tags: [LLM]
pin: false
published: true
description: Overview of LLMs (most of 2024)
---

# Intro
2024年，LLMs领域涌现了大量优秀的论文，本文将介绍2024年LLMs领域的一些重要论文，并对其进行简要的总结和评价。


## Cut Your Losses in Vocabulary
> 论文：[Cut Your Losses in Large-Vocabulary Language Models](https://arxiv.org/abs/2411.09009)  
> 2024-11  

**问题**：
- 大语言模型训练中交叉熵损失计算占用内存过大(约90%)
- 传统方法需要存储所有token的logits，造成内存浪费

**假设**：
- 通过只计算正确token的logit可以减少内存占用
- 利用softmax计算的稀疏性可以优化梯度计算

**方法**(Cut Cross-Entropy, CCE):
- 动态计算logits而非全量存储
- 自定义CUDA内核在SRAM中进行矩阵运算
- 跳过对梯度贡献微小的元素计算 (利用fp16最小值导致softmax结果中大部分严格等于0来实现的)

**效果**：
- 将交叉熵计算内存从24GB降至1MB
- 保持训练速度的同时显著减少内存占用
- 支持更大batch size的训练
- 在多个大模型上验证有效性


## Chain-of-Thought Reasoning Without Prompting
> 论文：[Chain-of-Thought Reasoning without Prompting](https://arxiv.org/abs/2402.10200)  
> 2024-02  

这篇论文的主要内容可以从以下五个方面总结：

**问题**：
- Prompt Engineering: 编码人类先验知识，来激发LLM的推理能力；需要不断经验以及不断调试
- Finetune: 成本较高
- 挑战现有观点:LLM在没有提示的情况下无法进行有效推理

**假设**：
- LLM可能具有内在的推理能力,只是在标准贪婪解码下无法体现
- 通过改变解码过程,可以激发模型的链式思维(CoT)推理能力
- 当存在CoT推理路径时,模型对最终答案会表现出更高的置信度

**方法**：
- 使用标准问答格式作为输入:"Q: [question]\nA:"
- 考虑top-k token中的替代解码路径,而不仅仅依赖贪婪解码
- 开发了CoT解码方法,基于答案置信度来筛选最可靠的解码路径

```
对比之下，当探索首次解码步骤中备选 top-k (k>0) 个 Token 时，出现了一个有趣的现象。<br> 
从这一点继续使用贪婪解码，在许多情况下会揭示自然的链式思维 (CoT) 推理。<br> 
这些发现表明，大型语言模型在预训练后就拥有许多任务的内在推理能力，但这些能力却被贪婪解码的普遍使用所掩盖。<br> 
通过结合替代解码路径，可以很容易地发现这些推理路径。
```

**效果**：
- 在没有提示的情况下,通过修改解码过程成功激发了模型的CoT推理
- 在数学和常识推理等任务中表现良好
- 比贪婪解码有显著改进

**结论**：
- LLM具有内在的推理能力,无需依赖复杂的提示技术
- 现有提示方法主要是将内在推理路径引导为顶级解码路径
- 对于复杂的人工合成任务,CoT路径较少出现,此时少样本提示起到"教学"作用
- 该方法可以更好地评估模型的真实推理能力,避免人类先验知识的干扰

## Evaluating the role of `Constitutions' for learning from AI feedback
> 论文：[Evaluating the role of `Constitutions' for learning from AI feedback](https://arxiv.org/abs/2411.10168)  
> 2024-11  

```
大型语言模型（LLMs）日益增强的能力使其被用作替代人类反馈的工具，以训练和评估其他LLMs。 <br> 
这些方法通常依赖于“宪法”，即批评模型用来提供反馈和改进生成内容的书面指南。<br> 
我们通过使用四种不同的宪法来改善医疗访谈中的以患者为中心的沟通，研究宪法选择如何影响反馈质量。<br> 
在215名人工评审员进行的成对比较中，我们发现详细的宪法在情感特质方面产生了更好的结果。<br> 
然而，没有任何宪法在学习与信息收集和提供相关的更实用技能方面超越基线。<br> 
我们的研究结果表明，尽管应优先考虑详细的宪法，但在某些领域，AI反馈作为奖励信号的有效性可能存在局限性。
```


## FPO: Feature-Level Preference Optimization
> 论文：[Direct Preference Optimization Using Sparse Feature-Level Constraints](https://arxiv.org/abs/2411.07618)
> 2024-11

```
大型语言模型（LLMs）与人类偏好的对齐仍然是一个关键挑战。<br> 
尽管后训练技术如基于人类反馈的强化学习（RLHF）和直接偏好优化（DPO）取得了显著成功，但它们往往引入计算效率低下和训练不稳定的问题。<br> 
本文提出了一种特征级约束偏好优化（FPO）的方法，旨在简化对齐过程，同时确保稳定性。<br> 
FPO利用预训练的稀疏自编码器（SAEs），并引入特征级约束，实现高效的稀疏性强制对齐。<br> 
我们的方法通过使用在经过良好训练的稀疏自编码器中激活的稀疏特征，结合特征级离线参考的序列KL散度质量，享有高效性。<br> 
基准数据集上的实验结果表明，与最先进的基线相比，FPO在胜率上实现了5.08%的绝对提升，同时计算成本大幅降低，使其成为高效且可控的LLM对齐的有前景的解决方案。
```

## Reverse Thinking Makes LLMs Stronger Reasoners
> 论文：[Reverse Thinking Makes LLMs Stronger Reasoners](https://arxiv.org/abs/2411.19865)
> 2024-11

```
逆向思维在人类大脑中占据了重要地位，我们不仅可以从问题推导到答案，也可以从答案回溯到问题。<br> 
想想我们做数学题，当算出答案后，我们是不是经常把答案代入题目中进行验证，而并非重新推导一次。<br> 
这种思维往往能增强整体的推理能力，提升推理过程整体的性能。
<br> 
<br> 
为了让大模型具备逆向思维的能力，
研究人员提出了Reverse-Enhanced Thinking(RevThink)，一种数据增强策略跟新的训练范式，考虑到了逆向推理的相关过程，在多个数据集上有更好的表现跟更好的数据效率。
<br> 
<br> 
其实核心是构建增强推理能力的样本，在Phi-4中应该应用了很多。
```

## Procedural Knowledge in Pretraining Drives Reasoning in Large Language Models
> 论文：[Procedural Knowledge in Pretraining Drives Reasoning in Large Language Models](https://arxiv.org/abs/2411.12580)
> 2024-11

```
interesting, skip temporarily
```

## Coconut
> 论文：[Training Large Language Models to Reason in a Continuous Latent Space](https://arxiv.org/abs/2412.06769)
> 2024-12

```
研究动机：<br>
由于现实世界的复杂性，模型需要思维链过程（CoT）已经是不争的事实。在GPT3以前的模型中，研究者普遍认为transformer模型具备了抽象出思维过程的能力，简单来说就是当输入为复杂问题时，模型应当有能力直接给出最终答案而无需额外的思考过程。这一假设的基础是transformer模型的图灵完备性，即可以拟合任何无事实矛盾的x->y集合的映射关系。在GPT3出现之后，研究者发现基于预训练的模型在给出适当的思维过程后模型效果会有质的提升，即模型本身对上下文（Context）、输入提示（Prompt）、Token之间的输出逻辑关系具有更苛刻的要求。因此基于Context的0-shot、1-shot等方式，更合理且严格的prompt engineering，以及代表token之间逻辑关系的CoT被越来越多的应用出来。例如最新推出的GPT4-o1系列模型，其推理的逻辑更长，且使用了一定的PRM数据（Process Reward Model）。


研究方法：
训练时，采用课程学习的思想，分成N+1个阶段。<br>
 - 初始阶段按照最原始的language的cot方式训练，
 - 然后在其中增加c个连续的hidden-state替换一个cot，
 - 逐渐替换掉所有的cot。

注意文章着重指出训练过程中mask掉了latent continuous thought的loss，这不是一种对cot的压缩，而是希望学到一种更合适的cot方式     

在推理时，可以通过训练一个二分类器来自动判断什么时候结束latent reasoning，也可以设置一个固定长度。本文发现这两种方式性能差不多，为了简化直接使用第二种 (类似Prefix-tuning)
```


## Are Your LLMs Capable of Stable Reasoning?
> 论文：[Are Your LLMs Capable of Stable Reasoning?](https://arxiv.org/abs/2412.13147)
> 2024-12
> 评估LLM的推理稳定性，可以用于评估微调模型

```
大型语言模型（LLMs）的快速发展在复杂推理任务中展示了显著的进展。<br> 
然而，基准性能与实际应用之间仍然存在显著差距。我们认为这一差距主要源于当前的评估协议和指标，这些指标未能充分捕捉LLM能力的全貌，特别是在复杂推理任务中，准确性和一致性至关重要。<br> 
本研究做出了两个关键贡献。首先，我们引入了G-Pass@k，这是一种新颖的评估指标，提供了模型性能在多次采样尝试中的连续评估，量化了模型的峰值性能潜力及其稳定性。其次，我们提出了LiveMathBench，这是一个动态基准，包含具有挑战性的当代数学问题，旨在最小化评估过程中的数据泄露风险。<br> 
通过在最先进的LLM上使用G-Pass@k和LiveMathBench进行广泛实验，我们提供了对其最大能力和操作一致性的全面洞察。<br> 
我们的研究结果揭示了LLM在“现实”推理能力方面有显著的改进空间，强调了更强健评估方法的必要性。<br> 
```

## Compressed Chain of Thought: Efficient Reasoning Through Dense Representations
> 论文：[Compressed Chain of Thought: Efficient Reasoning Through Dense Representations](https://arxiv.org/abs/2412.13171)
> 2024-12

```
没懂，先跳过
```

## Let's verify step by step
> 论文：[Let's verify step by step](https://arxiv.org/abs/2305.20050)
> 2023-05
> OpenAI

```
Let’s Verify Step by Step是OpenAI的verifier续作，发表在ICLR 2024.
作者对比了两种训练verifier的方式: ORM(Outcome-supervised reward models) vs PRM(process-supervised reward models).

关于verifier，最简单的理解方式是把它看作ranker(排序模型)，
当generator生成多个候选solution(CoT形式)，verifier对它们进行打分，
此外，还可以从强化学习的角度把verifier看作reward model (把训练verifier类比为LLM post-training中的training reward model)。

当我们从RL角度看待generator和verifier时，就容易理解outcome和process了:
- generator用于生成CoT, 这是一个token序列，如果问题很复杂，序列会很长，
- 然后verifier对solution打分，这个分数就是reward，
这种verifier就是outcome-supervised，用来监督整个CoT序列是否正确，由于一个序列只对应一个reward，明显是sparse reward，能不能用某种dense reward监督CoT序列的中间步骤呢？

这就是process-supervised reward的insight，
因为CoT包含了很多step(thought)，我们对每个step都标注正确与否，然后训练能对step打分的verifier，得到PRM。

Note：
我们前面读过的涉及verifier的论文都是ORM，
因为训练数据的标签都是根据solution中的answer是否正确标注的，即使是token-level的训练方式，
因为每个token的label都相同，来自solution中的answer是否正确PRM不是本文作者提出来的，
DeepMind在Solving math word problems with processand outcome-based feedback中首次对比了ORM和PRM，
本文作者属于是重新做实验进行对比，结论也有差异本文的结论是PRM优于ORM，
从RL角度很好理解，dense reward > sparse reward.
PRM最大的弊端是需要对step进行数据标注，而ORM只需要对最终答案进行标注.
```


## Test-time Computing: from System-1 Thinking to System-2 Thinking
> 论文：[Test-time Computing: from System-1 Thinking to System-2 Thinking](https://arxiv.org/abs/2501.02497)
> 2025-01
> TTA - Test-time Adaptation

```
起因：
- System-1 和 System-2 思考源于认知心理学，在 AI 中用于描述不同的处理策略。
    - System-1 模型依赖于模式识别和快速、直观的响应，缺乏鲁棒性和对分布变化的适应性。
    - System-2 模型则更注重逻辑推理和精确的计算，能够处理更复杂的问题，但速度较慢。

LLM - System-2早期尝试：
- CoT，ToT，GoT，DoT
- RAG
- Sampling
- Self-consistency

TTA: 
- 利用延长的推理时间来识别解码搜索空间内类似人类的推理。
    - feedback modeling: Feedback with LLM/Human, ORM / PRM
    - search strategy: 重复采样和自我校正
```

## BoostStep 
> 论文：[BoostStep: Boosting mathematical capability of Large Language Models via improved single-step reasoning](https://arxiv.org/abs/2501.03226)
> 2025-01

```
问题：
 - ICL示例中存在的两个关键问题限制了其改进潜力：
    - 粒度不匹配：在Step粒度，进行增强
    - 负效应噪声问题： 某个前置步骤的错误或无关示例，妨碍其专注于当前步骤，可能导致错误的推理结果。

解决方案：
- 粒度对齐： 推理粒度从问题粒度细化到step粒度
- 示例检索： 检索与当前step高度相关的示例
- 与MCTS结合： 优化候选生成和决策过程
```


## Meta Chain-of-Thought
> 论文：[Towards System 2 Reasoning in LLMs: Learning How to Think With Meta Chain-of-Thought](https://arxiv.org/abs/2501.04682)
> 2025-01

```
问题：
 - 具有「思维链」提示功能的语言模型是否真的能够表达任何函数，从而解决任意复杂的问题？
 - 前沿模型的能力足以解决一大类数学推理问题。但是，它们仍然难以解决高级问题，如 HARP 和 Omni-MATH

假设：
 - 预训练语料库中的推理数据并不代表真正的数据生成过程，尤其是复杂问题的数据生成过程，它是大量潜在推理的产物。此外，这一过程一般不会以从左到右、自回归的方式进行。
 - 如果我们遵循现有教科书中呈现的一些步骤或方法，我们最终可以得出解答。
 - 相比之下，复杂推理问题并不遵循这种模式。

解决方案：
- 提出了一种新的方法，通过在思维链中引入元思维链（Meta Chain-of-Thought），来增强语言模型的推理能力。
- 通过在思维链中引入元思维链，可以使得语言模型在推理过程中，能够更好地理解问题的本质，从而提高解决问题的能力。

```

