# Skills

Personal agent skills for Codex and other tools that support the open agent skills format.

## Available Skills

- `climb-abstraction-ladder` - Turn concrete engineering signals into operational mechanisms and strategic choices, then walk strategy back down into evidence.
- `go-code-review` - Review Go codebases for correctness, idiomatic design, maintainability, and concurrency risks.

## Install

```sh
npx skills@latest add DaniilHarik/skills/climb-abstraction-ladder
npx skills@latest add DaniilHarik/skills/go-code-review
```

## Repository Layout

Each skill lives in its own directory and includes a `SKILL.md` file:

```text
climb-abstraction-ladder/
  SKILL.md
  agents/
  references/

go-code-review/
  SKILL.md
  agents/
  references/
```
