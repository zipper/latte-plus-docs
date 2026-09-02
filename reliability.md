---
layout: default
title: Reliability
nav_order: 7
---

# Reliability

Latte+ reuses PhpStorm's own PHP, HTML, JavaScript and CSS support wherever it can, so
those features feel native rather than bolted on. And what it reports is measured
against what the real Latte engine accepts, so valid code stays clean: you get a
warning when something is genuinely wrong, not a wall of false positives.

## Measured on ten working templates

Ten ordinary templates from the demo project – the layout, presenter views, included
parts and two components:

| | reports on ten working templates |
|---|:---:|
| Latte+ | **0** |
| Another Latte plugin | 57 |

The largest single root of those 57 is one unsupported type annotation: a generic in
`{parameters}` or `{varType}` is rejected, the variable loses its type, and every
`{$image->width}` under it turns into "undefined property". One annotation is enough to
paint a whole template red.

## Aiming for the same answers as Latte

When a syntax rule is unclear, one of the things we look at is how current `latte/latte`
releases actually behave, alongside the documentation and the engine's own test suite.
Cases resolved that way are kept as regression tests, so they stay fixed.

The goal is for both directions to hold:

- **Valid templates should stay clean.** `{= $x & 1}` is a bitwise AND, `{ifset block
  foo}` asks about a block, `hasBlock(annot--x)` is a single identifier, and `**`, `@`,
  a first-class callable, an arrow function with a declared return type and a trailing
  comma in `{var}` or `{default}` are all ordinary PHP – none of them deserves a red
  underline.
- **What Latte rejects should be reported.** Quietly accepting a broken template only
  moves the error from the IDE to production.

Some rules are subtler than they look. A pipe opens a filter only when a lowercase
letter follows it – optionally after spaces or tabs, but never after a newline – so
`{$x| upper}` is a filter and `{$x|` + newline + `upper}` is not. Where it is not a
filter it is a plain bitwise OR, which is why Latte reads `{$x|Upper}` as
`$x | 'Upper'`. Whitespace *before* the pipe does not matter, so a multi-line filter
chain keeps working:

```latte
{$text|lower
      |truncate:20
      |noescape}
```
