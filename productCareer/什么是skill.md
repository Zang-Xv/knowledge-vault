# skill

在 AI Agent 领域里，**Skill（技能）**通常指：
Agent 可以调用的一个可复用能力模块，通常封装了一段能力（工具调用、API、代码执行、推理流程等），供 Agent 在完成任务时组合使用。

## skill 的演化

因为随着发展, ai agent 领域往往出现许多新的概念, 但实际上都是旧瓶装新酒
为了理清其中细微的概念差距, 所以我希望能够调研清除这个概念的发展历史

### 阶段1：Plugin（2023）

典型代表: ChatGPT Plugins
结构：LLM -> Plugin -> API

例如:
Expedia plugin
Wolfram plugin
Zapier plugin

问题：

- 插件依赖平台
- 难以组合
- 不适合 agent

于是逐渐演化。

### 阶段2：Tool（2023）

典型：

LangChain

OpenAI Function Calling

Tool 定义：

name
description
parameters

LLM 输出：

{
  "tool": "search",
  "arguments": {...}
}

这是 tool calling paradigm。

### 阶段3：Skill（2023–2024）

此时开始出现需要执行一长串的任务, 需要调用多个 tool
于是开始出现类似于 long chain, workflow, skill 之类的概念, 在不同的 agent 框架里面会具有不同的称呼
在2025年由 Anthropic 进一步规范化, Anthropic 的 Agent 文档强调：
skill = reusable capability

## skill 的结构

Skill 的结构

一个典型 skill 包含：

Skill
 ├ name
 ├ description
 ├ input schema
 ├ output schema
 ├ implementation
 └ optional prompt

### 为什么 agent 需要 skill

Skill 提供：

能力分层 abstraction

Agent
  ↓
Skill layer
  ↓
Tool layer

同时这种逐层暴露, 按需调用的 prompt 也能够更加节省 token
