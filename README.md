# My Journal — Guide

A Hugo blog. Source lives here; static output is built and deployed by GitHub Actions to `yxiaoli.github.io`.

> **TL;DR**: edit markdown in `content/`, push to `main`, site updates in ~2 min.

---

## 🚀 Three commands you'll use 90% of the time

```bash
cd ~/workplace/blog

# 1. Preview (drafts visible)
~/bin/hugo server --bind 127.0.0.1 --port 1313 -D

# 2. New draft
~/bin/hugo new engineering/tools/my-post.md

# 3. Publish: edit, then
git add . && git commit -m "Post: title" && git push
```

Browser: `http://localhost:1313` (SSH tunnel: `ssh -L 1313:localhost:1313 host`).

---

## 🗂 Where stuff lives

| Folder | What it is | When to touch |
|--------|------------|---------------|
| `content/` | Your markdown posts, organized by topic | Every time you write |
| `assets/css/custom.css` | All visual styling: colors, fonts, spacing | When you want to change how it *looks* |
| `layouts/` | HTML templates (post, list, homepage) | When you want to change *what shows where* |
| `layouts/partials/botanical-marker.html` | The SVG illustrations per category | To change category icons |
| `config/_default/params.toml` | Theme switches, giscus, support links | Toggling features |
| `config/_default/menus.en.toml` | Top nav items | Adding/removing nav |
| `archetypes/default.md` | Frontmatter template for new posts | Rarely |
| `static/` | Files served as-is (favicons, CNAME, .nojekyll) | Adding raw files |
| `themes/blowfish/` | Theme — submodule, **don't edit directly** | Override by copying files into `layouts/` |
| `public/` | Build output, **gitignored** | Never edit; regenerated each build |

**Key rule**: `layouts/` files **override** `themes/blowfish/layouts/`. Want to change something Blowfish controls? Copy the file into your `layouts/` (same path) and edit your copy.

---

## 🛠 The build, in 5 lines

1. `hugo` walks `content/` and parses frontmatter
2. Each post picks a layout: `layouts/_default/single.html` (your override) or theme's
3. CSS in `assets/css/*.css` gets bundled & minified into `public/css/main.bundle.min.<hash>.css`
4. `static/*` is copied verbatim into `public/`
5. `public/` is the entire site — pure HTML/CSS/JS, no server needed

Locally: `hugo` writes to `public/`. In CI: GitHub Actions runs the same `hugo` command, but uploads the result as a Pages artifact instead of leaving it on disk.

---

## ✏️ Frontmatter — the only metadata you need

```yaml
---
title: "Post Title"
date: 2026-05-29
summary: "One-line description for list pages."
tags: ["k8s", "kafka"]
categories: ["tech"]
topics: ["DevOps"]      # drives related posts
draft: true             # remove or set false to publish
---
```

`draft: true` = excluded from production. Use `hugo server -D` to see drafts locally.

---

## 🎨 Customizing the look

Everything visual lives in **one file**: `assets/css/custom.css`.

The top of that file declares CSS variables — change them and the whole site updates:

```css
:root {
  --paper:      #f5f2e8;   /* background */
  --paper-2:    #ede4d0;   /* code blocks */
  --ink:        #2d2a22;   /* body text */
  --sage:       #6b6450;   /* accents, links, illustrations */
  --sage-faded: #8a7c5e;   /* metadata */
  --divider:    #d4cdb8;   /* lines */
  --serif:      'EB Garamond', Georgia, serif;
  --sans:       'Inter', system-ui, sans-serif;
}
```

| Want to change... | Where |
|---|---|
| Background warmth | `--paper` |
| Body font | `--serif` (pair with `@import url('https://fonts.googleapis.com/css2?family=...')` at top) |
| Heading sizes | `h1`, `h2`, `h3` rules in same file |
| Link color | `article a` rule |
| Code block style | `pre`, `code` rules |
| Category SVG icons | `layouts/partials/botanical-marker.html` (paste any SVG) |

To restructure a page (add hero image, move related posts, etc.), edit the matching layout in `layouts/`.

---

## 💬 Comments

Powered by GitHub Discussions via [giscus](https://giscus.app). Already wired up:

```toml
# config/_default/params.toml
[giscus]
  repo = "yxiaoli/yxiaoli.github.io"
  repoId = "MDEwOlJlcG9zaXRvcnkxNzI2NzYxNzU="
  category = "General"
  categoryId = "DIC_kwDOCkrUT84C94ef"
```

Reactions enabled (👍❤️🎉🚀😄👀). Renders only on the deployed site, not localhost.

To moderate: go to repo → Discussions tab.

---

## 💸 Tip jar (Ko-fi / Patreon / BMC)

Fill any of these in `params.toml` to show buttons:

```toml
[support]
  kofi    = "your-username"   # https://ko-fi.com/your-username
  patreon = ""
  bmc     = ""                # buymeacoffee.com/bmc
```

Empty = hidden. No code changes needed.

---

## 🚢 Deployment

Pushing to `main` triggers `.github/workflows/deploy.yml`:
1. Checkout (with `themes/blowfish` submodule)
2. Install Hugo 0.139 extended
3. `hugo --minify`
4. Upload `public/` to Pages
5. Deploy

**Pages source must be set to "GitHub Actions"** (not "Deploy from a branch") at:
https://github.com/yxiaoli/yxiaoli.github.io/settings/pages

`static/.nojekyll` prevents Jekyll auto-processing on Pages.

---

## 📌 Pinned versions (don't upgrade casually)

- **Hugo 0.139.0 extended** at `~/bin/hugo`
- **Blowfish v2.86.0** (theme submodule)

Newer Blowfish needs Hugo 0.158+, which has GLIBC issues on this host. Either stay pinned or upgrade Hugo on a host with newer GLIBC first.

---

## 🧯 Troubleshooting

| Symptom | Fix |
|---------|-----|
| Build complains about Jekyll | Settings → Pages → Source: **GitHub Actions** |
| New post not appearing | Check `draft: true` was removed |
| CSS change not visible | Hard refresh: Cmd/Ctrl + Shift + R |
| Hugo server won't start | `pkill -f "hugo server"` then restart |
| Comments missing on localhost | Expected — only renders on real domain |
| Theme errors after `git pull` | Run `git submodule update --init --recursive` |

---

## 🗺 Where to pick up next

Likely next moves, in order of impact:

- [ ] Confirm first deploy succeeds (Actions tab green)
- [ ] Set custom domain → `xiaoli-yang.com`
- [ ] Add an About page (`content/about.md`)
- [ ] Refresh image-heavy drafts: 9 posts have broken Evernote URLs
- [ ] Decide on Ko-fi username (or skip support buttons)
- [ ] Optional: enable the Art section when ready

---

_Generated 2026-05-29. Edit freely._
