# AGENTS.md

## Project Overview

This is a Jekyll-based blog using the Chirpy theme. The blog is configured for deployment to GitHub Pages and contains technical posts with specific markdown extensions.

## Architecture

- **Framework**: Jekyll static site generator
- **Theme**: Chirpy (v7.4+)
- **Structure**:
  - `_posts/`: Blog posts in Markdown format
  - `_config.yml`: Site configuration
  - `assets/`: Static assets (images, CSS, JS)
  - `_data/`: Data files for dynamic content
  - `_tabs/`: Custom page tabs
  - `_plugins/`: Jekyll plugins
- **Output**: Generated static site in `_site/` directory

## Common Development Commands

### Local Development
- **Start development server**: `bundle exec jekyll serve` or `bash tools/run.sh`
- **Start with production settings**: `bash tools/run.sh --production`
- **Build site**: `bundle exec jekyll build`

### Testing
- **Run tests**: `bash tools/test.sh`
- **Test with custom config**: `bash tools/test.sh -c "_config.yml,_config.test.yml"`

### Dependencies
- **Install gems**: `bundle install`
- **Update gems**: `bundle update`

## Writing Posts

⚠️ **CRITICAL**: When creating or modifying blog posts, always reference `write-a-new-post.md` in the root directory. This file contains essential information about Chirpy's markdown extensions and requirements.

### Post Structure
- **File naming**: `YYYY-MM-DD-TITLE.EXTENSION` (extension: `md` or `markdown`)
- **Location**: `_posts/` directory
- **Front Matter**: Required fields include `title`, `date`, `categories`, `tags`

### Chirpy-Specific Features
- **Enhanced image handling**: Captions, sizing, positioning, dark/light mode support
- **Media embedding**: YouTube, Twitch, Bilibili, Spotify, custom video/audio files
- **Code syntax highlighting**: With language specification and line numbers
- **Math equations**: MathJax support for LaTeX expressions
- **Mermaid diagrams**: Flowcharts and diagrams
- **Table of Contents**: Automatic generation with manual override
- **Comments**: Configurable per post or globally
- **Prompts**: Tip, info, warning, and danger callouts

### Important Front Matter Fields
```yaml
---
title: Post Title
date: YYYY-MM-DD HH:MM:SS +/-TTTT
categories: [Main Category, Subcategory]
tags: [tag1, tag2]
description: Post summary for SEO
image: /path/to/preview-image  # 1200x630 recommended
toc: true  # Table of contents
comments: true  # Enable comments
math: true  # Enable MathJax
mermaid: true  # Enable Mermaid diagrams
---
```

### Media Resources
- **CDN support**: Configure `cdn` in `_config.yml` for media hosting
- **Subpath prefixing**: Use `media_subpath` in post front matter
- **Image optimization**: LQIP (Low Quality Image Placeholders) support

## Configuration Notes

- **Theme mode**: Set to `dark` by default (configurable in `_config.yml`)
- **Timezone**: Asia/Tokyo
- **Language**: English (`en`)
- **Pagination**: 10 posts per page
- **PWA**: Enabled for installable web app experience
- **SEO**: Optimized with jekyll-seo-tag

## Deployment

- **Platform**: GitHub Pages
- **Branch**: `main` (triggers automatic deployment)
- **Build environment**: `JEKYLL_ENV=production`
- **CI/CD**: GitHub Actions workflow in `.github/workflows/pages-deploy.yml`
