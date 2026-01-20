# TypeScript MCP Server on Amazon Bedrock AgentCore

# 在 Amazon Bedrock AgentCore 上运行 TypeScript MCP 服务器

## Overview

## 概述

This tutorial demonstrates how to host a TypeScript-based MCP (Model Context Protocol) server using the Amazon Bedrock AgentCore runtime environment.

本教程演示如何使用 Amazon Bedrock AgentCore 运行时环境托管基于 TypeScript 的 MCP（模型上下文协议）服务器。

### Tutorial Details

### 教程详情

| Information         | Details                                                   |
|:--------------------|:----------------------------------------------------------|
| Tutorial type       | Hosting typescript MCP server                             |
| Tool type           | MCP server                                                |
| Tutorial components | Hosting typescript MCP server on AgentCore Runtime        |
| Tutorial vertical   | Cross-vertical                                            |
| Example complexity  | Easy                                                      |
| SDK used            | Anthropic's typescript SDK for MCP                        |

| 信息                 | 详情                                                       |
|:--------------------|:----------------------------------------------------------|
| 教程类型             | 托管 TypeScript MCP 服务器                                 |
| 工具类型             | MCP 服务器                                                 |
| 教程组件             | 在 AgentCore Runtime 上托管 TypeScript MCP 服务器          |
| 教程行业             | 跨行业                                                     |
| 示例复杂度           | 简单                                                       |
| 使用的 SDK          | Anthropic 的 TypeScript MCP SDK                           |

## Prerequisites

## 前提条件

- Node.js v22 or later
- Docker (for containerization)
- Amazon ECR (Elastic Container Registry) for storing Docker images
- AWS account with access to Bedrock AgentCore

- Node.js v22 或更高版本
- Docker（用于容器化）
- Amazon ECR（弹性容器注册表）用于存储 Docker 镜像
- 具有 Bedrock AgentCore 访问权限的 AWS 账户

---

## AgentCore Runtime Service Contract

## AgentCore Runtime 服务契约

Refer to the [official service contract documentation](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/runtime-service-contract.html).

请参阅[官方服务契约文档](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/runtime-service-contract.html)。

**Runtime configuration:**
- **Host:** `0.0.0.0`
- **Port:** `8000`
- **Transport:** Stateless `streamable-http`
- **Endpoint Path:** `POST /mcp`

**运行时配置：**
- **主机：** `0.0.0.0`
- **端口：** `8000`
- **传输协议：** 无状态 `streamable-http`
- **端点路径：** `POST /mcp`

## Local Development

## 本地开发

1. Install dependencies

1. 安装依赖

```
npm install
```

2. Set up AWS credentials

2. 设置 AWS 凭证

```
aws configure
export AWS_ACCESS_KEY_ID=your_access_key
export AWS_SECRET_ACCESS_KEY=your_secret_key
export AWS_REGION=us-east-1
```

3. Start server

3. 启动服务器

```
npm run start
```

4. Test locally using [MCP inspector](https://github.com/modelcontextprotocol/inspector)

4. 使用 [MCP inspector](https://github.com/modelcontextprotocol/inspector) 进行本地测试

```
npx @modelcontextprotocol/inspector
```

## Docker Deployment

## Docker 部署

1. Create ECR Repository

1. 创建 ECR 仓库

```
aws ecr create-repository --repository-name mcp-server --region us-east-1
```

2. Build and Push Image to ECR

2. 构建并推送镜像到 ECR

```
# Get login token
# 获取登录令牌
aws ecr get-login-password --region us-east-1 | \
  docker login --username AWS --password-stdin [account-id].dkr.ecr.us-east-1.amazonaws.com

docker buildx --platform linux/arm64 \
  -t [account-id].dkr.ecr.us-east-1.amazonaws.com/mcp-server:latest --push .
```

3. Deploy to Bedrock AgentCore

3. 部署到 Bedrock AgentCore

    - Go to AWS Console → Bedrock → AgentCore → Create Agent
    - Choose MCP as the protocol
    - Configure Agent Runtime:
        - Image URI: [account-id].dkr.ecr.us-east-1.amazonaws.com/mcp-server:latest
        - Set IAM Permissions for Bedrock model access
        - Deploy and test in the Agent Sandbox

    - 进入 AWS 控制台 → Bedrock → AgentCore → 创建代理
    - 选择 MCP 作为协议
    - 配置代理运行时：
        - 镜像 URI：[account-id].dkr.ecr.us-east-1.amazonaws.com/mcp-server:latest
        - 设置 Bedrock 模型访问的 IAM 权限
        - 在代理沙箱中部署和测试

4. Construct the Encoded ARN MCP URL

4. 构建编码后的 ARN MCP URL

```
echo "agent_arn" | sed 's/:/%3A/g; s/\//%2F/g'
```

```
https://bedrock-agentcore.{region}.amazonaws.com/runtimes/{encoded_arn}/invocations?qualifier=DEFAULT
```

5. Use the MCP url with [MCP inspector](https://github.com/modelcontextprotocol/inspector).

5. 使用 [MCP inspector](https://github.com/modelcontextprotocol/inspector) 测试 MCP URL。

## References

## 参考资料

- https://aws.amazon.com/bedrock/agentcore/
- https://github.com/modelcontextprotocol/typescript-sdk
- https://github.com/modelcontextprotocol/inspector


