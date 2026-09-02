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

Latte+ ships **67 inspections** that validate templates as you type. Each one can be
toggled and given a severity (Error / Warning / Weak warning) under
**Settings → Editor → Inspections**, where most of them sit under **Latte** – grouped
into Templates, File resolution, Block references, Variables, n:attributes, Filters,
PHP tag and Custom extensions – and the rest under a separate **Latte+** node.

A core design goal is **few false positives**: what Latte+ reports is measured against
what the real Latte engine accepts, so valid templates stay clean and you can trust a
warning when you see one. The gaps that remain are written down under
[known limitations](../limitations.html).

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
- **Duplicate `{block}` or `{define}`** – two declarations of the same name in one
  layer of the template, which Latte refuses outright. A block declared inside an
  `{embed}` belongs to that embed's own layer, so a name used both in the main template
  and in an embed is fine. The exception is `{block local}`, which Latte registers
  globally: a later declaration of that name anywhere in the file, embed bodies
  included, does collide with it.
- An `n:attribute` is flagged for its value only where Latte itself refuses what you
  wrote: `n:nonce="self"`, `n:try="$x"`, `n:spaceless="$x"`, `n:ifcontent="$x"` and
  `n:else="$x"` do not compile, and neither does `n:if` or `n:class` left without a
  value. Everywhere Latte is happy – `n:translate` on its own, `n:first="3"`, a bare
  `n:nonce` – nothing is reported.

## Variables & types

- **Undefined Latte variable** – a `$x` that resolves nowhere: no local declaration, no
  layout chain, no implicit variable. Off by default and a weak warning when on,
  because expression shapes Latte+ does not follow yet (closures, `{php}` blocks) can
  make a declared variable look undeclared. Turn it on under
  **Settings → Editor → Inspections** for a template set you keep strict.
- **Undefined member** – `$obj->nonexistent` / unknown property.
- **Undefined class** – unresolved class in `{varType}`, `instanceof`, etc.
- **Type mismatch** – argument or filter input whose type doesn't fit.
- **Printing a value that cannot become a string** – `{$order}` where the class has no
  `__toString()`, or an enum; printing an array is a weak warning. The escaping context
  is taken into account, so a print inside `<script>` or an `on*` handler – where the
  value is encoded rather than stringified – stays quiet.
- **Printing a value that may be null** – a weak warning, and only where the print is
  not already guarded: inside `{if}` or `n:if`, in the truthy arm of a ternary, or
  behind an `&&` or `??` that has tested it, nothing is reported.
- **Iterating something that cannot be iterated** – `{foreach $count as $x}` on a
  scalar. Iterating an object is legal PHP and stays quiet, and so does an
  `n:attribute` that does not iterate at all, such as `n:if` or `n:class`.
- **Indexing a scalar** – `$name[0]` on a string is the first character, so only the
  genuinely impossible cases are reported.
- **Arithmetic on an object** – `{$order * 2}`, where the operand is a class type on
  every branch its declared type has.
- **Comparing unrelated types with `===` or `!==`** – a weak warning, and deliberately
  only those two operators: `==` coerces, so comparing two types with it is a normal
  thing to write.
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
- **Wrong number of arguments** – too few or too many, for a PHP function and for a
  filter alike, checked against the signature the project really has.
- **Calling a number** – `{= 10(5)}`, which Latte refuses to compile. Only a numeric
  literal is reported: in PHP plenty of other things are callable by value, so a wider
  rule would flag working code.
- **`|noescape` where Latte refuses it** – on a `{block}` declaration, and inside an
  HTML comment on Latte 3.1 or newer. Where the installed version cannot be read, the
  comment case stays silent rather than risk a false error.

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
- The optional forms (`{asset? '…'}`, `n:asset?`) stay quiet about a missing file – the
  runtime hands back null instead of throwing, so that is the point of them. They still
  report an unknown mapper, which throws either way.
- A question mark **inside** the reference is not that marker. `{asset '?logo.png'}`
  asks for a file whose name begins with one, so a missing file there is reported like
  any other; `{asset '?images:logo.gif'}` asks for a mapper called `?images`, and
  `n:asset="?images:logo.gif"` is a shape Latte refuses while parsing it. Both of the
  mapper-shaped ones are warnings that offer the same single edit: move the marker onto
  the tag or attribute name, where it belongs.
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

1. The current template, including names declared with `n:block` or `n:define` and
   their `n:inner-` / `n:tag-` forms.
2. Everything reachable upwards through `{import}` and `{layout}` / `{extends}` – the
   whole chain, not just the direct parent. Latte merges those blocks at runtime, so we
   follow the same path. A presenter's view needs no `{layout}` tag for this: the
   `@layout.latte` Nette attaches to it on its own is part of the chain too. A layout or
   import named by a concatenated path counts as part of that chain, so the blocks it
   declares are not reported as missing.
3. With `{include #name from 'file.latte'}`, that one file and nothing else. The `from`
   clause names a concrete target, so the chain is deliberately ignored.

**When it stays quiet even though the block was not found**

- **The name is dynamic.** `{include #$name}` or `{block foo-{$id}}` cannot be checked
  before runtime.
- **The include is guarded.** `{ifset #name}` and `{if hasBlock('name')}` are the
  documented way to ask whether a block was passed in, so anything inside them is
  treated as intentional.
- **The declaration is still being typed.** A syntax error only silences the block
  names it could plausibly have broken: an unfinished `{include}` / `{embed}` header,
  or a broken `{block}` / `{define}` header whose name is close to the one being
  checked. An error elsewhere in the template – even on the line above – no longer
  turns the check off for the whole file.

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
  `n:href` and in `{ifCurrent}` alike. All three are checked: an unknown presenter, an
  action the presenter does not have, and a signal whose handler Nette could not
  dispatch.
- **Form checks** – an unknown form or field, in `{form}` / `{input}` and in `n:name`
  alike, and an unknown container in `{formContainer}` / `n:formContainer`, each with a
  did-you-mean fix. A template that renders a form nobody in scope owns says so, and
  you can name the owner yourself in a `{* @form-owner *}` comment.
- **No-escape filter** – flags a `|noescape`-only modifier on a `{control}`.
- **Nonce attribute** – `n:nonce` used outside its valid scope.

## PHP tag mode

- Validates `{php}` / `{do}` bodies against the active
  [strict or permissive mode](./embedded-languages.html).

## Unused code

- **Unused define** – a `{define}` that no `{include}` in the project references. Off by
  default, since a define is sometimes put in place before its caller exists. Only
  `{define}` is checked: a `{block}` body renders on its own, so "unused" does not mean
  anything there.
