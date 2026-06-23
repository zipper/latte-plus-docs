---
layout: default
title: Custom extensions
parent: Configuration
nav_order: 4
---

# Custom extensions

Most non-trivial Latte projects register their own tags, filters, functions and
`n:attributes` through `Latte\Extension` subclasses. Latte+ makes those first-class
citizens in the editor.

## Automatic discovery

Latte+ scans your project for `Latte\Extension` subclasses and picks up the custom
tags, filters, functions and `n:attributes` they register. Once discovered they get:

- **Completion** alongside the built-in tags and filters,
- **Hover documentation** with a link back to the registering source code,
- a **did-you-mean** quick fix when a name is close but misspelled,
- validation by the relevant inspection instead of false *unknown tag/filter*
  warnings.

## Manual registration

When something can't be discovered automatically – a dynamically registered macro, an
extension defined outside the indexed sources – register it by hand under
**Settings → Languages & Frameworks → Latte+ → Custom Extensions**.

The settings UI is split into sub-tabs for the four kinds of extension:

- **Tags** – custom `{macro}` names (and whether they're paired, argument modes).
- **Filters** – name and parameter signature.
- **Functions** – name and signature.
- **n:attributes** – name and where it applies.

Manually registered entries behave identically to discovered ones in completion,
documentation and inspections.

## Tag signatures

A custom tag's argument shape can't be derived automatically, so you describe it with a
small DSL in the **Tags** sub-tab. The signature drives completion placeholders, inlay
parameter-name hints and the missing-arg / too-many-arg inspection. See
[Custom tag signatures](./custom-tag-signatures.html) for the full reference.
