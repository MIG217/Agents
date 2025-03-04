## ReACT

![image](https://github.com/user-attachments/assets/22646ece-5b12-46e9-a0dd-8b48c570a2ba)
https://react-lm.github.io/


### 流程图
![ReACT](https://github.com/user-attachments/assets/2680465a-03c8-4a8d-aefb-62252f969273)

### Prompt
```
Answer the following questions as best you can. You have access to the following tools:

{tools}

Use the following format:

Question: the input question you must answer
Thought: you should always think about what to do
Action: the action to take, should be one of [{tool_names}]
Action Input: the input to the action
Observation: the result of the action
... (this Thought/Action/Action Input/Observation can repeat N times)
Thought: I now know the final answer
Final Answer: the final answer to the original input question

Begin!

Question: {input}
Thought:{agent_scratchpad}
```
