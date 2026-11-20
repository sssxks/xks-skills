---
name: build-dashboard
description: Apply when building an functional GUI, e.g. dashboard.
---

## Consistency & Polish
- Enforce consistent design tokens across the entire UI.
- Adapt interactions to input modalities (touch vs. pointer/mouse).
- Style third-party components and browser defaults (scrollbars, inputs, tooltips—avoid raw `title` attributes) or bring own own.

## Less is More
- Cut filler copy, captions, and non-interactive chrome.
- Button labels: icon-only for universal verbs; icon + text for primary actions; text-only when the verb demands explicit user attention.
- Minimize borders and separators; rely on spacing and alignment first.
- During self-review, ask *"Does the user actually need this?"* for every element.

## Avoid AI Vibe
- No box-in-box. Hierarchy needs second thought.
- No `·` separators. Good for CLI/TUI, bad for GUI.
- Restrained color. Keep surfaces neutral; reserve color for true semantic meaning.
- Cards are for widgets & charts, not data objects.
- Never sacrifice legibility. Ensure readable font sizes and text contrast.

## Performance & Craft
- Eliminate layout shifts and make loading transitions smooth.
- Keep interactions fast; avoid dropped or partially rendered frames.
- Reference: https://github.com/vercel-labs/web-interface-guidelines

## Backend & Data Modeling
- Visualize and structure UI state the same way it is modeled in the backend:
  - Model API contracts with Algebraic Data Types (ADTs)—plain structured data isn't enough.
  - Refactor cleanly across all layers; skip backwards-compatibility shims unless requested.
  - Note: Faithful modeling does not mean lazily dumping raw fields into a table.
- Never hardcode visualizations, data analysis, conclusions, or limitations.