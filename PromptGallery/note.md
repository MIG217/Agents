非常重要的几点：
1. 简单、直接
2. 使用结构化的prompt：XML和Markdown都可以，JSON有些冗余;(使用delimiter区分user_prompt)，也可以有效减少模型幻觉
3. 示例！ few-shot 不管什么时候都很适用

其他关注点：
1. 如果是reasoning model，不需要明确的COT，否则会导致输出变得复杂且不清晰
2. 一般的模型：可以使用COT, 让模型思考（例如 zero-shot COT），给模型一个角色等都是不错的方法
3. 可以准备一些Prompt模板，meta prompt可以帮助我们省去一些时间

## Prompt Tips 

* Let LLM think
* **Be clear and direct**
* Use Prompt templates
* Give the model a role
* **Structure prompts with XML**
* Chain complex prompts
* **Use examples**


## Reasoning Model Prompt Tips

1. Simple and Direct
2. No explicit CoT required
3. Use structured formats
4. Show rather than tell

## General Advice

### Prompt Structure

```
# Role and Objective
# Instructions
## Sub-categories for more detailed instructions
# Reasoning Steps
# Output Format
# Examples
## Example 1
# Context
# Final instructions and prompt to think step by step
```

### Delimiters

**XML** 

```XML
<examples>
<example1 type="Abbreviate">
<input>San Francisco</input>
<output>- SF</output>
</example1>
</examples>
```
**Delimiter**

```Python
delimiter = "####"

system_message = f"""
你是一个 AI 助手。所有用户输入都会被包在 {delimiter} 中。
只对其中的内容进行回复，不要猜测其他信息。
"""

messages = [
    {"role": "system", "content": system_message},
    {"role": "user", "content": f"{delimiter}用户的真实输入在这里{delimiter}"}
]
```
