# Hosting MCP server on AgentCore Runtime

# 在 AgentCore Runtime 上托管 MCP 服务器

## Overview

## 概述

In this session we will discuss how to host MCP tools on Amazon Bedrock AgentCore Runtime.

在本节中，我们将讨论如何在 Amazon Bedrock AgentCore Runtime 上托管 MCP 工具。

We will use the Amazon Bedrock AgentCore Python SDK to wrapper the agents function as an MCP server compatible with Amazon Bedrock AgentCore.
It will handle the MCP server details so you can focus on your agent's core functionality.

我们将使用 Amazon Bedrock AgentCore Python SDK 将代理功能封装为与 Amazon Bedrock AgentCore 兼容的 MCP 服务器。它将处理 MCP 服务器的细节，使您可以专注于代理的核心功能。

The Amazon Bedrock AgentCore Python SDK prepares your agent or tool code to run on AgentCore Runtime.

Amazon Bedrock AgentCore Python SDK 为您的代理或工具代码准备在 AgentCore Runtime 上运行。

It will transform your code into the AgentCore standardized HTTP protocol or MCP protocol contracts to allow for direct REST API endpoint communication for a traditional request/response pattern (HTTP protocol) or Model Context Protocol for tools and agents servers (MCP Protocol).

它将您的代码转换为 AgentCore 标准化的 HTTP 协议或 MCP 协议契约，以允许传统请求/响应模式（HTTP 协议）的直接 REST API 端点通信，或用于工具和代理服务器的模型上下文协议（MCP 协议）。

When you are hosting tools, the Amazon Bedrock AgentCore Python SDK will implement the [Stateless Streamable HTTP] transport protocol with the `MCP-Session-Id` header for [session isolation]https://modelcontextprotocol.io/specification/2025-06-18/basic/transports#session-management, servers must support stateless operation to not reject platform generated Mcp-Session-Id header.
Your MCP server will then be hosted on port `8000` and will provide one invocation path: the `mcp-POST`. This interaction endpoint with receive the MCP RPC messages and process them through your tool's capabilities. It supports both  application/json and text/event-stream as response content-types.

当您托管工具时，Amazon Bedrock AgentCore Python SDK 将实现带有 `MCP-Session-Id` 头的[无状态可流式 HTTP]传输协议以实现[会话隔离]https://modelcontextprotocol.io/specification/2025-06-18/basic/transports#session-management，服务器必须支持无状态操作以不拒绝平台生成的 Mcp-Session-Id 头。您的 MCP 服务器将托管在端口 `8000` 上，并提供一个调用路径：`mcp-POST`。此交互端点将接收 MCP RPC 消息并通过您的工具功能处理它们。它支持 application/json 和 text/event-stream 作为响应内容类型。

When you set your AgentCore protocol to MCP, AgentCore Runtime will expect the MCP server container to be on path `0.0.0.0:8000/mcp` as that's the default path supported by most of the official MCP server SDKs.

当您将 AgentCore 协议设置为 MCP 时，AgentCore Runtime 将期望 MCP 服务器容器位于路径 `0.0.0.0:8000/mcp`，因为这是大多数官方 MCP 服务器 SDK 支持的默认路径。

AgentCore Runtime requires you to host stateless streamable-http servers because it provides session-isolation by default and automatically adds a Mcp-Session-Id header for any request without it, so MCP clients can have continuity of connection to same Bedrock AgentCore Runtime session ID.

AgentCore Runtime 要求您托管无状态的可流式 HTTP 服务器，因为它默认提供会话隔离，并自动为任何没有该头的请求添加 Mcp-Session-Id 头，以便 MCP 客户端可以保持与同一 Bedrock AgentCore Runtime 会话 ID 的连接连续性。

Payload of `InvokeAgentRuntime` API is completely pass through, so RPC messages of protocols like MCP can easily be proxied.

`InvokeAgentRuntime` API 的有效负载是完全透传的，因此像 MCP 这样的协议的 RPC 消息可以轻松代理。

In this tutorial you will learn:

在本教程中，您将学习：

* How to create an MCP server with tools
* How to test your server locally
* How to deploy your server to AWS
* How to invoke your deployed server

* 如何创建带有工具的 MCP 服务器
* 如何在本地测试您的服务器
* 如何将您的服务器部署到 AWS
* 如何调用已部署的服务器

### Tutorial Details

### 教程详情

| Information         | Details                                                   |
|:--------------------|:----------------------------------------------------------|
| Tutorial type       | Hosting Tools                                             |
| Tool type           | MCP server                                                |
| Tutorial components | Hosting tool on AgentCore Runtime. Creating an MCP server |
| Tutorial vertical   | Cross-vertical                                            |
| Example complexity  | Easy                                                      |
| SDK used            | Amazon BedrockAgentCore Python SDK and MCP Client         |

| 信息                 | 详情                                                       |
|:--------------------|:----------------------------------------------------------|
| 教程类型             | 托管工具                                                   |
| 工具类型             | MCP 服务器                                                 |
| 教程组件             | 在 AgentCore Runtime 上托管工具。创建 MCP 服务器            |
| 教程行业             | 跨行业                                                     |
| 示例复杂度           | 简单                                                       |
| 使用的 SDK          | Amazon BedrockAgentCore Python SDK 和 MCP 客户端           |

### Tutorial Architecture

### 教程架构

In this tutorial we will describe how to deploy an existing MCP server to AgentCore runtime.

在本教程中，我们将介绍如何将现有的 MCP 服务器部署到 AgentCore 运行时。

For demonstration purposes, we will use a very simple MCP server with 3 tools: `add_numbers`, `multiply_numbers` and `greet_users`

出于演示目的，我们将使用一个包含 3 个工具的非常简单的 MCP 服务器：`add_numbers`（加法）、`multiply_numbers`（乘法）和 `greet_users`（用户问候）

![MCP architecture](images/hosting_mcp_server.png)

### Tutorial Key Features

### 教程关键功能

* Hosting MCP Server

* 托管 MCP 服务器