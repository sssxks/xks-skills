![rust-dev](rust-dev/assets/icon.svg)

Human-crafted skills for Type-driven design in rust.

## Install (Codex)

codex → skill-installer → Paste following prompt

```
Install skill from https://github.com/sssxks/xks-skills/tree/main/rust-dev
```

Manual Install: drop the skill directory into `~/.codex/skills/`

## Usage

0. to ensure agents use this skill, mention `use rust-dev skill` in your project AGENTS.md 
1. do everything as usual, e.g. init project, develop a feature, fix, refactor.
2. if agent sometimes fail to follow it, you can mention the skill again explicitly.

## How does it work & Limitations

This skills is purely empirical and "code taste" instructions, no tools and scripts. 

not strictly ablated, but improvements are certainly observable.

Breakdown:

Part 1:

- Parse, don’t validate: pith of type-driven design
- Push error handling down to boundary layers: combat codex defensive behavior, corollary of parse don't validate
- Minimize visibility: protect invariants, corollary of parse don't validate
- prefer `Box<[T]>`: combat common overuse of Vec and String, [reference](https://youtu.be/A4cKi7PTJSs?si=SYOQX0a3gLWqdC7S)

Part2:

- Initialize a Repo and Implement v1: crate layout and type design from day one, combat chatbot "self-contained demo" behavior
- Implement a Feature: restate requirements helps better human-machine collaboration
- Refactor Existing Code: combat codex's preference of compatibility over completeness. experimenting helps reduce risk.
