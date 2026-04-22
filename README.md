# UXLens — Claude Code Skill

A Claude Code skill that lets Claude audit any website for UX, UI, and accessibility issues directly from your editor.

## Install

### Option 1: Clone into your Claude skills folder

```bash
git clone https://github.com/misothecod/uxlens-skill.git ~/.claude/skills/uxlens
```

### Option 2: Download manually

1. Create the skill directory:
   ```bash
   mkdir -p ~/.claude/skills/uxlens
   ```
2. Copy `SKILL.md` into that directory

### Option 3: Add to your project

```bash
mkdir -p .claude/skills/uxlens
# Copy SKILL.md into this directory
```

## Setup

1. Get a free API key at [https://uxlens.io/dashboard](https://uxlens.io/dashboard) (5 audits/month free)
2. Set your API key:
   ```bash
   export UXLENS_API_KEY=your_key_here
   ```

## Use

```
/ux-audit https://mysite.com
```

Claude will run the audit and read back the results:

```
Score: 3.8/5 — Good
Critical issues (2):
  1. [CRITICAL] 4 images missing alt text
  2. [CRITICAL] No skip navigation link
Major issues (3):
  1. [MAJOR] CTA button contrast ratio too low
  ...
Estimated fix time: 45 minutes
Shall I fix these?
```

## Pricing

| Tier | Price | Audits/month |
|------|-------|-------------|
| Free | $0 | 5 |
| Developer | $9.99/mo | 500 |
| Pro | $29/mo | 3,000 |

---

MIT License — [uxlens.io](https://uxlens.io)
