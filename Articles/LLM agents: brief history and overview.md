# LLM agents: brief history and overview

## Outline 

- What are LLM Agents?
- A brief history of LLM Agents
  - In the recent context of "LLM"
  - In the ancient context of "agents"
- On the future of LLM Agents

## 1. What is an Agent?

To understand LLM agents, we need to break the term into two parts: LLM and Agent. While LLM are widely known, the term "agent" requires further clarification.

In the context of AI, "agent" is a broad term encompassing various system- from autonomous vehicles and Go-playing algorithms to video game AI and conversional interfaces.

### 1.1 What is an agent?

In AI, an agent is an **"intelligent"** system that interacts with an **"environment"**. The type of agent depends on the nature of the environment:

- **Physical environments**: robots, autonomous vehicles, etc.
- **Digital environments**: DQN for Atari games, Siri, AlphaGo
- **Humans as environments**: Chatbots

<img width="263" alt="image" src="https://github.com/user-attachments/assets/6bb3c390-9c0f-4e24-903d-6731a14c3d8b" />

### 1.2 What is LLM agent?

Now that we understand what an agent is, let's explore the concept of an LLM agent.

LLM agents can be categorized into three distinct levels of sophistication:

<img width="518" alt="image" src="https://github.com/user-attachments/assets/aebe8565-0cc0-40ea-8a47-c41e8e25071c" />

Level 1: Text Agent
- Basic agents that process and respond to text input
- Examples: ELIZE, LSTM-DQN

Level 2: LLM Agent
- Advanced agents that leverage LLMs for direct action generation
- Examples: SayCan, Language Planner

Level 3: Reasoning Agent
- Use LLM to reason to act
- Examples: ReAct, AutoGPT

## 2. A Brief History of LLM Agents

### 2.1 ELIZA (1966): The Pioneer of Text Agents

The development of text-based agents dates back to the early days of AI. ELIZA, created in 1966, marked a significant milestone as one of the first chatbots. 

It's simple yet effective rule-based approach involved pattern matching and response templates to simulate human conversation. While users found ELIZA remarkably engaging, the system had inherent limitions:

- Limited to specific domains and use cases
- Require a extensive manual rule creation
- Unable to handle complex interactions or understanding

![image](https://github.com/user-attachments/assets/3c267a3c-c7a7-47f1-8b61-bbef70ca7958)

### 2.2 LSTM-DQN (2015): RL for Text Agents

Prior to the emergence of LLMs, RL was a dominant approach for developing text-based agents. This methodology treated text as both the observation and action space, similar to how traditional RL handles pixels and keyboard inputs in video games. The core idea was that optimizing for reward signals would natually lead emergence of language intelligence.

However, this approach faced several significant limitions:

- Domain-specific applications
- Dependence on explicit scalar reward signals
- Requires extensive training 

### 2.1 ReAct

### 2.2 Reflexion





