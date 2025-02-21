
# 大语言模型的推理进化：从直觉到深度思考

## Background 

我们已经看到AI的能力在快速变化， 就像最近几个月在O1、R1等模型上在各种推理基准测试中取得的重大进展一样。 这是一个相当年轻的领域，本次讨论将聚焦于模型的自我提升能力(self-improvement)。

为了更好地理解AI的推理机制，我们首先需要区分两种基本的思维模式：System 1和 System 2：
<img width="918" alt="image" src="https://github.com/user-attachments/assets/fcbc4884-73e6-4df4-9d19-70fcd1a37a08" />

**System 1：快速直觉系统**

这是一种类似人类直觉反应的快速思维系统，主要依赖关联性思维。在LLM中，这种能力体现在transformer神经网络的基础运作机制上。其主要特征包括：

- 每个token使用固定的计算资源
- 直接输出答案
- 局限性：容易学习到虚假关联，产生幻觉、迎合性回答、越界等问题

**System 2: 深度思考系统**

这代表了一种更深层次的思维模式，目前主要通过思维链（CoT）来实现。在生成最终答案之前，System 2会进行系统性的推理分析。它具有以下优势：

- 能够执行规划、搜索和验证等复杂任务。
- 具备动态计算能力，可以通过CoT、ToT等方式实现灵活推理

我们可以通过优化模型架构或权重来提升System 1的表现，也可以通过改进推理链的生成方式来增强System 2的表现。我们的最终目标是让AI具备自我学习能力，这包括3个关键方面：

- 能够自主设计具有挑战性的训练任务
- 评估任务的完成质量，形成自我奖励机制
- 根据理解和反馈，持续更新优化自身能力

## Some Pre-history 

## Post-training Techniques

### First try: improving reasoning via System 2 (LLMs)

### Self-training methods: Better reasoning via Self-improvement 

