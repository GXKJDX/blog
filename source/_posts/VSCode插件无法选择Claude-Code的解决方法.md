---
title: VSCode插件无法选择Claude Code的解决方法
date: 2026-03-13 11:43:00
tags: [vscode, claude, AI]
---

## 问题描述

在使用 VSCode 的 GitHub Copilot 插件时，发现在模型选择列表中无法找到 **Claude Code** 选项，导致无法切换到 Claude 模型进行代码辅助。

---

## 原因分析

### 1. Claude Code 与 GitHub Copilot 的区别

**Claude Code** 是 Anthropic 推出的独立 AI 编程助手，它与 GitHub Copilot 是两个完全独立的工具：

| 工具 | 提供商 | 使用方式 |
|------|--------|----------|
| GitHub Copilot | GitHub / Microsoft | VSCode 插件，支持多模型切换（包括 GPT-4o、Claude 等） |
| Claude Code | Anthropic | 独立 CLI 工具，通过终端或专属插件使用 |

### 2. 在 GitHub Copilot 中选择 Claude 模型

如果你希望在 **GitHub Copilot Chat** 中使用 Claude 模型（如 Claude 3.5 Sonnet），需要满足以下条件：

- 拥有 **GitHub Copilot Individual、Business 或 Enterprise** 订阅
- VSCode 已安装最新版本的 **GitHub Copilot** 和 **GitHub Copilot Chat** 插件
- GitHub 账号已开启多模型支持（部分功能需要 GitHub 开放权限）

---

## 解决方案

### 方案一：在 GitHub Copilot Chat 中切换模型

1. 确保已安装并登录 GitHub Copilot 插件
2. 打开 Copilot Chat 侧边栏（`Ctrl+Alt+I` 或点击侧边栏图标）
3. 在聊天输入框上方，点击模型选择下拉菜单
4. 若列表中出现 Claude 选项，直接选择即可

> 若没有出现 Claude 选项，说明你的 Copilot 订阅版本暂不支持，或该功能尚未向你的账号开放。

### 方案二：安装并使用 Claude Code（独立工具）

**Claude Code** 是一个独立的 AI 编程工具，使用方式如下：

#### 步骤 1：安装 Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

#### 步骤 2：配置 API Key

前往 [Anthropic 控制台](https://console.anthropic.com/) 获取 API Key，然后在终端中配置：

```bash
export ANTHROPIC_API_KEY=your_api_key_here
```

或者将其添加到 `~/.bashrc` / `~/.zshrc` 中永久生效：

```bash
echo 'export ANTHROPIC_API_KEY=your_api_key_here' >> ~/.zshrc
source ~/.zshrc
```

#### 步骤 3：在 VSCode 终端中使用

在 VSCode 内置终端中运行：

```bash
claude
```

即可进入 Claude Code 的交互式对话，直接针对当前项目进行代码问答和修改。

### 方案三：使用 Continue 插件接入 Claude

[Continue](https://marketplace.visualstudio.com/items?itemName=Continue.continue) 是一个开源的 VSCode AI 编程插件，支持接入多种 AI 模型，包括 Claude。

1. 在 VSCode 插件市场搜索并安装 **Continue**
2. 打开 Continue 配置文件（`~/.continue/config.json`）
3. 添加 Claude 模型配置：

```json
{
  "models": [
    {
      "title": "Claude 3.5 Sonnet",
      "provider": "anthropic",
      "model": "claude-3-5-sonnet-20241022",
      "apiKey": "your_api_key_here"
    }
  ]
}
```

4. 保存配置后，即可在 Continue 插件中选择并使用 Claude 模型

---

## 总结

| 场景 | 推荐方案 |
|------|----------|
| 已有 GitHub Copilot 订阅，想用 Claude | 在 Copilot Chat 中切换模型 |
| 想直接使用 Claude Code CLI | 安装 `@anthropic-ai/claude-code` |
| 想在 VSCode 中通过插件使用 Claude | 安装 Continue 插件并配置 Anthropic API Key |

选择适合自己需求的方案，即可在 VSCode 中顺利使用 Claude 进行 AI 辅助编程。
