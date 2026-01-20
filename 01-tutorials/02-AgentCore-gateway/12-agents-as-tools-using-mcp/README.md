# AWS re:Invent 2025 AIML301: Build End-to-End SRE Usecase with Bedrock AgentCore

# AWS re:Invent 2025 AIML301：使用 Bedrock AgentCore 构建端到端 SRE 用例

## Overview

## 概述

This workshop demonstrates how Site Reliability Engineers (SREs) can leverage Amazon Bedrock AgentCore to automate incident response, from diagnostics through remediation to prevention.

本研讨会演示了站点可靠性工程师（SRE）如何利用 Amazon Bedrock AgentCore 自动化事件响应，从诊断到修复再到预防。

**Workshop Scenario:** A CRM application deployed on AWS (EC2 + NGINX + DynamoDB) experiences faults. You will build a multi-agent system to diagnose issues, remediate them safely with approval workflows, and prevent recurrence through research and best practices.

**研讨会场景：** 部署在 AWS 上的 CRM 应用程序（EC2 + NGINX + DynamoDB）出现故障。您将构建一个多代理系统来诊断问题，通过审批工作流安全地修复问题，并通过研究和最佳实践防止问题再次发生。

## Learning Objectives

## 学习目标

By completing this workshop, you will:

完成本研讨会后，您将：

1. **Lab 1** - Verify prerequisites and set up a realistic CRM application stack with fault injection capabilities
2. **Lab 2** - Build a diagnostics agent that analyzes CloudWatch logs and metrics
3. **Lab 3a** - Create a remediation agent with approval workflows and code interpreter
4. **Lab 3b** - Implement fine-grained access control with custom Lambda interceptor
5. **Lab 4** - Implement a prevention agent using AgentCore Browser for research
6. **Lab 5** - Orchestrate all agents using a supervisor pattern with AgentCore Gateway and interactive Streamlit UI

1. **实验 1** - 验证先决条件并设置具有故障注入功能的真实 CRM 应用程序堆栈
2. **实验 2** - 构建分析 CloudWatch 日志和指标的诊断代理
3. **实验 3a** - 创建具有审批工作流和代码解释器的修复代理
4. **实验 3b** - 使用自定义 Lambda 拦截器实现细粒度访问控制
5. **实验 4** - 使用 AgentCore Browser 实现用于研究的预防代理
6. **实验 5** - 使用 AgentCore Gateway 和交互式 Streamlit UI 通过监督者模式编排所有代理

## Quick Start

## 快速开始

### Recommended Lab Flow

### 推荐的实验流程

```
Lab-01 (Prerequisites & Infrastructure)
   ↓
Lab-02 (Diagnostics Agent)
   ↓
Lab-03a (Remediation Agent)
   ↓
Lab-03b (Fine-Grained Access Control)
   ↓
Lab-04 (Prevention Agent)
   ↓
Lab-05 (Multi-Agent Orchestration + Streamlit UI)
```

```
实验-01（先决条件和基础设施）
   ↓
实验-02（诊断代理）
   ↓
实验-03a（修复代理）
   ↓
实验-03b（细粒度访问控制）
   ↓
实验-04（预防代理）
   ↓
实验-05（多代理编排 + Streamlit UI）
```

### Getting Started

### 开始使用

1. **Download the workshop** to your local machine
2. **Open Jupyter Notebook/Lab** in the workshop directory
3. **Start with `Lab-01-prerequisites-infra.ipynb`** and run all sections
4. **Follow labs sequentially** through Lab-05
5. **Clean up resources** when done using the cleanup cells in Lab-05

1. **下载研讨会**到您的本地机器
2. **在研讨会目录中打开 Jupyter Notebook/Lab**
3. **从 `Lab-01-prerequisites-infra.ipynb` 开始**并运行所有部分
4. **按顺序完成实验**直到实验-05
5. **完成后清理资源**使用实验-05 中的清理单元格

**⏱️ Estimated Time:**
- Complete workshop (Labs 1-5): **2 hours**

**⏱️ 预计时间：**
- 完成研讨会（实验 1-5）：**2 小时**

**✨ Everything happens within the notebook - no terminal commands needed!**

**✨ 所有操作都在笔记本中完成 - 无需终端命令！**

## How It Works

## 工作原理

### Everything Runs in Notebooks

### 所有内容都在笔记本中运行

- Open a notebook, run it from top to bottom
- All setup, configuration, and provisioning happens automatically
- No terminal commands needed
- Each notebook is self-contained
- Notebooks import helpers and utilities as needed

- 打开笔记本，从上到下运行
- 所有设置、配置和资源配置都自动完成
- 无需终端命令
- 每个笔记本都是独立的
- 笔记本根据需要导入辅助程序和工具

**Example of what happens inside a notebook:**
1. Install required Python packages via `pip install`
2. Configure AWS credentials and environment
3. Verify prerequisites
4. Provision AWS resources (EC2, DynamoDB, Lambda, etc.)
5. Implement and test agents
6. Inject faults for testing
7. Run diagnostics, remediation, or prevention workflows
8. Monitor results via CloudWatch
9. Clean up resources when done

**笔记本内部发生的示例：**
1. 通过 `pip install` 安装所需的 Python 包
2. 配置 AWS 凭证和环境
3. 验证先决条件
4. 配置 AWS 资源（EC2、DynamoDB、Lambda 等）
5. 实现和测试代理
6. 注入故障进行测试
7. 运行诊断、修复或预防工作流
8. 通过 CloudWatch 监控结果
9. 完成后清理资源

## Architecture

## 架构

The workshop implements a multi-agent system for automated incident response:

本研讨会实现了一个用于自动化事件响应的多代理系统：

![Architecture Diagram](architecture/architecture.png)

**Key Components:**

**关键组件：**

1. **CRM Application Stack**
   - EC2 instances running NGINX web servers
   - DynamoDB for data persistence
   - CloudWatch for logs and metrics

1. **CRM 应用程序堆栈**
   - 运行 NGINX Web 服务器的 EC2 实例
   - 用于数据持久化的 DynamoDB
   - 用于日志和指标的 CloudWatch

2. **Agent System**
   - **Diagnostics Agent**: Analyzes CloudWatch logs and metrics to identify issues
   - **Remediation Agent**: Executes fixes using Code Interpreter with approval workflows
   - **Prevention Agent**: Researches best practices using Browser tool
   - **Supervisor Agent**: Orchestrates all agents and manages workflow

2. **代理系统**
   - **诊断代理**：分析 CloudWatch 日志和指标以识别问题
   - **修复代理**：使用代码解释器执行修复，带有审批工作流
   - **预防代理**：使用浏览器工具研究最佳实践
   - **监督代理**：编排所有代理并管理工作流

3. **AgentCore Platform**
   - **Runtime**: Serverless deployment for agents
   - **Gateway**: MCP protocol for tool orchestration with JWT authentication
   - **Code Interpreter**: Safe execution environment for remediation scripts
   - **Browser**: Web research capabilities for prevention
   - **Memory**: Context persistence across interactions

3. **AgentCore 平台**
   - **Runtime**：代理的无服务器部署
   - **Gateway**：用于工具编排的 MCP 协议，带有 JWT 认证
   - **Code Interpreter**：用于修复脚本的安全执行环境
   - **Browser**：用于预防的 Web 研究功能
   - **Memory**：跨交互的上下文持久化

4. **Security & Access Control**
   - Cognito for user authentication
   - OAuth2 M2M for agent-to-agent communication
   - Lambda interceptor for fine-grained RBAC
   - JWT-based authorization

4. **安全和访问控制**
   - Cognito 用于用户认证
   - OAuth2 M2M 用于代理间通信
   - Lambda 拦截器用于细粒度 RBAC
   - 基于 JWT 的授权

5. **User Interface**
   - Streamlit web app for interactive agent interaction
   - Real-time streaming responses
   - Approval workflow integration

5. **用户界面**
   - Streamlit Web 应用程序用于交互式代理交互
   - 实时流式响应
   - 审批工作流集成

## Demo Video

## 演示视频

Watch the complete workshop walkthrough:

观看完整的研讨会演示：

![Workshop Demo](demo/aim301-multi-agent-mcp-agentcore-gateway.gif)

The demo shows:
- Setting up the CRM application infrastructure
- Injecting faults to simulate real incidents
- Running diagnostics to identify issues
- Executing remediation with approval workflows
- Researching prevention strategies
- Orchestrating all agents through the Streamlit UI

演示展示：
- 设置 CRM 应用程序基础设施
- 注入故障以模拟真实事件
- 运行诊断以识别问题
- 执行带有审批工作流的修复
- 研究预防策略
- 通过 Streamlit UI 编排所有代理

## Workshop Structure

## 研讨会结构

```
├── Lab-01-prerequisites-infra.ipynb             # Lab 1: Prerequisites & Infrastructure Setup
├── Lab-02-diagnostics-agent.ipynb               # Lab 2: Diagnostics Agent
├── Lab-03a-remediation-agent.ipynb              # Lab 3a: Remediation Agent + Approval
├── Lab-03b-remediation-agent-fgac.ipynb         # Lab 3b: Fine-Grained Access Control
├── Lab-04-prevention-agent.ipynb                # Lab 4: Prevention Agent
├── Lab-05-multi-agent-orchestration.ipynb       # Lab 5: Multi-Agent Orchestration + Streamlit
│
├── lab_helpers/                        # Helper modules imported by notebooks
│   ├── lab_01/                        # Lab 1 specific helpers
│   ├── lab_02/                        # Lab 2 specific helpers
│   ├── lab_03/                        # Lab 3 specific helpers
│   ├── lab_04/                        # Lab 4 specific helpers
│   ├── lab_05/                        # Lab 5 specific helpers (includes streamlit_app.py)
│   ├── constants.py                   # Configuration constants
│   ├── parameter_store.py             # AWS Parameter Store utilities
│   └── ...                            # Other shared utilities
├── requirements.txt                    # Python dependencies
└── README.md                           # This file
```

```
├── Lab-01-prerequisites-infra.ipynb             # 实验 1：先决条件和基础设施设置
├── Lab-02-diagnostics-agent.ipynb               # 实验 2：诊断代理
├── Lab-03a-remediation-agent.ipynb              # 实验 3a：修复代理 + 审批
├── Lab-03b-remediation-agent-fgac.ipynb         # 实验 3b：细粒度访问控制
├── Lab-04-prevention-agent.ipynb                # 实验 4：预防代理
├── Lab-05-multi-agent-orchestration.ipynb       # 实验 5：多代理编排 + Streamlit
│
├── lab_helpers/                        # 笔记本导入的辅助模块
│   ├── lab_01/                        # 实验 1 特定辅助程序
│   ├── lab_02/                        # 实验 2 特定辅助程序
│   ├── lab_03/                        # 实验 3 特定辅助程序
│   ├── lab_04/                        # 实验 4 特定辅助程序
│   ├── lab_05/                        # 实验 5 特定辅助程序（包括 streamlit_app.py）
│   ├── constants.py                   # 配置常量
│   ├── parameter_store.py             # AWS Parameter Store 工具
│   └── ...                            # 其他共享工具
├── requirements.txt                    # Python 依赖
└── README.md                           # 本文件
```

## Prerequisites

## 先决条件

Before starting, ensure you have:

开始之前，请确保您具备：

- Python 3.10 or higher
- Jupyter Notebook or JupyterLab installed
- AWS Account with permissions for EC2, DynamoDB, Lambda, CloudWatch, Bedrock
- AWS credentials configured locally (or will be set up in Lab 1)

- Python 3.10 或更高版本
- 已安装 Jupyter Notebook 或 JupyterLab
- 具有 EC2、DynamoDB、Lambda、CloudWatch、Bedrock 权限的 AWS 账户
- 本地配置的 AWS 凭证（或将在实验 1 中设置）

The `Lab-01-prerequisites-infra.ipynb` notebook will verify all these and install any missing dependencies.

`Lab-01-prerequisites-infra.ipynb` 笔记本将验证所有这些并安装任何缺失的依赖项。

## Lab Overview

## 实验概述

**Lab 1: Prerequisites & Infrastructure Setup**
- Verify Python version, AWS credentials, and dependencies
- Install workshop requirements and verify Bedrock access
- Deploy CRM application (EC2 + NGINX + DynamoDB)
- Set up Cognito for authentication
- Set up CloudWatch monitoring
- Create fault injection utilities

**实验 1：先决条件和基础设施设置**
- 验证 Python 版本、AWS 凭证和依赖项
- 安装研讨会要求并验证 Bedrock 访问
- 部署 CRM 应用程序（EC2 + NGINX + DynamoDB）
- 设置 Cognito 进行认证
- 设置 CloudWatch 监控
- 创建故障注入工具

**Lab 2: Diagnostics Agent**
- Build Strands agent to analyze CloudWatch logs
- Deploy Lambda function with diagnostic tools
- Create AgentCore Gateway with MCP protocol
- Test agent against real application logs

**实验 2：诊断代理**
- 构建 Strands 代理来分析 CloudWatch 日志
- 部署带有诊断工具的 Lambda 函数
- 创建带有 MCP 协议的 AgentCore Gateway
- 针对真实应用程序日志测试代理

**Lab 3a: Remediation Agent with Code Interpreter**
- Deploy agent to AgentCore Runtime
- Integrate AgentCore Code Interpreter for safe execution
- Implement OAuth2 M2M authentication
- Test remediation workflows

**实验 3a：带有代码解释器的修复代理**
- 将代理部署到 AgentCore Runtime
- 集成 AgentCore Code Interpreter 以实现安全执行
- 实现 OAuth2 M2M 认证
- 测试修复工作流

**Lab 3b: Fine-Grained Access Control**
- Create Lambda interceptor for request authorization
- Implement role-based access control (RBAC)
- Configure Cognito groups (Approvers vs SRE)
- Test access control with different user roles

**实验 3b：细粒度访问控制**
- 创建用于请求授权的 Lambda 拦截器
- 实现基于角色的访问控制（RBAC）
- 配置 Cognito 组（审批者 vs SRE）
- 使用不同用户角色测试访问控制

**Lab 4: Prevention Agent with Browser**
- Deploy Runtime agent with AgentCore Browser tool
- Research AWS documentation and best practices
- Generate prevention playbooks
- OAuth2 M2M authentication

**实验 4：带有浏览器的预防代理**
- 部署带有 AgentCore Browser 工具的 Runtime 代理
- 研究 AWS 文档和最佳实践
- 生成预防手册
- OAuth2 M2M 认证

**Lab 5: Multi-Agent Orchestration with Streamlit**
- Create supervisor agent to coordinate all three agents
- Set up central AgentCore Gateway with JWT authentication
- Reuse Lab 3b interceptor for RBAC
- Deploy multi-agent system
- Launch interactive Streamlit chat interface with real-time streaming
- Test end-to-end incident response workflow

**实验 5：使用 Streamlit 的多代理编排**
- 创建监督代理来协调所有三个代理
- 设置带有 JWT 认证的中央 AgentCore Gateway
- 重用实验 3b 的拦截器进行 RBAC
- 部署多代理系统
- 启动带有实时流式传输的交互式 Streamlit 聊天界面
- 测试端到端事件响应工作流

## Key Technologies

## 关键技术

- **Amazon Bedrock** - Foundation models (Claude 3.7 Sonnet)
- **AgentCore** - Serverless agent platform
  - Runtime (deployment)
  - Memory (context persistence)
  - Gateway (tool orchestration with JWT authentication)
  - Code Interpreter (remediation execution)
  - Browser (research and documentation)
  - Observability (monitoring and tracing)
- **Strands Framework** - Agent framework for tool-use patterns with streaming support
- **Streamlit** - Interactive web UI for real-time agent interaction
- **AWS Services** - EC2, DynamoDB, CloudWatch, Lambda, IAM, Cognito, Bedrock
- **Jupyter Notebooks** - Interactive learning environment

- **Amazon Bedrock** - 基础模型（Claude 3.7 Sonnet）
- **AgentCore** - 无服务器代理平台
  - Runtime（部署）
  - Memory（上下文持久化）
  - Gateway（带有 JWT 认证的工具编排）
  - Code Interpreter（修复执行）
  - Browser（研究和文档）
  - Observability（监控和追踪）
- **Strands Framework** - 支持流式传输的工具使用模式代理框架
- **Streamlit** - 用于实时代理交互的交互式 Web UI
- **AWS 服务** - EC2、DynamoDB、CloudWatch、Lambda、IAM、Cognito、Bedrock
- **Jupyter Notebooks** - 交互式学习环境

## Project Files

## 项目文件

### Lab Helpers (`lab_helpers/`)

### 实验辅助程序（`lab_helpers/`）

Python modules that notebooks import for cleaner code:
- `lab_01/` - Infrastructure deployment and fault injection
- `lab_02/` - Lambda deployment, MCP client, gateway setup
- `lab_03/` - Runtime deployment, OAuth2 setup, interceptor
- `lab_04/` - Runtime deployment, gateway setup, logging
- `lab_05/` - Supervisor agent code, Streamlit app, IAM setup
- `constants.py` - Configuration constants and parameter paths
- `parameter_store.py` - AWS Parameter Store utilities
- `config.py` - Workshop configuration
- `cognito_setup.py` - Cognito user pool and client setup
- `short_term_memory.py` - AgentCore Memory integration

笔记本导入的 Python 模块，使代码更简洁：
- `lab_01/` - 基础设施部署和故障注入
- `lab_02/` - Lambda 部署、MCP 客户端、网关设置
- `lab_03/` - Runtime 部署、OAuth2 设置、拦截器
- `lab_04/` - Runtime 部署、网关设置、日志记录
- `lab_05/` - 监督代理代码、Streamlit 应用程序、IAM 设置
- `constants.py` - 配置常量和参数路径
- `parameter_store.py` - AWS Parameter Store 工具
- `config.py` - 研讨会配置
- `cognito_setup.py` - Cognito 用户池和客户端设置
- `short_term_memory.py` - AgentCore Memory 集成

## Troubleshooting

## 故障排除

**If something goes wrong:**
1. Check the notebook output for error messages
2. Verify AWS credentials in the error output
3. Ensure you're in the correct AWS region
4. Review CloudWatch logs directly from the notebook
5. Run prerequisite verification again

**如果出现问题：**
1. 检查笔记本输出中的错误消息
2. 在错误输出中验证 AWS 凭证
3. 确保您在正确的 AWS 区域
4. 直接从笔记本查看 CloudWatch 日志
5. 再次运行先决条件验证

**Common issues:**
- Missing AWS credentials → Run `Lab-01-prerequisites-infra.ipynb` again
- Bedrock model not accessible → Ensure Bedrock is enabled in your region
- Lambda timeout → Check CloudWatch logs in notebook
- Resource already exists → Run cleanup notebook and retry

**常见问题：**
- 缺少 AWS 凭证 → 再次运行 `Lab-01-prerequisites-infra.ipynb`
- Bedrock 模型不可访问 → 确保在您的区域启用了 Bedrock
- Lambda 超时 → 在笔记本中检查 CloudWatch 日志
- 资源已存在 → 运行清理笔记本并重试

## After the Workshop

## 研讨会之后

To apply what you've learned:

应用您所学到的知识：

1. **In your own environment:**
   - Adapt agents to your monitoring systems
   - Integrate with your deployment pipeline
   - Connect to your incident management platform

1. **在您自己的环境中：**
   - 将代理适配到您的监控系统
   - 与您的部署管道集成
   - 连接到您的事件管理平台

2. **For production use:**
   - Deploy agents to AgentCore Runtime
   - Set up persistent memory for incident history
   - Enable observability and alerting
   - Establish team approval workflows

2. **用于生产环境：**
   - 将代理部署到 AgentCore Runtime
   - 为事件历史设置持久化记忆
   - 启用可观测性和告警
   - 建立团队审批工作流

3. **Advanced capabilities:**
   - Multi-team orchestration
   - Cross-account incident response
   - Custom tool development
   - Third-party integrations

3. **高级功能：**
   - 多团队编排
   - 跨账户事件响应
   - 自定义工具开发
   - 第三方集成

## Resources

## 资源

- [Amazon Bedrock Documentation](https://docs.aws.amazon.com/bedrock/)
- [AgentCore Documentation](https://docs.aws.amazon.com/agentcore/)
- [Strands Framework GitHub](https://github.com/aws-samples/strands-agents)
- [AWS re:Invent 2025](https://reinvent.awsevents.com/)

- [Amazon Bedrock 文档](https://docs.aws.amazon.com/bedrock/)
- [AgentCore 文档](https://docs.aws.amazon.com/agentcore/)
- [Strands Framework GitHub](https://github.com/aws-samples/strands-agents)
- [AWS re:Invent 2025](https://reinvent.awsevents.com/)

## License

## 许可证

This workshop is provided as-is under the MIT License.

本研讨会按原样提供，遵循 MIT 许可证。
