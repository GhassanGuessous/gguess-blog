# gguess-blog

Personal blog built with [Hugo](https://gohugo.io/) and the [Congo](https://github.com/jpanther/congo) theme, deployed to GitHub Pages:
**https://ghassanguessous.github.io/gguess-blog/**

## Prerequisites

- **Hugo extended**, v0.154.2 (the version CI uses). On macOS: `brew install hugo`
- **Git**, because the theme is a git submodule

Check your install:

```bash
hugo version   # must say "+extended"
```

## Getting the code

```bash
git clone --recurse-submodules https://github.com/GhassanGuessous/gguess-blog.git
cd gguess-blog
```

If you already cloned without submodules (the `themes/congo` folder is empty):

```bash
git submodule update --init --recursive
```

## Running locally

```bash
hugo server -D --renderToMemory
```

Then open http://localhost:1313/gguess-blog/. The site reloads in the browser when you save a file.

> **Why `--renderToMemory`?** Without it, `hugo server` writes its pages into `public/`, with `http://localhost:1313` URLs and a live-reload script added. That leaves dev-only changes in `public/` that you could commit by accident.

| Flag | Effect |
|------|--------|
| `-D` / `--buildDrafts` | Include posts marked `draft = true` |
| `-F` / `--buildFuture` | Include posts dated in the future |
| `--disableFastRender` | Rebuild everything on each change (use this if pages look stale) |
| `-p 1314` | Use a different port |

Leave out `-D` to see the site exactly as it will be published.

## Writing content

Create a new post from the archetype (it starts as a draft):

```bash
hugo new content posts/my-new-post.md
```

When the post is ready, set `draft = false` in its front matter. Draft posts are not published.

Content layout:

```
content/
├── _index.md          # Homepage intro text
├── about/index.md     # About page
└── posts/             # Blog posts
```

## Building

Production build (the same as CI):

```bash
hugo --gc --minify
```

The output goes to `public/`. To preview the built site:

```bash
hugo server --renderToMemory --environment production
```

## Deployment

### GitHub Actions (automatic)

Every push to `main` triggers [.github/workflows/hugo.yaml](.github/workflows/hugo.yaml), which:

1. Checks out the repo, including the theme submodule
2. Installs Hugo extended 0.154.2, Dart Sass, Go and Node
3. Runs `hugo --gc --minify --baseURL "https://ghassanguessous.github.io/gguess-blog/"`
4. Uploads `public/` and deploys it to GitHub Pages

You can also start the workflow by hand from the **Actions** tab ("Build and deploy" → **Run workflow**), or with:

```bash
gh workflow run "Build and deploy"
```

One-time setup in GitHub: **Settings → Pages → Build and deployment → Source: GitHub Actions**.

### Deploying from your machine

The normal path is to push to `main`:

```bash
git add content/
git commit -m "New post: ..."
git push origin main
```

Then follow the run in the **Actions** tab, or with `gh run watch`.

## Updating the theme

```bash
git submodule update --remote themes/congo
hugo server -D -M         # check that nothing broke
git add themes/congo
git commit -m "Update Congo theme"
```

## Configuration

Site settings live in [config/_default/](config/_default/):

| File | Purpose |
|------|---------|
| `hugo.toml` | Base URL, theme, outputs, pagination |
| `params.toml` | Congo theme options (header, homepage, article display, search) |
| `languages.en.toml` | Site title, date format, author profile and social links |
| `menus.en.toml` | Header and footer menus |
| `markup.toml` | Markdown rendering and syntax highlighting |
| `module.toml` | Minimum Hugo version |

Theme documentation: https://jpanther.github.io/congo/docs/
