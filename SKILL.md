---
name: adversarial-panel
description: >
  Embeds an adversarial C-suite review panel into the writing process for any formal, discoverable corporate document.
  The panel is not a post-production step — it is the writing process. Trigger automatically and without prompting
  whenever the user is creating or drafting any of the following: board memos, board presentations, shareholder letters,
  earnings communications, press releases, workforce announcements (reductions, restructuring, layoffs), executive speeches,
  regulatory filings, investor updates, M&A communications, crisis communications, incident disclosures, data breach
  notifications, product recall notices, policy announcements, public statements, or any document that could be subpoenaed,
  published, or cited. Trigger on intent — if the user is writing something that could be read by a board, regulator,
  investor, journalist, attorney, or employee beyond the immediate recipient, this skill applies. Do not wait for the
  user to ask for a review. The panel runs as part of producing the document, not after.
---

# Adversarial C-Suite Review Panel

## What This Skill Does to This Conversation

When this skill triggers, you — this conversation — own the full document loop. You draft. You orchestrate the panel. You revise. You output. The user does not see drafts, does not make decisions during the loop, and does not interact with the panel. They provide their intent at the start and receive the final document at the end.

The seven executive lens agents and the Intent Guardian run as independent Agent tool calls — isolated invocations with no shared context between them. This is what makes the panel genuinely adversarial: each lens reads the document cold, without knowing what any other lens thought.

---

## Session Start

Before writing anything:

**1. Capture intent precisely.** Establish and hold the user's original document purpose. Not "write a press release" — the specific thing this document must accomplish: what it communicates, to whom, with what effect, with what constraints. This is the Intent Guardian's reference for the entire session. It does not change regardless of what the panel flags or how many revision cycles run.

**2. Load profiles.** Check `profiles/custom/` first. For any role with a custom profile present, use it. For roles without one, use `profiles/default/`. You will pass the appropriate profile to each lens agent call.

**3. Draft the document.** Produce a complete, sendable document — not an outline, not a skeleton. If it passed the panel, the user could send it. Write to that standard from the first draft.

---

## The Panel Loop

After each draft, invoke all seven lens agents as independent Agent tool calls. Pass each agent:
- The contents of its agent file (`agents/[role]-lens.md`)
- The contents of its profile (`profiles/custom/[role].md` or `profiles/default/[role].md`)
- The current draft in full

Each agent returns: `CLEARED` or `FLAGGED` with a specific concern, the exact triggering language, and a resolution type (`EXECUTION` or `ESCALATE`).

**If any lens returns FLAGGED with EXECUTION:**
Revise the document to address all flagged concerns. Rules for revision:
- Reframe, reword, or add — do not remove substantive content to satisfy a lens
- If a concern cannot be addressed without removing substance, that is an ESCALATE, not an EXECUTION fix
- Do not introduce new problems while solving existing ones
- Run the panel again on the revised draft

**If any lens returns FLAGGED with ESCALATE:**
Surface a panel flag to the user (format below). Wait for their decision. Resume the loop with their answer in context.

**If all seven lenses return CLEARED:**
Proceed to the Intent Guardian.

---

## Intent Guardian

After the panel clears, invoke the Intent Guardian as an independent Agent tool call. Pass it:
- The contents of `agents/intent-guardian.md`
- The user's original intent statement (captured at session start)
- The final panel-cleared draft

**If the Intent Guardian returns CONFIRMED:** Output the document to the user.

**If the Intent Guardian returns DRIFT DETECTED:** Take one more pass — restore what drifted without reopening resolved panel concerns. Run the full panel again on the restored draft.

---

## Status Display

While the loop runs, display a lightweight status stream. Not prose. Not explanations. Just signal.

```
[Role] | [issue in three to five words] | [status]
```

Status values: `reviewing` · `flagged` · `revising` · `resolved` · `cleared`

Example:
```
CFO          | forward-looking statement        | flagged
GC           | discovery-safe language          | revising
CHRO         | employer brand tone              | resolved
CEO          | narrative consistency            | cleared
CMO          | headline risk in paragraph two   | flagged
Intent Guardian | purpose alignment check       | reviewing
```

New line per role per cycle. The labels are the communication. Do not explain the process.

---

## Panel Flag Format

Escalate to the user only when a concern requires a decision only the user can make — not a legal judgment call, not a stylistic choice, not anything you can resolve by choosing the more defensible option. When genuine escalation is required:

```
PANEL FLAG — [Role]
Concern: [What the executive cannot clear and why — specific]
Decision required: [The one thing the user must decide — not options, one decision]
```

Do not propose solutions. Present the tension. The user decides. The loop resumes.

---

## Loop Boundaries

- Maximum five revision cycles. Unresolved concerns after five cycles become panel flags.
- Each lens clears independently. A cleared lens does not re-run unless your revision touches their domain.
- Intent Guardian runs only after the panel clears.
- Do not skip lenses. All seven run every cycle unless already cleared.

---

## Revision Standard

The panel is not your adversary. Each lens is telling you how a specific reader in a specific role will react to specific language. Your job is to write the document so that reader does not have that reaction — not by removing what they would react to, but by writing it better.

A GC concern about admission language is not asking you to remove accountability. It is asking you to express accountability without creating legal exposure. Both are possible.

A CFO concern about an unhedged projection is not asking you to be vague. It is asking you to express confidence in a legally defensible way. Both are possible.

A CHRO concern about cold tone is not asking you to be maudlin. It is asking you to be specific and genuine rather than corporate and formulaic. Both are possible.

When in doubt: write the language that the most adversarial possible reader in each role would find least objectionable.

---

## Reference Files

- `agents/ceo-lens.md` through `agents/ciso-lens.md` — pass to each Agent tool call for panel review
- `agents/intent-guardian.md` — pass to Agent tool call after panel clears
- `profiles/default/` — default archetype profiles for each role
- `profiles/custom/` — user-provided profiles; check here first
- `profiles/README.md` — profile format and customization instructions
