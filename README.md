# AEO + Technical SEO Audit Prompt

This repository holds a reusable prompt for running **Answer Engine Optimization (AEO)**, **technical SEO**, and **AI crawlability** audits. The full instructions live in [`aeo_seo_audit_prompt.md`](aeo_seo_audit_prompt.md).

**Prompt version:** 2.0 — May 2026

## What it does

Paste the prompt (from the markdown file) into a capable AI assistant, replace `https://yoursite.com` with your real URL, and ask for the audit. The prompt requires the model to:

- **Discover real pages** via navigation + `/sitemap.xml`, fetch up to ~50 key URLs plus `robots.txt`, `sitemap.xml`, and `/llms.txt`
- **Detect the platform** (Framer, Webflow, WordPress, etc.) from HTML and tailor fix steps to that stack
- **Deliver output as a single dark-themed HTML report** (not plain text or markdown), with scoring, table of contents, issue cards, summary tables, and a prioritised action plan

## Audit coverage

Findings are grouped into four **AEO pillars**:

| Pillar | Focus |
|--------|--------|
| **Findable** | HTTPS, robots.txt, AI crawlers (e.g. GPTBot, ClaudeBot), sitemap, canonicals |
| **Quotable** | Meta descriptions, body copy depth, freshness signals, FAQ-style content (bonus) |
| **Understandable** | H1, titles, heading hierarchy, JSON-LD, Open Graph / Twitter cards, image alt text |
| **Trustworthy** | Internal links in body, external citations, `llms.txt` (bonus) |

Sections in the full prompt include: H1 tags, page titles, meta descriptions, structured data (JSON-LD), Open Graph & social tags, content structure, AI crawlability, `llms.txt`, content freshness, scoring, and a prioritised action plan.

## Design requirements (for the generated report)

The prompt specifies a production-style HTML report: dark background (`#0e0e0e`), priority colours (high / medium / low / pass), and typography via Google Fonts (**Instrument Serif**, **Inter**, **IBM Plex Mono**).

## Platform notes (summary)

The markdown file includes extra guidance for **Framer**, **WordPress** (Yoast / Rank Math), and **Webflow** — where to put JSON-LD, meta, OG images, and caveats (e.g. hosting `llms.txt` off-platform for Framer).

## How to use

1. Open [`aeo_seo_audit_prompt.md`](aeo_seo_audit_prompt.md).
2. Copy the fenced prompt block under **The Prompt**.
3. Set your site URL in the first line of the prompt.
4. Run it in your AI tool of choice; verify the assistant actually fetches URLs and only reports on **HTTP 200** responses as the prompt requires.

For the complete checklist, tables, example `robots.txt`, and mandatory output format, see [`aeo_seo_audit_prompt.md`](aeo_seo_audit_prompt.md).
