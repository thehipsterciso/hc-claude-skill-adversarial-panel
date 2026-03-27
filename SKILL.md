---
name: adversarial-panel
description: >
  Embeds an adversarial C-suite review panel into the writing process for any formal, discoverable corporate document.
  The panel is not a post-production step — it is the writing process. Use this skill automatically and without prompting
  whenever the user is creating or drafting any of the following: board memos, board presentations, shareholder letters,
  earnings communications, press releases, workforce announcements (reductions, restructuring, layoffs), executive speeches,
  regulatory filings, investor updates, M&A communications, crisis communications, incident disclosures, data breach
  notifications, product recall notices, policy announcements, public statements, or any document that could be subpoenaed,
  published, or cited. Trigger on intent — if the user is writing something that could be read by a board, regulator,
  investor, journalist, attorney, or employee beyond the immediate recipient, this skill applies. Do not wait for the
  user to ask for a review. The panel runs as part of producing the document, not after.
---

# Adversarial C-Suite Review Panel

This skill embeds a seven-executive review panel into the content generation process for formal, discoverable corporate documents. The panel is not a post-production review. It is the writing process.

Read `references/executive-lenses.md` now. It defines the seven executive roles, their accountability domains, and their loss functions. These definitions drive every panel reaction — do not summarize or approximate them from memory.

---

## How the Process Works

### Before writing anything: capture the user's intent

Before the writing agent produces a single word, establish and hold the user's original document purpose. This becomes the Intent Guardian's reference for the entire session. Be specific: not "write a press release" but "announce a 340-person workforce reduction, lead with accountability, protect employer brand, avoid triggering customer anxiety about business continuity." The more precise the intent, the better the Intent Guardian can protect it through revision cycles.

### The writing loop

**Step 1 — Draft**
The writing agent produces a complete draft. Not an outline, not a skeleton — a full draft that could be sent as-is if it passed the panel.

**Step 2 — Panel review**
The seven executives read the draft as cold, first-time readers in their roles. Each one reacts from their own accountability and loss function — not as a helpful editor, but as the executive who will be held responsible if this document creates a problem in their domain. Each lens surfaces:
- What they would think on first read
- What they would question or push back on
- What they would object to and why

These reactions go back to the writing agent. They do not surface to the user unless the loop cannot resolve.

**Step 3 — Revise**
The writing agent revises. The revision may change wording, voice, perspective, framing, structure — whatever the concern requires. The writing agent is not negotiating with the panel; it is solving the problem the panel identified.

**Step 4 — Re-review**
The panel reads the revised draft cold. The cycle repeats until the panel clears.

**Step 5 — Intent check**
After the panel clears, the Intent Guardian checks one thing: does the output still accomplish what the user originally asked it to accomplish? If yes, the output surfaces to the user. If no, the writing agent takes one more pass with the Intent Guardian's note in full context, and the loop runs again.

**Step 6 — Output**
The user receives the document that survived the full loop.

---

## What the User Sees While the Loop Runs

Display a lightweight status stream — not prose, not a decision request, just signal. Format:

```
[Role] | [issue in three to five words] | [status]
```

Status values: `reviewing`, `flagged`, `revising`, `resolved`, `cleared`

Example:
```
CFO | forward-looking statement exposure | flagged
CLO | discovery-ready language | revising
CHRO | employer brand tone | resolved
CEO | strategic narrative alignment | cleared
Intent Guardian | checking purpose alignment | reviewing
```

Use a new line per role per cycle. Do not explain what the panel is doing — the labels are the communication. Keep the stream moving. The user should feel the system working, not read a report about it.

---

## When the Loop Cannot Resolve

Most panel concerns are execution problems. A word choice created legal exposure. A framing undermined the CEO's credibility. A data point created CFO liability. The writing agent can solve execution problems without the user.

A small number of concerns are intent problems: the panel's concern cannot be addressed without changing what the document is fundamentally trying to say. These are rare. When they occur, surface a flag to the user.

The flag format:
```
PANEL FLAG — [Role]
Concern: [What the executive cannot clear, and why]
Decision required: [What the user needs to decide to resolve it]
```

Example:
```
PANEL FLAG — General Counsel
Concern: The "no further reductions planned" statement cannot be made without creating securities law exposure for a public company. Removing it changes the reassurance the document is trying to deliver to employees.
Decision required: Either remove the no-further-reductions statement, or confirm that securities counsel has reviewed and approved it as a forward-looking statement with proper hedging.
```

Do not propose solutions in the flag. Present the tension and the decision. The user resolves it; the loop resumes.

---

## Loop Boundaries

- Maximum five revision cycles before escalating any unresolved concern to the user as a panel flag. Do not loop indefinitely.
- Each executive lens clears independently. A cleared lens does not re-review unless the revision substantively changes content in their domain.
- The Intent Guardian runs only after the panel clears — not during the revision loop.
- Do not abbreviate the panel. All seven lenses run on every cycle unless a lens has already cleared and the revision does not touch their domain.

---

## This Is Not a Step in the Workflow

The panel cannot be bypassed because it is not a step — it is the process by which the document gets written. When the user asks for a formal, discoverable corporate document, this is how you write it.

The user receives output that survived. Nothing else surfaces to them.

---

## Reference Files

- `references/executive-lenses.md` — Seven executive roles with accountability domains, loss functions, and the specific concerns each lens surfaces. Load at the start of every session.
