# Culinary Assistant with Self-Managed Memory Strategy (With Citations)

# 带有自管理记忆策略的烹饪助手（带引用）

This sample demonstrates Amazon Bedrock AgentCore's self-managed memory strategy with enhanced citation tracking. This version extends the base culinary assistant example by adding comprehensive citation information to extracted long-term memories.

此示例演示了 Amazon Bedrock AgentCore 的自管理记忆策略，并增强了引用跟踪功能。此版本通过为提取的长期记忆添加全面的引用信息来扩展基础烹饪助手示例。

## What's Different

## 有何不同

This sample adds citation functionality to track the source of extracted memories:

此示例添加了引用功能来跟踪提取记忆的来源：

### Citation Features

### 引用功能

1. **Source Tracking**: Each extracted memory includes metadata about its origin:
   - Session ID and Actor ID
   - Starting and ending timestamps
   - S3 URI where the original short-term memory payload is stored
   - Extraction job ID

1. **来源跟踪**：每个提取的记忆包含关于其来源的元数据：
   - 会话 ID 和参与者 ID
   - 开始和结束时间戳
   - 存储原始短期记忆有效负载的 S3 URI
   - 提取作业 ID

2. **Citation Metadata**: Structured citation information is stored in the memory metadata:

2. **引用元数据**：结构化的引用信息存储在记忆元数据中：

   ```python
   citation_info = {
       'source_type': 'short_term_memory',
       'session_id': session_id,
       'actor_id': actor_id,
       'starting_timestamp': starting_timestamp,
       'ending_timestamp': timestamp,
       's3_uri': s3_location,
       's3_payload_location': s3_location,
       'extraction_job_id': job_id
   }
   ```

3. **Human-Readable Citations**: Each memory content includes an appended citation text:

3. **人类可读的引用**：每个记忆内容包含附加的引用文本：

   ```
   [Citation: Extracted from session {session_id}, actor {actor_id}, source: {s3_location}, job: {job_id}, timestamp: {timestamp}]
   ```

### Modified Files

### 修改的文件

#### `lambda_function.py`

The key changes are in the `MemoryExtractor` class:

主要更改在 `MemoryExtractor` 类中：

- `extract_memories()` method now accepts `s3_location` and `job_id` parameters
- `_format_extracted_memories()` method builds citation information and appends it to memory content
- Enhanced logging to track citation information

- `extract_memories()` 方法现在接受 `s3_location` 和 `job_id` 参数
- `_format_extracted_memories()` 方法构建引用信息并将其附加到记忆内容
- 增强的日志记录以跟踪引用信息

**Key Method**: `_format_extracted_memories` (line 97)
This method formats extracted memories with metadata and citation information, creating a traceable link from long-term memories back to their source in short-term memory.

**关键方法**：`_format_extracted_memories`（第 97 行）
此方法使用元数据和引用信息格式化提取的记忆，创建从长期记忆追溯到其短期记忆来源的可追踪链接。

#### `agentcore_self_managed_memory_demo.ipynb`

Updated to demonstrate the citation functionality in action, showing how extracted memories now include source attribution.

更新以演示引用功能的实际应用，展示提取的记忆现在如何包含来源归属。

## Use Cases

## 用例

This citation-enhanced version is particularly useful for:

此引用增强版本特别适用于：

1. **Audit Trails**: Maintaining a complete record of where memories originated
2. **Debugging**: Tracing back to the original conversation context
3. **Compliance**: Meeting requirements for data lineage and source attribution
4. **Memory Verification**: Ability to verify memory content against original source in S3

1. **审计跟踪**：维护记忆来源的完整记录
2. **调试**：追溯到原始对话上下文
3. **合规**：满足数据血统和来源归属的要求
4. **记忆验证**：能够根据 S3 中的原始来源验证记忆内容

## Prerequisites

## 前提条件

Same as the base culinary assistant example:
- Python 3.11+
- AWS credentials configured
- Amazon Bedrock access with Claude models
- Required AWS services: Lambda, S3, SNS, SQS

与基础烹饪助手示例相同：
- Python 3.11+
- 已配置 AWS 凭证
- 具有 Claude 模型的 Amazon Bedrock 访问权限
- 所需 AWS 服务：Lambda、S3、SNS、SQS

## Setup

## 设置

Follow the same setup process as the base culinary assistant example. The notebook will guide you through:

按照与基础烹饪助手示例相同的设置过程进行。笔记本将指导您完成：

1. Creating the Lambda function with citation support
2. Setting up the memory strategy with trigger conditions
3. Testing the enhanced citation functionality

1. 创建带有引用支持的 Lambda 函数
2. 设置带有触发条件的记忆策略
3. 测试增强的引用功能

## Comparison with Base Sample

## 与基础示例的比较

| Feature | Base Sample | With Citations |
|---------|------------|----------------|
| Memory extraction | ✅ | ✅ |
| S3 payload tracking | ❌ | ✅ |
| Source attribution | ❌ | ✅ |
| Job ID tracking | ❌ | ✅ |
| Timestamp context | ❌ | ✅ |
| Citation metadata | ❌ | ✅ |

| 功能 | 基础示例 | 带引用 |
|---------|------------|----------------|
| 记忆提取 | ✅ | ✅ |
| S3 有效负载跟踪 | ❌ | ✅ |
| 来源归属 | ❌ | ✅ |
| 作业 ID 跟踪 | ❌ | ✅ |
| 时间戳上下文 | ❌ | ✅ |
| 引用元数据 | ❌ | ✅ |

## Documentation

## 文档

For more information about self-managed memory strategies, see the [Amazon Bedrock AgentCore documentation](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/memory-self-managed-strategies.html).

有关自管理记忆策略的更多信息，请参阅 [Amazon Bedrock AgentCore 文档](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/memory-self-managed-strategies.html)。
