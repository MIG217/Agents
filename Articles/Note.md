### When (and when not) to use agents

When building applications with LLMs, we recommend finding the simplest solution possible, and only increasing complexity when needed. 

This might mean <mark>not building agentic systems at all. Agentic systems often trade latency and cost for better task performance, and you should consider when this tradeoff makes sense.</mark>

When more complexity is warranted, workflows offer predictability and consistency for well-defined tasks, whereas agents are the better option when flexibility and model-driven decision-making are needed at scale. 

For many applications, however, optimizing single LLM calls with retrieval and in-context examples is usually enough.

### What about retrieval and RAG?

- We can think of the retriveal corpus as "read-only" LTM (Long Term Memory)
  - Written by others (e.g., Wikipedia editors), not the agent itself
  - Retrieval[query]: a read action
 
- Limitations
  - Can only live "others' experience", which might not
  - The way corpus is written might not be optimal for

 - **Possible Improvement:** 
  - Agent memory: also be able to autonomously write to it!
 
  <img width="300" alt="image" src="https://github.com/user-attachments/assets/9962a38e-ee4d-4404-a732-262d752be1d5" />


### Long-term memory: Content 

https://en.wikipedia.org/wiki/Long-term_memory

|Type of content|Definition|Example|
|---|---|---|
|Episodic memory |Stores experience||
|Semantic memory |Stores knowledge ||
|Prodecual memory |Stores skills||



### ReAct

Reasoning help diagnose and control acting.


1. CoT是需要呈现给使用者的东西，在一定程度上优化了模型的幻觉、准确性等问题；
2. 对于开发者、orchestrator来说，更鼓励使用非推理模型，会更清楚你想要呈现的业务逻辑，同时也可以帮自己理清思路


