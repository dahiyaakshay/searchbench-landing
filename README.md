# Searchbench
 
Searchbench is a local SEO workbench and content operations platform. It reads the Search Console and Analytics exports you already download, shows the arithmetic behind every finding it produces, and writes the pages worth writing. Nothing is scored by a model, and your exports never leave your machine.
 
Currently in **private development**.
 
---
 
## What Searchbench Does
 
Searchbench reads your exports and produces:
 
- **Findings with their evidence** - 30+ deterministic checks, each carrying the rows it came from, a basis label (counted, read from page, or rule of thumb), and a size in clicks so the list can be ordered by what it recovers
- **Query-to-page mapping** - every search matched to the page that should answer it, measured where your export carries pairs, stemmed lexical overlap where it does not, and a manual pin that overrules both
- **Page Brief** - open any page for a full case file: how it performs, what is wrong with it, a checklist of specific instructions, every query it should own, and whatever the crawler read from it
- **AI Retrievability Crawl** - per-bot access for GPTBot, ClaudeBot and PerplexityBot, JS dependency, summary block presence, citable facts, chunk quality, boilerplate ratio and author signals
- **Page Set Studio** - hundreds of pages from one table, with title, slug and target query built from patterns, AI-computed columns, multi-step generation, conditional steps and a different model per step
- **Preflight** - every row checked before a single call is made: thin rows, near-duplicates, address clashes with real pages, ghost columns, and conditions no row satisfies
- **Brand Kit enforcement** - tone, banned words, reading level and sentence length applied while writing and verified afterwards by counting
- **Internal Link Intelligence** - targets chosen by the mapper from pages that exist, self-links excluded by path tail comparison, and invented links flagged after generation
- **Cost Ledger** - tokens and cost recorded per call, per model, per page, with a ceiling that stops a run and a resume that does not pay twice
- **Library and approval** - everything written waits to be read, blocking issues refuse approval outright, and export happens only on your word
- **Resync** - paste a changed table and only the rows that moved are rewritten, with the plan shown before anything runs
- **Presets** - a whole way of working saved and applied to another table
- 14 export kinds recognised from their column headings, not their filenames
- Segment analysis by device, country and search appearance
- Period-over-period decay detection when a second export is loaded
---
 
## Why It Exists
 
SEO data arrives as a pile of disconnected exports, and the tools that read them hide their arithmetic behind a score.
 
You end up trusting a number you cannot check, paying monthly whether you opened the tool or not, and then writing the pages somewhere else entirely. Meanwhile search itself has split in two: ranking on Google and being cited by an answer engine are now different problems with different causes.
 
Searchbench does the analysis as arithmetic you can verify against your own spreadsheet, checks the AI-retrievability signals a classic SEO tool ignores, and puts the writing in the same place as the finding that justified it.
 
---
 
## Architecture Overview
 
### Frontend
- Vanilla JavaScript ES modules, no framework
- No bundler, no build step, no node_modules
- Plain CSS with custom properties in a single design token file
- Hand-drawn inline SVG charts, no charting library
- Three-phase navigation: what happened, what to do, make it
- The entire interface is readable source
### Backend
- Python 3.11 + FastAPI
- Flat-file workspace, no database to install
- Project-scoped directories on your own disk
- Thread-pooled parallel generation with single-threaded result application
### Processing Pipeline (no model involved)
- Stemming and tokenisation with LRU caching across calls
- Shingled Jaccard similarity for near-duplicate detection
- Click-through-rate curve by position for performance comparison
- Heading-boundary chunking and readability scoring
- robots.txt parsed from a fetch made under the tool's own user agent
- All analysis is purely algorithmic. No model call ever produces a finding.
---
 
## Analysis Pipeline
 
Every upload passes through sequential stages:
 
1. File classification from column headings, with a cleaning report
2. Normalisation: thousands separators, totals rows, percentages written as less-than
3. Page and query aggregation across every export kind
4. Query-to-page mapping, measured pairs taking priority over inferred
5. Crawl, or import of an existing crawler export, for page content and AI signals
6. Thirty-odd checks across six disciplines
7. Finding ranking by recoverable clicks, with effort and basis labels
8. Brief assembly per page, on demand and free
9. Preflight over any page set before generation
10. Generation, with per-step models, conditions and budget enforcement
11. Post-generation verification: brand kit, internal links, schema validity
The mapping is built once behind a lock and memoised, because three views request it simultaneously on first paint.
 
---
 
## Finding Model
 
Every finding carries four things rather than a score:
 
| Attribute | What it means |
|---|---|
| Action | The recommendation as a verb, not a theme |
| Basis | Counted, read from the page, or a rule of thumb applied to its text |
| Evidence | The actual rows: impressions, position, click-through rate, counts |
| Recoverable clicks | A size for ordering the list, never presented as a forecast |
 
Where a page already ranks, recoverable clicks are impressions times the typical rate for that position, less the clicks earned. Where there is no position to work from, it is a flat fraction of the impressions at stake. The interface says which, because they are not the same kind of number.
 
---
 
## Check Categories
 
**Performance** - under-performing click-through for the position, decay against a previous period, a device or country far below the rest, brand searches landing on the wrong page
 
**Architecture** - families of pages with no hub, paths buried too deep, no internal links pointing in, a shared template dragging every child down
 
**Content** - very thin content, high boilerplate ratio, near-duplicate pages, reading age above the audience, images with no alt text
 
**GEO and retrievability** - AI crawlers blocked in robots.txt, content that needs JavaScript to read, no self-contained answer in the opening, no figures or dates or named sources, poor chunk quality, missing structured data, no author signal
 
**Mapping** - two pages competing for one search, searches with no page to land on, questions people ask that no page answers
 
**Programmatic** - near-duplicate rows, addresses clashing with pages that already exist, rows with nothing of their own to say
 
---
 
## Generation Pipeline
 
Page sets are written through a configurable multi-step pipeline:
 
1. Preflight over every row, free, before any call
2. Research step, typically on a cheap model
3. Conditional steps that skip rows they do not apply to
4. Draft step, typically on a better model
5. Brand kit verification by counting, not by opinion
6. Internal link selection from pages that exist, self-links excluded
7. Schema generation from facts the page already has, refusing to invent
8. Library approval, with blocking issues refusing outright
9. Export as Markdown or Word, for you to publish yourself
Steps carry a fingerprint of their instruction, model and condition. Changing any of them invalidates that step's cached result; everything else resumes without paying again.
 
---
 
## Export
 
- **Markdown** - per-page files with front matter, ready for a static site or a CMS import
- **Word (.docx)** - formatted documents for review and handoff
- **CSV** - the page set with every column, AI-computed ones included, under your own field names via the export map
- **Preset JSON** - a whole way of working, portable to another table or another project
---
 
## Reliability Design
 
- 439 tests, green before any change ships
- Several tests exist only to stop a fixed bug returning somewhere else: a page judged real from a single source, an em dash creeping back, a definition written for a screen that never shows it
- Version-guarded state writes with conflict detection
- Budget enforcement at batch boundaries, with overshoot reported honestly
- Partial-cost flagging when any call in a run could not be priced
- Resume without double-charging, via per-step fingerprint stamps
- Workspace isolation: each project writes to its own directory
- Crawl politeness: one request a second, robots.txt obeyed, only the pages you tick
---
 
## Performance
 
Measured against a 345-page site with 1,300 queries, which is 448,500 comparisons every time the mapping is built.
 
| Operation | Before | After |
|---|---|---|
| Post-crawl reload, three endpoints | 21.7s | 0.1s |
| Mapping view payload | 1,340 kB | 55 kB |
| Findings endpoint | 7.6s | 2.6s |
 
Caching the stemmer, memoising the mapping and building it once behind a lock accounted for most of it. The mapping view fell by sharing one page list across rows rather than giving each row its own copy.
 
---
 
## What It Deliberately Does Not Do
 
- No Search Console or Analytics API connection. You export the CSVs.
- No scheduled or unattended runs.
- No rank tracking over time.
- No backlink index.
- No publishing. It exports; you publish.
- No model anywhere in the analysis, which is why reading costs nothing and runs offline.
The analysis is English only. The arithmetic works on any language; the word matching does not.
 
---
 
## Advanced Intelligence (Coming Soon)
 
- Validation of generated content quality against a live model at scale
- Multi-site comparison across projects
- Scheduled local runs via the system task scheduler
- Expanded GEO checks: retrieval simulation and citation likelihood scoring
- Per-section rewrite rather than whole-page
---
 
## Repository Contents
 
This repository contains the portfolio landing page for Searchbench.
 
The full application (backend, frontend, analysis engine) is private and currently in development.
 
**Live page:** [searchbench.akshaydahiya.site](https://searchbench.akshaydahiya.site/)
 
### Hosting this page
 
The page is a single `index.html` with every style, script and illustration inlined. The only external request is the Urbanist font.
 
1. Push `index.html` to the repository root
2. **Settings → Pages → Deploy from a branch → `main` → `/ (root)`**
3. Add a CNAME record at your DNS provider pointing `searchbench` at `dahiyaakshay.github.io`
4. Enter the custom domain in Settings → Pages and tick **Enforce HTTPS**
All colours are CSS custom properties at the top of the `<style>` block. Changing `--lime` recolours every button, tile and glow on the page.
 
---
 
## Developer
 
Built and designed by **Akshay Dahiya**
 
Portfolio: [akshaydahiya.site](https://akshaydahiya.site)
LinkedIn: [linkedin.com/in/akshay-dahiya](https://www.linkedin.com/in/akshay-dahiya/)
Other tools: [AI Spider](https://aispider.akshaydahiya.site) · [MarAI](https://marai.akshaydahiya.site)
 
---
 
## Positioning
 
Searchbench operates at the intersection of:
 
- Deterministic SEO Analysis
- Generative Engine Optimisation (GEO)
- Programmatic SEO at Scale
- Local-First Content Operations
Not a dashboard. A workbench.
 
