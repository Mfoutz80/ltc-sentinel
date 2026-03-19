---
name: ltc-article
description: >
  Create a new blog article for the LTC Sentinel website and update the homepage to feature it.
  Use this skill whenever the user wants to add a new blog post, article, news story, or report
  to the LTC Sentinel site. Also use it when the user says things like "write a post about...",
  "add a new article on...", "publish a story about...", "write up a piece on...", or mentions
  adding content to ltcsentinel.com. Even if they just say "write about [some LTC topic]" in the
  context of this project, use this skill.
---

# LTC Sentinel Article Creator

You are writing articles for **LTC Sentinel**, a data journalism site covering the long-term care industry. The site lives at `D:/Code/ltc_health_data/site/`.

## What You're Building

Each article is a standalone HTML page in `site/articles/` that uses the site's design system (Playfair Display headlines, Inter body text, JetBrains Mono for figures, teal brand color). After creating the article, you update `site/data/content.json` so the homepage automatically shows the new content.

## Step-by-Step Process

### 1. Gather the Article Details

Ask the user (if not already provided):
- **Topic/title** — What's the story about?
- **Category** — One of: Ownership, Staffing, Quality, Chains, Decline Watch, Fines, Ratings, M&A, Investigation, Policy, Special Report
- **Key data points** — Numbers, facility names, states, trends to highlight
- **Whether to promote it** — Should it become the lead story on the homepage, or just go into the stories grid and latest feed?

### 2. Create the Article HTML

Read `references/article-template.html` for the exact HTML structure to follow. This is critically important — the article must match the existing site's design precisely.

Generate the article file at: `site/articles/article-<slug>.html`

The slug should be a short kebab-case identifier derived from the topic (e.g., `article-life-care-sale.html`).

**Content components you can use in the article body** (inside the `<div class="prose">` block):

- **Paragraphs**: `<p>Body text here with <strong>bold for emphasis</strong>.</p>`
- **Headings**: `<h2>Section Title</h2>` and `<h3>Subsection</h3>`
- **Stat cards** (grid of key numbers):
  ```html
  <div class="stat-row" style="grid-template-columns: repeat(2,1fr)">
      <div class="stat-item"><div class="stat-num text-signal-red">15</div><div class="stat-desc">Residents</div></div>
      <div class="stat-item"><div class="stat-num text-brand-500">79%</div><div class="stat-desc">Occupancy</div></div>
  </div>
  ```
  Colors: `text-brand-500` (teal), `text-signal-red`, `text-signal-green`, `text-signal-amber`, `text-ink-900` (black)

- **Data tables** (always wrapped for mobile scroll):
  ```html
  <div class="table-wrap"><table class="data-table">
      <thead><tr><th>Name</th><th style="text-align:right">Value</th></tr></thead>
      <tbody>
          <tr><td><strong>Row Label</strong></td><td class="fig" style="text-align:right">123</td></tr>
      </tbody>
  </table></div>
  ```
  Use `class="fig"` on numeric cells. Use `<span class="tag tag-green">3.5</span>` or `tag-red` for colored badges.

- **Callout boxes**:
  ```html
  <div class="callout bg-brand-50 border-brand-500">
      <p><strong>Key insight:</strong> The explanation here.</p>
  </div>
  ```
  Variants: `bg-brand-50 border-brand-500` (teal info), `bg-red-50 border-signal-red` (warning/danger)

- **Blockquotes**: `<blockquote><p>The pull quote text.</p></blockquote>`

- **Lists**: `<ul><li><strong>Label</strong> — explanation text.</li></ul>`

**Important HTML entities**: Use `&#8217;` for apostrophes, `&mdash;` for em dashes, `&#8220;`/`&#8221;` for smart quotes, `&#8594;` for arrows, `&#9733;` for stars.

### 3. Update content.json

Read the current `site/data/content.json`, then update these sections:

**Always add to `latest`** (insert at position 0 — the top):
```json
{
  "url": "articles/article-<slug>.html",
  "category": "Category",
  "title": "The Headline Here",
  "time": "Just now"
}
```

**Always add to `stories`** (insert at position 0 — the top):
```json
{
  "url": "articles/article-<slug>.html",
  "category": "Category",
  "title": "The Headline Here",
  "description": "One-sentence summary for the card.",
  "date": "March 20, 2026"
}
```

**If promoted to lead story**: Move the current `lead_story` object into the top of `stories` first, then replace `lead_story` with the new article:
```json
{
  "url": "articles/article-<slug>.html",
  "category": "Category",
  "title": "The Headline",
  "description": "Two-sentence summary.",
  "author": "LTC Sentinel Research",
  "date": "March 20, 2026",
  "read_time": "8 min read"
}
```

Write the updated JSON back to `site/data/content.json`.

### 4. Update the sitemap

Add a new `<url>` entry to `site/sitemap.xml`:
```xml
<url>
    <loc>https://ltcsentinel.com/articles/article-<slug>.html</loc>
    <lastmod>2026-03-20</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.7</priority>
</url>
```

### 5. Confirm to the user

Tell them:
- The article file path
- Which sections of content.json were updated
- The URL it will be at: `https://ltcsentinel.com/articles/article-<slug>.html`

## Writing Style

LTC Sentinel articles are **data journalism** — fact-heavy, analytical, and direct. The tone sits between a Wall Street Journal investigative piece and a healthcare trade publication. No fluff, no marketing language.

- Lead with the most striking data point
- Use specific numbers, facility names, and states — never vague generalizations
- Bold key statistics and entity names in the body
- Include at least one stat-row, one data table or callout, and one blockquote per article
- End with a forward-looking "what to watch" section or a memorable closing quote
- Article length: 800-1500 words typically, 5-9 minute read
