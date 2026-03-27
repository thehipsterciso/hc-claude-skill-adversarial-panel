# hc-claude-skill-adversarial-panel

A Claude Code skill that embeds an adversarial C-suite review panel into the content generation process for formal, discoverable corporate documents.

## What This Skill Does

The panel is not a post-production review. It is the writing process.

When you ask Claude to write any formal, discoverable corporate document — board memo, press release, shareholder letter, workforce announcement, regulatory filing, incident disclosure, or any communication that could be read by a board, investor, regulator, journalist, or attorney — this skill activates automatically. The document you receive has already survived a full adversarial review loop before it reaches you.

## How It Works

1. **Writing agent drafts** — A complete draft, not an outline
2. **Seven executive lenses review cold** — Each executive reads as a first-time reader in their role, reacting from their own accountability domain and loss function
3. **Concerns return to the writing agent** — Not to the user. The writing agent revises.
4. **Loop continues** until the panel clears
5. **Intent Guardian checks** — After the panel clears, a single check: does the output still accomplish what you asked it to accomplish?
6. **Output surfaces to the user** — Only what survived the full loop

## The Seven Lenses

| Role | Accountability Domain |
|------|----------------------|
| CEO | Organizational credibility, strategic narrative |
| CFO | Financial accuracy, forward-looking statement liability |
| General Counsel | Legal exposure, discoverable language, regulatory triggers |
| CHRO | Employee relations, discrimination risk, employer brand |
| CRO | Customer and commercial impact, pipeline risk |
| CMO | Brand integrity, message consistency, press coverage |
| CISO | Information security, sensitive disclosure, PII |

Plus an **Intent Guardian** — holds the user's original document purpose and ensures revisions haven't drifted from the goal.

## What You See

A lightweight status stream while the loop runs:

```
CFO | forward-looking statement exposure | flagged
CLO | discovery-ready language | revising
CHRO | employer brand tone | resolved
CEO | strategic narrative alignment | cleared
```

Not prose. Not a decision request. Just signal that the system found something and is handling it.

## When It Escalates

Most concerns are execution problems — a word choice, a framing, a data point. The writing agent solves these without you.

Occasionally a concern is an intent problem: no revision can address it without changing what the document fundamentally says. In that case, a panel flag surfaces:

```
PANEL FLAG — [Role]
Concern: [What can't be cleared, and why]
Decision required: [What you need to decide]
```

This case is rare. Most documents make it through without a flag.

## Installation

### Claude Code (CLI)
```bash
cp -r adversarial-panel ~/.claude/skills/
```

Or via symbolic link:
```bash
ln -s /path/to/adversarial-panel ~/.claude/skills/adversarial-panel
```

## File Structure

```
adversarial-panel/
├── SKILL.md                        # Main skill — workflow and loop mechanics
└── references/
    └── executive-lenses.md         # Seven roles: accountability domains, loss functions, failure modes
```

## License

MIT License. See [LICENSE](LICENSE).
