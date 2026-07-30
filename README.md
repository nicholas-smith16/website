# Academic Website — Nicholas Lloyd Smith

Quarto site deployed to GitHub Pages. Companion to the CV system in `../CV/`.

## Files

| File | Purpose |
|------|---------|
| `_quarto.yml` | Site config: nav, theme, footer. **The nav is defined here.** |
| `index.qmd` | Home — bio, research interests, contact |
| `research.qmd` | Work in progress, fellowships |
| `teaching.qmd` | Teaching (placeholder until TATTO) |
| `cv.qmd` | Links + embeds the CV PDF |
| `styles.css` | All custom styling (~130 lines) |
| `notes/` | Writing section — **built but hidden from the nav** |
| `assets/` | Headshot and CV PDF |

## Local preview

```bash
quarto preview
```

Live-reloads on save. `Ctrl-C` to stop.

## Build

```bash
quarto render
```

Output goes to `_site/` (git-ignored — it's rebuilt on deploy).

## Deploy

```bash
quarto publish gh-pages
```

Pushes the rendered site to the `gh-pages` branch. First run will prompt to
confirm the repo.

## Updating the CV

The CV lives in `../CV/` and that stays the source of truth. This site holds a
**copy**. After recompiling the CV, re-copy it:

```bash
cp ../CV/nicholas_smith_cv_public.pdf assets/nicholas_smith_cv.pdf
quarto render
```

## Publishing the notes section

`notes/` is fully built but deliberately absent from the nav, so there's no
stale-blog risk while it's empty. To turn it on:

1. Add a post at `notes/posts/YYYY-MM-DD-slug.qmd` with front matter:
   ```yaml
   ---
   title: "Post title"
   date: 2026-09-01
   description: "One-line summary shown in the listing."
   ---
   ```
2. Uncomment the `notes/index.qmd` entry in the `navbar.left` block of
   `_quarto.yml`.
3. `quarto render`

Until step 1, `quarto render` prints a harmless warning that the listing
matches no files. That is expected.

## Adding a paper

Edit `research.qmd`. The pattern:

```markdown
::: {.paper}
[Paper Title]{.title}

[with Coauthor Name · Status or venue]{.meta}
:::
```

Add an abstract as a plain paragraph inside the `:::` block when one exists.

## Domain

`nicholaslsmith.com` → GitHub Pages. The `CNAME` file in this directory is what
binds them; do not delete it, as `quarto publish` copies it into the deploy.

DNS is managed at **Cloudflare** (also the registrar). Cloudflare supports
CNAME flattening at the apex, so two CNAME records replace the four A records
GitHub's docs describe:

| Type | Name | Target | Proxy status |
|------|------|--------|--------------|
| CNAME | `@` | `nicholas-smith16.github.io` | **DNS only** (grey cloud) |
| CNAME | `www` | `nicholas-smith16.github.io` | **DNS only** (grey cloud) |

**The grey cloud matters.** With Cloudflare's proxy on (orange cloud), GitHub
cannot complete the ACME challenge to issue the TLS certificate, and *Enforce
HTTPS* stays greyed out. Leave both records DNS-only.

A static site on GitHub Pages gains little from the Cloudflare proxy, so the
simplest correct configuration is to leave it off permanently. If you ever do
enable it, set Cloudflare's SSL/TLS mode to **Full (strict)** first — anything
less causes a redirect loop.

Once DNS resolves, GitHub issues the certificate automatically (usually minutes,
occasionally up to an hour). Then enable **Enforce HTTPS** under
Settings → Pages.
