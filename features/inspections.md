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
- **Printing a value that cannot become a string** – `{$order}` where the class has no
  `__toString()`, or an enum; printing an array is a weak warning. The escaping context
  is taken into account, so a print inside `<script>` or an `on*` handler – where the
  value is encoded rather than stringified – stays quiet.
- **Static member via `->`** – a static method reached with `->` instead of `::`.

## Filters & functions

- **Undefined filter** – `|nosuchfilter`.
- **Filter type mismatch** – wrong input type for a filter.
- **Undefined function** – unresolved PHP function in an expression.
- **Filter Latte refuses to run** – calling `|escape` by hand. Latte reserves it for its
  own escaping and rejects the template, so the report is an error rather than a warning.
  It appears only where Latte actually refuses it: from Latte 3.1 on. On earlier versions
  the spelling is valid and stays quiet, and where the installed version cannot be read
  from `composer.lock` Latte+ says nothing rather than risk a false error.

## Files & includes

- **Missing file** – `{include 'does/not/exist.latte'}`.
- **Missing asset** – a file that `{asset}`, `{preload}` or `n:asset` names but that is
  not there. The message names the directory actually searched, which behind a mapper is
  not the default assets root.
- **Named asset mappers** – a reference like `images:logo.gif` names a *mapper*, not a
  directory. Latte+ reads the mappers from your `assets:` configuration, so it can tell
  the two failures apart: the file is missing under that mapper's own root, or no mapper
  of that name is configured at all. Where the configuration cannot be read – a path
  assembled from DI parameters that point nowhere on disk, or a mapper registered in PHP –
  nothing is claimed, and you can name the roots yourself in
  [settings](../configuration/asset-mapping.html).
- The optional forms (`{asset '?…'}`, `n:asset?`) stay quiet about a missing file – the
  runtime hands back null instead of throwing, so that is the point of them. They still
  report an unknown mapper, which throws either way.
- **`n:asset` spellings Latte refuses** – the optional marker belongs on the attribute
  name (`n:asset?="…"`), not inside the value, and a variable after a mapper has to be
  braced (`images:{$name}`); a bare one ends the attribute early. Both are reported with a
  quick fix, since each is a single mechanical edit.

If the template you are including doesn't exist yet, a quick fix creates it at the
resolved path – aliases included.

![A quick fix offering to create the missing Latte file referenced by an include]({{ '/assets/img/screens/S20-quickfix-create-file.png' | relative_url }})

## Blocks: when a missing block is reported

Block names are the one place where "does this exist?" has no simple answer. A block
can be declared in the template itself, inherited through a layout, pulled in with
`{import}`, or handed over from the outside by whoever embeds the template. Reporting
every name that is not declared right here would bury you in warnings about code that
works.

So Latte+ reports a missing block only when it is reasonably sure. Here is the whole
rule, in the order it is applied.

**Where the block is looked for**

1. The current template, including names declared with `n:block` / `n:define` (and the
   `n:inner-` / `n:tag-` forms).
2. Everything reachable upwards through `{import}` and `{layout}` / `{extends}` – the
   whole chain, not just the direct parent. Latte merges those blocks at runtime, so we
   follow the same path.
3. With `{include #name from 'file.latte'}`, that one file and nothing else. The `from`
   clause names a concrete target, so the chain is deliberately ignored.

**When it stays quiet even though the block was not found**

- **The name is dynamic.** `{include #$name}` or `{block foo-{$id}}` cannot be checked
  before runtime.
- **The include is guarded.** `{ifset #name}` and `{if hasBlock('name')}` are the
  documented way to ask whether a block was passed in, so anything inside them is
  treated as intentional.
- **The template does not parse.** While a template has a syntax error, the block
  structure is unreliable, so block warnings are suppressed until it parses again.

**Known limitation.** A block that reaches the template from the *outside* – supplied by
a child that extends this layout, or by the caller of `{embed}` – is reported. Latte+
resolves upwards along a deterministic path; the reverse direction, finding everyone who
embeds or extends this file, is not tracked.

Both spellings behave the same here. `{include #name}` and `{include name}` are one
instruction written two ways, so which one you use never changes the verdict – earlier
versions were quieter on the bare form, which made the more explicit spelling the riskier
one. To say that a block arrives from elsewhere, wrap the include in `{ifset #name}` or
`{if hasBlock('name')}`, or suppress the inspection for that line or file.

## Nette-specific

- **Undefined component** – `{control x}` with no matching factory (plus a
  did-you-mean fix).
- **Link target** – a destination that doesn't resolve, in `{link}` / `{plink}`, in
  `n:href` and in `{ifCurrent}` alike. Both halves are checked: an unknown presenter,
  and an action the presenter doesn't have.
- **Form checks** – form field / container / owner consistency.
- **No-escape filter** – flags a `|noescape`-only modifier on a `{control}`.
- **Nonce attribute** – `n:nonce` used outside its valid scope.

## PHP tag mode

- Validates `{php}` / `{do}` bodies against the active
  [strict or permissive mode](./embedded-languages.html).

## Unused code

- **Unused `{define}`** – a define with no `{include}` referencing it.
