---
layout: default
title: Navigation & refactoring
parent: Features
nav_order: 4
---

# Navigation & refactoring
{: .no_toc }

1. TOC
{:toc}

---

Latte+ treats blocks, variables, includes and even include argument keys as real
symbols, so the standard PhpStorm navigation and refactoring actions work on them –
including **across files**.

## Go to definition (`Ctrl+B` / `Ctrl+Click`)

- `{include #header from 'layout.latte'}` jumps to the `{block header}` /
  `{define header}` in `layout.latte`.
- `{control reviews}` jumps to the `createComponentReviews()` factory.
- Filters and PHP functions jump to their declaration.
- An `{include}` argument key jumps to the matching `{parameters}` / `{default}`
  declaration in the target template.

## Find Usages (`Alt+F7`)

Run Find Usages on a `{block}`, `{define}`, variable, component or filter to get every
reference across the project – for blocks and defines that means every `{include}`
caller, in every file.

## Rename (`Shift+F6`)

- **Variables** rename in-place (no dialog) and are **scope-aware**: renaming an
  outer `{var $item}` does not touch a `{foreach … as $item}` body that shadows it.
- **Blocks & defines** rename **atomically across the whole project** – every
  `{include}` caller is updated in one operation.
- **Include argument keys** rename together with all callers.

## Quick fixes

- **Create `{define}`** – when an `{include #foo}` points at a missing block, a
  one-click quick fix creates the `{define foo}` in the target file.
- **Did you mean…?** – misspelled tags, components and filters offer a
  Levenshtein-based suggestion to the closest valid name.
