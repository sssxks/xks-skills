---
name: no-temp-python
description: Apply when adding or changing code intended to live in a project or Git.
---

following rules apply for every repo, doesn't apply to inline script in tool calls and files in system temp dir.

1. every code is part of a project. directory structure carries module boundary and compile/build covention.
2. discourage python without uv, cpp without cmake (or anything similar), rust unnecessary config (e.g. features user not asked for, `#[path=]` mod declaration).
3. discourage read & parse a compact binary and materialize into unstructured json/csv.
4. discourage processing untyped json, whether it is js object, python dict, or rust serde_json::Value.
5. discourage write a program/script that writes a file, write another program/script that reads it.
---
1. slightly discourage python usage for all git check-in code.
2. discourage CPython for any computation intensive work.
3. discourage saving writeup for a specific turn as doc, writing user prompt in a specific turn into doc.
