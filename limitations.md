---
layout: default
title: Known limitations
nav_order: 7
---

# Known limitations

Latte+ aims to match the real Latte engine as closely as possible, but a handful of
edge cases are either platform constraints or deliberately out of scope for now. This
list is kept honest on purpose – if you hit something not listed here, please
[report it](./support.html).

## Editing edge cases

- **Enter indentation inside JS/CSS** – pressing Enter inside a `<script>` using
  `n:syntax="double"` may not re-indent the JavaScript ideally.
- **Renaming a `{php}` variable** opens a small dialog rather than letting you edit
  the name in place.
- **Live paired-tag rename does not update clauses** – renaming `{if}` to `{foreach}`
  updates `{/if}`, but `{elseif}` and `{else}` stay as they are. The synchronizer links
  exactly two points (opener and closer); IntelliJ behaves the same way for HTML.
- **The shorthand closing tag `{/}` is not synchronised**, because it carries no name.

## Syntax edge cases

- **Top-level `{syntax double}`** and **nested `{syntax}` switching** are accepted by
  the Latte runtime in theory but are not specially handled; they essentially never
  occur in real templates.
- A **literal `{/syntax}` inside a JS regex** within `n:syntax="double"` scope is not
  recognized (not seen in practice).
- **`{iterateWhile}` with a condition on the closing tag** is not parsed.
- **`n:try` with `{rollback}`** is out of scope.
- **Member access inside a bare string** – `"$user->name"` resolves `$user` but not
  the `->name` part (this matches the behavior of other Latte plugins).

## Type inference & hints

- Some **type inlay hints are currently turned off** because they could behave oddly
  while you're still typing. The same type information is still available through
  completion and hover.
- **Custom tag / `n:*` argument types aren't deeply validated.** The argument *count*
  and *literal* shapes are checked against the signature, but a variable or expression
  argument (`$x`, `$x->y`, `foo()`) is treated as compatible with any declared type –
  its runtime type is not inferred. A custom tag / `n:*` body is a flat positional token
  stream (so it tolerates hyphenated barewords like `image-xs`), so there's no
  structured expression to infer from; filters and `{include}` / `{embed}` parameters,
  which do have one, get full type validation. See
  [custom tag signatures]({{ '/configuration/custom-tag-signatures.html#validation' | relative_url }}).

## Other

- **Structure view lists Latte tags only** – HTML elements and `n:attributes` are not
  part of the tree. An `n:attribute` is not an attribute in the Latte tree at all; it
  belongs to the HTML side of the file, which is parsed separately. The tree is a map
  of the template's Latte structure, and that is also the scope other Latte plugins
  have.
- **Shared partial templates with no owner** – a layout or partial that has no
  matching presenter or component can't pick up the variables those would provide.
- **Latte expressions embedded in JSON** can briefly confuse highlighting while the
  surrounding JSON is incomplete.
- **Presenter layout is inferred when no configuration can be read** – Latte+ reads
  `application: mapping` from the NEON configuration nearest to the template (see
  [presenter mapping]({{ '/configuration/presenter-mapping.html' | relative_url }})),
  and only falls back to matching against the layouts the shipped Nette skeletons use
  when there is nothing to read: no configuration, a mapping assembled in PHP, or masks
  built out of DI parameters. In that fallback a presenter whose short name exists
  several times may not be offered in completion, and one shape is genuinely ambiguous:
  `App\Presentation\Admin\AdminPresenter` is read as the presenter `Admin`, which is
  what the default mapping means – a project on a flat mapping means `Admin:Admin` by
  it and gets the wrong name offered. Adding the mapping to your configuration settles
  both.
- **Only the presenters your mapping can route are offered** – once a mapping is read,
  a presenter no mask matches is left out of destination completion, because a link to
  it would fail at runtime. Navigation stays permissive, so `Ctrl+B` and the
  undefined-link inspection still reach such a class; the consequence is that a
  destination completion does not offer can still resolve without a warning.
- **One mapping is used for the whole project** – the mapping found for the template
  you are editing is applied to every presenter in the project. A configuration split
  sideways across `includes:` – per-feature modules of one application, each with its
  own `application:` section – is therefore only partly seen, and presenters the
  unread part maps may be missing from completion. Navigating to them keeps working.
  Packages of a monorepo that are separate applications are not affected: a link
  between them is one Nette refuses anyway.
- **A destination without a leading colon is treated as absolute** – Nette reads
  `{link Admin:Product:edit}` relative to the current presenter's module, so inside a
  module it means `<Module>:Admin:Product:edit`. Latte+ reads it as absolute, so such a
  link resolves and is not reported even where Nette would refuse it. Writing the
  leading colon (`{link :Admin:Product:edit}`) makes both readings agree.

> None of these block everyday template work. They're documented so you know exactly
> where the boundaries are.
