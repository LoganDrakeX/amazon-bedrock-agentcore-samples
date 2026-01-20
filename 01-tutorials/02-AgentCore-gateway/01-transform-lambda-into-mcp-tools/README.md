# Implement Lambda function tools for Gateway

# 为 Gateway 实现 Lambda 函数工具

## Overview

## 概述

Bedrock AgentCore Gateway provides customers a way to turn their existing Lambda functions into fully-managed MCP servers without needing to manage infra or hosting. Customers can bring their existing AWS Lambda functions, or add new Lambda functions to front their tools. Gateway will provide a uniform Model Context Protocol (MCP) interface across all these tools. Gateway employs a dual authentication model to ensure secure access control for both incoming requests and outbound connections to target resources. The framework consists of two key components: Inbound Auth, which validates and authorizes users attempting to access gateway targets, and Outbound Auth, which enables the gateway to securely connect to backend resources on behalf of authenticated users. Together, these authentication mechanisms create a secure bridge between users and their target resources, supporting both IAM credentials and OAuth-based authentication flows.

Bedrock AgentCore Gateway 为客户提供了一种将现有 Lambda 函数转换为完全托管的 MCP 服务器的方式，无需管理基础设施或托管。客户可以使用现有的 AWS Lambda 函数，或添加新的 Lambda 函数来支持其工具。Gateway 将为所有这些工具提供统一的模型上下文协议（MCP）接口。Gateway 采用双重身份验证模型，以确保对传入请求和到目标资源的出站连接进行安全访问控制。该框架由两个关键组件组成：入站认证（Inbound Auth），用于验证和授权尝试访问网关目标的用户；出站认证（Outbound Auth），使网关能够代表经过身份验证的用户安全地连接到后端资源。这些身份验证机制共同在用户和目标资源之间创建了一个安全桥梁，支持 IAM 凭证和基于 OAuth 的身份验证流程。

![How does it work](images/lambda-iam-gateway.png)

![How does it work](images/lambda-gw-iam-inbound.png)


### Understanding the Lambda context object

### 理解 Lambda 上下文对象

When Gateway invokes a Lambda function, it passes special context information through the context.client_context object. This context includes important metadata about the invocation, which your function can use to determine how to process the request.
The following properties are available in the context.client_context.custom object:
* bedrockagentcoreEndpointId: The ID of the Gateway endpoint that received the request.
* bedrockagentcoreTargetId: The ID of the Gateway target that routed the request to your function.
* bedrockagentcoreMessageVersion: The version of the message format used for the request.
* bedrockagentcoreToolName: The name of the tool being invoked. This is particularly important when your Lambda function implements multiple tools.
* bedrockagentcoreSessionId: The session ID for the current invocation, which can be used to correlate multiple tool calls within the same session.

当 Gateway 调用 Lambda 函数时，它会通过 context.client_context 对象传递特殊的上下文信息。此上下文包含有关调用的重要元数据，您的函数可以使用这些元数据来确定如何处理请求。
以下属性在 context.client_context.custom 对象中可用：
* bedrockagentcoreEndpointId：接收请求的 Gateway 端点的 ID。
* bedrockagentcoreTargetId：将请求路由到您的函数的 Gateway 目标的 ID。
* bedrockagentcoreMessageVersion：用于请求的消息格式版本。
* bedrockagentcoreToolName：正在调用的工具的名称。当您的 Lambda 函数实现多个工具时，这一点尤为重要。
* bedrockagentcoreSessionId：当前调用的会话 ID，可用于关联同一会话中的多个工具调用。

You can access these properties in your Lambda function code to determine which tool is being invoked and to customize your function's behavior accordingly

您可以在 Lambda 函数代码中访问这些属性，以确定正在调用哪个工具，并相应地自定义函数的行为

![How does it work](images/lambda-context-object.png)

### Response format and error handling

### 响应格式和错误处理

Your Lambda function should return a response that the Gateway can interpret and pass back to the client. The response should be a JSON object with the following structure:The statusCode field should be an HTTP status code indicating the result of the operation:
* 200: Success
* 400: Bad request (client error)
* 500: Internal server error

您的 Lambda 函数应返回 Gateway 可以解释并传递回客户端的响应。响应应该是具有以下结构的 JSON 对象：statusCode 字段应该是指示操作结果的 HTTP 状态码：
* 200：成功
* 400：错误请求（客户端错误）
* 500：内部服务器错误

The body field can be either a string or a JSON string representing a more complex response. If you want to return a structured response, you should serialize it to a JSON string

body 字段可以是字符串或表示更复杂响应的 JSON 字符串。如果您想返回结构化响应，应将其序列化为 JSON 字符串

### Error handling

### 错误处理

Proper error handling is important for providing meaningful feedback to clients. Your Lambda function should catch exceptions and return appropriate error responses

正确的错误处理对于向客户端提供有意义的反馈非常重要。您的 Lambda 函数应捕获异常并返回适当的错误响应

### Testing

### 测试

Note that the ```__context__``` field is not part of the actual event that will be passed to your function when invoked by Gateway. It's only used for testing purposes to simulate the context object.
When testing in the Lambda console, you'll need to modify your function to handle the simulated context. This approach allows you to test your Lambda function with different tool names and input parameters before deploying it as a Gateway target.

请注意，```__context__``` 字段不是 Gateway 调用时传递给函数的实际事件的一部分。它仅用于测试目的以模拟上下文对象。
在 Lambda 控制台中测试时，您需要修改函数以处理模拟的上下文。这种方法允许您在将 Lambda 函数部署为 Gateway 目标之前，使用不同的工具名称和输入参数对其进行测试。

### Cross-account Lambda access

### 跨账户 Lambda 访问

If your Lambda function is in a different AWS account than your Gateway, you need to configure a resource-based policy on the Lambda function to allow the Gateway to invoke it. Here's an example policy:

如果您的 Lambda 函数与 Gateway 位于不同的 AWS 账户中，您需要在 Lambda 函数上配置基于资源的策略以允许 Gateway 调用它。以下是一个示例策略：

```
{
  "Version": "2012-10-17",
  "Id": "default",
  "Statement": [
    {
      "Sid": "cross-account-access",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:role/GatewayExecutionRole"
      },
      "Action": "lambda:InvokeFunction",
      "Resource": "arn:aws:lambda:us-west-2:987654321098:function:MyLambdaFunction"
    }
  ]
}
```
In this policy:
- 123456789012 is the account ID where the Gateway is deployed
- GatewayExecutionRole is the IAM role used by the Gateway.
- 987654321098 is the account ID where the Lambda function is deployed.
- MyLambdaFunction is the name of the Lambda function.

在此策略中：
- 123456789012 是部署 Gateway 的账户 ID
- GatewayExecutionRole 是 Gateway 使用的 IAM 角色
- 987654321098 是部署 Lambda 函数的账户 ID
- MyLambdaFunction 是 Lambda 函数的名称

After adding this policy, you can specify the Lambda function ARN in your Gateway target configuration, even though it's in a different account.

添加此策略后，您可以在 Gateway 目标配置中指定 Lambda 函数 ARN，即使它位于不同的账户中。

### Tutorial Details

### 教程详情


| Information          | Details                                                   |
|:---------------------|:----------------------------------------------------------|
| Tutorial type        | Interactive                                               |
| AgentCore components | AgentCore Gateway, AgentCore Identity, AWS IAM            |
| Agentic Framework    | Strands Agents                                            |
| LLM model            | Anthropic Claude Haiku 4.5, Amazon Nova Pro              |
| Tutorial components  | Creating AgentCore Gateway and Invoking AgentCore Gateway |
| Tutorial vertical    | Cross-vertical                                            |
| Example complexity   | Easy                                                      |
| SDK used             | boto3                                                     |

| 信息                  | 详情                                                       |
|:---------------------|:----------------------------------------------------------|
| 教程类型              | 交互式                                                     |
| AgentCore 组件       | AgentCore Gateway、AgentCore Identity、AWS IAM            |
| 代理框架              | Strands Agents                                            |
| LLM 模型             | Anthropic Claude Haiku 4.5、Amazon Nova Pro               |
| 教程组件              | 创建 AgentCore Gateway 和调用 AgentCore Gateway            |
| 教程行业              | 跨行业                                                     |
| 示例复杂度            | 简单                                                       |
| 使用的 SDK           | boto3                                                     |

## Tutorial Architecture

## 教程架构

### Tutorial Key Features

### 教程关键功能

* Expose Lambda functions into MCP tools
* Secure the tools call using OAuth and IAM

* 将 Lambda 函数暴露为 MCP 工具
* 使用 OAuth 和 IAM 保护工具调用

## Tutorials Overview

## 教程概览

In these tutorials we will cover the following functionality:

在这些教程中，我们将涵盖以下功能：

- [Transform your AWS Lambda function into MCP tools with OAuth inbound Auth](01-gateway-target-lambda-oauth.ipynb)

- [使用 OAuth 入站认证将您的 AWS Lambda 函数转换为 MCP 工具](01-gateway-target-lambda-oauth.ipynb)

- [Transform your AWS Lambda function into MCP tools with AWS IAM inbound Auth](02-gateway-target-lambda-iam.ipynb)

- [使用 AWS IAM 入站认证将您的 AWS Lambda 函数转换为 MCP 工具](02-gateway-target-lambda-iam.ipynb)