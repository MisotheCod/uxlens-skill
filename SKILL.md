---
name: ux-audit
description: Audit any website for UX, UI, and accessibility issues before launching or during code review. Use when: checking a landing page's usability before launch, validating accessibility (WCAG) compliance, getting Core Web Vitals feedback, comparing scores before/after a redesign, or fixing UX issues found by other agents.
invoked: automatically when user asks about launching, auditing, or checking a website's quality.
---

# UXLens — Website UX Audit Skill

Run a full UX audit on any website in seconds. Get back a structured report with specific issues, severity levels, and prioritized fixes — plus an `agent_summary` paragraph you can read aloud and act on directly.

**Works on any URL. No copy-pasting. No leaving your editor.**

## When To Use This

- Before launching a landing page or portfolio
- During code review when checking a UI change
- After a redesign to verify improvements
- When a user reports "the site feels broken"
- To validate accessibility before shipping

## Setup

Get a free API key at **https://uxlens.io/dashboard** (5 audits/month free, no credit card).

Set your API key:

```bash
export UXLENS_API_KEY=your_key_here
```

Or pass it inline in the request (see examples below).

## How To Invoke

```
/ux-audit https://mysite.com
```

Or let Claude invoke it automatically when you describe checking a website's quality.

## API Reference

**Base URL:** `https://uxlens.io/api/audit`

### Single URL Audit

```
POST /api/audit
{
  "url": "https://example.com",
  "url": "https://example.com",
  "max_pages": 5
}
```

With API key (via Authorization header or inline):
```
Authorization: Bearer UXLENS_API_KEY
```

### Full Site Crawl

```
POST /api/audit
{
  "url": "https://example.com",
  "crawl_all": true,
  "max_pages": 20
}
```

### Diff Mode (before/after redesign)

First audit:
```bash
curl -X POST https://uxlens.io/api/audit \
  -H "Authorization: Bearer $UXLENS_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"url":"https://mysite.com"}'
# Save the audit_id from the response
```

Second audit with comparison:
```bash
curl -X POST https://uxlens.io/api/audit \
  -H "Authorization: Bearer $UXLENS_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"url":"https://mysite.com","compare_to":"uxl_xxxx"}'
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `overall` | number | 0-5 score (e.g. 3.5 = "Good") |
| `status` | string | World-Class / Excellent / Good / Fair / Poor |
| `agent_summary` | string | One-paragraph summary — safe to read aloud |
| `audit_id` | string | Use with `compare_to` for diff mode |
| `uxIssues[]` | array | Usability violations (critical/major/minor) |
| `uiIssues[]` | array | Design issues (spacing, typography, alignment) |
| `a11yIssues[]` | array | WCAG accessibility violations |
| `lighthouse` | object | Performance, SEO, Accessibility (0-100) |
| `coreWebVitals` | object | LCP, CLS, FCP, INP with pass/fail |
| `mobileResponsive` | object | Viewport, touch targets, font sizes |
| `metaCompleteness` | object | OG tags, Twitter cards, canonical |

### Example Response Snippet

```json
{
  "overall": 3.8,
  "status": "Good",
  "agent_summary": "UXLens audit of example.com: Score 3.8/5 (Good). 2 critical issues found — missing alt text on 4 images, no skip navigation link. 3 major issues including low contrast on CTA button. Estimated fix time: 45min.",
  "audit_id": "uxl_a1b2c3d4",
  "uxIssues": [
    {
      "id": "ux-1",
      "title": "Missing alt text on informational images",
      "severity": "critical",
      "issue": "4 images lack alt attributes",
      "fix": "Add descriptive alt text to all <img> elements",
      "element": "img[src*='hero']"
    }
  ]
}
```

## Example Audit Workflow

```
You: /ux-audit https://mysite.com

Claude: Running UXLens audit on https://mysite.com...

Score: 3.8/5 — Good
Critical issues (2):
  1. [CRITICAL] 4 images missing alt text
  2. [CRITICAL] No skip navigation link for keyboard users

Major issues (3):
  1. [MAJOR] CTA button contrast ratio 2.1:1 (needs 4.5:1)
  2. [MAJOR] No focus indicator on navigation links
  3. [MAJOR] Forms missing associated labels

Estimated fix time: 45 minutes

Shall I fix these now?
```

## Pricing

| Tier | Price | Audits |
|------|-------|--------|
| Free | $0/mo | 5/month |
| Developer | $9.99/mo | 500/month |
| Pro | $29/mo | 3,000/month |

## Security

- API keys are never stored by the skill
- All audit data is transient
- MIT-0 license
- Homepage: https://uxlens.io
