---
layout: default
title: Known limitations
nav_order: 6
---

# Known limitations

Latte+ aims to match the real Latte engine as closely as possible, but a handful of
edge cases are either platform constraints or deliberately out of scope for now. This
list is kept honest on purpose – if you hit something not listed here, please
[report it](./support.html).

## Editing edge cases

- **Enter indentation inside JS/CSS** – pressing Enter inside a `<script>` using
  `n:syntax="double"` may not re-indent the JavaScript ideally.
- **Renaming a `{php}` variable** opens a small dialog rather than letting you edit
  the name in place.

## Syntax edge cases

- **Top-level `{syntax double}`** and **nested `{syntax}` switching** are accepted by
  the Latte runtime in theory but are not specially handled; they essentially never
  occur in real templates.
- A **literal `{/syntax}` inside a JS regex** within `n:syntax="double"` scope is not
  recognized (not seen in practice).
- **`{iterateWhile}` with a condition on the closing tag** is not parsed.
- **`n:try` with `{rollback}`** is out of scope.
- **Member access inside a bare string** – `"$user->name"` resolves `$user` but not
  the `->name` part (this matches the behavior of other Latte plugins).

## Type inference & hints

- Some **type inlay hints are currently turned off** because they could behave oddly
  while you're still typing. The same type information is still available through
  completion and hover.
- **Custom tag / `n:*` argument types aren't deeply validated.** The argument *count*
  and *literal* shapes are checked against the signature, but a variable or expression
  argument (`$x`, `$x->y`, `foo()`) is treated as compatible with any declared type –
  its runtime type is not inferred. A custom tag / `n:*` body is a flat positional token
  stream (so it tolerates hyphenated barewords like `image-xs`), so there's no
  structured expression to infer from; filters and `{include}` / `{embed}` parameters,
  which do have one, get full type validation. See
  [custom tag signatures]({{ '/configuration/custom-tag-signatures.html#validation' | relative_url }}).

## Other

- **Shared partial templates with no owner** – a layout or partial that has no
  matching presenter or component can't pick up the variables those would provide.
- **Latte expressions embedded in JSON** can briefly confuse highlighting while the
  surrounding JSON is incomplete.

> None of these block everyday template work. They're documented so you know exactly
> where the boundaries are.
