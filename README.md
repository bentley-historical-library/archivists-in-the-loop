# Archivists in the Loop — Site Documentation

A site powered by the Hugo static site framework. The theme, called **umich-base-accessible**,
was styled with some inspiration from [digitalscholarship.umich.edu](https://digitalscholarship.umich.edu)
and the U-M Library's publishing framework, using the University of Michigan's brand palette as base colors, Noto Sans and Rajdhani typography, and a responsive layout. Assistance with porting the theme, hugo templates,
CSS, and documentation was provided by Claude Code. Underlying structures and accessibility features
may require further attention in future.

---

## Table of Contents

1. [Project Structure](#project-structure)
2. [Getting Started](#getting-started)
3. [Configuration](#configuration)
4. [Creating Content](#creating-content)
5. [Shortcodes](#shortcodes)
6. [Color Palette](#color-palette)

---

## Project Structure

```text
archivists-in-the-loop/
├── hugo.toml                       Site configuration (title, menu, params)
├── content/                        Your pages and posts (Markdown files)
├── static/                         Static assets (images, favicon, etc.)
└── themes/umich-base-accessible/
    ├── theme.toml                  Theme metadata
    ├── assets/css/main.css         All styles; UM colors defined as CSS variables
    └── layouts/
        ├── _default/
        │   ├── baseof.html         Base HTML wrapper (html, head, body)
        │   ├── single.html         Template for individual pages and posts
        │   └── list.html           Template for section index pages
        ├── partials/
        │   ├── head.html           <head> contents: fonts, stylesheet, meta tags
        │   ├── header.html         Sticky site header with logo and navigation
        │   └── footer.html         Site footer with optional logo, menu, and text
        ├── index.html              Homepage template (optional hero + content)
        └── shortcodes/             Reusable content components (see below)
```

---

## Getting Started

### Prerequisites

Install Hugo (extended version recommended):

```bash
brew install hugo          # macOS
```

Verify the install:

```bash
hugo version
```

### Run the local development server

```bash
cd archivists-in-the-loop
hugo server -D
```

Open [http://localhost:1313](http://localhost:1313) in your browser. The site reloads automatically when you save any file. The `-D` flag includes draft pages.

### Build the site for production

```bash
hugo
```

Output is written to the `public/` directory.

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

### Page front matter fields

| Field | Type | Description |
| --- | --- | --- |
| `title` | string | Page heading and browser tab title |
| `date` | date | Publication date (`YYYY-MM-DD`) |
| `draft` | bool | `true` = hidden from production build |
| `description` | string | Meta description for this page (overrides site default) |
| `hideMeta` | bool | `true` = hides the date line below the title |
| `fullWidth` | bool | `true` = content spans full viewport width (see below) |
| `bodyClass` | string | Extra CSS class added to `<body>` for per-page styling |

### Full-width pages

By default, page content is constrained to a readable center column (700px). Setting `fullWidth: true` removes that constraint so the content spans the full viewport width — useful for pages with wide tables, participant grids, or custom layouts.

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

Or use the `figure-block` shortcode for a captioned image (see below).

---

## Shortcodes

Shortcodes are reusable components you can drop into any Markdown page. Call them with `{{</* shortcode-name */>}}` syntax.

---

### `section-heading`

A styled heading with a colored accent bar underneath. Use this to introduce major sections on a page rather than bare `##` Markdown headings when you want the visual accent treatment.

**Parameters:**

| Parameter | Default | Options |
| --- | --- | --- |
| `level` | `2` | `2`, `3`, `4` |
| `color` | `maize` | `maize`, `teal`, `blue`, `red` |

**Usage:**

```hugo
{{</* section-heading level="2" color="maize" */>}}What is Digital Scholarship?{{</* /section-heading */>}}

{{</* section-heading level="3" color="teal" */>}}Our Partners{{</* /section-heading */>}}
```

---

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

---

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

---

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
