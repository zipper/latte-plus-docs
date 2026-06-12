---
layout: default
title: Known limitations
nav_order: 6
---

# Known limitations

Latte+ aims to match the real Latte engine as closely as possible, but a handful of
edge cases are either platform constraints or deliberately out of scope for now. This
list is kept honest on purpose — if you hit something not listed here, please
[report it](./support.html).

## Editing edge cases

- **Enter indentation inside injected JS/CSS** — pressing Enter inside a `<script>`
  using `n:syntax="double"` may not re-indent the JavaScript ideally. This is a
  platform injection constraint.
- **Inline rename for `{php}` variables** uses a dialog rather than the in-place
  editor, because the rename crosses the PHP↔Latte boundary.

## Parser / syntax scope

- **Top-level `{syntax double}`** and **nested `{syntax}` switching** are accepted by
  the Latte runtime in theory but are not specially handled; they essentially never
  occur in real templates.
- A **literal `{/syntax}` inside a JS regex** within `n:syntax="double"` scope is not
  recognized (not seen in practice).
- **`{iterateWhile}` with a condition on the closing tag** is not parsed.
- **`n:try` with `{rollback}`** is out of scope.
- **Member access inside a bare string** — `"$user->name"` resolves `$user` but not
  the `->name` part (this matches the behavior of other Latte plugins).

## Type inference & hints

- Some **type inlay hints are currently disabled** because of a mid-typing parser
  recovery edge case (e.g. while typing `name:<caret>`). The information is still
  available via completion and hover.

## Other

- **Shared partial templates with no UI owner** — a layout/partial that has no
  sibling presenter or component can't resolve presenter-derived context (by design).
- **JSON injection** with Latte holes embedded in JSON can put the parser into an
  interim state on broken input.

> None of these block everyday template work. They're documented so you know exactly
> where the boundaries are.
