# Capture session notes to Thinking log

*v2 — Research & decision capture*

Captures key takeaways from the current session and writes a note to a Thinking log. Auto-detects whether the session was **research/exploration** or a **decision**, and uses the appropriate template.

## Setup

Configure the output directory by setting `DONE_SKILL_OUTPUT_DIR` in your `CLAUDE.md`:

```
DONE_SKILL_OUTPUT_DIR=~/notes/thinking
```

If not set, notes are saved to `./thinking/` in the current working directory.

You can also configure separate directories for work and personal topics:

```
DONE_SKILL_WORK_DIR=~/notes/work/thinking
DONE_SKILL_PERSONAL_DIR=~/notes/personal/thinking
```

When both are set, the skill will classify the session as **work** or **personal** and save to the appropriate directory. When only `DONE_SKILL_OUTPUT_DIR` is set, all notes go to the same place.

## Arguments

`$ARGUMENTS` can include (in any order):
- *(none)* — auto-detect type, full capture
- `quick` — abbreviated capture (skip Open Questions, Follow-ups, and Context/Source sections)
- `research` — force research template
- `decision` — force decision template

Examples: `/done`, `/done quick`, `/done research`, `/done decision quick`

## Instructions

### Step 1: Determine Session Type

If `$ARGUMENTS` contains `research` or `decision`, use that template.

Otherwise, analyze the conversation to auto-detect:

- **Decision** — the session contains an explicit choice between options, a comparison that led to a conclusion, or a commitment to a specific course of action. Look for: "go with X", "decided on", choosing between alternatives, weighing pros/cons with a winner.
- **Research** — the session contains exploration, synthesis, learning, or analysis without converging on a binary choice. Look for: investigating a topic, reading/summarizing content, understanding how something works, gathering information.
- **Mixed** — if the session contains both research and a decision, prefer the **Decision** template. The research naturally becomes the Context and Rationale sections.
- **Genuinely ambiguous** — ask the user: "This session had both research and decision elements. Should I capture this as a decision or research note?"

### Step 2: Determine Output Directory

If separate work/personal directories are configured (`DONE_SKILL_WORK_DIR` and `DONE_SKILL_PERSONAL_DIR`), classify the session topic:

- **Work** — directly related to your job, company, product, engineering, hiring, customers, etc.
- **Personal** — everything else (personal research, family, investments, health, side projects, general knowledge, etc.)

Save to the matching directory. If only a single `DONE_SKILL_OUTPUT_DIR` is configured (or no config exists), save all notes there.

### Step 3: Extract Content

Review the full conversation and extract the relevant content for the detected template.

**For Decision sessions, extract:**
- The decision itself (clear, bold statement)
- Context that led to the decision
- Rationale (why this choice wins)
- Alternatives considered (and why they lost)
- Action items / next steps
- Sources (emails, documents, links, conversations referenced)

**For Research sessions, extract:**
- A 1-2 sentence summary of what was researched and why
- Key findings — the substantive takeaways. Be flexible with format: use prose paragraphs for nuanced points, bullet lists for enumerations, sub-headings (`###`) for distinct sub-topics, and blockquotes for exact quotes or citations. Match the format to the content.
- Open questions — unresolved items raised but not answered
- Follow-ups — concrete next actions (as checkboxes)
- Sources — documents, links, emails, or conversations referenced

**Important:** Only include sections that have real content. Never write "None", "None created", "N/A", or leave a section empty. If there are no open questions, omit the Open Questions section entirely.

### Step 4: Write the Note

Create a new file in the output directory named: `YYYY-MM-DD - Topic Name.md`

Use Title Case with spaces for the topic name (e.g., `2026-02-24 - EV Charging Selection.md`, `2026-02-26 - Trust Signing Authority Research.md`).

If a file with the same name already exists, append a number: `2026-02-26 - Topic Name 2.md`

#### Decision Template

```markdown
**Decision:** [Clear statement of what was decided]

**Context:**
- [Relevant background point]
- [Relevant background point]

**Rationale:**
- [Why this choice wins — specific, concrete reasons]
- [Why this choice wins]

**Alternatives Considered:**
- [Alternative 1]: [Why it wasn't chosen]
- [Alternative 2]: [Why it wasn't chosen]

**Action:** [Next step(s) — who does what]

**Source:** [Where the information came from — emails, docs, links, conversations]
```

#### Research Template

```markdown
# [Topic Name]

**Summary:** [1-2 sentence summary of what was researched and the key takeaway]

## Key Findings

[Flexible format. Use whatever structure best fits the content:
- Prose paragraphs for nuanced analysis
- Bullet points for lists of facts
- Sub-headings (###) for distinct sub-topics
- Blockquotes (>) for exact quotes or citations
- Combinations of all of the above]

## Open Questions
- [Unresolved question 1]
- [Unresolved question 2]

## Follow-ups
- [ ] [Concrete next action 1]
- [ ] [Concrete next action 2]

## Sources
- [Source 1 — document path, URL, email thread, etc.]
```

#### Quick Mode

For `quick` captures, use abbreviated versions:

**Quick Decision** — include only: Decision, Rationale (2-3 bullets max), and Action. Skip Context, Alternatives, and Source.

**Quick Research** — include only: the `#` title, Summary, and Key Findings (condensed to the most important 3-5 points). Skip Open Questions, Follow-ups, and Sources.

### Step 5: Confirm

Display a brief confirmation:

```
CAPTURED: [Topic Name]
Type: [Decision/Research]
Saved to: [directory]/[filename].md
```

If there are follow-ups, add:
```
Next: [Most important follow-up item]
```

## Error Handling

- **No substantive content:** If the session was just a quick lookup or greeting, report: "Nothing to capture — session was too brief."
- **File write fails:** Display the full note content in the terminal so it can be manually saved.
