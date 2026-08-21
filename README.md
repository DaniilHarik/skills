# Skills

Personal agent skills for Codex and other tools that support the open agent skills format.

## Available Skills

- `climb-abstraction-ladder` - Turn concrete engineering signals into operational mechanisms and strategic choices, then walk strategy back down into evidence.
- `create-mental-model` - Build a reusable mental model from concrete domain relationships and explain its limits.
- `go-code-review` - Review Go codebases for correctness, idiomatic design, maintainability, and concurrency risks.
- `improve-my-goal` - Coach users through turning draft individual goals into meaningful, outcome-focused goals.

## Install

```sh
npx skills@latest add DaniilHarik/skills/climb-abstraction-ladder
npx skills@latest add DaniilHarik/skills/create-mental-model
npx skills@latest add DaniilHarik/skills/go-code-review
npx skills@latest add DaniilHarik/skills/improve-my-goal
```

## Repository Layout

Each skill lives in its own directory and includes a `SKILL.md` file:

```text
climb-abstraction-ladder/
  SKILL.md
  agents/
  references/

create-mental-model/
  SKILL.md

go-code-review/
  SKILL.md
  agents/
  references/

improve-my-goal/
  SKILL.md
  agents/
  references/
```
