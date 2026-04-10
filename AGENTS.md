# Blog Writing Rules

Use the `chirpy-post-writer` skill for any task that creates, edits, restructures, or translates blog posts in this repository.

If the skill is missing information, unclear, or appears to conflict with Chirpy behavior, use the fetch MCP tool to read these official Chirpy reference posts:

- `https://raw.githubusercontent.com/cotes2020/jekyll-theme-chirpy/refs/heads/master/_posts/2019-08-08-write-a-new-post.md`
- `https://raw.githubusercontent.com/cotes2020/jekyll-theme-chirpy/refs/heads/master/_posts/2019-08-08-text-and-typography.md`

Treat those two official documents as the source of truth for Chirpy syntax and post-format behavior.

## Language Workflow

Every article must ultimately exist in three versions:

- Chinese
- English
- Japanese

Write the Chinese version first. Stop after the Chinese version unless the user clearly indicates that the Chinese draft is finished and the English and Japanese versions should now be created.

Do not generate the English or Japanese versions early just to be complete. This is required to avoid wasting time and tokens before the Chinese version has been reviewed.

Treat the Chinese version as the review source. The English and Japanese versions should be translations of the finalized Chinese version unless the user asks for something different.

## Translation Notice

The English and Japanese versions must include a tip block near the beginning of the post stating that the content was translated by AI.

Use Chirpy prompt syntax for that notice.

Example English notice:

```md
> This article was translated by AI from the Chinese version. Minor wording differences may exist.
{: .prompt-tip }
```

Example Japanese notice:

```md
> この記事は中国語版をもとに AI で翻訳したものです。表現に若干の差異がある場合があります。
{: .prompt-tip }
```

The Chinese version does not need this translation notice unless the user asks for it.

## File Naming

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

## Tags

Add a language tag to every post version:

- Chinese posts: include `chinese`
- English posts: include `english`
- Japanese posts: include `japanese`

Keep this language tag in addition to the post's topic tags.

## Media Layout

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

## Working Order

When asked to write a new article, follow this order:

1. Use `chirpy-post-writer`.
2. Create or update the Chinese post first.
3. Wait for the user to confirm the Chinese version is complete.
4. Create the English translation with the required AI-translation tip.
5. Create the Japanese translation with the required AI-translation tip.

If any instruction in this file conflicts with repository-specific user instructions in the current conversation, follow the user's current instruction.
