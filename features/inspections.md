---
layout: default
title: Inspections
parent: Features
nav_order: 5
---

# Inspections
{: .no_toc }

1. TOC
{:toc}

---

Latte+ ships 40+ inspections that validate templates as you type. Each one can be
toggled and given a severity (Error / Warning / Weak warning) under
**Settings → Editor → Inspections → Latte**.

A core design goal is a **near-zero false-positive rate**: the parser is validated
against the real Latte engine, so valid templates stay clean and you can trust a
warning when you see one.

## Template structure

- Unclosed or mismatched tags.
- Invalid clause ordering (e.g. `{else}` before its `{if}`).
- Circular `{include}` / `{import}` chains.

## Variables & types

- **Undefined variable** — `$missing` that was never declared or injected.
- **Undefined member** — `$obj->nonexistent` / unknown property.
- **Undefined class** — unresolved class in `{varType}`, `instanceof`, etc.
- **Type mismatch** — argument or filter input whose type doesn't fit.

## Filters & functions

- **Undefined filter** — `|nosuchfilter`.
- **Filter type mismatch** — wrong input type for a filter.
- **Undefined function** — unresolved PHP function in an expression.

## Files & includes

- **Missing file** — `{include 'does/not/exist.latte'}`.
- **Missing asset** — unresolved asset reference.

## Nette-specific

- **Undefined component** — `{control x}` with no matching factory (plus a
  did-you-mean fix).
- **Link target** — `{link}` / `n:href` destination that doesn't resolve.
- **Form checks** — form field / container / owner consistency.
- **No-escape filter** — flags a `|noescape`-only modifier on a `{control}`.
- **Nonce attribute** — `n:nonce` used outside its valid scope.

## PHP tag mode

- Validates `{php}` / `{do}` bodies against the active
  [strict or permissive mode](./embedded-languages.html).

## Unused code

- **Unused `{define}`** — a define with no `{include}` referencing it.
