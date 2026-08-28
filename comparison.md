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

It is not the whole picture, though. Asset support is compared separately
[in its own section below](#assets), including the rows that are still missing there.

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

## Assets

Assets were the weakest part of Latte+ for a long time, and this section used to run the
other way round. What is left of that gap is one row, and several rows now run in the
other direction — chief among them that Latte+ reads the mappers from the `assets:`
section of your NEON configuration, so it knows what `images:logo.gif` means without
being told twice.

| Working with assets | Latte+ | Latte Pro |
|---|:---:|:---:|
| `{asset}` / `{preload}` recognised, path checked | ✅ | ✅ |
| `n:asset` path checked | ✅ | ✅ |
| **Named mappers read from your NEON configuration** | ✅ | ❌ restate them in its own XML |
| **Unknown mapper reported as a mapper, not as a missing file** | ✅ | ✅ reported as an unknown protocol |
| **`Ctrl+B` from an asset to the file** | ✅ | ✅ |
| **`Ctrl+B` from a mapper prefix to its directory** | ✅ | – not measured |
| **Completion offers mapper prefixes** | ✅ | ✅ |
| **Completion lists the files under the named mapper** | ✅ | – not measured |
| **Completion inside `n:asset`, not only in the tag** | ✅ | – not measured |
| **Popup opens by itself, and again after a mapper's colon** | ✅ | – not measured |
| **Spellings Latte refuses are reported, with a quick fix** | ✅ | – not measured |
| **Renaming the file keeps the reference working** | ✅ | – not measured |
| `{asset?}` optional form handled as its own shape | ✅ | ✅ |
| Path points at a directory instead of a file | ❌ | ✅ |
| Configurable asset root | ⚠️ convention for the root, settings override for the mappers | ✅ |

Half the table says "not measured" on the right, and that is deliberate: those rows cover
behaviour added recently, and nobody has sat down with Latte Pro to check them. They are
listed because they are part of what Latte+ does, not as a claim about what it does not.

### What this looks like in practice

`{asset 'logo.svg'}`, `{preload}` and `<img n:asset="logo.svg">` all warn when the file is
not there. A reference that names a mapper is resolved against the root that mapper
actually points at, so `{asset 'images:logo.gif'}` is checked rather than skipped, and a
name no mapper declares is reported as **the mapper being wrong** instead of sending you
looking for a file.

`Ctrl+B` opens the file — through a mapper, through intermediate directories, from the tag
and from `n:asset` alike — and the mapper prefix itself opens the directory it stands for,
even when the filename beside it is a variable. Renaming the file rewrites the reference
as a path under the assets root, so the template still works afterwards.

Completion opens by itself inside `{asset '…'}` and inside `n:asset`, and again once you
type a mapper's colon; from that point it lists what lives under **that** mapper rather
than everything under the default root.

The two spellings Latte refuses are reported rather than completed into: the optional
marker belongs on the attribute name (`n:asset?="…"`, not `n:asset="?…"`), and a variable
after a mapper has to be braced (`images:{$name}`). Each comes with a quick fix, because
each is one mechanical edit.

The optional form stays quiet about a missing file, since the runtime hands back null
instead of throwing — but it still reports an unknown mapper, which throws either way.

### What is still open

The assets root itself is a convention rather than a setting: Latte+ walks up from the
template looking for `www/assets/` or `assets/`, and where your NEON says something else,
the mappers answer for it. For a mapping the configuration cannot express — a path built
from DI parameters that point nowhere on disk, or a mapper registered in PHP — there is
an [override in settings](./configuration/asset-mapping.html). A path that lands on a
directory instead of a file still passes as valid.

The difference in approach is worth calling out, because it is why the mapper rows moved.
Latte Pro asks you to restate the asset setup in its own XML configuration; Latte+ reads
the `assets:` section you have already written, so there is no second copy to keep in
sync.

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
