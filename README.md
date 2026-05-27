![rust-dev](skills/rust-dev/assets/icon.svg)

Human-crafted skills for coding in rust.

## Install (vercel `skills`)

```bash
# List available skills
npx skills add sssxks/xks-skills --list
# Install the skill
npx skills add sssxks/xks-skills
# Update all global skills
npx skills update -g -y
```

## Install (Codex)

codex → skill-installer → Paste following prompt

```
Install skill from https://github.com/sssxks/xks-skills/
```

Manual Install: drop the skill directory into `~/.codex/skills/`

## Usage

0. to ensure agents use this skill, mention `use rust-dev skill` in your project AGENTS.md 
1. do everything as usual, e.g. init project, develop a feature, fix, refactor.
2. if agent sometimes fail to follow it, you can mention the skill again explicitly.
