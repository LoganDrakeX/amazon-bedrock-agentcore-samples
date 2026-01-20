# LlamaIndex with AWS Bedrock AgentCore Memory Integration

# LlamaIndex 与 AWS Bedrock AgentCore Memory 集成

This project showcases enterprise-grade AI agents with persistent memory capabilities, demonstrating how LlamaIndex's ReAct framework integrates seamlessly with AWS Bedrock AgentCore Memory to create intelligent systems that learn, adapt, and evolve over time. Unlike traditional stateless agents, these implementations maintain contextual awareness across sessions, enabling sophisticated longitudinal analysis, cross-reference capabilities, and cumulative knowledge building that transforms how AI agents operate in professional environments.

本项目展示了具有持久记忆能力的企业级 AI 代理，演示了 LlamaIndex 的 ReAct 框架如何与 AWS Bedrock AgentCore Memory 无缝集成，以创建能够学习、适应和随时间演变的智能系统。与传统的无状态代理不同，这些实现在会话之间保持上下文感知，支持复杂的纵向分析、交叉引用能力和累积知识构建，从而改变 AI 代理在专业环境中的运作方式。

## 🚀 Key Features

## 🚀 主要特性

- **Native LlamaIndex Integration**: Direct memory passing with `agent.run(message, memory=agentcore_memory)`
- **Domain-Specific Examples**: Academic Research, Legal Document Analysis, Medical Knowledge, Investment Portfolio Management
- **Comprehensive Testing**: 8-10 systematic test cases per example with expected validation
- **Short & Long-term Memory**: Complete coverage of both memory types
- **Enterprise-Ready**: Simple, explicit APIs suitable for production environments

- **原生 LlamaIndex 集成**：通过 `agent.run(message, memory=agentcore_memory)` 直接传递记忆
- **特定领域示例**：学术研究、法律文档分析、医学知识、投资组合管理
- **全面测试**：每个示例包含 8-10 个系统化测试用例，带有预期验证
- **短期和长期记忆**：完整覆盖两种记忆类型
- **企业级就绪**：适用于生产环境的简单、显式 API

## 📁 Project Structure

## 📁 项目结构

```
├── 01-short-term-memory/
│   ├── academic-research-assistant-short-term-memory-tutorial.ipynb
│   ├── legal-document-analyzer-short-term-memory-tutorial.ipynb
│   ├── medical-knowledge-assistant-short-term-memory-tutorial.ipynb
│   └── investment-portfolio-advisor-short-term-memory-tutorial.ipynb
├── 02-long-term-memory/
│   ├── academic-research-assistant-long-term-memory-tutorial.ipynb
│   ├── legal-document-analyzer-long-term-memory-tutorial.ipynb
│   ├── medical-knowledge-assistant-long-term-memory-tutorial.ipynb
│   └── investment-portfolio-advisor-long-term-memory-tutorial.ipynb
└── requirements.txt
```

## 🎯 Use Cases

## 🎯 用例

### Academic Research Assistant

### 学术研究助手

- **Short-term**: Paper analysis, research synthesis within single session
- **Long-term**: Cross-session research evolution, grant proposal support over months
- **Memory Intelligence**: Tracks research themes, citation networks, and methodology evolution
- **Testing**: 8 comprehensive tests including contextual reasoning and cross-reference validation

- **短期**：单会话内的论文分析、研究综合
- **长期**：跨会话研究演进、数月内的基金申请支持
- **记忆智能**：跟踪研究主题、引用网络和方法论演变
- **测试**：8 个全面测试，包括上下文推理和交叉引用验证

### Legal Document Analyzer

### 法律文档分析器

- **Short-term**: Contract analysis, risk assessment, compliance checking
- **Long-term**: Multi-case precedent tracking, legal knowledge accumulation (12-month retention)
- **Memory Intelligence**: Builds case law database, tracks regulatory changes, maintains client history
- **Testing**: 9 systematic tests including precedent application and regulatory compliance

- **短期**：合同分析、风险评估、合规检查
- **长期**：多案例先例跟踪、法律知识积累（12 个月保留）
- **记忆智能**：构建判例法数据库、跟踪监管变化、维护客户历史
- **测试**：9 个系统测试，包括先例应用和监管合规

### Medical Knowledge Assistant

### 医学知识助手

- **Short-term**: Patient consultation, drug interactions, clinical guidelines
- **Long-term**: Longitudinal patient care, treatment outcomes, population health trends
- **Memory Intelligence**: Maintains patient histories, tracks treatment efficacy, learns from outcomes
- **Testing**: 10 comprehensive tests including clinical reasoning and treatment planning

- **短期**：患者咨询、药物相互作用、临床指南
- **长期**：纵向患者护理、治疗结果、人群健康趋势
- **记忆智能**：维护患者病史、跟踪治疗效果、从结果中学习
- **测试**：10 个全面测试，包括临床推理和治疗计划

### Investment Portfolio Advisor

### 投资组合顾问

- **Short-term**: Client profiling, portfolio analysis, investment recommendations
- **Long-term**: Multi-quarter performance tracking (Q1→Q2→Q3→Q4), market intelligence, wealth management
- **Memory Intelligence**: Tracks $3.2M→$3.45M portfolio evolution, market timing decisions, thesis adaptation
- **Testing**: 10 systematic tests including quarterly performance attribution and multi-year investment journey analysis

- **短期**：客户画像、投资组合分析、投资建议
- **长期**：多季度业绩跟踪（Q1→Q2→Q3→Q4）、市场情报、财富管理
- **记忆智能**：跟踪 320 万→345 万美元投资组合演变、市场时机决策、投资论点调整
- **测试**：10 个系统测试，包括季度业绩归因和多年投资旅程分析

## 🏗️ System Architecture

## 🏗️ 系统架构

*Architecture diagram will be added here*

*架构图将在此处添加*

## 🛠️ Prerequisites

## 🛠️ 前提条件

- Python 3.10+
- AWS account with Bedrock AgentCore Memory permissions
- AWS CLI configured with appropriate credentials
- Access to Claude 3.7 Sonnet inference profile (`us.anthropic.claude-3-7-sonnet-20250219-v1:0`)

- Python 3.10+
- 具有 Bedrock AgentCore Memory 权限的 AWS 账户
- 配置了适当凭证的 AWS CLI
- 访问 Claude 3.7 Sonnet 推理配置文件（`us.anthropic.claude-3-7-sonnet-20250219-v1:0`）

## 📦 Installation

## 📦 安装

```bash
# Install all dependencies including Jupyter
pip install -r requirements.txt

# Alternative: Install Jupyter separately
pip install jupyter ipykernel
```

## 🚀 Quick Start

## 🚀 快速开始

1. **Configure AWS credentials:**

1. **配置 AWS 凭证：**

   ```bash
   aws configure
   ```

2. **Choose a tutorial and open the notebook:**

2. **选择教程并打开笔记本：**

   ```bash
   jupyter notebook 01-short-term-memory/academic-research-assistant-short-term-memory-tutorial.ipynb
   ```

3. **Follow the step-by-step tutorial** with comprehensive testing

3. **按照分步教程操作**，包含全面测试

## 🏗️ Key Benefits

## 🏗️ 主要优势

- ✅ **Explicit Control**: Direct memory parameter vs hidden automation
- ✅ **Easy Debugging**: Visible memory operations vs background hooks
- ✅ **Simple API**: `agent.run(message, memory=memory)` vs complex setup
- ✅ **Comprehensive Testing**: Systematic validation with expected results
- ✅ **Domain Expertise**: Specialized use cases vs generic examples

- ✅ **显式控制**：直接记忆参数 vs 隐藏自动化
- ✅ **易于调试**：可见的记忆操作 vs 后台钩子
- ✅ **简单 API**：`agent.run(message, memory=memory)` vs 复杂设置
- ✅ **全面测试**：带有预期结果的系统验证
- ✅ **领域专业知识**：专业用例 vs 通用示例

## 📊 Testing Methodology

## 📊 测试方法

Each notebook includes **8-10 systematic tests** with clear validation:

每个笔记本包含 **8-10 个系统测试**，带有清晰的验证：

### Test Categories

### 测试类别

- **Test 1-2: Memory Storage** - Verify information persistence and tool integration
- **Test 3-4: Context Recall** - Validate identity, metrics, and detailed information retrieval
- **Test 5-6: Reasoning & Synthesis** - Test cross-reference capabilities and knowledge synthesis
- **Test 7-8: Practical Application** - Real-world scenario validation (grant proposals, case analysis)
- **Test 9-10: Session Boundaries** - Memory isolation and cross-session behavior verification

- **测试 1-2：记忆存储** - 验证信息持久化和工具集成
- **测试 3-4：上下文回忆** - 验证身份、指标和详细信息检索
- **测试 5-6：推理与综合** - 测试交叉引用能力和知识综合
- **测试 7-8：实际应用** - 真实场景验证（基金申请、案例分析）
- **测试 9-10：会话边界** - 记忆隔离和跨会话行为验证

### Validation Approach

### 验证方法

- **✅ Expected Results**: Each test shows expected outputs for comparison
- **🎯 Success Criteria**: Clear pass/fail indicators with specific metrics
- **📊 Progressive Complexity**: Tests build from basic recall to advanced reasoning
- **🔍 Edge Case Testing**: Session boundaries, memory limits, and error handling

- **✅ 预期结果**：每个测试显示预期输出以供比较
- **🎯 成功标准**：带有具体指标的明确通过/失败指示器
- **📊 渐进复杂性**：测试从基本回忆构建到高级推理
- **🔍 边缘案例测试**：会话边界、记忆限制和错误处理

### Example Test Pattern

### 示例测试模式

```python
# Test 4: Detailed Metrics Recall
response = await agent.run("What were the exact accuracy percentages?", memory=memory)
print("📊 Result:", response)
print("✅ Expected: Zhang et al - CNNs 95.2%, Johnson et al - BERT 89.1%")
# Users can verify: Does response contain both accuracy numbers?
```

## 🔧 Technical Overview

## 🔧 技术概述

**Key Long-Term Memory Components:**

**长期记忆关键组件：**

1. **Semantic Strategy Configuration**: Uses SemanticStrategy for automatic insight extraction with 365-day retention
2. **Cross-Session Persistence**: Same actor_id + memory_id, different session_id per period enables knowledge continuity
3. **Custom Memory Search Tool**: Wraps AgentCore's native search_long_term_memories() in LlamaIndex FunctionTool
4. **Semantic Processing Pipeline**: 90-120 second wait for conversational events → semantic memories conversion
5. **Dynamic Session Management**: Uses memory.context.session_id for flexible session handling

1. **语义策略配置**：使用 SemanticStrategy 进行自动洞察提取，365 天保留
2. **跨会话持久化**：相同的 actor_id + memory_id，每个时期不同的 session_id 实现知识连续性
3. **自定义记忆搜索工具**：在 LlamaIndex FunctionTool 中封装 AgentCore 的原生 search_long_term_memories()
4. **语义处理管道**：90-120 秒等待对话事件 → 语义记忆转换
5. **动态会话管理**：使用 memory.context.session_id 进行灵活的会话处理

## 🔧 Memory Configuration

## 🔧 记忆配置

### Short-term Memory

### 短期记忆

```python
context = AgentCoreMemoryContext(
    actor_id="user-id",
    memory_id=memory_id,
    session_id="session-id",
    namespace="/domain-specific"
)
agentcore_memory = AgentCoreMemory(context=context)
```

### Long-term Memory (12-Month Retention)

### 长期记忆（12 个月保留）

```python
# Cross-session persistence with semantic strategy
memory = memory_manager.get_or_create_memory(
    name='DomainSpecificLongTerm',
    strategies=[SemanticStrategy(name="domainLongTermMemory")],
    event_expiry_days=365  # 12-month retention
)

# Same context across sessions for persistence
context = AgentCoreMemoryContext(
    actor_id="advisor-id",      # Same actor across sessions
    memory_id=memory_id,        # Same memory store
    session_id="q1-session",    # Different per interaction
    namespace="/domain-specific"
)
```

### Memory Intelligence Examples

### 记忆智能示例

- **Investment Advisor**: Tracks quarterly performance (Q1: +8.2% → Q2: -2.1% → Q3: recovery)
- **Legal Analyzer**: Maintains precedent database across cases and regulatory changes
- **Medical Assistant**: Builds longitudinal patient care records and treatment outcomes
- **Research Assistant**: Evolves research themes and methodology insights over months

- **投资顾问**：跟踪季度业绩（Q1: +8.2% → Q2: -2.1% → Q3: 恢复）
- **法律分析器**：跨案例和监管变化维护先例数据库
- **医学助手**：构建纵向患者护理记录和治疗结果
- **研究助手**：在数月内演进研究主题和方法论洞察

## 🤝 Contributing

## 🤝 贡献

This project demonstrates best practices for LlamaIndex + AgentCore Memory integration. Contributions welcome for:

本项目展示了 LlamaIndex + AgentCore Memory 集成的最佳实践。欢迎贡献：

- Additional domain examples
- Enhanced testing methodologies
- Performance optimizations
- Documentation improvements

- 额外的领域示例
- 增强的测试方法
- 性能优化
- 文档改进

## 📄 License

## 📄 许可证

This project is licensed under the MIT License.

本项目根据 MIT 许可证授权。

## 🙋‍♂️ Support

## 🙋‍♂️ 支持

For questions about:
- **LlamaIndex Integration**: Refer to domain-specific notebooks
- **AgentCore Memory**: Check AWS Bedrock documentation
- **Testing Patterns**: Review comprehensive test examples

如有以下问题：
- **LlamaIndex 集成**：参考特定领域笔记本
- **AgentCore Memory**：查看 AWS Bedrock 文档
- **测试模式**：查看全面的测试示例

