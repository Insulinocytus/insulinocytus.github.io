---
title: Running Gemma 4 Locally — Low VRAM, Great Japanese, Practical for Translation
date: 2026-04-11 15:00:00 +0900
categories: [Tools]
tags: [gemma, ollama, translation, local-deployment]
media_subpath: /../assets/media/gemma4-local-experience
---

> This article was created collaboratively by the author and AI. The author provided the core content and original rough expression, often in a spoken or fragmented form, and AI helped organize it into clearer written prose, polish the wording, and improve readability. This English version was also translated by AI from the Chinese version, so minor wording differences may exist.
{: .prompt-tip }

Versions: [中文](/posts/cn-gemma4-local-experience/) | English (Current) | [日本語](/posts/jp-gemma4-local-experience/)

After Gemma 4 launched, I deployed it locally with Ollama and LM Studio, focusing on the E2B and E4B variants. The verdict: extremely low VRAM usage, standout Japanese capability among small models, and a solid translation experience.

## VRAM Usage and Cross-Device Running

E2B and E4B have very low VRAM requirements. E2B can even run directly on a phone, and its quality is perfectly adequate for basic tasks.

Until now, open-source small models have been mostly released by Chinese teams, and their Japanese support is generally poor: MiniMax can't handle Japanese at all, and GLM and Kimi occasionally mix Chinese characters into Japanese output. Gemma 4 doesn't have this problem. For Japanese users, it should be the top choice among small models right now.

## Use Cases for Small Models

Frankly, there aren't many. Tasks that are too simple you just do yourself, and tasks that are too hard are beyond what small models can handle. My primary use case is translation.

## Translation Workflow

### Translation Extension

I used to use Immersive Translate, then switched to Read Frog. Immersive Translate is too commercialized, bloated, and closed-source; Read Frog is an open-source alternative that feels much better, despite some minor bugs.

### Translation Model

Google previously released TranslateGemma, a 4B translation-specific model, but it couldn't distinguish between Simplified and Traditional Chinese when translating to Chinese — mediocre quality overall. After switching to Gemma 4, this is basically no longer an issue.

That said, these translation extensions have an inherent limitation: they translate segment by segment rather than the entire page at once, so errors from missing context are unavoidable. It's tolerable, though.

### Reasoning Toggle

Gemma 4 supports toggling reasoning (chain-of-thought) on and off via the system prompt. For translation, it's best to disable reasoning to avoid the extra thinking output slowing things down; for everyday chatting, you can enable it to get more detailed reasoning.

## Performance

I'm using the Unsloth dynamic quantization versions:

- RTX 4080: E4B at roughly 110 tokens/s
- M5 MacBook Pro: E2B at 60+ tokens/s

## Conclusion

Gemma 4 small models excel in VRAM efficiency, Japanese quality, and translation performance, and they rank highly on LMArena. If you're looking for a locally-runnable small model with strong Japanese and good translation capability, Gemma 4 is currently the top choice.

## References

- [Gemma 4 - Ollama](https://ollama.com/library/gemma4)
- [TranslateGemma - Ollama](https://ollama.com/library/translategemma)
- [Gemma 4 - Unsloth Docs](https://unsloth.ai/docs/models/gemma-4)
- [Read Frog - GitHub](https://github.com/mengxi-ream/read-frog)
