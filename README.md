# Archivists in the Loop — Site Documentation

## The Project

This repository is the website for **Archivists in the Loop**, an in-person symposium held in
June 2026, sponsored by the Bentley Historical Library and the University of Michigan School of Information. The site was originally built to support planning of that event, and it contains
information about the event itself, its schedule, participants, and planning materials. It is now
also being adapted to host the **project report** that documents the event and its outcomes.
Similar follow-on initiatives may be created in future and posted to this same site.

---

## Theme

This repo publishes a site powered by the Hugo static site framework. The theme, called **umich-base-accessible**,
was styled with some inspiration from [digitalscholarship.umich.edu](https://digitalscholarship.umich.edu)
and the U-M Library's publishing framework, using the University of Michigan's brand palette as base colors, Noto Sans and Rajdhani typography, and a responsive layout. Assistance with developing and adapting the theme, hugo templates,
CSS, and documentation was provided by Claude Code. Underlying structures and accessibility features
may require further attention in future.

---

## Table of Contents

1. [Project Structure](#project-structure)
2. [Getting Started](#getting-started)
3. [Configuration](#configuration)
4. [Creating Content](#creating-content)
5. [Reports](#reports)
6. [Shortcodes](#shortcodes)
7. [Color Palette](#color-palette)
8. [Homepage Image Carousel](#homepage-image-carousel)

---

## Project Structure

```text
archivists-in-the-loop/
├── hugo.toml                       Site configuration (title, menu, params)
├── archetypes/                     Templates for new content (updates.md, reports.md)
├── content/                        Your pages and posts (Markdown files)
│   └── reports/                    Reports section: _index.md plus one Markdown file per report
├── data/                           Files for data to power dynamic pages (JSON files)
├── assets/                         Files Hugo processes via resources.Get
│   └── images/                     Report cover images (coverImage field)
├── static/                         Files copied as-is (images, favicon, etc.)
│   └── documents/                  Report PDFs
└── themes/umich-base-accessible/
    ├── theme.toml                  Theme metadata
    ├── assets/css/main.css         All styles; UM colors defined as CSS variables
    └── layouts/
        ├── _partials/          Reusable content components (see below)
        |   ├── head.html           <head> contents: fonts, stylesheet, meta tags
        |   ├── opengraph.html      Link-preview (Open Graph) meta tags
        |   ├── report-license.html  Converts a license like CC-BY-NC-SA-4.0 to display text and a Creative Commons URL
        |   ├── report-author-names.html  Report authors/editors as names, linked to ORCID when provided
        |   ├── report-people-jsonld.html  Report authors/editors as schema.org Person entries
        |   ├── report-publisher-names.html  Report publishers as names, linked to their url when provided
        |   ├── report-publishers-jsonld.html  Report publishers as schema.org Organization entries
        |   ├── header.html         Sticky site header with logo and navigation
        |   └── footer.html         Site footer with optional logo, menu, and text
        ├── _shortcodes/        Reusable content components
        ├── reports/            Templates for the reports section
        |   ├── list.html           Reports index (date, authors, summary)
        |   └── single.html         Report page (metadata, PDF button, schema.org JSON-LD)
        ├── baseof.html         Base HTML wrapper (html, head, body)
        ├── home.html           Homepage template (optional hero + content)
        ├── single.html         Template for individual pages and posts
        ├── list.html           Template for section index pages
        └── participants.html   Template for participant list page
```

---

## Getting Started

To develop or run this site locally, it is recommended to install [Hugo](https://gohugo.io/), a static site generator framework.
See Hugo's [Quick Start guide](https://gohugo.io/getting-started/quick-start/).

### Run the local development server

To run the site locally, download the GitHub repository, unzip, and then run the `hugo server` command.
Open [http://localhost:1313](http://localhost:1313) in your browser.
The site reloads automatically when you save any file. The `-D` flag includes draft pages.

---

## Configuration

All site-wide settings live in `hugo.toml`. Key fields to update before deploying:

| Field | What it does |
| --- | --- |
| `baseURL` | Full URL of your published site (e.g. `https://username.github.io/repo/`) |
| `title` | Site name shown in the browser tab and wherever `{{ .Site.Title }}` is used |
| `[params] description` | Fallback meta description for SEO |
| `[params] logo` | Path to header logo image (place the file in `static/`) |
| `[params] footerLogo` | Path to a separate footer logo (can be a lighter version) |
| `[params] footerText` | Text or HTML shown in the footer above the copyright line |
| `[params] showRecentPosts` | Set to `true` to show a recent posts block on the homepage |

### Navigation menu

Edit the `[[menu.main]]` entries in `hugo.toml` to control the header navigation:

```toml
[[menu.main]]
  name   = "About"
  url    = "/about/"
  weight = 10        # lower weight = further left
```

An optional `[[menu.footer]]` section controls links in the footer.

### Homepage hero banner

Uncomment and fill in the `[params.hero]` block in `hugo.toml`:

```toml
[params.hero]
  heading    = "Welcome to Archivists in the Loop"
  subheading = "A short description of the project."
  [params.hero.cta]
    label = "Learn more"
    url   = "/about/"
```

---

## Creating Content

### Archetypes

Archetypes are the front matter templates in `archetypes/` that `hugo new content/...` fills in. Hugo picks one by the section the new file is in:

| Archetype | Used for | Command |
| --- | --- | --- |
| `reports.md` | Reports in `content/reports/` (full metadata fields; see [Creating Reports](#creating-reports)) | `hugo new content/reports/my-report.md` |
| `updates.md` | Posts in `content/updates/` (date, title, `layout: post`, author) | `hugo new content/updates/my-update.md` |
| *(none)* | Any other page or section; Hugo's built-in default (title, date, `draft: true`) is used | `hugo new content/about.md` |

### Using archetypes

1. Run `hugo new content/<section>/<file-name>.md`. The file name becomes the title (hyphens turn into spaces).
2. Open the new file and fill in the front matter. The report archetype has comments explaining each field.
3. Delete any optional fields you don't need, write the page body, and set `draft: false` to publish.

### Create a new page

```bash
hugo new content/about.md
```

This creates `content/about.md` with a pre-filled front matter header. Edit the file to add your content:

```markdown
---
title: "About"
date: 2024-01-01
draft: false
hideMeta: true          # hides the date line on non-post pages
---

Your page content goes here. Standard Markdown is supported.
```

Set `draft: false` when the page is ready to publish. While developing, `hugo server -D` shows draft pages too.

#### Page front matter fields

| Field | Type | Description |
| --- | --- | --- |
| `title` | string | Page heading and browser tab title |
| `date` | date | Publication date (`YYYY-MM-DD`) |
| `draft` | bool | `true` = hidden from production build |
| `description` | string | Meta description for this page (overrides site default) |
| `hideMeta` | bool | `true` = hides the date line below the title |
| `fullWidth` | bool | `true` = content spans full viewport width (see below) |
| `bodyClass` | string | Extra CSS class added to `<body>` for per-page styling |

#### Full-width pages

By default, page content is constrained to a narrower, readable column (700px) that is aligned the left. Setting `fullWidth: true` removes that constraint so the content spans the full viewport width — useful for pages with wide tables, participant grids, or custom layouts.

```markdown
---
title: "Participants"
fullWidth: true
---
```

With `fullWidth: true`:

- The content area drops its side padding and extends to the viewport edges
- The `banner-heading` shortcode still works correctly with full-bleed color bands
- Regular text and headings retain their own side padding so they never run to the browser edge
- Use the `banner-heading` and `flex-row` shortcodes to take advantage of the extra width

### Create a section (e.g. projects, events)

Create a directory inside `content/` and add an `_index.md` file for the section landing page:

```bash
mkdir content/projects
hugo new content/projects/_index.md
```

Individual items in the section are regular Markdown files:

```bash
hugo new content/projects/my-project.md
```

### Add an image to a page

Place the image in `static/images/` and reference it in Markdown:

```markdown
![Alt text](/images/my-photo.jpg)
```

Or use the `figure` shortcode for a captioned image (see below). Avoid bare Markdown image syntax (`![alt](path)`) for images in `static/` — it does not apply `relURL` and will break on GitHub Pages.

## Creating Reports

Reports (white papers, project reports) live in `content/reports/`, listed at `/reports/` and linked from the main menu. The section is meant to hold the symposium report and any later reports from follow-on initiatives.

### Add a new report

1. Put the PDF in `static/documents/` (e.g. `static/documents/my-report.pdf`).
2. If the report has a cover image, put it in **`assets/images/`** (e.g. `assets/images/my-report-cover.png`). It must be there, not in `static/images/`; the build fails if the file named in `coverImage` is not found in `assets/images/`.
3. Create the page from the archetype:

   ```bash
   hugo new content/reports/my-report.md
   ```

4. Fill in the front matter (below) and optionally add a longer description, key findings, or acknowledgments in the page body.
5. Set `draft: false` to publish. While `draft: true`, the report only appears with `hugo server -D`.

#### Report front matter fields

| Field | Description |
| --- | --- |
| `title` | Short report title; used for the page heading, the Reports list, and the browser tab (and the JSON-LD `name` if there is no `report_fulltitle`) |
| `report_fulltitle` | Optional full formal title, when `title` is a shorter page title. Shown as "Full title" in the report details and under the title in the Reports list; used as the JSON-LD `name` and `headline` (the short `title` becomes `alternateName`), and as the link-preview title |
| `description` | Short summary; shown on the report page, the Reports list, link previews, and the homepage callout |
| `date` | Issue date |
| `authors` | Optional (see `editors`). List of authors, one entry per author, each with a `name` and an optional `orcid`:<br>`- name: 'Jane Doe'`<br>`  orcid: '0000-0002-1825-0097'`<br>Enter the ORCID iD as **numbers only** (with its hyphens); do not include `https://orcid.org/`. The URL is added automatically. When an `orcid` is given, the author's name links to their ORCID profile and the ORCID is output in the JSON-LD as the person's `identifier` (a `PropertyValue` with the URL) and `sameAs`. Leave `orcid` empty or omit it for no link. Plain-string entries (just a name) still work |
| `editors` | Optional. Same format as `authors` (`name` plus optional numbers-only `orcid`, linked the same way). Shown on the report page as "Editor" or "Editors" (by count) after the authors, output as `editor` in the JSON-LD, and shown in the Reports list as "(eds.)" when a report has no authors. Use `authors`, `editors`, or both |
| `publisher` | List of publishing organizations, one entry per publisher, each with a `name`, an optional `url`, and an optional `type`:<br>`- name: 'Bentley Historical Library, University of Michigan'`<br>`  url: 'https://bentley.umich.edu/'`<br>`  type: 'Library'`<br>`type` is a schema.org subtype of [`Organization`](https://schema.org/Organization) used as the JSON-LD `@type` (e.g. `Library`, `EducationalOrganization`, `CollegeOrUniversity`); it defaults to `Organization` and does not affect the page display. Use a separate entry for each distinct publisher rather than combining them in one string. Shown on the report page as "Publisher" or "Publishers" (by count), with the name linked when a `url` is given, and output as `publisher` in the JSON-LD (one entry per publisher, typed by `type` and including `url` when given). Plain-string entries (just a name) work too |
| `doi` | DOI such as `10.xxxx/xxxxx` (the `https://doi.org/` prefix is added automatically) |
| `license` | Creative Commons license written as `CC-<terms>-<version>` with hyphens and no spaces, e.g. `CC-BY-NC-SA-4.0` (also `CC-BY-4.0`, `CC-BY-SA-4.0`, `CC-BY-NC-4.0`, `CC-BY-ND-4.0`, `CC-BY-NC-ND-4.0`, and `CC0-1.0`). It is displayed as `CC BY NC SA 4.0`, linked to the matching creativecommons.org page (here `https://creativecommons.org/licenses/by-nc-sa/4.0/`, built automatically), and the URL is output as the JSON-LD `license`. Any other text is shown as-is, with no link |
| `reportPDF` | PDF filename only, e.g. `my-report.pdf`, not a path; the file must be in `static/documents/`. Adds a download button, a JSON-LD PDF link, and the PDF link in the `report-callout` |
| `coverImage` | Optional cover graphic (jpg/png). Give the **filename only**, e.g. `my-report-cover.png`, not a path; the file must be in `assets/images/` (not `static/images/`) or the build fails. Shown beside the summary, as a thumbnail in the Reports list, in the JSON-LD, and as the link-preview image |
| `coverAlt` | Alt text for the cover; defaults to "Cover of <title>" |
| `pdfSize` | File size shown on the download button, e.g. `2.4 MB` |
| `citation` | Suggested citation text |

Empty fields are simply not displayed.

#### Descriptive metadata for reports (microdata)

Each report page includes [schema.org `Report`](https://schema.org/Report) metadata as a JSON-LD block, generated from the front matter above (name from `report_fulltitle` or `title`, headline from `report_fulltitle`, alternate name from `title`, abstract from `description`, date, authors and editors with ORCID identifiers, one typed `Organization` entry per publisher, DOI (as a `PropertyValue` identifier plus `sameAs`), license (as the Creative Commons URL), cover image, PDF link). No extra work is needed when adding a report.

#### Homepage button and callout

- The homepage "Read the report" button is set in `hugo.toml` under `[params.hero.cta]`. Update its `url` when pointing to a different report.
- The [`report-callout`](#report-callout) shortcode on `content/_index.md` pulls its summary from a report's `description`.

---

## Shortcodes

Shortcodes are reusable components you can drop into any Markdown page. Call them with `{{</* shortcode-name */>}}` syntax.

### `section-heading`

A styled heading with a colored accent bar underneath. Use this to introduce major sections on a page rather than bare `##` Markdown headings when you want the visual accent treatment.

**Parameters:**

| Parameter | Default | Options |
| --- | --- | --- |
| `level` | `2` | `2`, `3`, `4` |
| `color` | `maize` | `maize`, `teal`, `blue`, `red` |
| `id` | generated from the heading text | Any anchor id, e.g. `low-cost`; set it to keep links stable if the heading is reworded |
| `anchor` | `true` | `false` hides the `#` link icon (shown on hover/focus). The heading `id` is added either way |

**Usage:**

```hugo
{{</* section-heading level="2" color="maize" */>}}What is Digital Scholarship?{{</* /section-heading */>}}

{{</* section-heading level="3" color="teal" */>}}Our Partners{{</* /section-heading */>}}
```

### `callout`

A highlighted aside block for notes, tips, warnings, or important information. Renders with a colored left border and a light tinted background.

**Parameters:**

| Parameter | Default | Options |
| --- | --- | --- |
| `color` | `maize` | `maize`, `teal`, `blue`, `red` |
| `title` | *(none)* | Any text — displayed as a bold heading inside the callout |

**Usage:**

```hugo
{{</* callout color="teal" title="Note" */>}}
This workshop requires registration. Seats are limited.
{{</* /callout */>}}

{{</* callout color="red" title="Deadline" */>}}
Applications close on March 31.
{{</* /callout */>}}
```

### `figure`

A replacement for Hugo's built-in `figure` shortcode that correctly applies `relURL` to the image path. Use this whenever embedding images from `static/` in content pages — bare Markdown image syntax does not resolve subpaths correctly on GitHub Pages.

**Parameters:**

| Parameter | Required | Description |
| --- | --- | --- |
| `src` | yes | Path to the image (e.g. `/images/photo.jpg`) — leading slash required |
| `alt` | yes | Alt text for screen readers and SEO |
| `title` | no | Tooltip text on the image element |
| `caption` | no | Caption text displayed below the image |

**Usage:**

```hugo
{{</* figure src="/images/workshop.jpg" alt="Participants at a digital scholarship workshop" caption="Spring 2023 workshop." */>}}
```

### `figure-block`

An image with an optional caption. Prefer this over bare Markdown images when you need a caption or want consistent styling.

**Parameters:**

| Parameter | Required | Description |
| --- | --- | --- |
| `src` | yes | Path to the image (e.g. `/images/photo.jpg`) |
| `alt` | yes | Alt text for screen readers and SEO |
| `caption` | no | Caption text displayed below the image |

**Usage:**

```hugo
{{</* figure-block src="/images/workshop.jpg" alt="Participants at a digital scholarship workshop" caption="Spring 2023 workshop at the Shapiro Library." */>}}
```

### `flex-row`

Places content side by side in a responsive two-column (or more) layout. Columns stack vertically on small screens. Separate columns with a line containing only `|||`.

**Usage:**

```hugo
{{</* flex-row */>}}
**Left column heading**

Some text for the left side.

|||

**Right column heading**

Some text for the right side.
{{</* /flex-row */>}}
```

You can use full Markdown inside each column, including lists, links, and other shortcodes.

### `report-callout`

A callout that pulls the title, summary (`description`), report page link, and PDF link from a report's front matter, so the text is maintained in one place. If no report is found (for example, it is still a draft), it shows a "coming soon" message.

**Parameters:**

| Parameter | Default | Options |
| --- | --- | --- |
| `path` | newest report | Content path of a report, e.g. `reports/my-report` |
| `color` | `teal` | `maize`, `teal`, `blue`, `red` |

**Usage:**

```hugo
{{</* report-callout */>}}

{{</* report-callout path="reports/my-report" color="blue" */>}}
```

---

## Color Palette

All UM brand colors are defined as CSS custom properties in `themes/umich-base-accessible/assets/css/main.css` and can be used in any custom CSS you add.

| Name | Variable | Hex |
| --- | --- | --- |
| Michigan Blue | `--color-um-blue` | `#00274C` |
| Maize | `--color-um-maize` | `#FFCB05` |
| Taubman Teal | `--color-taubman-teal` | `#00B2A9` |
| Arboretum Blue | `--color-arboretum-blue` | `#2F65A7` |
| Tappan Red | `--color-tappan-red` | `#9A3324` |
| Ross Orange | `--color-ross-orange` | `#D86018` |
| Rackham Green | `--color-rackham-green` | `#75988D` |
| A2 Amethyst | `--color-a2-amethyst` | `#702082` |
| Matthaei Violet | `--color-matthaei-violet` | `#575294` |
| Peony Pink | `--color-peony-pink` | `#E01F7C` |
| Puma Black | `--color-puma-black` | `#131516` |

The shortcode `color` parameter values (`maize`, `teal`, `blue`, `red`) map to Maize, Taubman Teal, Michigan Blue, and Tappan Red respectively.

---

## Homepage Image Carousel

The homepage supports an optional image carousel powered by [Splide.js](https://splidejs.com/), a lightweight and accessible slider library. The carousel appears between the hero banner and the main page content, and is only rendered when at least one slide is defined in `hugo.toml`.

Splide.js includes full keyboard navigation, ARIA roles and labels, and autoplay controls that pause on hover and focus — meeting common accessibility requirements.

### Adding slides

Place image files in `static/images/`, then add one `[[params.carousel.slides]]` entry per image in `hugo.toml`:

```toml
[[params.carousel.slides]]
  image   = "images/symposium-photo.jpg"
  alt     = "Archivists gathered for a workshop discussion"
  caption = "Optional caption displayed below the slide"

[[params.carousel.slides]]
  image   = "images/bentley-reading-room.jpg"
  alt     = "The Bentley Historical Library reading room"
```

Slides appear in the order they are listed. The `caption` field is optional; omit it to show the image without any text below.

### Carousel settings

The `[params.carousel]` block in `hugo.toml` controls autoplay behavior:

| Field | Default | Description |
| --- | --- | --- |
| `autoplay` | `true` | Set to `false` to disable automatic slide advancement |
| `autoplayInterval` | `5000` | Milliseconds between slides when autoplay is on |

```toml
[params.carousel]
  autoplay         = true
  autoplayInterval = 5000
```

Splide's CSS and JS are loaded from a CDN only on the homepage and only when slides are defined, so there is no performance impact on other pages.

### Changing header and footer colors

The header and footer background and text colors are controlled by semantic alias variables near the top of `themes/umich-base-accessible/assets/css/main.css`, in the `:root` block under `/* Semantic aliases */`. Change the value on the right side to any UM brand variable (or a raw hex value) from the palette table above.

```css
/* Header */
--color-header-bg:   var(--color-um-blue);      /* background */
--color-header-text: #FFFFFF;                   /* text and nav links */

/* Footer */
--color-footer-bg:   var(--color-taubman-teal); /* background */
--color-footer-text: #FFFFFF;                   /* text and links */
```

For example, to set the footer to Michigan Blue with Maize text:

```css
--color-footer-bg:   var(--color-um-blue);
--color-footer-text: var(--color-um-maize);
```

No other changes are needed — all header and footer styles reference these variables.

### Notes for Coding Assistants (How to Work in this Project)

When working in this repository:

- Treat existing event and planning content, dated from early in 2026 until June 2026, as a record of a completed event. Do not rewrite, remove, or change the tense of it unless asked.
- Expect new content (report sections, follow-on project pages) to be added alongside the existing content; if new content suggests a need for a new page archetype or structure, this may be suggested, but for the most part maintain structure, navigation, and URLs so the site remains usable, flexible, and do not break existing links.
- Follow the conventions documented below (Hugo, the `umich-base-accessible` theme, shortcodes,
  front matter, and accessibility practices) rather than introducing new patterns.
- Do not invent factual details about the event, participants, or findings. Note the issue and ask the maintainers if information is missing.
- Do not build the site (`hugo`) or temporarily edit content files to test changes. The maintainer runs `hugo server` and checks the display themselves. Make the requested edits and describe them; only build or test when explicitly asked.
- Git workflow: make changes on a new feature branch, never directly on `main` (pushes to `main` deploy the site). Commit step by step as work progresses, with a clear message for each logical change. Stage only the files you changed (check `git status` first) so the maintainer's unrelated or already-staged edits are not swept into your commits. Do not push or merge unless asked.
- Report metadata (authors, editors, issue date, DOI, license, publisher, summary, citation, PDF details) comes from the maintainers. Never invent or guess it. If it is unknown, leave a clearly marked `PLACEHOLDER` or empty field and prompt the maintainer for the information.
- Make only the edits requested. When asked to change tense or make another specific writing modification, do only that: do not rewrite, reword, fix, or change anything else unless specifically asked. If you notice other issues (typos, outdated statements), mention them rather than changing them.
- CSS and layout alignment: when changing layout CSS, check that left edges still line up with the header's `.site-header__inner` pattern: no padding on the outer container, `max-width: var(--max-width-wide)` centered, then `padding-left: var(--site-padding)` inside. Default content width is `--max-width-default` (700px) and wide is `--max-width-wide` (1240px); `fullWidth: true` pages follow the same pattern so content aligns with the site name and nav at all viewport widths.
- The maintainer edits files directly in their editor too. If a file has changed on disk since it was last read by an agent/coding assistant, treat that as deliberate. Never revert, redo, or "fix" those changes without an explicit request and confirmation from the maintainer. If an agent must comment, or notes that the changes have caused a problem elsewhere or have a typoe, note that. Otherwise state that changes appear to be fine. Never make any change without the maintainer's permission.
- Be succinct, don't waste the maintainer's time with reams of repetitive or vacuous text. Do not restate or summarize every change, only significant and notable ones, which may affect site behavior and change usage. Note where the maintainer should look to get details (in readme doc, new files, or elsewhere). Be concise!
