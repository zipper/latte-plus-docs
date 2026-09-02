---
layout: default
title: Comparison
nav_order: 6
---

# How Latte+ compares

Latte+ was built to push the depth of language support further than the Latte plugins
that came before it, especially around **type inference**, **embedded languages** and
**cross-file refactoring**. The tables below say where it goes beyond what other Latte
plugins typically offer; the right-hand column is a general guide rather than a promise
about any particular one. Where a row was measured against another plugin, the footnote
says so.

| Capability | Latte+ | Other Latte plugins |
|---|:---:|:---:|
| Latte 3.x syntax highlighting | ✅ | ✅ |
| Tag / `n:attribute` completion | ✅ | ✅ (varies) |
| **Native HTML support** (completion & inspections, no false "unclosed tag") | ✅ | ⚠️ varies[^html] |
| **PHP type-aware autocomplete** (`foreach` item types, member chains) | ✅ | ⚠️ partial |
| **`list<T>` generics** in `{varType}` / `{parameters}` | ✅ | ❌ often flagged as error |
| **Modern PHPDoc types** – array shapes, `int<0, 100>`, `(A&B)\|null`, `non-empty-string`, `class-string` | ✅ | ❌ commonly flagged as errors, one report per line |
| **`??` on a variable that may not be set** (`{$x ?? 'default'}`, `{$x->y ?? …}`, `{$x['k'] ?? …}`) | ✅ quiet, the way Latte itself treats it | ❌ often still reported as undefined |
| **Full support inside `{php}` / `{do}`** | ✅ | ❌ |
| **Cross-file block/define rename & Find Usages** | ✅ | ⚠️ varies |
| **Find Usages on a template parameter** (lists the `{include}` arguments that pass it) | ✅ | ❌ |
| **Live paired-tag rename** (closing tag follows as you type, like HTML) | ✅ | ❌ |
| **Sticky lines for Latte tags** | ✅ | ❌ |
| **Configurable `n:attribute` color** | ✅ | ❌ |
| **Automatic completion trigger** (after `{`, `'`, `#`) | ✅ | ⚠️ varies |
| **`Ctrl+B` on a link offers the action's template, not only the presenter method** | ✅ template first, method second | – not measured |
| `{control}` factory & render-method completion | ✅ | ⚠️ varies |
| Embedded JS / CSS support inside templates | ✅ | ⚠️ varies[^jscss] |
| **Custom extension discovery** (tags/filters/functions read from your PHP) | ✅ automatic | ❌[^ext] |
| **Bitwise operators with real precedence** (`&`, `^`, `~`, and shifts `<<` / `>>`) | ✅ parsed into a precedence tree | ⚠️ varies[^bitwise] |
| **Union types with a bare class name** (`{varType int\|Foo $x}`) | ✅ | ⚠️ varies[^union] |
| **Glued identifiers** (`{ifset foo-bar}`, `hasBlock(a.b)`, `foo--bar`) | ✅ | ✅ |
| **`{ifset block X}` and `{ifset #X}` block markers** | ✅ | ✅ |
| **`{* @noinspection … *}` suppresses one line** | ✅ | ⚠️ varies[^noinsp] |

[^html]: One other plugin was measured and matches the platform here; the rest were not.
[^jscss]: One other plugin injects them too; the rest were not measured.
[^ext]: The plugin measured reports a project's own tag, filter and function as unknown until they are restated in its own XML configuration.
[^bitwise]: No plugin measured reports these forms as an error; whether the precedence is modelled was not measured.
[^union]: A lowercase-only pipe rule can misread the second name as a filter, though the plugin measured accepts this form.
[^noinsp]: A file-wide effect turns the whole file quiet instead of the one line.

## What the template keeps alive

A factory or a render method exists because a template asks for it, and the two share
no text: the template writes `contact`, the method is spelled `createComponentContact`.
A word search never opens the template, so the IDE concludes nobody calls the method –
greying out live code and letting a rename walk away from it.

| A declaration named only by a template | Latte+ | Other Latte plugins |
|---|:---:|:---:|
| Class, property, method or constant written out in full (`{varType}`, `{$x->y}`, `{Foo::BAR}`) | stays live | ✅ stays live |
| `createComponentBreadcrumb()` behind `{control breadcrumb}` | stays live | ❌ greyed as unused |
| `createComponentContact()` behind `{form contact}` | stays live | ❌ greyed as unused |
| `createComponentSearch()` behind `<form n:name="search">` | stays live | ❌ greyed as unused |
| `renderDetail()` behind `{link Product:detail}` | stays live | ❌ greyed as unused |
| `renderThumb()` behind `{control gallery:thumb}` | stays live | ❌ greyed as unused |
| **Renaming the factory rewrites the template too** | ✅ | ❌ PHP is renamed, the template is left behind |
| Declarations nothing really uses | ✅ still reported | ✅ still reported |

The same knowledge feeds `Alt+F7`: Find Usages on a factory lists the template lines that call
it, and on a template variable it separates the line that **writes** the value from the lines
that only **read** it.

## Assets

Assets were the weakest part of Latte+ for a long time, and this section used to run the
other way round. What is left of that gap is one row, and several rows now run in the
other direction – chief among them that Latte+ reads the mappers from the `assets:`
section of your NEON configuration, so it knows what `images:logo.gif` means without
being told twice.

| Working with assets | Latte+ | Another Latte plugin |
|---|:---:|:---:|
| `{asset}` / `{preload}` recognised, path checked | ✅ | ✅ |
| `n:asset` path checked | ✅ | ✅ |
| **Named mappers read from your NEON configuration** | ✅ | ❌[^assetxml] |
| **Unknown mapper reported as a mapper, not as a missing file** | ✅ | ✅ |
| **`Ctrl+B` from an asset to the file** | ✅ | ✅ |
| **`Ctrl+B` from a mapper prefix to its directory** | ✅ | – not measured |
| **Completion offers mapper prefixes** | ✅ | ✅ |
| **Completion lists the files under the named mapper** | ✅ | – not measured |
| **Completion inside `n:asset`, not only in the tag** | ✅ | – not measured |
| **Popup opens by itself, and again after a mapper's colon** | ✅ | – not measured |
| **Spellings Latte refuses are reported, with a quick fix** | ✅ | – not measured |
| **Renaming the file keeps the reference working** | ✅ | – not measured |
| `{asset? …}` optional form, and the comma tail after the path | ✅ | – not measured |
| **A `?` inside the string read as a path, not as the marker** | ✅ | – not measured |
| **`<img n:asset="…">` counts as having `src`** (and `alt` passed through the attribute) | ✅ | ❌[^nasset] |
| Path points at a directory instead of a file | ❌ | ✅ |
| Configurable asset root | ⚠️ partial[^assetroot] | ✅ |

[^assetxml]: The asset setup has to be restated in the plugin's own XML configuration.
[^nasset]: Every image whose `src` comes from `n:asset` is reported as missing `src` and `alt`.
[^assetroot]: The assets root is a convention Latte+ walks up to; individual mappers can be overridden in settings.

Half the asset table says "not measured" on the right, deliberately: those rows cover
behaviour added recently and nobody has sat down with another plugin to check them. They
are listed because they are part of what Latte+ does, not as a claim about what anything
else does not.

Asset support is described where it is configured and where it is reported: see
[asset mappers](./configuration/asset-mapping.html) for how Latte+ reads the `assets:`
section of your NEON configuration, and [inspections](./features/inspections.html) for
what it reports when a path, a mapper or an optional marker is wrong.

## Migrating from another plugin

Latte+ registers the `.latte` file type, so **disable or uninstall any other Latte
plugin before installing** – running two at once causes conflicting highlighting and
duplicated inspections. See [Installation](./installation.html).

Your color scheme is independent: Latte+ defines its own
[configurable token colors](./configuration/colors-code-style.html), so you can tune
the look to match what you had before.
