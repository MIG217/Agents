
# 大语言模型的自我提升与推理能力进化 

[ZH]  https://mp.weixin.qq.com/s/o3_bNj3sjG-Co5cPlNhxPg

AI 能力正在快速发展，如 O1、R1 等模型在推理基准测试中取得的突破性进展。本文将聚焦于模型的自我提升能力(self-improvement)。
为了更好地理解AI的推理机制，我们首先需要区分两种基本的思维模式：System 1和 System 2：
<img width="903" alt="image" src="https://github.com/user-attachments/assets/062996f3-9a83-4d14-9b17-9fdffa89bd66" /> 

**System 1：快速直觉系统**

这是一种类似人类直觉反应的快速思维系统，主要依赖关联性思维。在LLM中，这种能力体现在transformer神经网络的基础运作机制上。其主要特征包括： 
- 每个token使用固定的计算资源；
- 直接输出答案；
- 局限性：容易学习到虚假关联，产生幻觉、迎合性回答、越界等问题；

**System 2: 深度思考系统**

这代表了一种更深层次的思维模式，目前主要通过思维链（CoT）来实现。在生成最终答案之前，System 2会进行系统性的推理分析。它具有以下优势： 
- 能够执行规划、搜索和验证等复杂任务；
- 具备动态计算能力，可以通过CoT、ToT等方式实现灵活推理；

我们可以通过优化模型架构或权重来提升System 1的表现，也可以通过改进推理链的生成方式来增强System 2的表现；最终目标是让AI具备自我学习能力，这包括3个关键方面： 
- 能够自主设计具有挑战性的训练任务
- 评估任务的完成质量，形成自我奖励机制
- 根据理解和反馈，持续更新优化自身能力 

接下来，我们将分两个部分展开讨论：首先回顾语言模型的历史发展历程，然后深入探讨过去一年中的重要研究进展。

## 1. LLM Post-training：O1/R1 之前的优化之路 

### 1.1 Instrcut GPT
InstructGPT 是在 2022 年提出的一种语言模型优化方法[1]，它结合了监督学习和基于人类反馈的强化学习（RLHF），比单纯的监督微调更为先进。这种方法包含三个关键步骤(如图）：

1. 监督微调（SFT）阶段：通过收集人类标注的示范数据，对基础模型进行初步的行为调整；
2. 奖励模型训练阶段：根据人类对模型多个输出的排序评估，训练一个能够判断输出质量的奖励模型；
3. 强化学习优化阶段：利用奖励模型的反馈评分，通过 PPO 算法持续优化模型输出；
<img width="887" alt="image" src="https://github.com/user-attachments/assets/bd448a4c-5d9f-444e-843c-f444da6840f0" />

这种训练方法通过引入人类反馈和自我优化机制，使模型能够持续改进其输出质量。这不仅提升了模型的性能，而是朝着自我训练的目标迈进。

### 1.2 Direct Preference Optimization (DPO)

DPO，即直接偏好优化[2]，作为RLHF的替代方案，近年来受到了广泛关注。DPO旨在简化模型的训练过程，并直接基于人类的偏好数据进行优化，无需显式地训练奖励模型（如图）

<img width="913" alt="image" src="https://github.com/user-attachments/assets/4840bdfd-e6f8-4e1a-bb56-fe8926156ad1" />

DPO的主要步骤：
1. 偏好数据收集：收集包含多个输出的偏好数据集；例如，给定提示后生成两个结果，由人类选择更符合预期的输出；
2. 目标函数转化：通过数学变换，将强化学习的目标函数转化为分类问题，基于Bradley-Terry模型优化策略差异；
3. 策略优化：通过最大化偏好数据的似然函数，优化模型参数；

总的来说，InstructGPT 和 DPO 等 Post-Training 技术，通过引入人类反馈和偏好，极大地提升了 LLM 的指令跟随能力和输出质量。虽然这个阶段还没有明确的CoT推理机制，但相比原始的预训练语言模型，已经有了显著的进步。

## 2. 通过System 2提升推理能力：Prompting方法 

### 2.1 Chain-of-Verification (CoVe) 减少 LLM 中的幻觉

CoVe 的核心思想是，让LLM不仅仅生成一个初步的答案（可以看作是初稿），还要对这个初步答案进行自我验证[3]。CoVe 的工作流程如下：
1. 生成初步答案 (Baseline Response): 模型根据用户提问（Query）生成一个初步答案；
2. 规划验证问题 (Plan Verifications): 模型基于初步答案，生成一系列验证性问题；
3. 执行验证 (Execute Verifications): 模型针对每个验证问题，生成简短的回答；
4. 生成最终验证后的答案对比(Final Verified): 模型对比初步答案和验证答案，发现并修正错误，生成最终答案；
<img width="924" alt="image" src="https://github.com/user-attachments/assets/0ff14a63-5e3f-4757-8833-400047ece15c" />

研究表明，模型在处理简短的回答对时，通常比生成长篇回答更准确。通过规划和执行这些验证问题，LLM能够发现并纠正自身在初步答案中存在的矛盾和错误；在各类知识密集型任务中，采用CoVe方法可以将LLM回答的准确率显著提升（如图）：

### 2.2 System 2 Attention(S2A): 让注意力更专注
由于 LLM 采用“软注意力”机制（而非“硬注意力”），导致模型易受“语义泄露”和“谄媚”问题的影响，即整个上下文（包括不相关信息）都会影响输出。为解决此问题，"System 2 Attention" 论文提出了 S2A 方法[4]，利用 CoT 推理**重写原始指令**，消除偏差。S2A 步骤（如图）：
<img width="502" alt="image" src="https://github.com/user-attachments/assets/90583d60-3e60-4cf5-8bd8-325a6184ad70" />

- 重写指令 (Rewrite Prompt): 提示 LLM 重写原始问题，去除其中的不相关信息或偏差；
- 基于重写后的问题进行回答 (Answer with Rewritten Prompt): 使用重写后的问题再次输入 LLM，生成最终答案；

CoT 和 System 2 推理的应用远不止于数学问题。通过利用这些中间 token 进行推理，模型可以在各种类型的任务中进行思考，并有效解决许多问题。

### 2.3 Branch-Solve-Merge (BSM)：分解复杂任务
当任务复杂、指令难以理解时，即使是像 GPT-4 这样强大的模型也会出错。BSM 就像“分而治之”的策略，将一个大问题拆解成几个小问题，逐个击破，最后再将结果整合起来。BSM 流程 (如图所示)：

1.  **分支 (Branch):** 给定一个复杂任务，使用特定的 Prompt 让 LLM 生成一个计划，将任务分解为多个相互独立的子任务（分支）。
2.  **解决 (Solve):** 针对每个分支，独立地解决其对应的子任务。每个分支的求解过程互不干扰。
3.  **合并 (Merge):** 给定每个分支的部分解决方案，使用特定的 Prompt 让 LLM 将它们合并成最终的解决方案。合并过程不仅仅是简单的拼接，而是需要进行整合和优化。

Prompting 方法展示了通过精心设计的提示和引导, 我们可以显著提升 LLM 在复杂任务上的表现。然而，这些方法仍然依赖于人工干预，需要为每个任务设计特定的 Prompt。 
我们真正想要的是，模型能够 *自主地* 进行推理，而不仅仅是依赖于巧妙设计的 Prompt。 因此，下一个研究浪潮，也是我们目前所处的阶段，就是**通过优化模型本身来实现自我提升，从而获得更强大的推理能力**。

## 3. 通过自我提升实现更好的推理

传统机器学习 (ML) 中，人类监督的是比自己能力弱的 AI 系统（左）。然而，要实现超级智能的对齐，人类将需要监督比自己更强大的 AI 系统（中）[6]。How can we continue improving superhuman models?

### 3.1 Self-Rewarding LLMs

There’s two observations that can help us to solve this. Observation 1 is LLMs can countinue improving if provided good judgements on response quality[7][8]. Observation 2 is LLMs can provide good judgements on model generation[9][10].


参考文献：

[1] Ouyang, Long, Jeff Wu, Xu Jiang, Diogo Almeida, Carroll L. Wainwright, and others. "Training Language Models to Follow Instructions with Human Feedback." arXiv, March 4, 2022.

[2] Rafailov, Rafael, Archit Sharma, Eric Mitchell, Stefano Ermon, Christopher D. Manning, and Chelsea Finn. "Direct Preference Optimization: Your Language Model is Secretly a Reward Model." arXiv, July 29, 2024.

[3] Dhuliawala, Shehzaad, Mojtaba Komeili, Jing Xu, Roberta Raileanu, Xian Li, Asli Celikiyilmaz, and Jason Weston. "Chain-of-Verification Reduces Hallucination in Large Language Models." arXiv, September 25, 2023. 

[4] Weston, Jason, and Sainbayar Sukhbaatar. "System 2 Attention (Is Something You Might Need Too)." arXiv, November 20, 2023.

[5] Saha, Swarnadeep, Omer Levy, Asli Celikiyilmaz, Mohit Bansal, Jason Weston, and Xian Li. "Branch-Solve-Merge Improves Large Language Model Evaluation and Generation." arXiv, June 7, 2024.

[6] Burns, Collin, Pavel Izmailov, Jan Hendrik Kirchner, Bowen Baker, Leo Gao, Leopold Aschenbrenner, Yining Chen, Adrien Ecoffet, Manas Joglekar, Jan Leike, Ilya Sutskever, and Jeff Wu. "Weak-to-Strong Generalization: Eliciting Strong Capabilities With Weak Supervision." arXiv, December 14, 2023.

[7] Bai, Yuntao, Andy Jones, Kamal Ndousse, Amanda Askell, Anna Chen, and others. "Training a Helpful and Harmless Assistant with Reinforcement Learning from Human Feedback." *arXiv*, April 12, 2022.

[8] Touvron, Hugo, Louis Martin, Kevin Stone, Peter Albert, Amjad Almahairi, and others. "Llama 2: Open Foundation and Fine-Tuned Chat Models." *arXiv*, July 19, 2023.

[9] Zheng, Lianmin, Wei-Lin Chiang, Ying Sheng, Siyuan Zhuang, Zhanghao Wu, and others. "Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena." *arXiv*, December 24, 2023.

[10] Dubois, Yann, Xuechen Li, Rohan Taori, Tianyi Zhang, Ishaan Gulrajani, and others. "AlpacaFarm: A Simulation Framework for Methods that Learn from Human Feedback." *arXiv*, January 8, 2024.

[11] Yuan, Weizhe, Richard Yuanzhe Pang, Kyunghyun Cho, Xian Li, Sainbayar Sukhbaatar, Jing Xu, and Jason Weston. "Self-Rewarding Language Models." *arXiv*, February 8, 2024.

