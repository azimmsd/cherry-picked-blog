# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A bilingual (English + Persian/Farsi) personal blog called **CherryPicked** built with [Hugo](https://gohugo.io/) and the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme. Deployed automatically to GitHub Pages on every push to `main`.

## Commands

```bash
# Start local dev server (live reload)
hugo server

# Build the site to ./public/
hugo

# Create a new post (use this archetype pattern)
hugo new content/en/posts/my-post-slug.md
hugo new content/fa/posts/my-post-slug.md
```

## Architecture

### Bilingual content
Content is split into two directories, one per language:
- `content/en/` — English (default language, served at `/`)
- `content/fa/` — Persian, served at `/fa/`

To link a post's translations together, both files must share the same `translationKey` frontmatter field. Without it, Hugo won't know they're the same post and won't show a language-switcher link.

```toml
# content/en/posts/my-post.md and content/fa/posts/my-post.md both need:
translationKey = 'my-post'
```

### Theme and layout overrides
The PaperMod theme lives in `themes/PaperMod/` as a **git submodule**. Never edit files inside `themes/PaperMod/` directly — override them by placing files at the same relative path under `layouts/`.

Current overrides in `layouts/partials/`:
- `home_info.html` — adds a circular profile photo above the home page intro text
- `extend_head.html` — injects RTL CSS and the Vazirmatn font when the active language is `fa`

### Persian/RTL support
The `extend_head.html` partial handles all RTL layout for the Persian version. Code blocks inside Persian pages are explicitly kept LTR. To add RTL-safe HTML in a Persian template, apply the `.ltr` CSS class to force left-to-right direction on a specific element.

### i18n strings
Custom translation strings live in `i18n/en.toml` and `i18n/fa.toml`. These supplement (not replace) PaperMod's built-in i18n strings found in `themes/PaperMod/i18n/`.

### Deployment
GitHub Actions (`.github/workflows/hugo.yml`) builds with `hugo --gc --minify` and deploys to GitHub Pages. The `public/` directory is a build artifact — it is not gitignored but should not be manually edited.
