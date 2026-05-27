---
name: rust-dev
description: Rust best practices and workflows. Make sure to use this skill whenever working on Rust projects.
---

# Rust Development Skill

## Coding Style

- **Parse, don’t validate.** Prefer encoding constraints into types, rather than repeating validations along business paths.
  - **Core business code & reusable libraries:** call sites must not use `panic!/expect/unwrap/unsafe`. `unsafe` is allowed *inside* types/modules that maintain invariants, but add `debug_assert!` and safety comments.
  - **Tests & entrypoints:** `expect/unwrap` are allowed.

- **Push error handling down to boundary layers** (I/O, model, and other edges). Keep the business layer focused on business actions and their meaningful failure modes.
  - Avoid defensive branching and meaningless fallbacks in the business path; use the type system to eliminate impossible states.
  - Remove dead code and no-ops on critical paths. Any remaining branch must have real semantics or a verifiable future purpose.

- **Minimize visibility.** If invariants rely on private fields or private methods, expose access only to the minimum necessary scope. A rule of thumb: if a type is a *proper subset* of the Cartesian product of its fields, those fields should remain private. Examples:
  - A `User` requires at least one of `email` or `phone`, so fields should be private and validated in setters/constructors.
  - If an invariant concerns only a single field, wrap that field in a newtype instead of making the whole struct private. Example: `Email(Box<str>)` rather than `User { email: Box<str>, ... }`.
  - For logic-bearing types like `Service/Manager/Controller/...`, fields should almost always be private.
  - Exception: DTOs and other “named tuple” types may keep fields public.

Micro-optimization tips:

- In type definitions, prefer `Box<[T]>` or `Rc<[T]>` by default; introduce `Vec<T>` or `Arc<[T]>` only when needed. Same for strings: prefer `Box<str>` / `Rc<str>`, and use `String` / `Arc<str>` only when necessary.

## Development Workflow

### 1. Initialize a Repo and Implement v1

- Things to think about from day one: system boundaries, end-to-end happy path, error model.
- Prefer splitting crates early. Minimal example: each crate contains a `lib.rs` with a few dozen to a hundred lines. Split into modules further if needed.
- Design types, and draft definitions in pseudocode. Requirements:
  - Use newtypes for (almost) everything. Examples: `UserId(u32)`, `Email(Box<str>)`.
  - Apply the visibility rules in **Coding Style** carefully.
  - Design efficient memory layout for hot types, but prioritize semantics; don’t optimize prematurely.
- Deliverables: a requirements doc (with use cases and test cases), a project skeleton, and a type system.

### 2. Implement a Feature

- Investigate the baseline and understand current behavior. (You may run code, add temporary logs, etc.)
- Restate the requirement to avoid rework. Express it as a precise “behavior diff” whenever possible.
- Don’t break layering. Put new code in the correct module.

### 3. Refactor Existing Code

#### Principles

- Refactoring is about making code easier to reason about while, by default, preserving externally observable behavior.
- If a behavior change is intentional, treat it as a requirement change and describe the behavior diff explicitly.

- Prefer refactors that handle complexity:
  - No configuration when convention is sufficient.
  - Make behavior more data-driven, declarative - e.g. tables, enums, policies - over scattered control flow.
  - If a macro argument may be evaluated lazily, conditionally, or more than once(i.e. most of the time), use closure syntax `|| ...` rather than a plain `expr`.
- Prefer types that expose valid state transitions over wide mutable surfaces and unconstrained setters.
- Avoid refactors that only split large functions or modules without reducing the invariants and preconditions each unit must rely on. This usually makes control-flow more scattered.

#### Steps
- Investigate the baseline and understand current behavior.
- Restate and confirm whether existing business semantics are still necessary. If requirements are obsolete, may change behavior.
- Ensure refactors are thorough: check bypass paths, delete dead code. Do not leave wrappers or re-export. If not otherwise stated, assume no external code dependencies on internal crates of a rust app(not a library).
- For large refactors, create an experimental crate.
  - To make reviewing experimental crate easier, comment heavily.
