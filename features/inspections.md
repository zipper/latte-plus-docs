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

A core design goal is **few false positives**: Latte+ accepts the same templates the
real Latte engine accepts, so valid templates stay clean and you can trust a warning
when you see one.

A typo in an `n:attribute`, a filter, a property, a block name or a component is caught
in the same pass – here six of them in one short template, each naming the closest
valid alternative.

![Six different reports on one template, listed together in the Problems panel]({{ '/assets/img/screens/S18-inspections.png' | relative_url }})

Most reports come with a quick fix – a misspelled `n:attribute`, filter, tag or component
offers the closest valid name, and you can apply the correction without leaving the
keyboard.

![A quick fix offering to replace a misspelled n:attribute with the correct one]({{ '/assets/img/screens/S19-quickfix-nattr.png' | relative_url }})

[More screenshots]({{ site.baseurl }}/screenshots.html#inspections-and-quick-fixes)

## Template structure

- Unclosed or mismatched tags.
- Invalid clause ordering (e.g. `{else}` before its `{if}`).
- Circular `{include}` / `{import}` chains.

## Variables & types

- **Undefined variable** – `$missing` that was never declared or injected.
- **Undefined member** – `$obj->nonexistent` / unknown property.
- **Undefined class** – unresolved class in `{varType}`, `instanceof`, etc.
- **Type mismatch** – argument or filter input whose type doesn't fit.

## Filters & functions

- **Undefined filter** – `|nosuchfilter`.
- **Filter type mismatch** – wrong input type for a filter.
- **Undefined function** – unresolved PHP function in an expression.

## Files & includes

- **Missing file** – `{include 'does/not/exist.latte'}`.
- **Missing asset** – unresolved asset reference.

If the template you are including doesn't exist yet, a quick fix creates it at the
resolved path – aliases included.

![A quick fix offering to create the missing Latte file referenced by an include]({{ '/assets/img/screens/S20-quickfix-create-file.png' | relative_url }})

## Nette-specific

- **Undefined component** – `{control x}` with no matching factory (plus a
  did-you-mean fix).
- **Link target** – `{link}` / `n:href` destination that doesn't resolve.
- **Form checks** – form field / container / owner consistency.
- **No-escape filter** – flags a `|noescape`-only modifier on a `{control}`.
- **Nonce attribute** – `n:nonce` used outside its valid scope.

## PHP tag mode

- Validates `{php}` / `{do}` bodies against the active
  [strict or permissive mode](./embedded-languages.html).

## Unused code

- **Unused `{define}`** – a define with no `{include}` referencing it.
