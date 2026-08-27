---
layout: default
title: Comparison
nav_order: 6
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

It is not the whole picture, though. There is one area where a competitor is
currently ahead of us, and it has [its own section below](#where-latte-pro-is-ahead-assets).

| Capability | Latte+ | Other Latte plugins |
|---|:---:|:---:|
| Latte 3.x syntax highlighting | ✅ | ✅ |
| Tag / `n:attribute` completion | ✅ | ✅ (varies) |
| **Native HTML support** (completion & inspections, no false "unclosed tag") | ✅ | ⚠️ partial |
| **PHP type-aware autocomplete** (`foreach` item types, member chains) | ✅ | ⚠️ partial |
| **`list<T>` generics** in `{varType}` / `{parameters}` | ✅ | ❌ often flagged as error |
| **Full support inside `{php}` / `{do}`** | ✅ | ❌ |
| **Cross-file block/define rename & Find Usages** | ✅ | ⚠️ varies |
| **Find Usages on a template parameter** (lists the `{include}` arguments that pass it) | ✅ | ❌ |
| **Live paired-tag rename** (closing tag follows as you type, like HTML) | ✅ | ❌ |
| **Sticky lines for Latte tags** | ✅ | ❌ |
| **Configurable `n:attribute` color** | ✅ | ❌ |
| **Automatic completion trigger** (after `{`, `'`, `#`) | ✅ | ⚠️ varies |
| `{control}` factory & render-method completion | ✅ | ⚠️ varies |
| Embedded JS / CSS support inside templates | ✅ | ⚠️ varies |
| Custom extension discovery (tags/filters/functions) | ✅ | ✅ (varies) |
| **Bitwise operators with real precedence** (`&`, `^`, `~`, and shifts `<<` / `>>`) | ✅ | ⚠️ partial – operators may lex, but without a precedence tree; shifts are often unsupported |
| **Union types with a bare class name** (`{varType int\|Foo $x}`) | ✅ | ⚠️ varies – a lowercase-only pipe rule can misread the second name as a filter |
| **Glued identifiers** (`{ifset foo-bar}`, `hasBlock(a.b)`, `foo--bar`) | ✅ | ✅ |
| **`{ifset block X}` and `{ifset #X}` block markers** | ✅ | ⚠️ varies |

## Where Latte Pro is ahead: assets

A comparison page that only lists wins is not much use, so here is the honest other
side. **Asset support is the weakest part of Latte+ today**, and Latte Pro still handles
it better in most respects.

| Working with assets | Latte+ | Latte Pro |
|---|:---:|:---:|
| `{asset}` / `{preload}` recognised, path checked | ✅ | ✅ |
| **`n:asset` path checked** | ✅ | ✅ |
| **Ctrl+B from an asset to the file** | ❌ | ✅ |
| Path points at a directory instead of a file | ❌ | ✅ |
| Unknown mapper or protocol (`vite:`, `front:`) | ❌ | ✅ |
| Completion offers mapper prefixes | ❌ | ✅ |
| **Configurable asset root and mappings** | ❌ convention only, no setting | ✅ |
| `{asset?}` optional form handled as its own shape | ✅ | ✅ |

In practice, `{asset 'logo.svg'}`, `{preload}` and `<img n:asset="logo.svg">` now all
warn when the file is not there, the optional `{asset?}` / `n:asset?` forms are
recognised as their own shape and stay quiet by design, and typing inside `{asset '…'}`
completes the files under the assets root. A reference that names a mapper
(`{asset 'images:logo.gif'}`) is left alone instead of being reported – but it is not
resolved either, so it gets you neither a check nor completion.

What is still missing is the rest of the table. There is no `Ctrl+B` from an asset to
the file it names, a path that lands on a directory passes as valid, and the assets root
is a convention rather than a setting: Latte+ walks up from the template looking for a
`www/assets/` or `assets/` directory and stays quiet if it finds neither. Nothing is read
from the `assets:` section of your NEON configuration yet.

**This is still the area we are working on.** `n:asset` is done; navigation comes next,
then a configurable root, then mappers.

One difference is worth calling out, because it shapes how we intend to get there.
Neither plugin derives its asset configuration automatically today: Latte Pro asks you
to restate the setup in its own XML. Latte+ already understands NEON, so the intent is
to read `assets:` from the configuration you have already written, rather than ask you
to maintain a second copy of it.

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

### Aiming for the same answers as Latte

When a syntax rule is unclear, one of the things we look at is how current `latte/latte`
releases actually behave, alongside the documentation and the engine's own test suite.
Cases resolved that way are kept as regression tests, so they stay fixed.

The goal is for both directions to hold:

- **Valid templates should stay clean.** `{= $x & 1}` is a bitwise AND,
  `{ifset block foo}` asks about a block, and `hasBlock(annot--x)` is a single
  identifier – none of them deserves a red underline.
- **What Latte rejects should be reported.** Quietly accepting a broken template only
  moves the error from the IDE to production.

Some rules are subtler than they look. A pipe starts a filter only when a lowercase
letter follows it, so `{$x|upper}` is a filter while `{$x|` + newline + `upper}` is not.
Whitespace *before* the pipe does not matter, so multi-line filter chains keep working:

```latte
{$items|filter:fn($i) => $i->active
       |sort
       |first}
```
