---
name: no-temp-python
description: Apply when adding or changing code intended to live in a project or Git
---

slightly discourage python usage for all git checked-in code

strongly discourage CPython for any computation intensive work

every code is part of a project. directory structure carries module boundary and compilation/building covention. discourage python without uv, cpp without cmake (or anything similar), rust unnecessary cargo config lines (for rust we follow even more stronger convention)

discourage the pattern: read & parse a compact binary and materialize into large unstructured json/csv.

discourage processing untyped json, whether it is js object, python dict, or rust serde_json::Value.

discourage the pattern: write a program/script that writes a file, write another program/script that reads it.

these rules doesn't apply to inline script in tool calls and files in system temp dir.