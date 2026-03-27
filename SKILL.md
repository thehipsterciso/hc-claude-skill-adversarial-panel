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

## Architecture

Seven independent executive agents review every draft. Each agent runs as a separate, isolated call — no shared context between lens agents. A writing agent orchestrates the loop. An Intent Guardian protects the user's original purpose through every revision cycle.

**Agents:**
- `agents/writing-agent.md` — Orchestrates the full loop. Drafts, revises, manages status, handles escalation.
- `agents/ceo-lens.md` — Organizational credibility and strategic narrative
- `agents/cfo-lens.md` — Financial accuracy and disclosure liability
- `agents/gc-lens.md` — Legal exposure and discoverable language
- `agents/chro-lens.md` — Employee relations and workforce risk
- `agents/cro-lens.md` — Customer and commercial impact
- `agents/cmo-lens.md` — Brand integrity and message consistency
- `agents/ciso-lens.md` — Information security and sensitive disclosure
- `agents/intent-guardian.md` — Intent drift detection after panel clears

**Profiles:**
- `profiles/default/` — Composite archetype profiles for each role
- `profiles/custom/` — User-provided profiles for real individuals; loaded in place of defaults when present
- `profiles/README.md` — Profile format and customization instructions

---

## Session Start

Before producing any content:

1. **Load profiles.** Check `profiles/custom/` first. For any role with a custom profile present, use it. For roles without a custom profile, use `profiles/default/`. Each lens agent receives its own profile.

2. **Capture intent.** Ask the user — or infer from context — the specific purpose of this document. Record it precisely. Not "write a press release" but "announce an 18% workforce reduction, lead with accountability, protect employer brand, keep customers from panicking about service continuity." This becomes the Intent Guardian's reference for the full session and does not change regardless of how the document evolves through revision.

3. **Begin the writing loop.** See `agents/writing-agent.md`.

---

## Profile System

Profiles define who is in each executive role for this document and this organization. They give lens agents personality, career history, specific hot-button concerns, and communication style — the difference between a generic CFO reaction and the reaction of a specific person with specific scars.

**To use custom profiles:**
- Place profile files in `profiles/custom/` using the format in `profiles/README.md`
- Or tell the skill at session start: "Use [Name] as the [role]" — the skill will prompt for or infer profile details and construct a temporary profile for the session
- Custom profiles replace defaults entirely for that role; they are not merged

See `profiles/README.md` for the full format and examples.

---

## Status Display

While the loop runs, display a lightweight status stream. Not prose. Not explanations. Just signal.

Format:
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

New line per role per cycle. Do not explain what the panel is doing. The labels are the communication.

---

## Escalation to the User

Most panel concerns are execution problems — a word choice, a framing, a missing disclaimer. The writing agent resolves these without user involvement.

Escalate to the user only when:
- A concern cannot be addressed without changing what the document is fundamentally trying to say, AND
- The writing agent cannot determine the right resolution without knowing the user's actual intent or a fact only the user has

Do not escalate legal judgment calls. Do not escalate stylistic disagreements between lenses. Do not escalate anything the writing agent can resolve by adding a safe harbor, reframing a sentence, or choosing the more defensible of two equivalent options.

When escalation is genuinely necessary:

```
PANEL FLAG — [Role]
Concern: [What the executive cannot clear and why — specific, not general]
Decision required: [The one thing the user needs to decide — not a legal opinion request, not a list of options, one decision]
```

The user decides. The loop resumes with their answer in context.

---

## Loop Boundaries

- Maximum five revision cycles. If a concern is unresolved after five cycles, escalate as a panel flag.
- Each lens agent clears independently. A cleared lens does not re-run unless the revision touches content in their domain.
- Intent Guardian runs after the panel clears — not during revision cycles.
- Do not skip lens agents to save time. All seven run every cycle unless already cleared.

---

## Output

The user receives the document that survived the full loop. Nothing else surfaces to them except the status stream and, in rare cases, a panel flag requiring their decision.

The panel cannot be bypassed. It is not a step in the workflow — it is how the document gets written.
