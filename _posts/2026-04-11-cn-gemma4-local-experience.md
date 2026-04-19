---
title: 本地部署 Gemma 4 小模型：显存友好、日语优秀、翻译实用
date: 2026-04-11 15:00:00 +0900
categories: [Tools]
tags: [gemma, ollama, translation, local-deployment, chinese]
media_subpath: /../assets/media/gemma4-local-experience
---

> 本文由作者提供核心内容与原始表达，并与 AI 协作完成。AI 主要负责将偏口语化、碎片化的内容整理为更清晰的书面表达，并辅助润色结构与可读性。
{: .prompt-tip }

版本：中文（当前） | [English](/posts/en-gemma4-local-experience/) | [日本語](/posts/jp-gemma4-local-experience/)

Gemma 4 发布后我用 Ollama 和 LM Studio 在本地部署试了一下，重点关注 E2B 和 E4B 这两个小尺寸版本。核心结论是：**显存占用极低、日语能力在同类小模型中突出、本地翻译体验非常好**。

## 🚀 为什么说它最适合日本用户？
E2B 和 E4B 的显存占用非常少，甚至连 E2B 可以直接用手机跑，对于基础任务完全够用。

目前市面上的开源小模型，大部分都是中国团队主导的。但这些模型的日语处理能力普遍不理想：MiniMax 在日语上是空白，而 GLM 和 Kimi 翻译日语时经常会偶尔混入中文词汇。Gemma 4 完全没有这个问题。对于目标用户群体（日本用户）来说，目前这是最好的选择。

## 💻 小模型的主要用途
说实话，小模型的应用场景不会特别多。太简单的事情自己能做，太难的问题它也解决不了。对我个人而言，现在最大的用武之地就是**翻译**。

## 🛠️ 实战工作流分析

### 翻译插件选择
我从以前的“沉浸式翻译”换到了“陪读蛙”。沉浸式的商业化痕迹和臃肿度太重了；而陪读蛙是开源替代品，体验更舒服很多，虽然偶尔也会遇到一些小 Bug。

### 翻译模型比较
Google 之前出过 TranslateGemma（4B 的专用翻译模型），但它的问题是翻译到中文时，对简体和繁体识别不准确，质量一般。换用 Gemma 4 后，这个问题几乎没有了。

不过，这类翻译插件本身有个固定的限制：它们只能按“片段”来翻译，无法做到整页的上下文理解，因此上下文中缺失导致的错误是必然存在的，这一点需要接受。

### 控制思维链 (Reasoning)
Gemma 4 支持通过系统提示词（System Prompt）开关推理过程（reasoning）。在进行**翻译任务时，建议关闭 reasoning 开关**。这样能避免模型多余的思考步骤输出，从而保证速度最快；若用于日常聊天或需要深入分析，则可以开启它。

## 📊 性能数据参考
我测试的是 Unsloth 的动态量化版本：
*   **RTX 4080:** E4B 跑在约 110 tokens/s
*   **M5 MacBook Pro:** E2B 跑在 60+ tokens/s

## ⚠️ 重要部署注意事项（关键补充）
我发现一个问题：使用 Unsloth 的动态量化版本，在接入 Claude Code 等工具调用流程后，会出现无法正常执行 `tools call` 的情况。因此，为了保证最佳的兼容性和稳定性，**强烈建议大家优先使用 Ollama 和 LM Studio 官方提供的原版或其官方推荐的量化版本进行本地部署和测试**。

## 总结与推荐 (Final Takeaway)
Gemma 4 小模型在显存占用、日语准确性以及翻译效果上都非常突出，LMArena 的排名也证明了这一点。如果你正在寻找一个本地可部署、日文好用且适合做翻译的小模型，现在 Gemma 4 确实是首选方案。

## 参考资料
- [Gemma 4 - Ollama](https://ollama.com/library/gemma4)
- [TranslateGemma - Ollama](https://ollama.com/library/translategemma)
- [Gemma 4 - Unsloth 文档](https://unsloth.ai/docs/models/gemma-4)
- [陪读蛙 - GitHub](https://github.com/mengxi-ream/read-frog)

---