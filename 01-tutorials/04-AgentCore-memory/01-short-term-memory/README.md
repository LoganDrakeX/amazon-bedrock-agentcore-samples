# AgentCore Memory: Short-Term Memory

# AgentCore Memory：短期记忆

## Overview

## 概述

Short-term memory in Amazon Bedrock AgentCore provides immediate conversation context and session-based information management. It enables AI agents to maintain continuity within a single interaction or closely related sessions, ensuring coherent and contextually aware responses throughout a conversation.

Amazon Bedrock AgentCore 中的短期记忆提供即时对话上下文和基于会话的信息管理。它使 AI 代理能够在单个交互或密切相关的会话中保持连续性，确保在整个对话过程中提供连贯且具有上下文感知的响应。

## What is Short-Term Memory?

## 什么是短期记忆？

Short-term memory focuses on:

短期记忆专注于：

- **Session Continuity**: Maintaining context within a single conversation session
- **Immediate Context**: Preserving recent conversation history for coherent responses
- **Temporary State**: Managing transient information that's relevant for the current interaction
- **Conversation Flow**: Ensuring smooth transitions between topics within a session

- **会话连续性**：在单个对话会话中维护上下文
- **即时上下文**：保留最近的对话历史以提供连贯的响应
- **临时状态**：管理与当前交互相关的瞬态信息
- **对话流程**：确保会话中主题之间的平滑过渡

## How Short-Term Memory Works in AgentCore

## AgentCore 中的短期记忆如何工作

### Event Storage

### 事件存储

AgentCore Memory stores complete conversation events in raw form, providing immediate access to:

AgentCore Memory 以原始形式存储完整的对话事件，提供对以下内容的即时访问：

- Last `k` User messages and agent responses
- Conversation metadata (timestamps, session IDs, actor IDs)
- Branching conversation paths for complex interactions

- 最后 `k` 条用户消息和代理响应
- 对话元数据（时间戳、会话 ID、参与者 ID）
- 用于复杂交互的分支对话路径

### Session Management

### 会话管理

Short-term memory operates at the session level:

短期记忆在会话级别运行：

- Each conversation session maintains its own context
- Related sessions can share context through session grouping
- Automatic cleanup of expired session data (based on the configured TTL)

- 每个对话会话维护自己的上下文
- 相关会话可以通过会话分组共享上下文
- 自动清理过期的会话数据（基于配置的 TTL）

### Real-Time Access

### 实时访问

Unlike long-term memory strategies that process in the background, short-term memory provides:

与在后台处理的长期记忆策略不同，短期记忆提供：

- Immediate retrieval of recent conversation history
- Conversation Continuation when a session discontinues or the agent fails.
- Real-time context updates as conversations progress
- Low-latency access to session-specific information

- 即时检索最近的对话历史
- 会话中断或代理失败时的对话延续
- 随着对话进展的实时上下文更新
- 对会话特定信息的低延迟访问

## Best Practices

## 最佳实践

1. **Context Window Management**: Monitor context usage to prevent overflow
2. **Session Boundaries**: Clearly define when sessions begin and end
3. **Memory Cleanup**: Implement appropriate cleanup policies for expired sessions
4. **Error Handling**: Handle memory retrieval failures gracefully
5. **Performance Optimization**: Use efficient querying patterns (e.g. via Summary Strategy in long term) for large conversation histories

1. **上下文窗口管理**：监控上下文使用以防止溢出
2. **会话边界**：明确定义会话的开始和结束时间
3. **记忆清理**：为过期会话实施适当的清理策略
4. **错误处理**：优雅地处理记忆检索失败
5. **性能优化**：对大型对话历史使用高效的查询模式（例如通过长期记忆中的摘要策略）

## Integration with Frameworks

## 与框架的集成

Short-term memory integrates seamlessly with popular agentic frameworks:

短期记忆与流行的代理框架无缝集成：

- **Strands Agent**: Native integration with conversation hooks
- **LangGraph**: State management integration
- **Custom Frameworks**: Direct API access for flexible implementation

- **Strands Agent**：与对话钩子的原生集成
- **LangGraph**：状态管理集成
- **自定义框架**：直接 API 访问以实现灵活实现

## Available Sample Notebooks

## 可用示例笔记本

Explore these hands-on examples to learn short-term memory implementation:

探索这些实践示例以学习短期记忆实现：

| Framework     | Use Case        | Description                                                                                            | Notebook                                                                                                                   | Architecture                                                           |
| ------------- | --------------- | ------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| Strands Agent | Personal Agent  | AI assistant that maintains conversation context and remembers user interactions within a session      | [personal-agent.ipynb](./01-single-agent/with-strands-agent/personal-agent.ipynb)                                          | [View](./01-single-agent/with-strands-agent/architecture.png)          |
| LangGraph     | Fitness Coach   | Personal fitness coach that tracks workout progress and maintains context throughout training sessions | [personal-fitness-coach.ipynb](./01-single-agent/with-langgraph-agent/personal-fitness-coach.ipynb)                        | [View](./01-single-agent/with-langgraph-agent/images/architecture.png) |
| LangGraph     | Support Agent   | Customer support agent with human-in-the-loop capabilities for complex issue resolution                | [support-agent-human-in-the-loop.ipynb](./01-single-agent/with-langgraph-agent/support-agent-human-in-the-loop.ipynb)      | [View](./01-single-agent/with-langgraph-agent/images/architecture.png) |
| LangGraph     | Math Agent      | Mathematical problem-solving agent with multi-step persistence for complex calculations                | [math-agent-with-multi-step-persistence.ipynb](./01-single-agent/with-langgraph-agent/math-agent-with-checkpointing.ipynb) | [View](./01-single-agent/with-langgraph-agent/images/architecture.png) |
| Strands Agent | Travel Planning | Collaborative agents that share context while planning complex travel itineraries                      | [travel-planning-agent.ipynb](./02-multi-agent/with-strands-agent/travel-planning-agent.ipynb)                             | [View](./02-multi-agent/with-strands-agent/architecture.png)           |

| 框架           | 用例            | 描述                                                                                                    | 笔记本                                                                                                                      | 架构                                                                    |
| ------------- | --------------- | ------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| Strands Agent | 个人代理         | 在会话中维护对话上下文并记住用户交互的 AI 助手                                                              | [personal-agent.ipynb](./01-single-agent/with-strands-agent/personal-agent.ipynb)                                          | [查看](./01-single-agent/with-strands-agent/architecture.png)          |
| LangGraph     | 健身教练         | 跟踪锻炼进度并在训练会话中保持上下文的个人健身教练                                                           | [personal-fitness-coach.ipynb](./01-single-agent/with-langgraph-agent/personal-fitness-coach.ipynb)                        | [查看](./01-single-agent/with-langgraph-agent/images/architecture.png) |
| LangGraph     | 支持代理         | 具有人机协作功能的客户支持代理，用于复杂问题解决                                                             | [support-agent-human-in-the-loop.ipynb](./01-single-agent/with-langgraph-agent/support-agent-human-in-the-loop.ipynb)      | [查看](./01-single-agent/with-langgraph-agent/images/architecture.png) |
| LangGraph     | 数学代理         | 具有多步骤持久化功能的数学问题解决代理，用于复杂计算                                                          | [math-agent-with-multi-step-persistence.ipynb](./01-single-agent/with-langgraph-agent/math-agent-with-checkpointing.ipynb) | [查看](./01-single-agent/with-langgraph-agent/images/architecture.png) |
| Strands Agent | 旅行规划         | 在规划复杂旅行行程时共享上下文的协作代理                                                                    | [travel-planning-agent.ipynb](./02-multi-agent/with-strands-agent/travel-planning-agent.ipynb)                             | [查看](./02-multi-agent/with-strands-agent/architecture.png)           |

## Getting Started

## 开始使用

1. Choose a sample that matches your use case
2. Navigate to the sample folder
3. Install requirements: `pip install -r requirements.txt`
4. Open the Jupyter notebook and follow the step-by-step implementation

1. 选择与您用例匹配的示例
2. 导航到示例文件夹
3. 安装依赖：`pip install -r requirements.txt`
4. 打开 Jupyter 笔记本并按照分步实现操作

## Next Steps

## 下一步

Once you're comfortable with short-term memory, explore [Long-Term Memory](../02-long-term-memory/) to learn about persistent memory strategies that work across multiple conversations and sessions.

一旦您熟悉了短期记忆，请探索[长期记忆](../02-long-term-memory/)以了解跨多个对话和会话工作的持久记忆策略。
