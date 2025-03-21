# How to design effective reasoning techniques

[ZH] https://mp.weixin.qq.com/s/KOqATaijYwOOwJMvsCe4kg


**Catelog**   
1. [Trigger the LLM to generate long chain-of-thought (CoT)](#trigger-the-llm-to-generate-long-chain-of-thought-cot)
2. [Chain-of-thought prompting](#chain-of-thought-prompting)
3. [Basic prompting techniques](#basic-prompting-techniques)  
   3.1 [Zero-shot CoT](#zero-shot-cot)   
   3.2 [LLM as the optimizer to iteratively improve the prompt](#llm-as-the-optimizer-to-iteratively-improve-the-prompt)  
   3.3 [Least-to-most prompting](#least-to-most-prompting)  
   3.4 [Self-Discover: instruct the LLM to compose reasoning structures for each task](#self-discover-instruct-the-llm-to-compose-reasoning-structures-for-each-task)  
4. [Summary](#summary)

## 1. Trigger the LLM to generate long chain-of-thought (CoT) 
- **Few-shot CoT prompting** 
- **Instruction prompting**
- Instruction tuning
- Reinforcement learning

## 2. Chain-of-thought prompting
- Chain-of-thought prompting: <mark>variable computation</mark> of the thought process adapting to tasks of different difficulty levels
  - More complex questions -> more reasoning steps in the chain-of-thought

- Reasoning strategies enabled by CoT
  - Decomposition
  - Planning
  - … 

- We can explicitly instruct the LLM with the desired reasoning strategies for problem solving

## 3. Basic prompting techniques
Use **more token budget** to generate **a single solution**
- **Standard prompting**
  - Before the advancement in post-training techniques, standard prompting performance is poor on reasoning benchmarks.
  - Issue: standard few-shot exemplars only provide information on the final solution format, but not the **rationale** to derive the solution.

- **Chain-of-thought prompting: providing thoughts in the exemplars**
  - CoT performance improves more significantly with the increase of the model size.
  - Better models benefit more with CoT generation
  - Note: these experiments used pretrained-only LLMs
 
- **Analogical prompting: instruct the LLM to generate exemplars**
Prompt the LLM to **first recall relevant exemplars**, before solving the test problem.

### 3.1 Zero-shot CoT
- Weaker LLMs benefit less from analogical prompting, though it does not hurt the zero-shot performance
- With stronger LLMs, analogical prompting outperforms CoT with manually-designed or retrieved exemplars
  - The generated CoT is more tailored to the underlying LLM
<img width="831" alt="image" src="https://github.com/user-attachments/assets/12b558f5-ab52-45a4-a6c4-25ce15c9c02d" />

### 3.2 LLM as the optimizer to iteratively improve the prompt
- Core idea: instruct the LLM to <mark>leverage the past optimization trajectory</mark>, represented as sorted (solution, score) pairs

- Optimizer: the LLM to propose a new instruction given old ones and task exemplars
- Evaluator: the LLM to evaluate the accuracy of an instruction
<img width="680" alt="image" src="https://github.com/user-attachments/assets/dd220e53-c0f6-48d6-b488-9634534d3745" />

### 3.3 Least-to-most prompting
- SCAN 是一个用于评估组合泛化能力的基准，它模拟了如何通过有限的训练数据学习并推理新的指令组合
<img width="905" alt="image" src="https://github.com/user-attachments/assets/7a3a5b58-92d1-4bff-a5eb-40829ecc96c4" />


### 3.4 Self-Discover: instruct the LLM to compose reasoning structures for each task
- Different reasoning tasks require different reasoning structures, i.e., different ways to decompose the task and plan for each stage.
- Self-Discover composes task-specific reasoning structures without manually-written demonstrations.
<img width="868" alt="image" src="https://github.com/user-attachments/assets/babd381b-6ecd-4c3c-9e07-6c4e73a13711" />

### <mark>4. Summary</mark>
- Chain-of-thought generation: <mark>**variable computation**</mark> of the thought process adapting to tasks of differently levels

- How to improve the CoT performance at inference time
  - Few-shot prompting with labeling of thoughts
  - Instruction prompting to trigger CoT generation
  - Instruct the LLM to automate the prompt design
 
  - Note: the best prcatice to interact with LLMs evolves over time
    - The principles of how to discover good prompting strategies for reasoning hold true
      - Encourage longer CoT for complex tasks
      - Support reasoning strategies required for the task

## 附：anthropic文章中给到的建议
1. 给模型足够的 token 来“思考”，从而避免它进入死胡同；
2. 文本的输出格式，与此类文本在互联网上的常见格式保持一致，因为大模型就是在互联网数据上进行训练的；
3. 确保没有任何格式“开销”（例如需要准确记录几千行代码，或对代码进行转义）。

一个经验法则：在人机界面（HCI）上投入了多少努力，就在 agent-computer interfaces（ACI）上投入同样多的努力。 如何做到这一点：
1. 换位思考，多站在模型的角度思考问题。
  - 根据给定的描述和参数，作为自然人是一看就懂，还是需要思考一下才能判断？自然人是什么反应，模型也很可能是什么反应。
  - 一个好的工具定义通常包括示例用法、边界情况、输入格式要求以及明确与其他工具的界限。
2. 如何重命名参数或改进文档，使工具的描述更简洁直白？可以将这个过程当做为团队中的新人编写一个优秀的 docstring。当工具很多而且存在一些类似时，这一点尤其重要。
3. 测试模型如何使用你的工具：运行一些示例输入，看看模型犯了什么错误，并进行迭代。
4. 工具的防呆（Poka-yoke）。

## 相关Paper
> Large Language Models Are Human-Level Prompt Engineers： https://arxiv.org/abs/2211.01910
> 
> Large Language Models as Optimizers：https://arxiv.org/abs/2309.03409
>
> <mark>Self-Discover: Large Language Models Self-Compose Reasoning Structures https://arxiv.org/abs/2402.03620</mark>

> Wei et al., Chain-of-Thought Prompting Elicits Reasoning in Large Language Models, NeurIPS 2022.
> 
> Wei et al., Emergent Abilities of Large Language Models, TMLR 2022.  


