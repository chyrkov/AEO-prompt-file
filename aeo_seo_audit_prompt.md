# AEO + Technical SEO Audit Prompt

---

## The Prompt

```
I need you to audit my website for AEO (Answer Engine Optimization) and technical SEO issues.

My website URL is: https://yoursite.com

---

## OUTPUT FORMAT — MANDATORY

Deliver the entire audit as a single, self-contained HTML file using a dark-themed,
production-grade design. Do not deliver the audit as plain text or markdown.

The HTML report must include:

- **Header**: site URL, detected platform (Framer / Webflow / WordPress / etc.),
  audit date, and a score band showing total issue counts by priority
  (High / Medium / Low / Pass)
- **Table of contents** with working anchor links to each section
- **Per-section issue cards** containing:
  - Priority badge: HIGH (red) / MEDIUM (orange) / LOW (blue) / PASS (green)
  - "✗ What's wrong" label with the problem explained clearly
  - "✓ How to fix it" label with step-by-step instructions
  - Platform-specific fix location (e.g. exactly which Framer/Webflow/WordPress
    panel or plugin to use)
  - Syntax-highlighted, ready-to-paste code blocks for all fixes
- **Summary tables** for H1s, page titles, meta descriptions, and technical checks
  with status indicators: ✓ PASS / ⚠ WARN / ✗ FAIL
- **Final prioritised action plan table** ranking all issues by impact,
  with estimated effort and fix type

Design spec:
- CSS variables for all colors; dark background #0e0e0e
- IBM Plex Mono for code and labels; Instrument Serif for display headings;
  Inter for body text (load from Google Fonts)
- Colour system: red #ff5c5c = HIGH · orange #ff9a3c = MEDIUM ·
  blue #5ca0ff = LOW · green #50e0a0 = PASS
- Glowing score dots, bordered issue cards, collapsible sections if needed
- All code blocks syntax-highlighted with token colors

---

## STEP 1 — DISCOVER & FETCH PAGES

Do NOT assume a fixed page structure. Every website is different.

First, discover what pages actually exist on this site:
1. Fetch the homepage (/) and extract all internal navigation links.
2. Fetch /sitemap.xml — if it returns valid XML, extract all URLs listed there.
3. Use both sources to build a list of real pages that exist on this site.

Then fetch and fully analyse:
- The homepage (/)
- Up to 50 of the most important pages you discovered (choose based on what actually exists —
  pick the pages linked most prominently in the navigation or listed first in the sitemap)
- At least one individual article, blog post, or detail page if any exist
- Also fetch: /robots.txt · /sitemap.xml · /llms.txt

CRITICAL: Only include pages in the report that you actually fetched and that returned HTTP 200.
Do NOT guess, invent, or assume page paths. Do NOT add a row for a page you did not fetch.
Base every finding entirely on the real content returned by each URL.

Identify the platform (Framer, Webflow, WordPress, Squarespace, custom, etc.)
from the HTML source (look for meta-generator, class names, script URLs).
Tailor ALL fix instructions to that specific platform's interface.

---

## AUDIT SECTIONS

### 1. H1 TAGS

For every page fetched:
- Count H1 tags in the DOM. Flag if count ≠ 1.
- Check if the H1 is descriptive of page content (not just "About me",
  "Services", or a brand name alone).
- Flag duplicate H1s (same text on multiple pages).
- Flag missing H1s.

**Deliverable:** A summary table with columns:
Page · H1 Count · Current H1 Text · Char Count · Status

For every page that fails, provide a corrected H1 suggestion that:
- Is 50–70 characters
- Contains the page's primary keyword
- Describes who + what, not just a label

---

### 2. PAGE TITLES

For every page fetched:
- Extract the `<title>` tag value.
- Check length: flag if under 30 chars (too short) or over 60 chars
  (will be truncated in SERPs).
- Check uniqueness: flag if the same title appears on multiple pages.
- Check pattern: ideal format is `[Page Topic] — [Brand Name]`

**Deliverable:** A summary table with columns:
Page · Current Title · Char Count · Pattern OK? · Status

For every page that fails, provide a corrected title suggestion
(50–60 chars, keyword-first, includes brand name).

---

### 3. META DESCRIPTIONS

For every page fetched:
- Extract the meta description content.
- Check length: flag if under 50 chars (too short) or over 160 chars
  (will be truncated).
- Check uniqueness: flag if the same description appears on multiple pages.
- Check quality: is it descriptive, does it include a call to action,
  does it reflect actual page content?

**Deliverable:** A summary table with columns:
Page · Current Description · Char Count · Status

For every page that fails or is suboptimal, provide a replacement meta
description that:
- Is 140–155 characters (safe zone for desktop and mobile)
- Accurately summarises the page
- Includes the primary keyword naturally
- Ends with an implicit or explicit call to action where appropriate
- Is written in first or third person consistently with the site's voice

---

### 4. STRUCTURED DATA (JSON-LD)

For every page:
- Check for any existing `<script type="application/ld+json">` blocks.
- List which schema types are present and which are missing.

**Provide ready-to-paste JSON-LD blocks for each page you actually fetched.**

For each page, choose the most appropriate schema type based on its real content:
- A page that introduces the business or person → `Organization` or `Person`
- A page listing products, services, or offerings → `ItemList` of `Service` or `Product` nodes
- A page showing a portfolio, gallery, or collection → `CollectionPage`
- A page describing the company or individual → `AboutPage`
- A blog or news index → `Blog`
- An individual article, post, or case study → `BlogPosting` or `Article`
- A contact or enquiry page → `ContactPage`
- Any other page → `WebPage` with appropriate properties

All JSON-LD blocks must:
- Use real data extracted from the fetched pages (names, URLs, descriptions from actual content)
- Include `sameAs` arrays with all detected social/external profile URLs
- Be immediately copy-pasteable with no placeholders left unfilled
  (except CMS dynamic fields, which must be clearly annotated)

For Framer: note that JSON-LD goes in Page Settings → Custom Code → Head.
For WordPress: recommend Yoast SEO or RankMath plugin.
For Webflow: note the site-wide and page-level `<head>` code embed locations.

---

### 5. OPEN GRAPH & SOCIAL TAGS

For every page:
- Check for: `og:title` · `og:description` · `og:image` · `og:url` · `og:type`
- Check for: `twitter:card` · `twitter:title` · `twitter:description` · `twitter:image`
- Check if `og:image` is unique per page or the same sitewide (flag sitewide reuse)
- Check `twitter:card` type (should be `summary_large_image` for visual sites)

**Deliverable:** A status table and specific fixes for any missing or incorrect tags.
For pages with sitewide og:image reuse, provide instructions on how to set
page-specific images in the detected platform.

---

### 6. CONTENT STRUCTURE

For every page:
- Map the heading hierarchy (H1 → H2 → H3). Flag any skipped levels or
  headings used purely for styling.
- Check word count: flag pages with fewer than 100 words of readable body text
  as "thin content" (AI cannot quote pages that are mostly images or short copy).
- Check if the primary topic is stated clearly in the first 100 words
  (critical for AEO — AI engines weight early content heavily).
- Check for **FAQ-style content**: H2 or H3 headings that are phrased as
  questions (ending in "?"), or the presence of `FAQPage` schema.
  This is a BONUS check — adds points, never penalises.
- Check **image alt text coverage**: flag if fewer than 50% of meaningful
  images have descriptive alt text (under 125 chars, descriptive of content).
- Check **internal links in body content**: flag if fewer than 3 contextual
  links appear in the page body (nav and footer links do not count).
- Check **external citation links**: flag if fewer than 2 outbound links to
  external sources exist (signals authority and credibility to AI engines).

For thin pages (under 100 words): provide specific content suggestions based
on the page topic, including example copy that matches the site's voice.

---

### 7. AI CRAWLABILITY — FINDABLE CHECKS

#### 7a. HTTPS
- Confirm all fetched URLs are served over HTTPS.
- Flag any HTTP URLs found in internal links or canonical tags.

#### 7b. robots.txt
- Fetch `/robots.txt` and display its full content.
- Check that it does NOT block the following AI crawlers:
  - `GPTBot` (OpenAI / ChatGPT)
  - `ClaudeBot` (Anthropic / Claude)
  - `PerplexityBot` (Perplexity AI)
  - `Google-Extended` (Google Gemini / SGE)
  - `cohere-ai`, `Bytespider`, `CCBot` (other major LLM crawlers)
- If any of the above are blocked via `Disallow`, flag as HIGH priority
  and provide the corrected robots.txt.
- Confirm that `/robots.txt` references the sitemap URL.

**Ready-to-paste robots.txt** (if current one needs fixing):
```
User-agent: *
Allow: /

User-agent: GPTBot
Allow: /

User-agent: ClaudeBot
Allow: /

User-agent: PerplexityBot
Allow: /

User-agent: Google-Extended
Allow: /

Sitemap: https://[domain]/sitemap.xml
```

#### 7c. sitemap.xml
- Fetch `/sitemap.xml` and validate it returns XML (not a 404 or HTML page).
- Check that it lists all key pages found during the audit.
- Flag missing pages.
- Confirm the sitemap URL is referenced in robots.txt.
- Advise submitting to Google Search Console under Sitemaps.

#### 7d. Canonical Tags
- Check every fetched page for a `<link rel="canonical">` tag.
- Confirm the canonical URL matches the page's actual URL (self-referencing).
- Flag any pages with missing, incorrect, or conflicting canonicals.

---

### 8. LLMS.TXT — AI DISCOVERABILITY

- Attempt to fetch `/llms.txt`.
- If found: display its content, evaluate whether it is properly structured
  (markdown format, site summary, annotated links, page descriptions),
  and suggest improvements.
- If missing: flag as BONUS MISSING (adds points if present, does not
  penalise if absent — but strongly recommended for AI-forward sites).

**Provide a ready-to-deploy `/llms.txt` file** tailored to the site,
including:
- Site name and one-paragraph summary (using facts extracted from the pages)
- `## Key Pages` section with annotated links and one-line descriptions
- `## Key Facts` section with quantifiable credentials (years of experience,
  number of projects, awards, student count, etc.)
- `## Optional` section for secondary pages

Note platform-specific hosting instructions (e.g. Framer cannot serve
arbitrary text files natively — suggest Netlify/Vercel static file, or
Cloudflare Pages deployment).

---

### 9. CONTENT FRESHNESS

For every page:
- Check for date signals in any of: JSON-LD `datePublished`/`dateModified`,
  `<meta>` date tags, or `<lastmod>` in sitemap.
- Flag pages with no date signals as lacking freshness indicators.
- Flag pages where the most recent detected date is older than 6 months
  as potentially stale.
- For blog posts: check that `datePublished` and `dateModified` are in
  JSON-LD (ISO 8601 format).

---

## PRIORITISED ACTION PLAN

At the end of the report, include a full action plan table with columns:
`#` · `Priority` · `Category` · `Action` · `Page(s) Affected` · `Effort` · `Impact`

Sort by Priority (HIGH first), then by Impact within each priority tier.

---

## SCORING SUMMARY

Include a visual score band at the top of the report showing:
- Total checks run
- HIGH issues count (red)
- MEDIUM issues count (orange)  
- LOW issues count (blue)
- PASS count (green)
- Bonus checks passed (purple/accent)

Group checks into four AEO pillars:
- 🔍 **Findable** — HTTPS, robots.txt, sitemap.xml, canonical, AI crawlers allowed
- 💬 **Quotable** — meta description, substantial text, content freshness, FAQ content
- 🧠 **Understandable** — H1, heading hierarchy, JSON-LD, Open Graph, image alt text
- 🤝 **Trustworthy** — internal links, external citations, llms.txt

Show a pass/fail/warn score for each pillar.
```

---

## Notes on Using This Prompt

**Platform detection** — Claude will identify your CMS/builder from the page source
and tailor every fix instruction to your specific platform. If it misidentifies,
correct it in a follow-up message.

**For Framer sites specifically:**
- JSON-LD → Page Settings → Custom Code → Head (per page)
  OR Site Settings → Custom Code → Head (sitewide, e.g. Organization schema)
- Meta title/description → Page Settings → SEO
- OG image → Page Settings → Social Image
- robots.txt and sitemap.xml are auto-generated by Framer — verify at
  yourdomain.com/robots.txt and yourdomain.com/sitemap.xml
- llms.txt cannot be hosted natively in Framer — use Netlify, Vercel,
  or Cloudflare Pages to serve it, or ask Claude for a workaround

**For WordPress sites:**
- Install Yoast SEO or RankMath for meta tags, OG tags, sitemap, and schema
- JSON-LD can also be added via a Custom HTML block or functions.php

**For Webflow sites:**
- Meta tags → Page Settings → SEO tab
- JSON-LD → Page Settings → Custom Code → Inside `<head>` tag
- sitemap.xml is auto-generated; submit in Google Search Console

---

## What This Prompt Checks (Full List)

### 🔍 Findable
| Check | What it looks for |
|-------|-------------------|
| HTTPS enabled | All URLs served over https:// |
| robots.txt present | File exists at /robots.txt |
| AI crawlers allowed | GPTBot, ClaudeBot, PerplexityBot, Google-Extended not blocked |
| sitemap.xml present | Valid XML at /sitemap.xml, all key pages listed |
| Canonical URL set | `<link rel="canonical">` on every page, self-referencing |

### 💬 Quotable
| Check | What it looks for |
|-------|-------------------|
| Meta description (50–160 chars) | Unique, appropriate length, reads naturally |
| Substantial text content | At least 100 words of readable body copy |
| Content freshness signals | Date in JSON-LD, meta, or sitemap lastmod (within 6 months) |
| FAQ-style content *(bonus)* | H2/H3 questions or FAQPage schema |

### 🧠 Understandable
| Check | What it looks for |
|-------|-------------------|
| Single H1, descriptive | Exactly one H1, not generic, 50–70 chars |
| Page title optimised | 50–60 chars, keyword-first, unique, [Topic] — [Brand] pattern |
| Heading hierarchy clean | H1 → H2 → H3, no skipped levels, not used for styling only |
| JSON-LD structured data | Appropriate schema type per page, fully populated |
| Open Graph tags complete | og:title, og:description, og:image, og:url all present |
| Twitter Card tags | twitter:card, twitter:title, twitter:description, twitter:image |
| Image alt text ≥50% coverage | Descriptive alt on at least half of meaningful images |

### 🤝 Trustworthy
| Check | What it looks for |
|-------|-------------------|
| Internal links in body (≥3) | Contextual links in page body (not nav/footer) |
| External citation links (≥2) | Outbound links to authoritative external sources |
| llms.txt present *(bonus)* | Valid markdown file at /llms.txt, well-structured |

---

*Prompt version: 2.0 — May 2026*
*Covers: SEO + AEO + AI crawlability + Content quality + Social tags*
