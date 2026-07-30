# Academic Website — Nicholas L. Smith

> **Status:** Planned, awaiting approval to execute. Written 2026-07-29.

## Context

Entering Year 2 of an Economics PhD at Emory, targeting a tenure-track position with a
Year 5 job market (2029–2030). No personal website yet; the LaTeX CV at
`../CV/nicholas_smith_cv_public.tex:63` carries a
`% TODO: Add personal website URL here once created` placeholder waiting on this.

The site is a **four-year asset**, not a one-off. It needs to accumulate working papers,
teaching experience (TATTO 605/610 in Years 3–4), and presentations as they happen, and
be credible to a hiring committee in 2030. So maintainability and a low friction cost per
update matter more than initial polish.

## Decisions made

| Decision | Choice | Rationale |
|---|---|---|
| Toolchain | **Quarto** | Verified that `edrub.in` — the favorite reference — *is* Quarto (`generator: quarto-1.9.36`). Its entire custom design is [76 lines of CSS](https://edrub.in/styles.css). Quarto also renders R/Python/Stata and reads `.bib`, matching the existing stack. |
| Visual direction | **Spartan / dead-simple** | Inherit Quarto's layout; keep the stylesheet minimal. |
| Blog | **Scaffold but hide** | Machinery built, unlinked from nav. One config line reveals it when there's a first post. Avoids stale-blog risk. |
| Hosting | **GitHub Pages + custom domain** | Free, git-versioned (matches the existing CV repo workflow), survives leaving Emory. |
| Domain | **nicholaslsmith.com** | Verified available 2026-07-29. Matches CV byline exactly; the middle initial is what makes a common name findable. |

### Rejected, with reasons

- **Jekyll + Chirpy** — blog-shaped rather than CV-shaped, and macOS system Ruby 2.6
  means installing rbenv/Homebrew Ruby before anything runs.
- **Serif academic (jftripp.org style)** — not the preferred direction.
- **Hand-coded HTML** — would mean *more* design iteration, not less, since every
  layout decision stays open. Quarto narrows the conversation to type and color.
- **Quarto stripped toward spartan** — highest iteration cost; fights the framework.
- **Domain hack (`smi.th`, à la `edrub.in`)** — `.in` is unrestricted, but `.th` requires
  a Thai residential/office address per THNIC and sells mainly at the third level.
  Also: hacks are ambiguous spoken aloud, hurt findability for a common name, and carry
  ccTLD policy risk (cf. `.io` in 2024). A vanity domain can 301-redirect here later.

## Environment (verified 2026-07-29)

- Homebrew present at `/opt/homebrew/bin/brew`
- **`quarto` and `gh` are NOT installed**
- `git` present; `../CV/` is already a git repo
- Headshots are 2.8–8.2 MB at up to 3908×4396 — all need web-sized derivatives

## Approach

New Quarto project in this `Website/` folder, deployed to GitHub Pages. Reuse the
discipline of `../CV/README.md` — a documented, explicit update workflow — since that
pattern demonstrably works here.

### 1. Setup

- `brew install --cask quarto`
- `git init`, with `.gitignore` covering `_site/`, `.quarto/`, `.DS_Store`

### 2. Structure

```
Website/
  _quarto.yml          # nav, theme, site metadata
  index.qmd            # bio, research interests, contact, links
  research.qmd         # works in progress w/ abstracts
  teaching.qmd         # placeholder until TATTO (Year 3)
  cv.qmd               # embeds + links the PDF
  styles.css           # the spartan stylesheet
  notes/               # blog — built, NOT in nav
    index.qmd          # listing page
    .gitkeep
  assets/
    headshot.jpg       # web-sized derivative
    nicholas_smith_cv_public.pdf
  CNAME                # nicholaslsmith.com
  README.md            # update workflow
```

### 3. Content sources

All drawn from files already in this folder — no invention. Anything not evidenced in
these sources gets left as an explicit placeholder rather than filled in.

- **Bio / interests** — `../4-year Plan/Student Profile.md`,
  `../CV/nicholas_smith_cv_public.tex` (Health Economics, Labor Economics, Causal
  Inference, Economics of Risky Behaviors)
- **Research** — two WIP papers from the CV: "Male Entry to Female-Dominated Occupations"
  (with Sara J. Markowitz) and "Effects of Sports Gambling Legalization on Health
  Outcomes." Abstracts are **not** in any source file → placeholder, to be written.
- **Contact / affiliation** — CV header (Emory Dept. of Economics, R. Randall Rollins
  Building R400A-4, nicholas.smith@emory.edu). Omit the personal phone number from a
  public page.
- **Fellowship** — AIBM Graduate Research Fellowship, 2026–2027
- **Prior career** — CDC/Guidehouse PFAS Multi-site Study, condensed to 2–3 lines

### 4. Styling

A deliberately small `styles.css` in the spirit of Ed Rubin's 76 lines — set body
font/size/color, link color and hover, page width, and heading treatment; inherit
everything else. Ed's specific palette (Inconsolata, `#696969`, `#663399`, `#FF1493`)
is his signature, so pick a distinct but equally restrained set rather than copying it.
Leaning spartan: system or near-system font stack, high-contrast text, one accent color.

Ship an initial take, then iterate — this is where the 3–4 rounds belong.

### 5. Assets

- `sips` the chosen headshot to ~600px wide (from 2.8 MB to well under 200 KB).
  Recommendation: `Headshots/headshot_2026-05-06.jpg` as the least cropping-dependent.
- Copy `../CV/nicholas_smith_cv_public.pdf` into `assets/`. This is a **copy** — the CV
  repo stays the source of truth, and the README documents re-copying on update.

### 6. Deploy (needs action from Nicholas)

1. Register `nicholaslsmith.com`
2. Create the GitHub repo — needs `gh` installed + authed, or create it in the browser
3. `quarto publish gh-pages` for the first deploy
4. DNS at the registrar: four `A` records → GitHub Pages IPs, plus `CNAME` for `www`
5. Enable HTTPS in repo settings once DNS propagates (1–48 hrs)

Live at `*.github.io` immediately; custom domain follows propagation.

### 7. Follow-up

Update `../CV/nicholas_smith_cv_public.tex:63-64` — uncomment the website line and insert
the URL. Same edit in the master `nicholas_smith_cv.tex`, per the master-first workflow in
`../CV/README.md`. Recompile both.

## Verification

1. `quarto preview` — all pages render, nav works, no broken links
2. Confirm `notes/` is reachable by URL but absent from the nav
3. Confirm the CV PDF opens and the headshot loads at a sane file size
4. Narrow the browser to phone width — Quarto's responsive nav should collapse cleanly
5. After deploy: load the live URL, verify HTTPS, re-check the CV PDF link over the network
6. `curl -sI` against the live domain to confirm the redirect and cert

## Estimate

~1.5–2 hrs of working time before iteration; viewable local preview at ~45–60 min.
Then 3–4 iteration rounds on styling. Blocked-on-him items (domain, repo, DNS) total
~15 min plus propagation wait.

## Open items

- [ ] Which headshot (recommendation: `headshot_2026-05-06.jpg`)
- [ ] Abstracts for the two WIP papers — placeholders until supplied
- [ ] Whether to list the CDC/Guidehouse role prominently or keep it brief
- [ ] Register `nicholaslsmith.com`
