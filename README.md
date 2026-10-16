# Academic Markdown Typesetter

A browser-based academic typesetting tool. Write in Markdown, add semantic tags for definitions / theorems / examples, and print or export a clean A4 document — with cover page, table of contents, running headers, footers, and page numbers.

Everything runs client-side. No build step, no server, no dependencies to install. Open the HTML file in a browser and start writing.

---

## Table of Contents

- [Features](#features)
- [Quick Start](#quick-start)
- [Syntax Reference](#syntax-reference)
  - [Document Metadata](#document-metadata)
  - [Cover Page Tags](#cover-page-tags)
  - [Running Header & Footer](#running-header--footer)
  - [Content Item Tags](#content-item-tags)
  - [Item Attributes](#item-attributes)
  - [Math](#math)
  - [Standard Markdown](#standard-markdown)
- [Numbering Rules](#numbering-rules)
- [Pagination & Page Numbers](#pagination--page-numbers)
- [Settings Panel](#settings-panel)
- [Export & Printing](#export--printing)
- [Customization](#customization)
- [Notes & Limitations](#notes--limitations)
- [License](#license)

---

## Features

- **Semantic tagging** — definitions, theorems, corollaries, lemmas, examples, and tips with automatic hierarchical numbering.
- **Automatic cover page** — generated from `<zTitle>`, `<zAuthor>`, and `<zNote>`.
- **Automatic table of contents** — one or more dedicated pages, with dot leaders and page numbers.
- **Running headers & footers** — driven by `<zHeader>` and `<zFooter>`.
- **Roman numerals for front matter, Arabic numerals for the body** — page numbering switches automatically at the body.
- **Boxed callouts** — examples and tips are rendered inside light-grey boxes to set them apart from body text.
- **KaTeX math** — inline `$...$` and display `$$...$$` math, rendered with custom macro shortcuts.
- **Print-ready A4 layout** — page dimensions, margins, and pagination are computed in the browser, so `Ctrl/Cmd + P` produces a correctly paginated PDF.
- **Standalone HTML export** — export a self-contained document you can share or archive.

---

## Quick Start

1. Download `academic-typesetter.html` (or whatever you name the file).
2. Open it in a modern browser (Chrome, Edge, Firefox, Safari).
3. Write Markdown in the left pane. The right pane previews the paginated document.
4. Click **Print / Save PDF** to export a PDF, or **Export HTML** to save a standalone file.

An example document loads on startup so you can see every feature in action.

---

## Syntax Reference

### Document Metadata

These tags must appear **at the top of the file**, before any body content. Each one may appear at most once.

| Tag | Purpose |
| --- | --- |
| `<zTitle>...</zTitle>` | Document title (used on the cover page and as the browser/export filename). |
| `<zAuthor>...</zAuthor>` | Author name(s) — free-form. |
| `<zNote>...</zNote>` | Any additional note shown on the cover page (institution, term, version, etc.). |

The cover page is generated only if at least one of these three tags is present. If the **Cover Page** option is disabled in the settings panel, the tags are still consumed but no cover is produced.

Inline Markdown is supported inside these tags — e.g. `**bold**`, `*italic*`, and math like `$G$`.

```markdown
<zTitle>Lecture Notes on Abstract Algebra</zTitle>
<zAuthor>Jane Doe</zAuthor>
<zNote>Department of Mathematics · Spring 2025</zNote>
```

### Running Header & Footer

| Tag | Purpose |
| --- | --- |
| `<zHeader>...</zHeader>` | Text shown in the running header of every TOC and body page. |
| `<zFooter>...</zFooter>` | Text shown in the running footer of every TOC and body page. |

- The cover page **never** receives a header or footer.
- If a header is present, it is centered and underlined by a thin rule.
- The footer text is left-aligned; the page number is centered on the same line.
- Inline Markdown is supported inside these tags.

```markdown
<zHeader>Lecture Notes on Abstract Algebra</zHeader>
<zFooter>Department of Mathematics</zFooter>
```

### Content Item Tags

Six item types are supported. All of them share the same structure: an auto-numbered bold label, followed by the item body. Items participate in the same numbering sequence as headings.

| Tag | Rendered label |
| --- | --- |
| `<zDefinition>...</zDefinition>` | 定义 (Definition) |
| `<zTheorem>...</zTheorem>` | 定理 (Theorem) |
| `<zCorollary>...</zCorollary>` | 推论 (Corollary) |
| `<zLemma>...</zLemma>` | 引理 (Lemma) |
| `<zExample>...</zExample>` | 例 (Example) |
| `<zTip>...</zTip>` | 点拨 (Tip) |

**Examples and tips are boxed.** They render inside a light-grey panel with a thin border, visually separated from body text. Definitions, theorems, corollaries, and lemmas are rendered inline with the rest of the content.

Each item body is full Markdown, including multi-paragraph content, lists, and display math.

```markdown
<zDefinition>A group is a pair $(G, \cdot)$ where $G$ is a non-empty set
and $\cdot$ is an associative binary operation with an identity and inverses.</zDefinition>

<zTheorem Description="Lagrange">Let $G$ be a finite group and $H \le G$.
Then $|H|$ divides $|G|$.</zTheorem>

<zExample>In $(\mathbb{Z}, +)$, every subgroup is of the form $n\mathbb{Z}$.</zExample>

<zTip>When checking whether $H \le G$, first verify that the identity lies in $H$.</zTip>
```

### Item Attributes

Items accept an optional `Description` attribute. When present, the value is appended to the auto-generated number in parentheses.

```markdown
<zTheorem Description="Lagrange">...</zTheorem>
```

renders as:

> **Theorem 1.2.1 (Lagrange)** ...

The attribute is case-insensitive on the name (`Description`, `description`, `DESCRIPTION` all work), and both single and double quotes are accepted.

### Math

Math is rendered with KaTeX.

| Delimiter | Mode |
| --- | --- |
| `$...$` | Inline math |
| `$$...$$` | Display math |
| `\(...\)` | Inline math |
| `\[...\]` | Display math |

The following macros are predefined:

| Macro | Expands to |
| --- | --- |
| `\RR` | `\mathbb{R}` |
| `\NN` | `\mathbb{N}` |
| `\ZZ` | `\mathbb{Z}` |
| `\QQ` | `\mathbb{Q}` |
| `\CC` | `\mathbb{C}` |
| `\FF` | `\mathbb{F}` |
| `\dd` | `\mathrm{d}` |
| `\ee` | `\mathrm{e}` |

```markdown
The group $(\ZZ, +)$ is cyclic.

$$
\int_0^1 x^2 \dd x = \frac{1}{3}
$$
```

### Standard Markdown

Standard GitHub-Flavored Markdown is supported via `marked`:

- Headings `#` through `######`
- Bold, italic, strikethrough, inline code
- Ordered and unordered lists (including nested lists)
- Blockquotes
- Fenced code blocks
- Tables
- Links and images
- Horizontal rules

**Raw HTML blocks containing Markdown are supported.** If you wrap Markdown inside a `<div>`, `<section>`, `<blockquote>`, or similar HTML block, the Markdown inside is parsed. This lets you attach custom inline styles to a section while keeping the content readable.

```markdown
<div style="border-left: 3px solid #8b95a3; padding-left: 1.1em;">
**Note.** The Markdown inside this block is still parsed —
inline math like $\pi$ and *emphasis* work as expected.
</div>
```

---

## Numbering Rules

Numbering is **hierarchical and driven by the document structure**.

- Each heading level (`h1`–`h6`) occupies its own level in the counter.
- Every item (`zDefinition`, `zTheorem`, `zExample`, …) occupies **the level immediately below the current heading**, and increments its own counter there.
- Unused levels are omitted — you will never see a trailing `.0`.
- Encountering a new heading resets the item counter at that level.

Example:

```markdown
# Part One                 →  1
## Chapter A               →  1.1
<zDefinition>...</zDefinition>   →  Definition 1.1.1
<zDefinition>...</zDefinition>   →  Definition 1.1.2
<zTheorem Description="X">...</zTheorem>  →  Theorem 1.1.3 (X)
## Chapter B               →  1.2
<zLemma>...</zLemma>             →  Lemma 1.2.1
```

Items inherit the counter path of the enclosing heading, so different item types share one sequence within the same section.

---

## Pagination & Page Numbers

- The document is laid out on A4 pages (210 mm × 297 mm) with 20 mm margins.
- Content flows into pages in the browser. Page breaks happen **between block elements**, so a paragraph, list, table, or item is never split across pages unless it is taller than one page.
- **Page numbers** are optional (see the settings panel). When enabled:
  - TOC pages use **lowercase Roman numerals** (`i`, `ii`, `iii`, …).
  - Body pages use **Arabic numerals**, restarting at `1`.
  - The cover page has no page number.

---

## Settings Panel

Open the panel with the **⚙ Settings** button in the toolbar.

### Layout

| Option | Range | Default |
| --- | --- | --- |
| Font size | 12–18 px | 15 px |
| Line height | 1.2–2.0 | 1.55 |
| Typeface | Serif / Sans | Serif |

The serif stack is `Latin Modern Roman → Times New Roman → Songti SC → SimSun → STSong`. The sans stack is `Helvetica Neue → Helvetica → PingFang SC → Microsoft YaHei → Arial`.

### Auto-generated Elements

| Option | Default | Effect |
| --- | --- | --- |
| Cover page | On | Generate a cover when `<zTitle>` / `<zAuthor>` / `<zNote>` are present. |
| Table of contents | On | Generate one or more TOC pages between the cover and the body. |
| TOC includes items | Off | Include definitions, theorems, etc. in the TOC. |
| Header & footer | On | Render `<zHeader>` / `<zFooter>` on TOC and body pages. |
| Page numbers | On | Roman numerals for the TOC, Arabic numerals for the body. |

All options are live — toggling any checkbox re-renders the document immediately.

---

## Export & Printing

### Print / Save PDF

Click **Print / Save PDF** (or press `Ctrl/Cmd + P`). The print stylesheet:

- Hides the toolbar, editor pane, and settings panel.
- Forces each page to break cleanly (`break-after: page`).
- Applies the document's own margins, headers, footers, and page numbers.
- Preserves the light-grey backgrounds on example and tip boxes.

Choose **Save as PDF** in the print dialog. The result is a paginated PDF identical to the on-screen preview.

### Export HTML

Click **Export HTML** to download a standalone HTML file. The exported file:

- Contains all rendered markup and inline styles.
- Loads KaTeX from the same CDN, so math renders in any browser with network access.
- Includes a print stylesheet, so it can be printed to PDF from any machine.
- Uses the document title (from `<zTitle>` or the first `<h1>`) as the filename.

---

## Customization

The tool is a single HTML file. To extend it:

- **Add a new item type.** Edit the `Z_ITEM_TYPES` map near the top of the script:

  ```js
  const Z_ITEM_TYPES = {
    zDefinition: 'Definition',
    zTheorem:    'Theorem',
    // ...
    zRemark:     'Remark'   // ← add your own
  };
  ```

  The parser, numbering, and TOC pickup are driven by this map, so the new tag is available immediately.

- **Add a metadata tag.** Extend `Z_META_TAGS` in the same way:

  ```js
  const Z_META_TAGS = {
    zTitle:  'title',
    zAuthor: 'author',
    zNote:   'note',
    zHeader: 'header',
    zFooter: 'footer',
    zDate:   'date'   // ← new tag
  };
  ```

  You will also need to render it on the cover — see `buildCover()`.

- **Add a KaTeX macro.** Extend the `macros` object inside `renderMath()`.

- **Change page geometry.** Adjust the `@page` rule and the `.page` padding in the CSS. The pagination routine reads the page height from the DOM, so it adapts automatically.

- **Restyle item boxes.** The example and tip panels are targeted by:

  ```css
  .z-item[data-ztype="zExample"],
  .z-item[data-ztype="zTip"] { /* ... */ }
  ```

---

## Notes & Limitations

- **Requires an internet connection on first load**, because KaTeX and `marked` are loaded from jsDelivr. To work fully offline, vendor the three libraries locally and update the `<script>` and `<link>` tags.
- **Pagination is computed client-side** and may occasionally leave a small amount of whitespace at the bottom of a page when a large block would have crossed the boundary. This is intentional — it prevents blocks from being split.
- **A block taller than one page** (for example, a very long table) is allowed to overflow its page rather than being cut. The page grows vertically and the print stylesheet paginates it naturally.
- **Page numbers in the TOC** are computed against the final page layout, so they reflect the true position of each heading after pagination.
- **Only one of each metadata tag is honored.** If `<zTitle>` appears twice, the first is used and the second is discarded.
- **Unclosed item tags** are tolerated: the parser closes them implicitly at end of input.

---

## License

MIT. Use it, fork it, adapt it to your own writing workflow.
