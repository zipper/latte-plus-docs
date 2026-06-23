---
layout: default
title: Comparison
nav_order: 5
---

# How Latte+ compares
{: .no_toc }

1. TOC
{:toc}

---

There are a few Latte plugins for JetBrains IDEs – the free *Latte* plugin and the
commercial *Latte Pro* being the best known. Latte+ was built to push the depth of
language support further, especially around **type inference**, **embedded
languages** and **cross-file refactoring**.

The table below highlights where Latte+ goes beyond what other plugins typically
offer. It reflects Latte+'s capabilities; treat the "other plugins" column as a
general guide rather than a guarantee about any specific version of a competitor.

| Capability | Latte+ | Other Latte plugins |
|---|:---:|:---:|
| Latte 3.x syntax highlighting | ✅ | ✅ |
| Tag / `n:attribute` completion | ✅ | ✅ (varies) |
| **Native HTML support** (completion & inspections, no false "unclosed tag") | ✅ | ⚠️ partial |
| **PHP type-aware autocomplete** (`foreach` item types, member chains) | ✅ | ⚠️ partial |
| **`list<T>` generics** in `{varType}` / `{parameters}` | ✅ | ❌ often flagged as error |
| **Full support inside `{php}` / `{do}`** | ✅ | ❌ |
| **Cross-file block/define rename & Find Usages** | ✅ | ⚠️ varies |
| **Sticky lines for Latte tags** | ✅ | ❌ |
| **Configurable `n:attribute` color** | ✅ | ❌ |
| **Automatic completion trigger** (after `{`, `'`, `#`) | ✅ | ⚠️ varies |
| `{control}` factory & render-method completion | ✅ | ⚠️ varies |
| Embedded JS / CSS support inside templates | ✅ | ⚠️ varies |
| Custom extension discovery (tags/filters/functions) | ✅ | ✅ (varies) |

## Migrating from another plugin

Latte+ registers the `.latte` file type, so **disable or uninstall any other Latte
plugin before installing** – running two at once causes conflicting highlighting and
duplicated inspections. See [Installation](./installation.html).

Your color scheme is independent: Latte+ defines its own
[configurable token colors](./configuration/colors-code-style.html), so you can tune
the look to match what you had before.

## A note on reliability

Latte+ reuses PhpStorm's own PHP, HTML, JavaScript and CSS support wherever it can, so
those features feel native rather than bolted on. And because Latte+ accepts the same
templates the real Latte engine accepts, valid code stays clean – you get warnings
when something is genuinely wrong, not a wall of false positives.
