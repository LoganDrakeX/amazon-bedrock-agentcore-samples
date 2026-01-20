# Integrate your MCP Server with AgentCore Gateway

# 将您的 MCP 服务器与 AgentCore Gateway 集成

## Overview

## 概述

Amazon Bedrock AgentCore Gateway now supports MCP servers as native targets alongside existing REST APIs and AWS Lambda functions. This enhancement allows organizations to integrate their MCP server implementations through a unified interface, eliminating the need for writing custom client code per MCP Server. The Gateway addresses key enterprise challenges in scaling AI agent deployments across multiple teams and servers by centralizing tool management, authentication, and routing.

Amazon Bedrock AgentCore Gateway 现在支持将 MCP 服务器作为原生目标，与现有的 REST API 和 AWS Lambda 函数并列。此增强功能允许组织通过统一接口集成其 MCP 服务器实现，无需为每个 MCP 服务器编写自定义客户端代码。Gateway 通过集中化工具管理、身份验证和路由，解决了跨多个团队和服务器扩展 AI 代理部署的关键企业挑战。

The Gateway employs a centralized management framework that simplifies tool discovery, standardizes security protocols, and reduces operational complexity when scaling from dozens to hundreds of MCP servers. This unified approach allows enterprises to maintain consistent security and operational standards while efficiently managing their AI agent infrastructure through a single interface, eliminating the need for multiple separate gateways and reducing the overall maintenance burden.

Gateway 采用集中化管理框架，简化工具发现、标准化安全协议，并在从数十个扩展到数百个 MCP 服务器时降低运营复杂性。这种统一方法允许企业通过单一接口高效管理其 AI 代理基础设施，同时保持一致的安全和运营标准，消除了对多个独立网关的需求并减少了整体维护负担。

![How does it work](images/mcp-server-target.png)

### Refreshing tool definitions of your MCP servers in AgentCore Gateway

### 在 AgentCore Gateway 中刷新 MCP 服务器的工具定义

The SynchronizeGateway API enables on-demand synchronization of tools from MCP server targets through a sequence of carefully orchestrated steps. An Ops Admin initiates the process by making a SynchronizeGateway API call to the AgentCore Gateway, launching an asynchronous operation to update tool definitions. This control is particularly valuable after modifying MCP server configurations.

SynchronizeGateway API 通过一系列精心编排的步骤，实现从 MCP 服务器目标按需同步工具。运维管理员通过向 AgentCore Gateway 发起 SynchronizeGateway API 调用来启动该过程，启动异步操作以更新工具定义。此控制在修改 MCP 服务器配置后特别有价值。

For OAuth-authenticated targets, the AgentCore Gateway first communicates with the AgentCore Identity service to obtain and validate credentials. The Identity service acts as an OAuth resource credentials provider, returning the necessary tokens. If credential validation fails at this stage, the synchronization process immediately terminates, and the target transitions to a FAILED state.

对于 OAuth 认证的目标，AgentCore Gateway 首先与 AgentCore Identity 服务通信以获取和验证凭证。Identity 服务充当 OAuth 资源凭证提供者，返回必要的令牌。如果在此阶段凭证验证失败，同步过程将立即终止，目标将转换为 FAILED 状态。

Upon successful authentication (or immediately for targets configured without authentication), the Gateway initializes a session with the MCP server, establishing a secure connection. The Gateway then makes paginated calls using the tools/list capability, processing tools in efficient batches of 100 to optimize performance and resource utilization.

身份验证成功后（或对于未配置身份验证的目标立即执行），Gateway 与 MCP 服务器初始化会话，建立安全连接。然后 Gateway 使用 tools/list 功能进行分页调用，以 100 个为一批高效处理工具，以优化性能和资源利用率。

As tools are retrieved, the Gateway normalizes their definitions by adding target-specific prefixes to prevent naming conflicts with other targets. This normalization process maintains consistency while preserving essential metadata from the original MCP server definitions. Throughout the process, the Gateway enforces a strict limit of 10,000 tools per target to ensure system stability. The API implements optimistic locking during synchronization to prevent concurrent modifications that could lead to inconsistent states. The cached tool definitions ensure consistent high performance for ListTools operations between synchronizations.

在检索工具时，Gateway 通过添加特定于目标的前缀来规范化其定义，以防止与其他目标的命名冲突。此规范化过程在保留原始 MCP 服务器定义中的基本元数据的同时保持一致性。在整个过程中，Gateway 强制执行每个目标 10,000 个工具的严格限制以确保系统稳定性。API 在同步期间实现乐观锁定，以防止可能导致不一致状态的并发修改。缓存的工具定义确保同步之间 ListTools 操作的一致高性能。

![How does it work](images/mcp-server-target-explicit-sync.png)

### Implicit synchronization of tools schema

### 工具模式的隐式同步

During CreateGatewayTarget and UpdateGatewayTarget operations, AgentCore Gateway automatically syncs tool schemas that differs from the explicit SynchronizeGateway API. This built-in sync ensures new or updated MCP targets are ready for immediate use and maintains data consistency. While this makes create/update operations slower compared to other target types, it guarantees that targets marked as READY have valid tool definitions and prevents issues from targets with unvalidated tool definitions.

在 CreateGatewayTarget 和 UpdateGatewayTarget 操作期间，AgentCore Gateway 会自动同步工具模式，这与显式 SynchronizeGateway API 不同。此内置同步确保新的或更新的 MCP 目标可以立即使用并保持数据一致性。虽然这使得创建/更新操作比其他目标类型慢，但它保证标记为 READY 的目标具有有效的工具定义，并防止未验证工具定义的目标出现问题。

![How does it work](images/mcp-server-target-implicit-sync.png)

### Tutorial Details

### 教程详情

| Information          | Details                                                                |
|:---------------------|:-----------------------------------------------------------------------|
| Tutorial type        | Interactive                                                            |
| AgentCore components | AgentCore Gateway, AgentCore Identity, AgentCore Runtime               |
| Agentic Framework    | Strands Agents                                                         |
| Gateway Target Type  | MCP Server                                                             |
| Inbound Auth IdP     | Amazon Cognito, but can use others                                     |
| Outbound Auth        | Amazon Cognito, but can use others                                     |
| LLM model            | Anthropic Claude Sonnet 4                                              |
| Tutorial components  | Creating AgentCore Gateway with MCP Target and synchronize the tools   |
| Tutorial vertical    | Cross-vertical                                                         |
| Example complexity   | Easy                                                                   |
| SDK used             | boto3                                                                  |

| 信息                 | 详情                                                                   |
|:---------------------|:-----------------------------------------------------------------------|
| 教程类型             | 交互式                                                                 |
| AgentCore 组件       | AgentCore Gateway、AgentCore Identity、AgentCore Runtime               |
| 代理框架             | Strands Agents                                                         |
| Gateway 目标类型     | MCP 服务器                                                             |
| 入站认证 IdP         | Amazon Cognito，但可以使用其他                                         |
| 出站认证             | Amazon Cognito，但可以使用其他                                         |
| LLM 模型             | Anthropic Claude Sonnet 4                                              |
| 教程组件             | 创建带有 MCP 目标的 AgentCore Gateway 并同步工具                       |
| 教程垂直领域         | 跨垂直领域                                                             |
| 示例复杂度           | 简单                                                                   |
| 使用的 SDK           | boto3                                                                  |

## Tutorial Architecture

## 教程架构

### Tutorial Key Features

### 教程关键特性

* Integrate the MCP Server with AgentCore Gateway
* Perform explicit and implicit synchronization to refresh tool definitions

* 将 MCP 服务器与 AgentCore Gateway 集成
* 执行显式和隐式同步以刷新工具定义

## Tutorials Overview

## 教程概述

In these tutorials we will cover the following functionality:

在这些教程中，我们将涵盖以下功能：

- [Integrate the MCP Server with AgentCore Gateway](01-mcp-server-target.ipynb)
- [Perform explicit and implicit synchronization to refresh tool definitions](02-mcp-target-synchronization.ipynb)

- [将 MCP 服务器与 AgentCore Gateway 集成](01-mcp-server-target.ipynb)
- [执行显式和隐式同步以刷新工具定义](02-mcp-target-synchronization.ipynb)
