# Claude Skills

Reusable skills for [Claude Code](https://claude.ai/claude-code).

## Skills

### done

Captures session takeaways to a Thinking log in your notes vault. Auto-detects whether the session was **research/exploration** or a **decision**, and uses the appropriate template.

**Usage:** `/done`, `/done quick`, `/done research`, `/done decision quick`

## Installation

Copy a skill folder into your project's `.claude/skills/` directory:

```bash
cp -r done/ /path/to/your/project/.claude/skills/done/
```

Then configure the output directory in your `CLAUDE.md`:

```
DONE_SKILL_OUTPUT_DIR=~/notes/thinking
```

Or set separate work/personal directories:

```
DONE_SKILL_WORK_DIR=~/notes/work/thinking
DONE_SKILL_PERSONAL_DIR=~/notes/personal/thinking
```
