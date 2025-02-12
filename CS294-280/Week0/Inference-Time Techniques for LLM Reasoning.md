# Inference-Time Techniques for LLM Reasoning

## Trigger the LLM to generate long chain-of-thought (CoT) 
- **Few-shot CoT prompting** 
- **Instruction prompting**
- Instruction tuning
- Reinforcement learning

## Chain-of-thought prompting
### Zero-shot CoT

### Analogical prompting

### 

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


