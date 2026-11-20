# CLAUDE.md

Personal Hugo blog using the Congo theme, deployed to GitHub Pages at https://ghassanguessous.github.io/gguess-blog/.

## Commands

```bash
hugo server -D --renderToMemory     # dev server with drafts → http://localhost:1313/gguess-blog/
hugo --gc --minify                  # production build into public/
hugo new content posts/<slug>.md    # new post (starts as draft = true)
git submodule update --init --recursive   # fetch the theme if themes/congo is empty
```

Requires Hugo **extended**. CI pins v0.154.2 (`HUGO_VERSION` in the workflow); keep local and CI versions in sync.

## Layout

- `config/_default/`: all site config, split by concern (`hugo.toml`, `params.toml` for theme options, `languages.en.toml` for title and author, `menus.en.toml`, `markup.toml`, `module.toml`). There is no root `hugo.toml`.
- `content/`: `_index.md` (homepage intro), `about/index.md`, `posts/*.md`
- `archetypes/default.md`: front matter template (TOML `+++`, `draft = true`)
- `themes/congo/`: **git submodule**. Do not edit files here. Override a theme file by copying it to the same path under the root `layouts/`, `assets/` or `i18n/`.
- `layouts/`, `assets/`, `static/`, `data/`, `i18n/`: empty for now; this is where overrides go.
- `.github/workflows/hugo.yaml`: builds and deploys to GitHub Pages on every push to `main`.

## Conventions

- Front matter is TOML (`+++`) with `date`, `draft`, `title`.
- Post filenames are kebab-case slugs. Posts are in English.
- The site is served under the `/gguess-blog/` subpath. Use `relref` or relative links, never hard-coded `/` root paths.
- Theme options are documented at https://jpanther.github.io/congo/docs/configuration/

## Gotchas

- Always pass `--renderToMemory` (`-M`) to `hugo server`. Without it, Hugo 0.154 writes dev output into `public/` with `localhost:1313` URLs and a livereload script.
- `public/` and `.hugo_build.lock` are build output and are gitignored. CI rebuilds `public/` from scratch on each deploy. Never edit or commit them.
- CI overrides `baseURL` with `--baseURL`. If the URL changes, update both the workflow and `config/_default/hugo.toml`.
- Draft posts (`draft = true`) are not published. Check this before telling the user a post is live.
