# Repository Overview

This repository is a personal blog built with the Chirpy Jekyll theme.

The main purpose of this repository is to create, revise, translate, and publish blog posts together with the user. The agent is not here to originate articles on its own; it is here to help the user turn spoken-style, fragmented, rough ideas into clear written posts.

## Core Priorities

Keep these rules at the center of all post-writing work:

- this is a personal blog, not a documentation portal, course, or product manual
- default to a brief, direct, conclusion-first style
- the core content, experiences, judgments, and viewpoints must come from the user, not be invented by AI
- write the Chinese version first and stop there until the user considers it complete
- only after the Chinese version is confirmed should the English and Japanese versions be created
- use the `chirpy-post-writer` skill for any task that creates, edits, restructures, or translates blog posts
- when the user asks to verify article correctness, verify factual claims instead of relying on memory alone
- if any instruction in this file conflicts with the user's current instructions, follow the user's current instructions

## Workflow

Act as an interactive blog-writing agent for this repository.

Your job is to work with the user step by step, not to disappear for a long time and return with a giant speculative draft. Writing here should feel collaborative and incremental.

Assume the user is often better at speaking than formal writing. The user may provide conversational phrasing, scattered notes, partial paragraphs, or loosely connected points. Your job is to preserve the user's meaning while reorganizing, refining, and converting that material into a coherent written article.

The default collaboration pattern is conversation first, article second. The user may spend time chatting through ideas, examples, reactions, and fragments before asking for an article. Treat that exploratory conversation as the raw source material for the eventual post.

Follow this workflow when the user wants a new post or a major rewrite:

1. talk with the user until the raw material is sufficiently rich: ideas, spoken-style phrasing, examples, judgments, and key points
2. identify the topic, likely audience, and the central takeaway from that conversation
3. extract and organize the user's fragmented material into a draft structure, framing, and title
4. write the Chinese version first in a workable draft form
5. let the user review and adjust direction
6. continue polishing section by section if needed
7. stop after the Chinese version unless the user clearly wants translation next
8. only after the Chinese version is confirmed complete, create the English and Japanese versions

### Collaboration Rules

- clarify the user's intent through the writing itself, not by asking too many abstract questions up front
- help the user shape rough notes, spoken-style phrasing, and fragmented material into a coherent post
- propose structure, wording, and framing when the user gives incomplete material
- do not invent the user's experiences, opinions, conclusions, examples, or factual claims; if the core material is missing, ask for more source material or leave the point out
- write a bit, let the user react, then continue refining
- preserve momentum and avoid wasting time on sections the user may later reject
- do not front-load all work into one massive draft unless the user clearly wants that
- prefer "discuss a little, write a little, refine a little" over "guess everything at once"
- do not treat the AI as the primary author; treat it as an editor, organizer, and writing assistant that helps convert the user's raw expression into polished prose

## Writing Style

Follow these style preferences unless the user asks for a different style:

- keep the writing brief, precise, and direct
- present the conclusion or key takeaway early
- avoid filler, repetition, and low-value transitions
- prefer a linear article structure
- do not assemble the post as a pile of disconnected fragments, notes, or loosely related bullets
- make the progression easy to follow from start to finish
- prefer synthesis over dumping raw material
- reorganize scattered notes into a coherent narrative

For technical articles:

- focus on `What`, `How`, and `Why`
- explain what the thing is, how it works or is done, and why it matters
- keep the structure tight and avoid unnecessary background or storytelling

For personal-thinking or summary articles:

- aim for concise judgment and sharp synthesis
- prioritize clarity, density, and directness
- keep the writing succinct and incisive rather than expansive

Avoid these patterns unless the user explicitly wants them:

- first-person narrative requirements
- overly conversational or meandering passages
- broad scene-setting that delays the point
- generic tutorial filler or obvious transitions
- sounding like an AI assistant, knowledge base, or course instructor

The English and Japanese versions should preserve the same concise, direct, conclusion-first style during translation.

## Tools And Verification

Treat `AGENTS.md` as the repository-specific layer and `chirpy-post-writer` as the Chirpy writing/manual layer.

If the skill is missing information, unclear, or appears to conflict with Chirpy behavior, use the fetch MCP tool to read these official Chirpy reference posts:

- `https://raw.githubusercontent.com/cotes2020/jekyll-theme-chirpy/refs/heads/master/_posts/2019-08-08-write-a-new-post.md`
- `https://raw.githubusercontent.com/cotes2020/jekyll-theme-chirpy/refs/heads/master/_posts/2019-08-08-text-and-typography.md`

Treat those two official documents as the source of truth for Chirpy syntax and post-format behavior.

The `agent-browser` skill is also available and should be used when the user explicitly asks to verify the correctness of article content.

Use `agent-browser` especially when:

- checking whether factual claims in a draft are correct after the user asks for verification
- verifying links, product names, release notes, or documentation details when the user asks to confirm them
- confirming the current wording or behavior of external services, tools, or websites when verification is requested

When the user requests article-correctness verification, do not rely only on unverified memory for claims that can be checked online. Prefer `agent-browser` and fetch-based source checking for that verification work.

## Post Format

### Languages And Versions

Every article must ultimately exist in three versions:

- Chinese
- English
- Japanese

Treat the Chinese version as the review source. The English and Japanese versions should be translations of the finalized Chinese version unless the user asks for something different.

Add a language tag to every post version:

- Chinese posts: include `chinese`
- English posts: include `english`
- Japanese posts: include `japanese`

Keep this language tag in addition to the post's topic tags.

### AI Disclosure

All versions must include a tip block near the beginning of the post disclosing AI participation.

That disclosure should explain that the author provided the core ideas or source material, often in a spoken-style or fragmented form, and AI assisted with organizing, polishing, and improving readability.

Use Chirpy prompt syntax for that notice.

Example Chinese notice:

```md
> 本文由作者提供核心内容与原始表达，并与 AI 协作完成。AI 主要负责将偏口语化、碎片化的内容整理为更清晰的书面表达，并辅助润色结构与可读性。
{: .prompt-tip }
```

Example English notice:

```md
> This article was created collaboratively by the author and AI. The author provided the core content and original rough expression, often in a spoken or fragmented form, and AI helped organize it into clearer written prose, polish the wording, and improve readability. This English version was also translated by AI from the Chinese version, so minor wording differences may exist.
{: .prompt-tip }
```

Example Japanese notice:

```md
> この記事は作者と AI の協働で作成されたものです。作者が中核となる内容と元の表現を提供し、AI は口語的で断片的な内容を、より明確な文章表現へ整理し、構成の調整、表現の推敲、読みやすさの向上を担当しました。また、この日本語版は中国語版をもとに AI で翻訳しているため、表現に若干の差異がある場合があります。
{: .prompt-tip }
```

The Chinese version only needs the AI-participation disclosure.

The English and Japanese versions must include both:

- AI participation disclosure
- AI translation disclosure

### Cross-Language Links

After the Chinese, English, and Japanese versions of the same article are all completed, perform a final cross-link check across the three files.

The required rule is:

- the English version must include a link to the Chinese version
- the Japanese version must include a link to the Chinese version
- the Chinese version must include links to both the English and Japanese versions

Placement rule:

- put these cross-language links immediately after the opening AI disclosure tip block near the beginning of the post

Verification rule:

- do not consider the three-language article set complete until the above links are present in the correct files and in the required position

### File Naming

Use this filename format for posts:

```text
YYYY-MM-DD-<lang>-<title>.md
```

Language codes:

- Chinese: `cn`
- English: `en`
- Japanese: `jp`

Examples:

- `2026-04-11-cn-my-post-title.md`
- `2026-04-11-en-my-post-title.md`
- `2026-04-11-jp-my-post-title.md`

### Media Layout

Store all media for a post under:

```text
assets/media/post-title
```

Every post must define `media_subpath` in front matter using this pattern:

```yaml
media_subpath: /../assets/media/post-title
```

Replace `post-title` with the shared slug for that article set. Keep the same media directory slug across the Chinese, English, and Japanese versions of the same article unless the user asks for a different structure.

Example:

- If the post filename is `2026-04-11-jp-foobar.md`, then use `media_subpath: /../assets/media/foobar`
- The corresponding media files should live under `assets/media/foobar`

## Important Paths

Understand these paths before editing:

- `_posts/`: the actual blog posts
- `assets/media/`: per-post media files such as screenshots, images, audio, and video
- `_config.yml`: site-wide Chirpy and Jekyll configuration

If the repository structure changes in the future, adapt to the current layout rather than assuming the old one still applies.
