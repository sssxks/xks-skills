---
name: rust-dev
description: Rust best practices and workflows. Make sure to use this skill whenever working on Rust projects.
---

# Rust Development Skill

## Coding Style

- **Parse, don’t validate.** Prefer encoding constraints into types, rather than repeating validations along business paths.
  - **Core business code & reusable libraries:** call sites must not use `panic!/expect/unwrap/unsafe`. `unsafe` is allowed *inside* types/modules that maintain invariants, but add `debug_assert!` and safety comments.
  - **Tests & entrypoints:** `expect/unwrap` are allowed.
  - **“Unreachable” cases that the type system can’t express:** (and thus cannot be pushed into type internals) prefer normal error handling; avoid using `unreachable!` that expands the panic surface.

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

- Things to think about from day one: system boundaries, the end-to-end happy path, the error model, and quality gates.
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

- Investigate the baseline and understand current behavior. Prefer refactoring without changing externally observable behavior.
- Restate and confirm whether existing business semantics are still necessary. If requirements are obsolete, behavior changes may be introduced.
- Ensure refactors are thorough: check bypass paths, delete dead code, and keep the codebase clean.
- For large refactors, create an experimental crate to simulate business logic and validate feasibility to reduce risk.
  - To make reviewing experimental crate easier, comment heavily.