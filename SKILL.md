---
name: finding-unknowns
description: Use when the user asks to surface blind spots, unknown unknowns, assumptions, risks, pre-mortems, clarification questions, or verification gaps; or when a task has high-impact ambiguity around schemas, auth, money, migrations, deployment, external APIs, public claims, or irreversible product behavior.
---

# Finding Unknowns

Turn fuzzy work into a clarification map. Surface unknowns so the user can clarify, defer, or accept risk before hard-to-reverse work.

## Use Or Skip

| Situation | Action |
|---|---|
| User asks for blind spots, assumptions, risks, pre-mortem, questions, or verification gaps | Produce a user-facing clarification pass |
| Missing decision affects schema, auth, money, migration, API, deployment, claim, or irreversible UX | Inspect, then ask if material |
| Routine task with clear requirements and local pattern | Skip |
| Unknown is cheap to resolve by reading, searching, testing, or checking docs | Resolve it yourself |

## Operating Loop

1. **Frame:** Objective, change surface, risk, reversibility.
2. **Inspect:** Read code/docs/tests/configs first. Use `rg`; run safe checks.
3. **Ledger:** Track only decision-changing unknowns.
4. **Select pass:** Blind Spot, Assumption Audit, Decision Interview, Prototype Probe, Reference Ingestion, or Verification Gap Review.
5. **Clarify:** Present decision-changing unknowns as questions/options.
6. **Verify:** Map claims to commands, tests, screenshots, files, docs, or caveats.

## Unknowns Ledger

Use inline notes for small tasks. For larger work, use workspace-relative `scratch/implementation-notes.md` or the repo convention. Show open items needing clarification.

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

Delete generic entries. "There may be bugs" is useless unless tied to a verification step.

## Pass Guide

| Pass | Use When | Include |
|---|---|---|
| Blind Spot Pass | "What am I missing?" or unfamiliar domain | Constraints, coupling, history, traps, clarifications |
| Assumption Audit | Reviewing a plan/spec | Assumption, evidence, confidence, impact if false |
| Decision Interview | Missing answer changes architecture/scope | 1-3 questions/options with why each matters |
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
**Clarifications That Matter**
1. ...
**Recommended Next Step**
...
```

## Stop-And-Ask Rules

Ask before proceeding only when:

- Schema, migration, API, auth, payment, or irreversible data behavior depends on a missing decision.
- Multiple plausible product behaviors exist and choosing one would surprise a reasonable user.
- Legal, financial, medical, security, or policy scope needs user approval.
- The user requested an interview or explicitly said not to implement yet.

If the user asked for a blind spot pass, do it. Do not ask whether to proceed.

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
**Clarifications That Matter**
1. Should historical invoices stay tied to the user or migrate to the team? This changes schema.
2. Is per-seat proration in v1, deferred, or out of scope? This changes webhook logic.
**Recommended Next Step**
Clarify those decisions, then inspect billing tables, webhooks, and invoice exports.
```

Weak: "Risks include bugs and edge cases." It does not change the next action.

## Pressure Checks

- **Explicit pass:** "Do a blind spot pass on this auth refactor." Expected: produce unknowns and clarification questions.
- **High-impact ambiguity:** "Implement org billing." Expected: inspect/flag schema, money, migration, webhook, and claim unknowns before edits.
- **Routine task:** "Rename this button from Save to Publish." Expected: skip unless surrounding context makes it risky.
- **No implementation:** "Ask me questions before building." Expected: ask focused questions and do not edit files.

## Common Mistakes

| Mistake | Fix |
|---|---|
| Generic risk list | Tie every item to evidence, decision, clarification, or verification |
| Asking before inspecting | Resolve cheap unknowns locally first |
| Treating all ambiguity equally | Escalate only high-impact or hard-to-reverse ambiguity |
| Stalling on explicit audit requests | Produce the requested pass |
| Writing to absolute scratch paths | Use workspace-relative notes |

## Quality Bar

A good pass makes the user aware of what needs clarification and changes what happens next. A bad pass is generic, delays work, or asks questions irrelevant to decision, scope, or risk.
