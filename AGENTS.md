# Academic Website Skill (al-folio + GitHub Pages)

Use this file as your Codex instructions (`AGENTS.md` in the repo root, or paste
into Codex's system/context prompt). It tells Codex exactly how this site is
built, where things live, and how to make safe, simple edits without breaking
the build.

## What this project is

- Theme: **al-folio** (https://github.com/alshedivat/al-folio), a Jekyll
  static-site theme for academic personal websites.
- Hosting: **GitHub Pages**, served from a repo named `<username>.github.io`.
- Deployment is automatic: pushing to the main branch triggers a GitHub Action
  that builds the Jekyll site and publishes it. Never hand-edit anything in a
  `_site/` or `gh-pages` output folder — always edit the **source** files
  below and let the Action rebuild.
- Content is separated from design: almost every edit is a Markdown or YAML
  file, not HTML/CSS/Ruby. Don't touch the Ruby/Jekyll plumbing unless asked.

## Where things live (edit these, not the theme internals)

| I want to change...                   | Edit this                                                 |
| ------------------------------------- | --------------------------------------------------------- |
| Name, title, bio, photo, social links | `_config.yml` and `_pages/about.md`                       |
| Publications list                     | `_bibliography/papers.bib` (BibTeX format)                |
| CV                                    | `_pages/cv.md` (or `assets/pdf/cv.pdf` if using a PDF CV) |
| News / announcements                  | `_news/` (one Markdown file per item)                     |
| Projects / portfolio                  | `_projects/` (one Markdown file per project)              |
| Blog posts                            | `_posts/` (one Markdown file per post)                    |
| Teaching page                         | `_pages/teaching.md`                                      |
| Site colors / theme                   | `_sass/_variables.scss` (only if explicitly asked)        |
| Navbar structure                      | `_data/navigation.yml`                                    |
| Profile photo & images                | `assets/img/`                                             |
| Site title, description, URL          | `_config.yml`                                             |

**Rule of thumb:** if the task is "add/update content" (a paper, a bio line, a
news item, a photo), only touch Markdown/YAML/BibTeX files in the tables
above. Never modify `_layouts/`, `_includes/`, `_plugins/`, or `Gemfile`
unless the user explicitly asks for a design/feature change — those control
the theme itself and are easy to break.

## Standard workflow for any edit

1. Make the content change in the appropriate file from the table above.
2. If possible, preview locally: `bundle exec jekyll serve -l -H localhost`,
   then check `http://localhost:4000`. If Jekyll/Ruby isn't installed, skip
   preview and just describe the change clearly instead of guessing.
3. Commit with a clear, specific message (e.g. `Add 2026 JMLR paper to
publications`, not `update`).
4. Push to the branch GitHub Pages builds from (usually `main`).
5. Confirm to the user: "Pushed. GitHub Actions will rebuild the site in
   ~1-2 minutes; refresh the live URL after that."

Never force-push. Never rewrite git history. Never delete `_config.yml`,
`Gemfile`, or `.github/workflows/` files.

## Common edit recipes

**Add a publication**
Append a new BibTeX entry to `_bibliography/papers.bib`. Keep the existing
entry format/fields consistent with what's already there. Common optional
fields the theme supports: `abbr`, `pdf`, `code`, `video`, `award`,
`abstract`.

**Add a news item**
Create a new file in `_news/` following the naming and front-matter pattern of
existing files there (usually date-prefixed, e.g. `2026-08-01-new-paper.md`).

**Update the bio / homepage**
Edit the front matter and body text in `_pages/about.md`. Keep the YAML front
matter (the `---` delimited block at the top) intact — only change values,
not keys, unless asked.

**Swap the profile photo**
Replace the image file in `assets/img/` with the new photo, keeping the same
filename if `_pages/about.md`/`_config.yml` already reference it — otherwise
update the filename reference too.

**Change a color or font**
Only do this if explicitly requested. Edit variables in
`_sass/_variables.scss` (or the theme's documented color config in
`_config.yml` if it exposes one) rather than adding inline CSS.

**Change the theme color**
Edit `--global-theme-color` in `_sass/_themes.scss` (pick from the theme's
preset color names, or use a custom hex). For deeper styling — fonts,
spacing, link colors, dark/light defaults — override values in
`_sass/_variables.scss`. Don't touch files under `_layouts/` or `_includes/`
for a color change; it's always a variable swap in `_sass/`.

**Add a navbar tab / page**

1. Duplicate an existing file in `_pages/` that's structurally similar to
   what's needed (e.g. copy `_pages/teaching.md` to make `_pages/talks.md`).
2. Update its front matter: `title`, `permalink`, `nav: true`, and
   `nav_order` (controls left-to-right position in the navbar).
3. Fill in the page content in Markdown below the front matter.
4. If it should be a full content type (like publications or projects) rather
   than a single static page, add it as a new collection in `_config.yml`
   instead (see "collections" below), with its own folder and a landing page
   modeled on `_pages/projects.md`.

**Remove a navbar tab / page**

- To hide without deleting: set `nav: false` in that page's front matter.
- To remove entirely: delete the corresponding file in `_pages/` (and its
  folder under `_news/`, `_projects/`, etc. if it's a collection).

**Add a new content collection (beyond news/projects)**

1. Add a new entry under `collections:` in `_config.yml`.
2. Create a matching folder (e.g. `_talks/`) with content files in it.
3. Add a landing page in `_pages/` (copy `_pages/projects.md` as a starting
   point) and set its `nav`/`nav_order` front matter.

## Theme preset: Dark / Violet (reference-inspired)

The site should be reskinned toward a dark background with a violet/purple
accent, inspired by a reference site (sharmaswastik.github.io). This is a
**variable-only change** — do not touch `_layouts/` or `_includes/` for this.

**1. Colors — edit `_sass/_themes.scss`**
Set (adjust exact hex to taste once previewed, these are close starting points):

```scss
// Dark mode (make this the default if the theme supports a default toggle)
--global-bg-color: #0d0d0f;
--global-code-bg-color: #16161a;
--global-text-color: #f2f2f2;
--global-text-color-light: #a1a1aa; // secondary/gray text
--global-theme-color: #9333ea; // primary violet accent (links, active nav, icons)
--global-hover-color: #a855f7; // lighter violet for hover states
--global-footer-bg-color: #0d0d0f;
--global-footer-text-color: #a1a1aa;
--global-footer-link-color: #a855f7;
--global-distill-app-color: #a1a1aa;
--global-divider-color: rgba(168, 85, 247, 0.25);
--global-card-bg-color: #111114;
```

If `_config.yml` has a `theme:` or dark-mode-default setting, set it so the
site loads in dark mode by default rather than requiring a toggle click,
since the reference site is dark-only.

**2. Card / project accents — `_sass/_variables.scss` or the projects partial**
The reference site uses a violet left-border accent on project/publication
cards. If al-folio's project cards support a border variable, set it to
`var(--global-theme-color)` at low opacity (e.g. `rgba(147, 51, 234, 0.4)`)
on the left edge only, not a full border box.

**3. Typography accent (optional, matches reference)**
The reference site uses a monospace font for the subtitle line ("Ph.D.
Candidate at..."). If desired, import a monospace Google Font (e.g.
`JetBrains Mono` or `IBM Plex Mono`) via `_includes/head.html`'s font import
section, and apply it only to the subtitle/tagline element — not site-wide.

**4. (Stretch, optional) Sticky sidebar**
The reference site's photo/bio panel stays fixed while content scrolls.
al-folio's default About layout is not built this way. If requested later,
this needs a small custom CSS rule (`position: sticky; top: 2rem;`) on the
profile/author container in the about page's layout, tested carefully so it
doesn't break on mobile (should fall back to normal stacked flow on small
screens, e.g. via a max-width media query).

## Keep all theme values editable (single source of truth)

Every color, font, and background used anywhere on the site must live as a
CSS custom property in `_sass/_themes.scss` (colors/backgrounds) and
`_sass/_variables.scss` (fonts, spacing, sizing) — never hardcoded inline in
a layout, include, or page. This is what makes future changes a one-line
edit instead of a hunt through the codebase.

**Rules for any styling change:**

- If a color/font/spacing value doesn't already have a variable, add one in
  `_themes.scss`/`_variables.scss` first, with a short comment saying what it
  controls (e.g. `--global-theme-color: #9333ea; // primary accent: links, active nav, icons`), then reference that variable everywhere it's needed.
- Never write a raw hex code, `px` size, or font name directly inside
  `_layouts/`, `_includes/`, or component-level SCSS files — always
  `var(--variable-name)`.
- Fonts should be declared once as variables too, e.g.:
  ```scss
  --global-font-family: "Inter", sans-serif; // body text
  --global-mono-font-family: "JetBrains Mono", monospace; // tagline/code accents
  --global-heading-font-family: var(--global-font-family);
  ```
  and referenced via `font-family: var(--global-font-family);` rather than
  restating the font name in multiple places.
- Keep light-mode and dark-mode variable blocks clearly separated and
  parallel (same variable names, different values per mode) so toggling or
  retheming later doesn't require touching component code at all.
- When asked to change "the theme," "the colors," or "the font," the correct
  move is almost always: update the variable value(s) in these two files and
  nothing else. If a requested change can't be done by editing a variable,
  flag that to the user before writing new CSS rules.

## Guardrails

- Don't introduce new gems/dependencies without flagging it to the user first
  — GitHub Pages' build environment is picky about supported plugins.
- Don't change `baseurl`/`url` in `_config.yml` unless the user is changing
  domains — this breaks all internal links if set wrong.
- Don't commit personal files (drafts, unpublished papers, private notes)
  outside of what's meant to be public — this is a public repo.
- If a request is ambiguous (e.g. "make it look nicer"), pick the smallest
  reasonable interpretation, make the change, and say what you changed and
  why, rather than doing a broad redesign unprompted.
- If the live site doesn't reflect a pushed change after a few minutes, check
  the repo's **Actions** tab for a failed build before assuming the edit was
  wrong.

## First-time setup checklist (only needed once)

1. Use al-folio's "Use this template" button on GitHub, or fork it, into a
   repo named `<username>.github.io`.
2. In repo Settings → Pages, confirm the source is set to GitHub Actions
   (al-folio ships with a workflow that handles this automatically).
3. Edit `_config.yml`: set `name`, `title`, `email`, `description`, `url`,
   and social links.
4. Replace the placeholder profile photo in `assets/img/`.
5. Clear out the example publications/news/projects and replace with real
   content using the recipes above.
6. Push — the site goes live at `https://<username>.github.io` within a
   couple of minutes.

## Ownership transfer note (for this specific project)

This site is being built in one GitHub account and will later move to the
site owner's own account, either via GitHub's repo-transfer feature or by her
using "Use this template" on the finished repo. If asked to prepare for
transfer, make sure:

- No secrets, API keys, or personal credentials are committed anywhere.
- The repo name is (or will be renamed to) `<her-username>.github.io`.
- `_config.yml`'s `url`/`github_username`/social fields point to her accounts,
  not the builder's.
