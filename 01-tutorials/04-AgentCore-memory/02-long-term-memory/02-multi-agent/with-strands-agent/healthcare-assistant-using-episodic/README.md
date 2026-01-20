# Multi-Agent Healthcare System with Episodic Memory

# 具有情景记忆的多代理医疗系统

Episodic memory captures meaningful interaction slices, identifying important moments and summarizing them into compact, organized records for focused retrieval without noise.

情景记忆捕获有意义的交互片段，识别重要时刻并将其总结为紧凑、有组织的记录，以便进行无噪声的集中检索。

Reflections analyze episodes to surface insights, patterns, and conclusions—helping the system understand why events matter and how they should influence future behavior, turning experience into actionable guidance.

反思分析情景以呈现洞察、模式和结论——帮助系统理解事件为何重要以及它们应该如何影响未来行为，将经验转化为可操作的指导。

A comprehensive example demonstrating **multi-agent coordination with episodic memory** using Amazon Bedrock AgentCore Memory. This tutorial shows how AI agents can learn from past interactions and improve decision-making over time.

一个使用 Amazon Bedrock AgentCore Memory 演示**具有情景记忆的多代理协调**的综合示例。本教程展示了 AI 代理如何从过去的交互中学习并随时间改进决策。

## Overview

## 概述

This tutorial showcases a healthcare assistant system with:
- **Supervisor Agent**: Routes patient questions to specialized agents
- **Claims Agent**: Handles insurance claims and billing queries
- **Demographics Agent**: Manages patient demographic information
- **Medication Agent**: Handles medication and prescription queries

本教程展示了一个医疗助手系统，包括：
- **监督代理**：将患者问题路由到专业代理
- **理赔代理**：处理保险理赔和账单查询
- **人口统计代理**：管理患者人口统计信息
- **药物代理**：处理药物和处方查询

Each agent maintains isolated short-term memory through **memory branching**, while sharing long-term insights through **episodic memory strategies**.

每个代理通过**记忆分支**维护隔离的短期记忆，同时通过**情景记忆策略**共享长期洞察。

## Architecture

## 架构

<div style="text-align:left">
    <img src="architecture.png" width="75%" />
</div>

## Memory Strategy

## 记忆策略

### Episodic

### 情景记忆

The system uses a episodic memory strategy with:

系统使用情景记忆策略，包括：

**Extraction**: Converts conversation events into structured episodes
- Prompt: "Extract patient interactions with healthcare agents"
- Namespace: `healthcare/{actorId}/{sessionId}`

**提取**：将对话事件转换为结构化情景
- 提示："提取患者与医疗代理的交互"
- 命名空间：`healthcare/{actorId}/{sessionId}`

**Consolidation**: Merges related episodes
- Prompt: "Consolidate healthcare conversations"

**整合**：合并相关情景
- 提示："整合医疗对话"

**Reflection**: Generates cross-session insights
- Prompt: "Generate insights from patient care patterns"
- Namespace: `healthcare/{actorId}` (exact namespace prefix)

**反思**：生成跨会话洞察
- 提示："从患者护理模式生成洞察"
- 命名空间：`healthcare/{actorId}`（精确的命名空间前缀）

### Memory Branching

### 记忆分支

Each agent operates on its own memory branch:
- `main`: Supervisor agent routing decisions
- `claims_agent`: Insurance and billing conversations
- `demographics_agent`: Patient information updates
- `medication_agent`: Prescription discussions

每个代理在自己的记忆分支上运行：
- `main`：监督代理路由决策
- `claims_agent`：保险和账单对话
- `demographics_agent`：患者信息更新
- `medication_agent`：处方讨论

**Benefits:**
- Agents don't see each other's conversations
- Clean separation of concerns
- All agents contribute to shared long-term memory
- Patient-level insights span all interactions

**优势：**
- 代理看不到彼此的对话
- 关注点清晰分离
- 所有代理都为共享的长期记忆做出贡献
- 患者级别的洞察跨越所有交互

## Prerequisites

## 前提条件

### AWS Services

### AWS 服务

- **Amazon Bedrock**: Access to Claude Sonnet 4 model
- **Amazon Bedrock AgentCore Memory**: For episodic memory strategy
- **Amazon HealthLake** (optional): FHIR datastore with patient data
  - Can create new datastore with Synthea data during setup
  - Or use existing datastore

- **Amazon Bedrock**：访问 Claude Sonnet 4 模型
- **Amazon Bedrock AgentCore Memory**：用于情景记忆策略
- **Amazon HealthLake**（可选）：带有患者数据的 FHIR 数据存储
  - 可以在设置期间使用 Synthea 数据创建新的数据存储
  - 或使用现有数据存储

### IAM Permissions

### IAM 权限

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "bedrock:InvokeModel",
        "bedrock:InvokeModelWithResponseStream"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "healthlake:DescribeFHIRDatastore",
        "healthlake:CreateFHIRDatastore",
        "healthlake:ReadResource",
        "healthlake:SearchWithGet"
      ],
      "Resource": "*"
    }
  ]
}
```

### Python Environment

### Python 环境

- Python 3.10+
- Jupyter Notebook or JupyterLab

- Python 3.10+
- Jupyter Notebook 或 JupyterLab

## Installation

## 安装

1. Install dependencies:

1. 安装依赖：

```bash
pip install -r requirements.txt
```

2. Configure AWS credentials:

2. 配置 AWS 凭证：

```bash
aws configure
```

## Usage

## 使用方法

### Quick Start

### 快速开始

1. Open the notebook:

1. 打开笔记本：

```bash
jupyter notebook healthcare-data-assistant.ipynb
```

2. Run cells sequentially:
   - **Step 1**: Environment Setup
   - **Step 2**: Configure HealthLake Datastore
   - **Step 3**: Create Memory as tool for Long-term memory with Episodic Strategy
   - **Step 4**: Create Memory Hook Provider with Branch Support for Short-term memory
   - **Step 5**: Create Multi-Agent Healthcare Architecture with Memory Branching
   - **Step 6**: Test with interactive chat
   - **Step 7**: Inspect healthcare memory branches
   - **Step 8**: Validate long-term memory (episodes and reflections)

2. 按顺序运行单元格：
   - **第一步**：环境设置
   - **第二步**：配置 HealthLake 数据存储
   - **第三步**：创建具有情景策略的长期记忆工具
   - **第四步**：创建支持分支的短期记忆钩子提供程序
   - **第五步**：创建具有记忆分支的多代理医疗架构
   - **第六步**：使用交互式聊天进行测试
   - **第七步**：检查医疗记忆分支
   - **第八步**：验证长期记忆（情景和反思）

### Interactive Inputs

### 交互式输入

The notebook prompts for:
- **HealthLake datastore ID**: Existing datastore or create new with Synthea data ( no real patient information is used)
- **HealthLake region**: AWS region for HealthLake

笔记本会提示输入：
- **HealthLake 数据存储 ID**：现有数据存储或使用 Synthea 数据创建新的（不使用真实患者信息）
- **HealthLake 区域**：HealthLake 的 AWS 区域

### Testing the System

### 测试系统

The interactive chat (Step 7) allows you to:
- Ask about insurance claims
- Request demographic information
- Query medications and prescriptions
- See supervisor routing in action
- Observe memory branching

交互式聊天（第六步）允许您：
- 询问保险理赔
- 请求人口统计信息
- 查询药物和处方
- 查看监督代理路由的实际操作
- 观察记忆分支

Example questions:

示例问题：

```
You: What's the status of my insurance claim?
You: Can you tell me about my medications?
You: What's my current address on file?
```

Type `quit`, `exit`, or `q` to end the chat session.

输入 `quit`、`exit` 或 `q` 结束聊天会话。

## Memory Browser Integration

## 记忆浏览器集成

After running the notebook, you can visualize the memory using the memory browser:

运行笔记本后，您可以使用记忆浏览器可视化记忆：

1. Note the Memory ID from the configuration summary
2. Open - [Memory Browser](https://github.com/awslabs/amazon-bedrock-agentcore-samples/tree/main/01-tutorials/04-AgentCore-memory/03-advanced-patterns/04-memory-browser) - Visualize and explore memory events, episodes, and reflections at `http://localhost:8000`
3. Enter the Memory ID to explore:
   - **Short-term memory**: Events by branch
   - **Episodes**: Session-level consolidated memories
   - **Reflections**: Patient-level insights

1. 从配置摘要中记下 Memory ID
2. 打开 - [记忆浏览器](https://github.com/awslabs/amazon-bedrock-agentcore-samples/tree/main/01-tutorials/04-AgentCore-memory/03-advanced-patterns/04-memory-browser) - 在 `http://localhost:8000` 可视化和探索记忆事件、情景和反思
3. 输入 Memory ID 进行探索：
   - **短期记忆**：按分支的事件
   - **情景**：会话级别的整合记忆
   - **反思**：患者级别的洞察

**⏱️ Note**: Episode and reflection generation takes 10-15 minutes after conversations. Check back later if no episodes/reflections appear immediately.

**⏱️ 注意**：情景和反思生成在对话后需要 10-15 分钟。如果没有立即出现情景/反思，请稍后再查看。

## Key Concepts Demonstrated

## 演示的关键概念

### 1. Multi-Agent Coordination

### 1. 多代理协调

- Supervisor pattern for routing
- Specialized agents with domain expertise
- Dynamic tool usage for real-time data

- 用于路由的监督模式
- 具有领域专业知识的专业代理
- 用于实时数据的动态工具使用

### 2. Memory Branching

### 2. 记忆分支

- Isolated conversations per agent
- Branch-specific event storage
- Shared session context

- 每个代理的隔离对话
- 特定于分支的事件存储
- 共享会话上下文

### 3. Episodic Memory

### 3. 情景记忆

- extraction, consolidation, and reflection prompts
- Session-level episodes
- Patient-level reflections

- 提取、整合和反思提示
- 会话级别的情景
- 患者级别的反思

### 4. HealthLake Integration

### 4. HealthLake 集成

- Dynamic FHIR queries
- Real-time patient data access
- All data is synthetic (generated by Synthea) - no real patient information is used

- 动态 FHIR 查询
- 实时患者数据访问
- 所有数据都是合成的（由 Synthea 生成）- 不使用真实患者信息

## Customization

## 自定义

### Adding New Agents

### 添加新代理

```python
@tool
def get_patient_allergies(patient_id: str = PATIENT_ID) -> dict:
    """Get patient allergies from HealthLake"""
    return query_healthlake('AllergyIntolerance', {'patient': patient_id})

allergy_agent = Agent(
    model="global.anthropic.claude-sonnet-4-20250514-v1:0",
    system_prompt="You handle patient allergies. Use get_patient_allergies tool.",
    tools=[get_patient_allergies]
)
```

### Using Different Models

### 使用不同模型

Change the `model` parameter in agent creation:

在创建代理时更改 `model` 参数：

```python
Agent(
    model="anthropic.claude-3-5-sonnet-20241022-v2:0",  # Different model
    system_prompt="...",
    tools=[...]
)
```

## Additional Resources

## 其他资源

- [Episodic Memory Best Practices](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/episodic-memory-strategy.html#memory-episodic-retrieve-episodes) - Learn how to retrieve episodes to improve agentic performance

- [情景记忆最佳实践](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/episodic-memory-strategy.html#memory-episodic-retrieve-episodes) - 了解如何检索情景以提高代理性能

## Troubleshooting

## 故障排除

### Branch Creation Errors

### 分支创建错误

If you see "Branch rootEventId is required when creating a branch":
- **Restart the Jupyter kernel** (Kernel → Restart)
- **Re-run all cells** from the beginning to reload the corrected `HealthcareMemoryHooks` class
- The fix ensures the main branch has an initial event before forking specialized agent branches

如果您看到 "Branch rootEventId is required when creating a branch"：
- **重启 Jupyter 内核**（Kernel → Restart）
- **从头开始重新运行所有单元格**以重新加载修正后的 `HealthcareMemoryHooks` 类
- 此修复确保主分支在分叉专业代理分支之前有一个初始事件

### Memory Hook Errors

### 记忆钩子错误

If you see "MemorySession.add_turns() got an unexpected keyword argument 'branch_name'":
- This indicates the notebook is using cached/old code
- **Restart the kernel** and re-run all cells to pick up the API fixes
- The corrected code uses `branch={"name": branch_name}` format

如果您看到 "MemorySession.add_turns() got an unexpected keyword argument 'branch_name'"：
- 这表明笔记本正在使用缓存的/旧代码
- **重启内核**并重新运行所有单元格以获取 API 修复
- 修正后的代码使用 `branch={"name": branch_name}` 格式

### Model Not Available

### 模型不可用

If you see "serviceUnavailableException", ensure:
- Using global inference profile: `global.anthropic.claude-sonnet-4-20250514-v1:0`
- Or region-specific profile for your region

如果您看到 "serviceUnavailableException"，请确保：
- 使用全局推理配置文件：`global.anthropic.claude-sonnet-4-20250514-v1:0`
- 或您所在区域的特定区域配置文件

### HealthLake Access Denied

### HealthLake 访问被拒绝

Verify IAM permissions include:
- `healthlake:DescribeFHIRDatastore`
- `healthlake:ReadResource`
- `healthlake:SearchWithGet`

验证 IAM 权限包括：
- `healthlake:DescribeFHIRDatastore`
- `healthlake:ReadResource`
- `healthlake:SearchWithGet`

### Memory Creation Failed

### 记忆创建失败

Check that:
- IAM role has Bedrock invoke permissions

检查：
- IAM 角色具有 Bedrock 调用权限

## Cleanup

## 清理

After completing the tutorial, you can clean up resources to avoid ongoing charges:

完成教程后，您可以清理资源以避免持续收费：

1. Run the **Cleanup** cell at the end of the notebook
2. You'll be prompted to delete:
   - **Memory**: AgentCore Memory instance
   - **HealthLake Datastore**: FHIR datastore (optional)

1. 在笔记本末尾运行**清理**单元格
2. 系统将提示您删除：
   - **记忆**：AgentCore Memory 实例
   - **HealthLake 数据存储**：FHIR 数据存储（可选）

Each resource can be deleted independently based on your needs.

每个资源可以根据您的需要独立删除。

### Manual Cleanup

### 手动清理

If needed, you can also delete resources manually:

如果需要，您也可以手动删除资源：

```bash
# Delete memory
aws bedrock-agentcore-cp delete-memory --memory-id <MEMORY_ID> --region us-east-1

# Delete HealthLake datastore
aws healthlake delete-fhir-datastore --datastore-id <DATASTORE_ID> --region <REGION>
```

## Learn More

## 了解更多

- [AgentCore Memory Documentation](https://docs.aws.amazon.com/bedrock/latest/userguide/agentcore-memory.html)
- [Strands Agents Guide](https://strandsagents.com)
- [HealthLake FHIR API](https://docs.aws.amazon.com/healthlake/latest/devguide/working-with-FHIR-healthlake.html)
- [Memory Branching Patterns](https://docs.aws.amazon.com/bedrock/latest/userguide/agentcore-memory-branching.html)

- [AgentCore Memory 文档](https://docs.aws.amazon.com/bedrock/latest/userguide/agentcore-memory.html)
- [Strands Agents 指南](https://strandsagents.com)
- [HealthLake FHIR API](https://docs.aws.amazon.com/healthlake/latest/devguide/working-with-FHIR-healthlake.html)
- [记忆分支模式](https://docs.aws.amazon.com/bedrock/latest/userguide/agentcore-memory-branching.html)
