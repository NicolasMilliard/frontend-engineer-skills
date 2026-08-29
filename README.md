# Frontend Engineer Skills

Opinionated AI skills for frontend engineers. The repository favors a small
number of skills that encode real engineering workflows and judgment over a
large collection of generic prompts.

## Skills

### `frontend-code-review`

Reviews bounded React, Next.js, and frontend TypeScript changes with an
experienced PR-review mindset. It prioritizes correctness and material risk,
respects the existing architecture, and deliberately avoids low-value comment
volume.

The first version lives in
[`skills/frontend-code-review`](skills/frontend-code-review).

### `linkedin-writing-partner`

Helps software engineers turn real technical experience into credible LinkedIn
posts. It lives in
[`skills/linkedin-writing-partner`](skills/linkedin-writing-partner).

## Design principles

- Capture real engineering workflows and the judgment behind them.
- Prefer evidence-backed findings over exhaustive checklists.
- Keep `SKILL.md` focused and load detailed references only when relevant.
- Add guidance in response to observed failures, not imagined completeness.
- Evaluate semantic behavior and false positives instead of exact wording.

## Repository structure

```text
skills/
  frontend-code-review/
    SKILL.md
    references/
  linkedin-writing-partner/
    SKILL.md
    references/
evals/
  frontend-code-review/
```

## Installation

List the available skills:

```sh
npx skills add https://github.com/NicolasMilliard/frontend-engineer-skills --list
````

Install a specific skill:
```sh
npx skills add https://github.com/NicolasMilliard/frontend-engineer-skills --skill frontend-code-review
npx skills add https://github.com/NicolasMilliard/frontend-engineer-skills --skill linkedin-writing-partner
```
