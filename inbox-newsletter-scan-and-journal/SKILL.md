---
name: inbox-newsletter-scan-and-journal
description: Scans inbox for AI/Tech and Fintech newsletters, extracts key items, and creates a curated journal entry.
argument-hint: "[--days N] [--date YYYY-MM-DD]"
allowed-tools: Bash(gog *), Read, Write
---

## Purpose
Search email for newsletters from key sources, extract and prioritize items based on relevance, and create a curated daily digest.

## Setup

This skill requires:
- [gog CLI](https://github.com/steipete/gogcli) for Gmail access
- An output directory for digest files (default: `~/SecondBrain/Newsletter Digest/`)

Configure in your `CLAUDE.md`:
```
GOG_ACCOUNT=you@example.com
NEWSLETTER_DIGEST_DIR=~/SecondBrain/Newsletter Digest
```

### Relevance Profile
Customize the relevance profile below to match your interests. This controls how items are prioritized.

- **Current work**: [What you're building / your role]
- **Key interests**: [Topics that should be flagged as high priority]

### Newsletter Sources
Update the sender patterns to match your subscriptions:

| Source | Sender Pattern |
|--------|----------------|
| TLDR AI/Fintech | `from:dan@tldrnewsletter.com` |
| AINews | `from:swyx+ainews@substack.com` |
| a16z | `from:a16z@substack.com` |
| The Information | `from:hello@theinformation.com` |
| Claude Team | `from:no-reply@email.claude.com` |
| Cursor Team | `from:team@mail.cursor.com` |

## Arguments
| Argument | Default | Description |
|----------|---------|-------------|
| `--days N` | all | Number of days to look back (use "all" for entire inbox) |
| `--date YYYY-MM-DD` | today | Specific date to create digest for |

## Process

### Step 1: Create folder structure
```bash
mkdir -p "$NEWSLETTER_DIGEST_DIR"
```

### Step 2: Search for newsletters
For each source, search email:
```bash
# Default (all inbox emails):
gog gmail search 'in:inbox from:SENDER_PATTERN' --max 10 --account $GOG_ACCOUNT

# With --days N restriction:
gog gmail search 'in:inbox from:SENDER_PATTERN newer_than:{days}d' --max 10 --account $GOG_ACCOUNT
```

### Step 3: Read newsletter content
For each newsletter found:
```bash
gog gmail get <message-id> --account $GOG_ACCOUNT
```

### Step 4: Extract and prioritize
For each newsletter, extract items and categorize:
- **High priority**: Items directly relevant to your work and key interests
- **Medium priority**: Major industry developments, developer tools, notable launches
- **Low priority**: General tech news, tangential items

**IMPORTANT**: Extract the article URL for each item. Article titles should NOT be linked directly — instead, link the source name to the article URL (e.g., `**Title** — Description ([TLDR AI](article-url))`).

### Step 5: Create digest note
Write to `$NEWSLETTER_DIGEST_DIR/YYYY-MM-DD.md`

### Step 6: Archive processed newsletters
For each newsletter that was processed, archive it by removing from inbox:
```bash
gog gmail thread modify <thread-id> --remove INBOX --account $GOG_ACCOUNT
```

---

## Output Template

**Note on content volume**: The template shows structure examples only. Use judgment to:
- Include all relevant items found (not limited to 1-2 per subsection)
- Skip subsections entirely if no content fits
- Add/remove subsections based on actual newsletter content
- Top Stories should be 2-3 most important items

```markdown
# Newsletter Digest — YYYY-MM-DD

## Top Stories
The most important items from your inbox.

### Story Title
**Source:** [Newsletter Name](article-url) (Date)
[1-2 sentence summary of why this matters]

---

## Industry & Market
Items directly relevant to your work.

### Funding & Market Moves
- **Title** — Description ([Source](article-url))

### Strategy & Analysis
- **Title** — Description ([Source](article-url))

---

## Developer Tools & AI Infrastructure
Updates relevant to building with AI.

### Model Updates
- **Title** — Description ([Source](article-url))

### Tools & Frameworks
- **Title** — Description ([Source](article-url))

---

## AI/Tech
Notable developments worth tracking.

- **Title** — Description ([Source](article-url))

---

## Quick Hits

- **Title** — One-liner ([Source](article-url))

---

*Sources: Newsletter A (Date), Newsletter B (Date)*
```

---

## Output Summary
Report after completion:
```
Newsletters processed: X
Newsletters archived: X
Top stories: X
Total items: X
Digest: $NEWSLETTER_DIGEST_DIR/YYYY-MM-DD.md
```
