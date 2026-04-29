---
title: 如何在mac上安装openclaw，并使用本地大模型
date: 2026-04-24 09:56:17
tags: [openclaw, mac]
mathjax: true
comments: true
---

在Mac上搭建本地大模型环境，OpenClaw（一款开源的大模型管理/运行工具）搭配Ollama（轻量本地大模型运行器）是非常便捷的组合——无需复杂配置，新手也能快速上手。本文将详细补全每一步操作，从Ollama安装、模型选择，到Homebrew配置、OpenClaw安装，再到最终对接测试，全程实操无废话，亲测可用（适配Intel芯片和Apple Silicon芯片Mac）。

# 安装ollama（如果你不喜欢ollama，或者想本地模型运行效率更高，也可以安装llama.cpp替代）

Ollama是核心工具，负责本地运行大模型，支持一键下载、启动模型，无需手动配置依赖，是Mac上运行本地大模型的首选工具，兼容所有Mac系统（macOS 10.15及以上）。

## 下载、安装ollama

官网直接下载（推荐）
访问Ollama官方网站：[https://ollama.com/](https://ollama.com/)，点击页面中间的「Download for macOS」，下载后得到.dmg安装包，双击打开，将Ollama拖拽到「应用程序」文件夹即可完成安装。

注意：若提示“无法打开”，先点击弹窗上的取消，需在「系统设置→隐私与安全性」中，允许“来自未知开发者”的Ollama运行或者open anyway（仅首次安装需要）。

## 选择适合自己的本地大模型

Ollama支持多种主流本地大模型（如Llama 3、Qwen、Gemini、Mistral等），不同模型的体积、性能、显存要求不同，需根据自己Mac的配置（主要是内存/显存）选择，避免出现“模型无法启动”“运行卡顿”的问题。

Ollama的模型名称都带有后缀，后缀代表模型的量化版本，直接决定模型体积和显存占用，新手优先选择「Q4_K_M」后缀，兼顾性能和占用：

- Q2_K：最小量化，体积最小，显存占用最低，性能一般，适合4GB内存的Mac；
- Q4_K_M：平衡版（推荐），体积适中，显存占用较低，性能接近原始模型，适合8GB及以上内存的Mac；
- Q6_K：高量化，体积较大，性能较好，适合16GB内存的Mac；
- Q8_0：几乎接近原始模型，体积最大，性能最好，适合32GB及以上内存的Mac。

常见推荐模型（Mac适配度高）：

- 入门首选：llama3:8b-q4_k_m（Llama 3 8B，平衡版，适合大多数Mac）；
- 轻量首选：qwen:7b-q4_k_m（通义千问7B，中文支持更好，体积略小）；
- 性能首选：gemini:pro-q4_k_m（Gemini Pro，多模态支持，适合16GB内存Mac）。

本地大模型运行的核心限制是「显存」（Mac的内存和显存是共享的，所以内存大小直接决定能运行的模型），简单计算方法：模型参数（如8B、7B）× 量化系数 ≈ 所需显存（单位：GB）。

举个例子：

- llama3:8b-q4_k_m：8B参数 × 0.45（Q4_K_M量化系数）≈ 3.6GB显存，8GB内存的Mac可流畅运行；
- qwen:14b-q4_k_m：14B参数 × 0.45 ≈ 6.3GB显存，建议16GB内存的Mac运行；
- llama3:70b-q4_k_m：70B参数 × 0.45 ≈ 31.5GB显存，仅适合32GB及以上内存的Mac。

注意：Mac运行模型时，系统本身会占用2-3GB内存，所以选择模型时，需预留至少2GB内存，避免系统卡顿。

## 安装并确认是否可以正常使用

安装完成并选择好模型后，先测试Ollama是否能正常启动模型，步骤如下：

1.  打开终端，输入模型下载命令（以入门推荐的llama3:8b-q4_k_m为例）：

> ollama pull llama3:8b-q4_k_m

2.  等待下载完成（模型大小约4GB，下载速度取决于网络，建议连接WiFi）；

3.  下载完成后，输入启动命令，测试模型响应：

> ollama run llama3:8b-q4_k_m

4.  终端出现「>>>」提示符后，输入任意问题（如“介绍一下OpenClaw”），模型会自动生成回答，说明Ollama和模型都能正常使用；

5.  退出模型：输入/bye，回车即可退出。

常见问题：若下载模型时提示“网络超时”，可尝试切换网络（如手机热点），或重复输入pull命令；若启动模型时提示“out of memory”，说明模型显存要求超过Mac内存，需更换更小的模型（如换成qwen:7b-q4_k_m）。

# 安装homebrew（如果前序步骤已经安装，可以跳过）

Homebrew是Mac上的包管理工具，相当于Windows的“应用商店”。其实openclaw的一键安装脚本中已经包括了homebrew的安装，我们单独安装它，主要是为了：① 替换国内源，解决官方源下载速度慢、卡顿的问题；② 避免后续安装OpenClaw时出现sudo权限不足的问题（很多终端命令需要sudo权限，Homebrew可自动处理）。

注意：如果你的Mac已经安装过Homebrew，可跳过这一步，但建议按照下面的“国内源配置”，替换成国内源，避免后续安装出错。

## 通过国内的源安装

官方Homebrew源在国内访问速度较慢，这里使用「中科大源」安装，步骤如下：

1.  打开终端，输入以下命令（一键安装，包含国内源配置），回车后按照提示操作：
`/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"`
2.  执行命令后，会出现源选择界面，输入「1」（中科大源，推荐），回车；
3.  然后按照提示，输入Mac的开机密码（输入时终端不会显示密码，直接输入即可，回车确认）；
4.  等待安装完成，过程约5-10分钟（取决于网络），出现“安装成功”的提示即完成。

## 重启命令行！！然后确认是否安装成功

**重要：安装完成后，必须关闭当前所有终端窗口，重新打开一个新的终端（这一步不能省略，否则Homebrew无法正常生效）。**

确认安装成功：在新终端中输入 brew --version，若输出类似「Homebrew 4.2.20」的版本信息，说明安装成功；若提示“command not found: brew”，说明未重启终端，或安装失败，可重新执行安装命令。

补充：若已安装Homebrew，仅需替换国内源，可输入以下命令（依次执行）：

> brew tap --custom-remote --force-auto-update homebrew/core https://mirrors.ustc.edu.cn/homebrew-core.git

> brew tap --custom-remote --force-auto-update homebrew/cask https://mirrors.ustc.edu.cn/homebrew-cask.git

# 安装openclaw

OpenClaw是一款开源的本地大模型管理工具，支持图形化界面（也可终端操作），能快速对接Ollama、ChatGLM等本地大模型，还能实现模型切换、对话记录保存等功能，比直接用终端操作Ollama更便捷
。
注意：OpenClaw依赖Homebrew，必须先安装好Homebrew，才能正常安装OpenClaw。

## 命令行一键安装

打开新的终端，输入以下命令（一键安装，自动依赖Homebrew），回车后等待安装完成：

> curl -fsSL https://openclaw.ai/install.sh | bash

## 配置openclaw

安装完成后就会自动进入第一次简单配置（具体选啥可以从下边找，不确定的一般直接按默认来就行，后边都能改）。

这里按照安装完成后，在终端输入：openclaw config，进入配置会看到：

> Select sections to configure:
> workspace / model / gateway / web tools / daemon / channels ...

初次配置只搞这三个：

- workspace
- model
- gateway

### workspace配置

直接回车使用默认路径即可：~/.openclaw

这是 OpenClaw 的工作区，后面所有配置都在这里。

### model配置

选择：

- Provider: ollama
- Base URL: http://127.0.0.1:11434

然后选择你已经通过 Ollama 下载好的模型，例如：

- llama3.2:latest
- qwen2.5:latest
- deepseek-r1:7b

### gateway配置

这样选：

| 选项                 | 选择       |
| ------------------ | -------- |
| Gateway bind mode  | loopback |
| Gateway auth       | token    |
| Token source       | generate |
| Tailscale exposure | off      |

这些组合代表：只在本机运行，自动生成安全 token，不暴露到外网。

Token记得复制一下，后边openclaw图形界面登录需要。

# 整体运行

## ollama运行

> ollama serve

> ollama launch openclaw

## openclaw运行

> openclaw gateway start

## 整体测试

在openclaw的可视化页面上，默认是http://127.0.0.1:18789/，和大模型对话，看看是否可以跑通

# 安装llama.cpp替代ollama

[https://llama-cpp.com/getting-started/](https://llama-cpp.com/getting-started/)官网写得非常清楚了，跟着执行就行

需要注意的是：Step 4: Enable Apple Metal Acceleration，执行前先`cd ..`返回到build的上一级目录，也就是llama.cpp这个文件夹中

安装好之后，可以去huggingface或者modelscope下载gguf格式的模型文件，比如
[https://modelscope.cn/models/unsloth/Qwen3.6-35B-A3B-GGUF/files](https://modelscope.cn/models/unsloth/Qwen3.6-35B-A3B-GGUF/files)

下载完成之后，执行

```
llama-server \\\
  -m Qwen3.6-35B-A3B-UD-Q8_K_XL.gguf \\\
  --host 127.0.0.1 \\\
  --port 11434 
```

就可以在本地服务器运行模型了，你也可以自由的配置上下文长度等各项参数，我在自己的mac上常用的执行命令如下：

```
llama-server \
  -m Qwen3.6-35B-A3B-UD-Q8_K_XL.gguf \
  --host 127.0.0.1 \
  --port 11434 \
  --ctx-size 262144 \
  --n-gpu-layers 999 \
  --threads 18 \
  --batch-size 2048 \
  --ubatch-size 512 \
  --flash-attn 1\
  --mlock \
  --no-mmap
```

# Openclaw配置llama.cpp

打开openclaw.json，修改默认模型（注意把具体模型名称替换成自己的）：
```
"agents": {
    "defaults": {
        "llm": {
        "idleTimeoutSeconds": 300
        },
        "model": {
        "primary": "llamacpp/Qwen3.6-35B-A3B-UD-Q8_K_XL",
        "fallbacks": [
            "ollama/gemma4"
        ]
        },
        "models": {
        "ollama/gemma4": {},
        "llamacpp/Qwen3.6-35B-A3B-UD-Q8_K_XL": {}
        },
        "workspace": "/Users/mengxi/.openclaw/workspace"
    }
},
```

在models、provider中，增加
```
"llamacpp": {
    "baseUrl": "http://127.0.0.1:11434/v1",
    "apiKey": "llamacpp",
    "api": "openai-completions",
    "models": [
        {
        "id": "Qwen3.6-35B-A3B-UD-Q8_K_XL",
        "name": "Qwen3.6-35B-A3B-UD-Q8_K_XL",
        "contextWindow": 262144
        }
    ]
    }
```

openclaw更新之后需要在命令行运行`openclaw models auth paste-token --provider llamacpp`，然后再命令行中数据llamacpp，回车，会自动生成auth-profiles.json

最后，如果你不巧前边已经运行过ollama的session了，切换llama.cpp可能会导致openclaw陷入假死，需要进入~/.openclaw/agents/main/sessions/这个目录，把里边的内容清空

最后执行`openclaw gateway stop && openclaw gateway start`，完工！