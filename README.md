# Website Editing Guide

This site is built with **al-folio**, a Jekyll theme. Almost everything is a
plain text file — you don't need to know how to code to update content.
This guide tells you exactly which file to open for each kind of change.

There's also an `AGENTS.md` file in this repo — that one is written for
Codex (the AI coding assistant), so you can just tell it what you want
changed in plain English and it'll know where to look. This README is the
version for *you*, in case you want to peek at a file yourself or double
check what Codex changed.

---

## ✏️ Editing content

| I want to change...                          | Edit this file                          |
|-----------------------------------------------|-------------------------------------------|
| Name, title, bio text, photo, social links     | `_config.yml` and `_pages/about.md`       |
| Publications list                              | `_bibliography/papers.bib`                |
| CV                                              | `_pages/cv.md` (or `_data/cv.yml` / `assets/pdf/cv.pdf`) |
| News / announcements                           | Add a new file in `_news/`                |
| Projects / portfolio                           | Add a new file in `_projects/`            |
| Blog posts                                     | Add a new file in `_posts/`               |
| Teaching page                                  | `_pages/teaching.md`                      |
| Profile photo                                  | Replace the image in `assets/img/`        |
| Site title, email, social links                | `_config.yml`                             |

**Adding a new item** (a paper, a news post, a project) usually means
copying an existing file in that folder, renaming it, and editing the text
inside — the format (front matter + Markdown) stays the same.

**Adding or removing a navbar tab (About / CV / Publications / etc.)**
- To hide a tab without deleting it: open its file in `_pages/` and set
  `nav: false` in the top section (the part between the `---` lines).
- To remove it completely: delete that file from `_pages/`.
- To add a new tab: copy an existing `_pages/` file, rename it, and set its
  `title`, `permalink`, and `nav_order` (controls left-to-right position) in
  the front matter.

---

## 🎨 Editing style (colors, fonts, backgrounds)

**Everything visual lives in two files, on purpose** — so you only ever
need to change a value in one place, not hunt through the whole site.

| I want to change...                    | Edit this file            | What to look for |
|------------------------------------------|-----------------------------|--------------------|
| Accent color (links, active tab, buttons) | `_sass/_themes.scss`      | `--global-theme-color` |
| Background color                          | `_sass/_themes.scss`      | `--global-bg-color` |
| Main text color                           | `_sass/_themes.scss`      | `--global-text-color` |
| Card/panel background                     | `_sass/_themes.scss`      | `--global-card-bg-color` |
| Body font                                 | `_sass/_variables.scss`   | `--global-font-family` |
| Accent/monospace font (e.g. tagline)      | `_sass/_variables.scss`   | `--global-mono-font-family` |
| Page max width / layout width             | `_config.yml`              | `max_width` |

To change a color, just replace the hex code after the variable name, e.g.:
```scss
--global-theme-color: #9333ea;   // change this hex to change the accent color everywhere
```
You never need to touch `_layouts/` or `_includes/` for a color or font
change — if you (or Codex) find yourselves editing those folders just to
change a color, something's off; the value should be a variable instead.

---

## 🔍 Previewing before you publish

If you have Ruby/Jekyll set up locally:
```
bundle exec jekyll serve -l -H localhost
```
Then open `http://localhost:4000` in your browser to see changes before
pushing them live.

If you don't have Jekyll installed locally, that's fine — just push your
change and check the live site a minute or two later (see below).

---

## 🚀 Publishing a change

1. Save your edits.
2. In the terminal (or via VS Code's Git panel):
   ```
   git add -A
   git commit -m "describe what you changed"
   git push
   ```
3. GitHub Actions rebuilds the site automatically. Check the **Actions** tab
   on GitHub if the live site doesn't update within a couple of minutes —
   that's the first place to look if something looks broken.

---

## 🙅 Things not to touch

Unless you're intentionally changing site structure or behavior (not just
content or style), avoid editing:
- `_layouts/`, `_includes/`, `_plugins/` — these control the theme's
  underlying structure
- `Gemfile` / `Gemfile.lock` — these control installed dependencies
- `.github/workflows/` — this controls how the site auto-deploys

If something in one of those seems like it needs to change, it's worth a
second look before editing — these are the files most likely to break the
whole site if edited incorrectly.
