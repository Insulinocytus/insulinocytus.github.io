---
title: 本地部署 Gemma 4 小模型：显存友好、日语优秀、翻译实用
date: 2026-04-11 15:00:00 +0900
categories: [工具]
tags: [gemma, ollama, 翻译, 本地部署]
media_subpath: /../assets/media/gemma4-local-experience
---

> 本文由作者提供核心内容与原始表达，并与 AI 协作完成。AI 主要负责将偏口语化、碎片化的内容整理为更清晰的书面表达，并辅助润色结构与可读性。
{: .prompt-tip }

版本：中文（当前） | [English](/posts/en-gemma4-local-experience/) | [日本語](/posts/jp-gemma4-local-experience/)

Gemma 4 发布后我用 Ollama 和 LM Studio 在本地部署试了一下，重点关注 E2B 和 E4B 两个小尺寸版本。结论：显存占用极低，日语能力在同类小模型中突出，做本地翻译体验很好。

## 显存占用与跨设备运行

E2B 和 E4B 的显存占用非常小。E2B 甚至可以直接在手机上跑，效果对于基础任务完全够用。

在此之前，开源小模型基本是中国团队主导发布，但他们的模型日语普遍不行：MiniMax 完全无法使用日语，GLM 和 Kimi 翻译日语时会偶尔混入中文。Gemma 4 没有这个问题。对日本用户来说，应该是目前小模型里的首选。

## 小模型的使用场景

坦白讲，小模型的应用场景不算多。太简单的任务自己就做了，太难的任务小模型也做不来。我目前最主要的用途就是翻译。

## 翻译工作流

### 翻译插件

以前用沉浸式翻译，后来换成陪读蛙。沉浸式翻译商业化太重、臃肿、闭源；陪读蛙是开源替代，体验舒服很多，虽然有些小 bug。

### 翻译模型

Google 之前出过 TranslateGemma，4B 的翻译专用模型，但翻译到中文时分不清简体和繁体，质量一般。换成 Gemma 4 之后基本没问题了。

不过这类翻译插件有个固有限制：它们是按段翻译而非整页翻译，上下文缺失导致的错误没法完全避免。但这是可以忍的。

### Reasoning 开关

Gemma 4 支持通过系统提示词控制 reasoning（思维链）的开关。翻译时建议关闭 reasoning，避免额外的思考输出拖慢速度；日常聊天则可以开启，让模型给出更详细的推理过程。

## 性能数据

我用的是 Unsloth 动态量化版本：

- RTX 4080：E4B 约 110 tokens/s
- M5 MacBook Pro：E2B 约 60+ tokens/s

## 总结

Gemma 4 小模型在显存占用、日语质量和翻译效果上都表现出色，LMArena 上的排名也很靠前。如果你在找一个本地可跑、日语好、适合翻译的小模型，Gemma 4 目前是首选。

## 参考资料

- [Gemma 4 - Ollama](https://ollama.com/library/gemma4)
- [TranslateGemma - Ollama](https://ollama.com/library/translategemma)
- [Gemma 4 - Unsloth 文档](https://unsloth.ai/docs/models/gemma-4)
- [陪读蛙 - GitHub](https://github.com/mengxi-ream/read-frog)
