# Workflow & Agent

原文参考
1. https://www.anthropic.com/research/building-effective-agents
2. https://langchain-ai.github.io/langgraph/tutorials/workflows/#building-blocks-the-augmented-llm

## 1. 定义

### 1.1 什么是Agent

一些用户将智能体定义为**完全自主的系统**，能够在长时间内独立运行，并利用各种工具来完成复杂的任务。 另一些用户则用这个术语来描述更具规范性的实现，这些实现**遵循预定义的工作流程**。 

在 Anthropic，我们将所有这些变体都归类为Agentic Systems，但在架构上对 Workflows 和 Agents 进行了重要的区分：

- Workflows: 通过**预定义的代码路径来编排 LLM 和工具的系统**。
- Agents: LLM **动态地指导自身过程和工具使用的系统**，对如何完成任务保持控制权。

下图是一种简单的可视化方式，可以帮助理解Agent和Workflow的区别：
![image](https://github.com/user-attachments/assets/36d0e0e9-1bf2-4e78-b876-23a7efd72ff8)

### 1.2 何时使用Agent

在使用 LLM 构建应用程序时，我们建议**尽可能寻找最简单的解决方案，仅在必要时才增加复杂性**。 
- 这可能意味着完全不构建智能体系统。
- 智能体系统通常会牺牲延迟和成本来换取更好的任务性能，您应该考虑这种权衡何时有意义。

当需要更高的复杂性时：
- 工作流 (Workflows) 为定义明确的任务提供了可预测性和一致性
- 需要大规模的灵活性和模型驱动的决策时，智能体 (Agents) 是更好的选择
- 但对于许多应用程序来说，通过 Retrieval 和 In-Context Examples 来优化单个 LLM 调用通常就足够了

### 1.3 如何使用框架

有许多框架可以使智能体系统的实现更容易，包括：

- LangGraph from LangChain
- Amazon Bedrock's AI Agent framework
- Rivet, a drag and drop GUI LLM workflow builder
- Vellum, another GUI tool for building and testing complex workflows

## 2. 构建Agent和Workflow

### 2.1 Building Block: The augmented LLM

Agentic systems 的基础构建块是 augmented LLM，它通过retrieval, tools, and memory等功能进行了增强。Augmented LLM 能够自主生成搜索查询、选择工具和保留信息。

![image](https://github.com/user-attachments/assets/069343ef-9702-4f5e-a4f2-3513a5c26457)

建议关注实现的两个关键方面：

- 根据您的特定用例定制这些能力
- 确保它们为您的 LLM 提供一个简单、文档齐全的接口

### 2.2 Workflow: Prompt Chaining

Prompt Chaining 将任务分解为一系列步骤，其中每个 LLM 调用处理前一个调用的输出。

![image](https://github.com/user-attachments/assets/f734473d-3f8a-4538-894b-cdc1766afb34)


**何时使用此工作流：** 当任务可以轻松、清晰地分解为固定的子任务时，此工作流非常理想。主要目标是通过使每个 LLM 调用成为更简单的任务来牺牲延迟以获得更高的准确性。

**Prompt Chaining 适用的示例：**

- 生成营销文案，然后将其翻译成不同的语言。
- 编写文档大纲，检查大纲是否符合某些标准，然后根据大纲编写文档。

### 2.3 Workflow: Routing 

Routing 对输入进行分类并将其定向到后续任务，此工作流允许关注点分离，并构建更专业的提示。如果没有此工作流，针对一种输入进行优化可能会损害其他输入的性能。

![image](https://github.com/user-attachments/assets/c6cba1f5-bc0e-4288-95cd-2248cea5af82)

**何时使用此工作流：** Routing 非常适用于存在不同类别且最好单独处理的复杂任务，并且分类可以由 LLM 或更传统的分类模型/算法准确处理。

**Routing 适用的示例：**

- 将不同类型的客户服务查询（一般问题、退款请求、技术支持）定向到不同的下游流程、提示和工具。
- 将简单/常见问题路由到较小的模型（如 Claude 3 Haiku），将困难/不常见问题路由到功能更强大的模型（如 Claude 3 Sonnet），以优化成本和速度。

### 2.4 Workflow: Parallelization

Parallelization 将一个任务分解为多个可以同时执行的子任务，或者对同一个任务执行多次，然后将这些子任务的结果聚合起来，得到最终结果。

目的:

- 提高速度: 通过并行处理，可以显著缩短任务完成时间。
- 提高可靠性/准确性: 通过多个 LLM 独立处理或多次处理，可以减少单个 LLM 的偏差或错误，提高结果的可靠性和准确性。

两种主要变体： Sectioning 和 Voting

![image](https://github.com/user-attachments/assets/59b92ba0-faf3-455e-b1ad-dacc30be19d2)

**何时使用此工作流：** 当划分的子任务可以并行化以提高速度，或者当需要多个视角或尝试以获得更高置信度的结果时，Parallelization是有效的。

**Parallelization适用的示例：**

**Sectioning**

- 其中一个模型实例处理用户查询，而另一个模型实例筛选不当内容或请求。这往往比让同一个 LLM 调用同时处理护栏和核心响应表现更好。
- 自动化评估以评估 LLM 性能，其中每个 LLM 调用评估模型在给定提示上的性能的不同方面。

**Voting**

- 审查一段代码是否存在漏洞，其中多个不同的提示审查代码并在发现问题时标记。
- 评估给定内容是否不适当，多个提示评估不同方面或需要不同的投票阈值来平衡误

### 2.5 workflow: Orchestrator-Worker

在Orchestrator-Worker一个中央 LLM 动态地分解任务，将它们委派给worker LLM，并综合它们的结果。

![image](https://github.com/user-attachments/assets/e2799222-d680-4e45-b487-e92cab8403a8)

**合适使用此工作流：** 此工作流非常适合您无法预测所需子任务的复杂任务（例如，在编码中，需要更改的文件数量和每个文件中更改的性质可能取决于任务）。虽然它在拓扑上与Parallelization 相似，但关键区别在于它的灵活性——子任务不是预定义的，而是由编排器根据特定输入确定的。

**Orchestrator-Worker适用的示例：**

- 代码生成/修改： 需要对多个文件进行复杂更改的软件开发项目。
- 复杂信息检索与分析： 需要从多个来源收集和分析信息，并从中提取相关信息的搜索任务。

### 2.6 workflow: Evaluator-optimizer

Evaluator-optimizer工作流采用了一种迭代优化的策略。它包含两个关键的 LLM 角色：
- Generator: 一个 LLM 负责生成初步的响应或解决方案。
- Evaluator: 另一个 LLM 负责评估生成器生成的响应，并提供具体的、可操作的反馈意见。

![image](https://github.com/user-attachments/assets/e62b0a36-f49f-497a-a7e5-670b895b6031)

**何时使用此工作流：** 当我们有明确的评估标准，并且当迭代改进提供可衡量价值时，此工作流特别有效。 良好适应的两个标志是，首先，当人类表达他们的反馈时，LLM 响应可以得到明显改进；其次，LLM 可以提供此类反馈。 这类似于人类作家在制作精美文档时可能经历的迭代写作过程。

**Evaluator-optimizer适用的示例：**

- 文学翻译，其中存在翻译器 LLM 最初可能无法捕捉到的细微差别，但评估器 LLM 可以提供有用的评论。
- 复杂的搜索任务，需要多轮搜索和分析才能收集全面的信息，评估器决定是否需要进一步搜索。

### 2.7 Agents

![image](https://github.com/user-attachments/assets/1e6b3337-be5e-4b48-8fb4-191737945a9c)


