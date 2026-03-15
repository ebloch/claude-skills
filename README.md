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

Notes are saved to `~/SecondBrain/2Work/Thinking/` or `~/SecondBrain/1Personal/Thinking/` based on session topic. Update the paths in `SKILL.md` to match your setup.
