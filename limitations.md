---
layout: default
title: Known limitations
nav_order: 8
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
- **A block name mid-deletion** – while a `{block}` header stands with its name
  erased, a reference to that name can report as missing for a moment. It clears as
  soon as you type a character.

## Syntax edge cases

- **Top-level `{syntax double}`** and **nested `{syntax}` switching** are accepted by
  the Latte runtime in theory but are not specially handled; they essentially never
  occur in real templates.
- A **literal `{/syntax}` inside a JS regex** within `n:syntax="double"` scope is not
  recognized (not seen in practice).
- **`n:try` with `{rollback}`** is out of scope.
- **Member access inside a bare string** – `"$user->name"` resolves `$user` but not
  the `->name` part.
- **A closing tag with no opening tag** (`{/if}` with no `{if}`) ends Latte parsing
  for the rest of the file: tags written below it are not recognized, so nothing
  there completes, navigates or gets reported. Adding the opener – or removing the
  stray closer – brings the file back.
- **A `#` before a block name** is accepted in `{block #foo}`, `{define #foo}` and
  `{ifset #foo}`, but `{ifset block #foo}` and `n:ifset="#foo"` still report it.
- **An unclosed tag next to a stray closing tag** (`{if true}{/foreach}`) reports
  twice, the second time in raw parser wording. They are two separate mistakes, and
  that raw message is the only report the unmatched closing tag gets.

## Type inference & hints

- **Custom tag / `n:*` argument types aren't deeply validated.** The argument *count*
  and *literal* shapes are checked against the signature, but a variable or expression
  argument is treated as compatible with any declared type. A custom tag body is a flat
  token stream, so there is nothing to infer from; filters and `{include}` parameters,
  which do have an expression tree, get full type validation. See
  [custom tag signatures]({{ '/configuration/custom-tag-signatures.html#validation' | relative_url }}).

## Other

- **Structure view lists Latte tags only** – HTML elements and `n:attributes` are not
  part of the tree, because an `n:attribute` belongs to the HTML side of the file. The
  tree is a map of the template's Latte structure.
- **Shared partial templates with no owner** – a layout or partial that has no
  matching presenter or component can't pick up the variables those would provide.
- **A layout chosen at runtime is not followed** – a view with no `{layout}` tag
  inherits the `@layout.latte` Nette attaches to it by itself, but `setLayout('other')`
  in the presenter is PHP the template does not spell out. Blocks and variables from
  `@otherLayout.latte` are therefore not seen, and an `{include #block}` aimed at one
  is reported as missing.
- **Only `@layout.latte` is searched for** – other layout names, including
  `@layout.<locale>.latte`, are not. Writing `{extends auto}` counts as naming the
  parent yourself, so no automatic layout is attached there either. Without an
  `application: mapping` the search can read, it does not climb above the presenter's
  own level, so a layout shared by a whole module is not found. Only a presenter's
  view inherits a layout this way – a partial or a component template does not.
- **A `{block}` written where Latte does not read one** – inside a JavaScript string,
  say, or in a `{syntax double}` region – still counts as a declaration, so a
  reference to that name is not reported. Missing-block reporting errs on the quiet
  side here.
- **Latte expressions embedded in JSON** can briefly confuse highlighting while the
  surrounding JSON is incomplete.
- **Presenter layout is inferred when no configuration can be read** – Latte+ reads
  `application: mapping` from the NEON configuration nearest to the template (see
  [presenter mapping]({{ '/configuration/presenter-mapping.html' | relative_url }})),
  and falls back to the layouts the Nette skeletons use only when there is nothing to
  read. In that fallback, a presenter whose short name exists several times may not be
  offered, and `App\Presentation\Admin\AdminPresenter` is read as `Admin` rather than
  `Admin:Admin`. Adding the mapping to your configuration settles both.
- **Only the presenters your mapping can route are offered** – a presenter no mask
  matches is left out of destination completion, because a link to it would fail at
  runtime. Navigation stays permissive, so `Ctrl+B` still reaches such a class.
- **One mapping is used for the whole project** – the mapping found for the template
  you are editing is applied to every presenter. A configuration split sideways across
  `includes:`, each part with its own `application:` section, is therefore only partly
  seen: presenters the unread part maps may be missing from completion, though
  navigating to them keeps working.
- **A destination without a leading colon is treated as absolute** – Nette reads
  `{link Admin:Product:edit}` relative to the current module, Latte+ reads it as
  absolute, so such a link resolves even where Nette would refuse it. Writing the
  leading colon makes both readings agree.

> None of these block everyday template work. They're documented so you know exactly
> where the boundaries are.
