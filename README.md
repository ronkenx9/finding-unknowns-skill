# Finding Unknowns Skill

Finding Unknowns is a Codex skill for surfacing blind spots so the user can clarify them before hard-to-reverse work. It helps an agent separate cheap-to-resolve uncertainty from the few unknowns that can change architecture, invalidate results, or create expensive rework.

Use it when a user asks for a blind spot pass, unknown unknowns, assumption audit, risk review, pre-mortem, decision interview, clarification questions, or verification gap review. It is also useful when a task has high-impact ambiguity around schemas, authentication, money, migrations, deployment, external APIs, public claims, or irreversible product behavior.

## What It Teaches

The skill gives agents a compact user-facing loop:

1. Frame the objective, change surface, cost of being wrong, and reversibility.
2. Inspect local code, docs, tests, configs, and notes before asking questions.
3. Track only decision-changing unknowns.
4. Choose the smallest pass that fits the request.
5. Present decision-changing unknowns as concrete questions or options the user can clarify.
6. Map important claims to verification evidence.

It is deliberately not a generic "ask more questions" skill. The quality bar is whether the pass makes the user newly aware of what needs clarification and changes what happens next.

## Included Files

```text
finding-unknowns-skill/
├── SKILL.md
└── agents/
    └── openai.yaml
```

- `SKILL.md` is the skill body and required YAML frontmatter.
- `agents/openai.yaml` provides Codex UI metadata and a default prompt.

## Installation

Install as a global Codex skill:

```bash
mkdir -p ~/.codex/skills/finding-unknowns
cp -R SKILL.md agents ~/.codex/skills/finding-unknowns/
```

Or clone directly into your Codex skills directory:

```bash
git clone https://github.com/ronkenx9/finding-unknowns-skill.git ~/.codex/skills/finding-unknowns
```

After installation, start a new Codex session so the skill metadata is discovered.

## Example Prompts

```text
Use $finding-unknowns to surface the unknowns in this auth refactor so I can clarify them.
```

```text
Before we add team billing, find the unknown unknowns.
```

```text
Use $finding-unknowns to audit the assumptions in this migration plan.
```

```text
Ask me the questions that matter before we build this.
```

## Expected Output Shape

For an explicit blind spot pass, a good answer should include:

- Objective
- Known ground
- Likely hidden constraints
- Assumptions to verify
- Clarifications that matter
- Recommended next step

It should not produce a vague list like "there may be bugs and edge cases." Every item should connect to evidence, a decision, a clarification, or a verification step.

## Validation

If you have the Codex skill creator validator available, run:

```bash
python /path/to/skill-creator/scripts/quick_validate.py /path/to/finding-unknowns-skill
```

The current package has been validated for:

- Required `SKILL.md` frontmatter.
- Valid `name` and `description`.
- Portable `agents/openai.yaml`.
- No local machine paths in the skill package.
- A compact skill body with pressure checks and common mistakes.

## Design Principles

- Make the user aware of unknowns they can clarify, defer, or accept as risk.
- Prefer evidence over speculation.
- Resolve cheap unknowns locally before asking the user.
- Escalate only high-impact or hard-to-reverse ambiguity.
- Do not stall when the user explicitly requests an audit pass; produce the pass.
- Use workspace-relative notes if a task needs an unknowns ledger.

## Repository Scope

This repository contains only the portable Codex skill. It intentionally excludes private brain notes, project journals, local validation environments, and machine-specific paths.

## Contributing

When changing the skill:

1. Keep `SKILL.md` concise enough to load frequently.
2. Add or update pressure checks for any new behavior.
3. Validate the skill metadata before pushing.
4. Avoid adding extra docs unless they directly help an agent use the skill.

## License

No license has been declared yet. Treat the contents as all rights reserved unless a license is added later.
