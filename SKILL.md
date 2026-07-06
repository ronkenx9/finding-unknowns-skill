---
name: finding-unknowns
description: Use when the user explicitly asks for a blind spot pass, unknown unknowns, assumption audit, risk review, pre-mortem, decision interview, or verification gap review; or when a task has high-impact ambiguity around schemas, auth, money, migrations, deployment, external APIs, public claims, or irreversible product behavior.
---

# Finding Unknowns

Turn fuzzy work into a known-risk plan. Surface only unknowns that can change implementation or cause expensive rework.

## Use Or Skip

| Situation | Action |
|---|---|
| User asks for blind spots, assumptions, risks, pre-mortem, or verification gaps | Produce the requested pass |
| Missing decision affects schema, auth, money, migration, API, deployment, claim, or irreversible UX | Inspect first, then ask if material |
| Routine task with clear requirements and local pattern | Skip |
| Unknown is cheap to resolve by reading, searching, testing, or checking docs | Resolve it yourself |

## Operating Loop

1. **Frame:** Objective, change surface, cost of being wrong, reversibility.
2. **Inspect:** Read code/docs/brain notes/tests/configs first. Use `rg`; run safe behavior checks.
3. **Ledger:** Track only decision-changing unknowns.
4. **Select pass:** Blind Spot, Assumption Audit, Decision Interview, Prototype Probe, Reference Ingestion, or Verification Gap Review.
5. **Gate:** Ask only when evidence cannot answer a material hard-to-reverse decision.
6. **Verify:** Map claims to commands, tests, screenshots, files, docs, or caveats.

## Unknowns Ledger

Use inline notes for small tasks. For larger work, use workspace-relative `scratch/implementation-notes.md` or the repo's notes convention.

```markdown
- [resolved] Unknown: ...
  Evidence: ...
  Decision: ...
- [open] Unknown: ...
  Why it matters: ...
  How to resolve: ...
- [accepted-risk] Unknown: ...
  Assumption: ...
  Fallback: ...
```

Delete generic entries. "There may be bugs" is not useful unless tied to a concrete verification step.

## Pass Guide

| Pass | Use When | Include |
|---|---|---|
| Blind Spot Pass | "What am I missing?" or unfamiliar module/domain | Constraints, coupling, history, traps |
| Assumption Audit | Reviewing a plan/spec | Assumption, evidence, confidence, impact if false |
| Decision Interview | Missing answer changes architecture/scope | 1-3 questions with why each matters |
| Prototype Probe | UX/visual/copy preference is unclear | Small alternatives before production edits |
| Reference Ingestion | Target behavior has precedent | Reference and semantics to preserve |
| Verification Gap Review | Before finalizing claims | Claim-to-evidence map and caveats |

## Blind Spot Format

```markdown
**Objective**
...
**Known Ground**
- ...
**Likely Hidden Constraints**
- ...
**Assumptions To Verify**
- ...
**Questions That Matter**
1. ...
**Recommended Next Step**
...
```

## Stop-And-Ask Rules

Ask before proceeding only when:

- Schema, migration, public API, auth, payment, or irreversible data behavior depends on a missing decision.
- Multiple plausible product behaviors exist and choosing one would surprise a reasonable user.
- Legal, financial, medical, security, or public-policy scope needs user approval.
- The user requested an interview or explicitly said not to implement yet.

If the user asked for a blind spot pass, do it. Do not ask whether to do it.

## Example

User: "Before we add team billing, find the unknown unknowns."

Good shape:

```markdown
**Objective**
Add team billing without breaking invoices.
**Likely Hidden Constraints**
- Invoices are legal records; do not rewrite ownership casually.
- Webhooks may assume one subscription owner.
- Tax/export may assume one buyer per subscription.
**Questions That Matter**
1. Do historical invoices stay tied to the user or migrate to the team? This changes schema and migration.
2. Is per-seat proration in v1? This changes webhook logic.
**Recommended Next Step**
Inspect billing tables, webhooks, and invoice exports before proposing migration.
```

Weak: "Risks include bugs and edge cases." It does not change the next action.

## Pressure Checks

- **Explicit pass:** "Do a blind spot pass on this auth refactor." Expected: produce the pass, not a clarifying question first.
- **High-impact ambiguity:** "Implement org billing." Expected: inspect/flag schema, money, migration, webhook, and claim unknowns before edits.
- **Routine task:** "Rename this button from Save to Publish." Expected: skip unless surrounding context makes it risky.
- **No implementation:** "Ask me questions before building." Expected: ask focused questions and do not edit files.

## Common Mistakes

| Mistake | Fix |
|---|---|
| Generic risk list | Tie every item to evidence, a decision, or verification |
| Asking before inspecting | Resolve cheap unknowns locally first |
| Treating all ambiguity equally | Escalate only high-impact or hard-to-reverse ambiguity |
| Stalling on explicit audit requests | Produce the requested pass |
| Writing to absolute scratch paths | Use workspace-relative notes |

## Quality Bar

A good pass changes what happens next. A bad pass is generic, delays work, or asks questions whose answers would not affect implementation.
