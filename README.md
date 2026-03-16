# Claude Skills

Reusable skills for [Claude Code](https://claude.ai/claude-code).

## Skills

### done

Captures session takeaways to a Thinking log in your notes vault. Auto-detects whether the session was **research/exploration** or a **decision**, and uses the appropriate template.

**Usage:** `/done`, `/done quick`, `/done research`, `/done decision quick`

### inbox-newsletter-scan-and-journal

Scans your email inbox for newsletters, extracts and prioritizes items based on your relevance profile, and creates a curated daily digest. Requires [gog CLI](https://github.com/steipete/gogcli) for Gmail access.

**Usage:** `/inbox-newsletter-scan-and-journal`, `/inbox-newsletter-scan-and-journal --days 3`

## Installation

Copy a skill folder into your project's `.claude/skills/` directory:

```bash
cp -r done/ /path/to/your/project/.claude/skills/done/
```

Then update paths and configuration in the skill's `SKILL.md` to match your setup.
