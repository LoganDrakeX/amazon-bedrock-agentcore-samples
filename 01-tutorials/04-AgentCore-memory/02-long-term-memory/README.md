# AgentCore Memory: Long-Term Memory Strategies

# AgentCore Memory：长期记忆策略

## Overview

## 概述

Long-term memory in Amazon Bedrock AgentCore enables AI agents to maintain persistent information across multiple conversations and sessions. Unlike short-term memory that focuses on immediate context, long-term memory extracts, processes, and stores meaningful information that can be retrieved and applied in future interactions, creating truly personalized and intelligent agent experiences.

Amazon Bedrock AgentCore 中的长期记忆使 AI 代理能够在多个对话和会话中维护持久信息。与专注于即时上下文的短期记忆不同，长期记忆提取、处理和存储有意义的信息，这些信息可以在未来的交互中检索和应用，从而创建真正个性化和智能的代理体验。

## What is Long-Term Memory?

## 什么是长期记忆？

Long-term memory provides:

长期记忆提供：

- **Cross-Session Persistence**: Information that survives beyond individual conversations
- **Intelligent Extraction**: Automatic identification and storage of important facts, preferences, and patterns
- **Semantic Understanding**: Vector-based storage that enables natural language retrieval
- **Personalization**: User-specific information that enables tailored experiences
- **Knowledge Accumulation**: Continuous learning and information building over time

- **跨会话持久化**：超越单个对话持续存在的信息
- **智能提取**：自动识别和存储重要事实、偏好和模式
- **语义理解**：支持自然语言检索的基于向量的存储
- **个性化**：支持定制体验的用户特定信息
- **知识积累**：随时间持续学习和信息构建

## How Long-Term Memory Strategies Work

## 长期记忆策略如何工作

Long-term memory operates through **Memory Strategies** that define what information to extract and how to process it. The system works automatically in the background:

长期记忆通过**记忆策略**运行，这些策略定义要提取什么信息以及如何处理它。系统在后台自动工作：

### Processing Pipeline

### 处理管道

1. **Conversation Analysis**: Saved conversations are analyzed based on configured strategies
2. **Information Extraction**: Important data (facts, preferences, summaries) is extracted using AI models
3. **Structured Storage**: Extracted information is organized in namespaces for efficient retrieval
4. **Semantic Indexing**: Information is vectorized for natural language search capabilities
5. **Consolidation**: Similar information is merged and refined over time

1. **对话分析**：根据配置的策略分析已保存的对话
2. **信息提取**：使用 AI 模型提取重要数据（事实、偏好、摘要）
3. **结构化存储**：提取的信息在命名空间中组织以便高效检索
4. **语义索引**：信息被向量化以支持自然语言搜索功能
5. **整合**：相似信息随时间合并和精炼

**Processing Time**: Typically takes ~1 minute after conversations are saved, with no additional code required.

**处理时间**：通常在对话保存后约 1 分钟完成，无需额外代码。

### Behind the Scenes

### 幕后工作

- **AI-Powered Extraction**: Uses foundation models to understand and extract relevant information
- **Vector Embeddings**: Creates semantic representations for similarity-based retrieval
- **Namespace Organization**: Structures information using configurable path-like hierarchies
- **Automatic Consolidation**: Merges and refines similar information to prevent duplication
- **Incremental Learning**: Continuously improves extraction quality based on conversation patterns

- **AI 驱动的提取**：使用基础模型理解和提取相关信息
- **向量嵌入**：创建用于基于相似性检索的语义表示
- **命名空间组织**：使用可配置的路径式层级结构组织信息
- **自动整合**：合并和精炼相似信息以防止重复
- **增量学习**：基于对话模式持续改进提取质量

## Long-Term Memory Strategy Types

## 长期记忆策略类型

AgentCore Memory supports four distinct strategy types for long-term information storage:

AgentCore Memory 支持四种不同的长期信息存储策略类型：

### 1. Semantic Memory Strategy

### 1. 语义记忆策略

Stores factual information extracted from conversations using vector embeddings for similarity search.

使用向量嵌入存储从对话中提取的事实信息，以便进行相似性搜索。

```python
{
    "semanticMemoryStrategy": {
        "name": "FactExtractor",
        "description": "Extracts and stores factual information",
        "namespaces": ["support/user/{actorId}/facts"]
    }
}
```

**Best for**: Storing product information, technical details, or any factual data that needs to be retrieved through natural language queries.

**最适合**：存储产品信息、技术详情或任何需要通过自然语言查询检索的事实数据。

### 2. Summary Memory Strategy

### 2. 摘要记忆策略

Creates and maintains summaries of conversations to preserve context for long interactions.

创建和维护对话摘要，为长时间交互保留上下文。

```python
{
    "summaryMemoryStrategy": {
        "name": "ConversationSummary",
        "description": "Maintains conversation summaries",
        "namespaces": ["support/summaries/{sessionId}"]
    }
}
```

**Best for**: Providing context in follow-up conversations and maintaining continuity across long interactions.

**最适合**：在后续对话中提供上下文，并在长时间交互中保持连续性。

### 3. User Preference Memory Strategy

### 3. 用户偏好记忆策略

Tracks user-specific preferences and settings to personalize interactions.

跟踪用户特定的偏好和设置以个性化交互。

```python
{
    "userPreferenceMemoryStrategy": {
        "name": "UserPreferences",
        "description": "Captures user preferences and settings",
        "namespaces": ["support/user/{actorId}/preferences"]
    }
}
```

**Best for**: Storing communication preferences, product preferences, or any user-specific settings.

**最适合**：存储通信偏好、产品偏好或任何用户特定设置。

### 4. Custom Memory Strategy

### 4. 自定义记忆策略

Allows customization of prompts for extraction and consolidation, providing flexibility for specialized use cases.

允许自定义提取和整合的提示，为专业用例提供灵活性。

```python
{
    "customMemoryStrategy": {
        "name": "CustomExtractor",
        "description": "Custom memory extraction logic",
        "namespaces": ["user/custom/{actorId}"],
        "configuration": {
            "semanticOverride": { # You can also override Summary or User Preferences.
                "extraction": {
                    "appendToPrompt": "Extract specific information based on custom criteria",
                    "modelId": "global.anthropic.claude-haiku-4-5-20251001-v1:0",
                },
                "consolidation": {
                    "appendToPrompt": "Consolidate extracted information in a specific format",
                    "modelId": "global.anthropic.claude-haiku-4-5-20251001-v1:0",
                }
            }
        }
    }
}
```

**Best for**: Specialized extraction needs that don't fit the standard strategies.

**最适合**：不适合标准策略的专业提取需求。

## Understanding Namespaces

## 理解命名空间

Namespaces organize memory records within strategies using a path-like structure. They can include variables that are dynamically replaced:

命名空间使用路径式结构在策略中组织记忆记录。它们可以包含动态替换的变量：

- `support/facts/{sessionId}`: Organizes facts by session
- `user/{actorId}/preferences`: Stores user preferences by actor ID
- `meetings/{memoryId}/summaries/{sessionId}`: Groups summaries by memory

- `support/facts/{sessionId}`：按会话组织事实
- `user/{actorId}/preferences`：按参与者 ID 存储用户偏好
- `meetings/{memoryId}/summaries/{sessionId}`：按记忆分组摘要

The `{actorId}`, `{sessionId}`, and `{memoryId}` variables are automatically replaced with actual values when storing and retrieving memories.

`{actorId}`、`{sessionId}` 和 `{memoryId}` 变量在存储和检索记忆时会自动替换为实际值。

## Example: How It Works in Practice

## 示例：实际工作原理

Let's say a user tells your customer support agent: _"I'm vegetarian and I really enjoy Italian cuisine. Please don't call me after 6 PM."_

假设用户告诉您的客户支持代理：_"我是素食者，我非常喜欢意大利菜。请不要在下午 6 点后给我打电话。"_

After you save this conversation, the configured strategies automatically:

保存此对话后，配置的策略会自动：

**Semantic Strategy** extracts:

**语义策略**提取：

- "User is vegetarian"
- "User enjoys Italian cuisine"

- "用户是素食者"
- "用户喜欢意大利菜"

**User Preference Strategy** captures:

**用户偏好策略**捕获：

- "Dietary preference: vegetarian"
- "Cuisine preference: Italian"
- "Contact preference: no calls after 6 PM"

- "饮食偏好：素食"
- "菜系偏好：意大利菜"
- "联系偏好：下午 6 点后不打电话"

**Summary Strategy** creates:

**摘要策略**创建：

- "User discussed dietary restrictions and contact preferences"

- "用户讨论了饮食限制和联系偏好"

All of this happens automatically in the background - you only need to save the conversation and the strategies handle the rest.

所有这些都在后台自动发生 - 您只需保存对话，策略会处理其余部分。

## Available Sample Notebooks

## 可用示例笔记本

Explore these hands-on examples to learn long-term memory strategy implementation:

探索这些实践示例以学习长期记忆策略实现：

| Integration Method        | Use Case            | Description                                                                             | Notebook                                                                                                       | Architecture                                                                               |
| ------------------------- | ------------------- | --------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| Strands Agent Hooks       | Customer Support    | Complete support system with semantic and preference memory strategies                  | [customer-support.ipynb](./01-single-agent/using-strands-agent-hooks/customer-support/customer-support.ipynb)  | [View](./01-single-agent/using-strands-agent-hooks/customer-support/architecture.png)      |
| Strands Agent Hooks       | Math Assistant      | Math tutor assistant that remembers user learning preferences and progress              | [math-assistant.ipynb](./01-single-agent/using-strands-agent-hooks/simple-math-assistant/math-assistant.ipynb) | [View](./01-single-agent/using-strands-agent-hooks/simple-math-assistant/architecture.png) |
| LangGraph Agent Hooks     | Nutrition Assistant | Nutrition advisor that saves user dietary preferences and health goals for personalized recommendations | [nutrition-assistant-with-user-preference-saving.ipynb](./01-single-agent/using-langgraph-agent-hooks/nutrition-assistant-with-user-preference-saving.ipynb) | [View](./01-single-agent/using-langgraph-agent-hooks/architecture.png) |
| Strands Agent Memory Tool | Culinary Assistant  | Food recommendation agent that learns dietary preferences and cooking styles            | [culinary-assistant.ipynb](./01-single-agent/using-strands-agent-memory-tool/culinary-assistant.ipynb)         | [View](./01-single-agent/using-strands-agent-memory-tool/architecture.png)                 |
| Multi-Agent               | Agent Collaboration | Travel Assistant with multiple agents sharing and utilizing long-term memory strategies | [travel-booking-assistant.ipynb](./02-multi-agent/with-strands-agent/travel-booking-assistant.ipynb)           | [View](./02-multi-agent/with-strands-agent/architecture.png)                               |

| 集成方法                   | 用例                | 描述                                                                                     | 笔记本                                                                                                          | 架构                                                                                        |
| ------------------------- | ------------------- | --------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| Strands Agent 钩子        | 客户支持             | 具有语义和偏好记忆策略的完整支持系统                                                        | [customer-support.ipynb](./01-single-agent/using-strands-agent-hooks/customer-support/customer-support.ipynb)  | [查看](./01-single-agent/using-strands-agent-hooks/customer-support/architecture.png)      |
| Strands Agent 钩子        | 数学助手             | 记住用户学习偏好和进度的数学辅导助手                                                        | [math-assistant.ipynb](./01-single-agent/using-strands-agent-hooks/simple-math-assistant/math-assistant.ipynb) | [查看](./01-single-agent/using-strands-agent-hooks/simple-math-assistant/architecture.png) |
| LangGraph Agent 钩子      | 营养助手             | 保存用户饮食偏好和健康目标以提供个性化建议的营养顾问                                          | [nutrition-assistant-with-user-preference-saving.ipynb](./01-single-agent/using-langgraph-agent-hooks/nutrition-assistant-with-user-preference-saving.ipynb) | [查看](./01-single-agent/using-langgraph-agent-hooks/architecture.png) |
| Strands Agent 记忆工具    | 烹饪助手             | 学习饮食偏好和烹饪风格的美食推荐代理                                                        | [culinary-assistant.ipynb](./01-single-agent/using-strands-agent-memory-tool/culinary-assistant.ipynb)         | [查看](./01-single-agent/using-strands-agent-memory-tool/architecture.png)                 |
| 多代理                    | 代理协作             | 具有多个代理共享和利用长期记忆策略的旅行助手                                                 | [travel-booking-assistant.ipynb](./02-multi-agent/with-strands-agent/travel-booking-assistant.ipynb)           | [查看](./02-multi-agent/with-strands-agent/architecture.png)                               |

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

## Best Practices

## 最佳实践

1. **Strategy Selection**: Choose appropriate strategies based on your use case requirements
2. **Namespace Design**: Plan namespace hierarchies for efficient information organization
3. **Extraction Tuning**: Customize extraction prompts for domain-specific information
4. **Performance Monitoring**: Track memory extraction quality and retrieval performance
5. **Privacy Considerations**: Implement appropriate data retention and privacy policies

1. **策略选择**：根据您的用例需求选择适当的策略
2. **命名空间设计**：规划命名空间层级以高效组织信息
3. **提取调优**：为特定领域信息自定义提取提示
4. **性能监控**：跟踪记忆提取质量和检索性能
5. **隐私考虑**：实施适当的数据保留和隐私政策

## Next Steps

## 下一步

After mastering long-term memory strategies, explore:

掌握长期记忆策略后，探索：

- Combining short-term and long-term memory for comprehensive agent experiences
- Advanced custom strategy configurations
- Multi-agent memory sharing patterns
- Production deployment considerations

- 结合短期和长期记忆以获得全面的代理体验
- 高级自定义策略配置
- 多代理记忆共享模式
- 生产部署注意事项
