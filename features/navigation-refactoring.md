---
layout: default
title: Navigation & refactoring
parent: Features
nav_order: 4
---

# Navigation & refactoring
{: .no_toc }

1. TOC
{:toc}

---

Latte+ treats blocks, variables, includes and even include argument keys as real
symbols, so the standard PhpStorm navigation and refactoring actions work on them –
including **across files**.

## Go to definition (`Ctrl+B` / `Ctrl+Click`)

- `{include #header from 'layout.latte'}` jumps to the `{block header}` /
  `{define header}` in `layout.latte`.
- `{control reviews}` jumps to the `createComponentReviews()` factory.
- A `{link}` / `n:href` destination offers **two** targets at once: the action's template
  first, the presenter's `render`/`action` method second – see
  [one destination, two places to land](./nette-integration.html#one-destination-two-places-to-land).
- Filters and PHP functions jump to their declaration.
- An `{include}` argument key jumps to the matching `{parameters}` / `{default}`
  declaration in the target template.
- An asset reference opens the file it names – in `{asset}`, `{preload}` and `n:asset`
  alike, and through a named mapper (`images:logo.gif`) as readily as a plain path.
  Intermediate directories are navigable too, and the mapper prefix opens the directory it
  stands for – even when the filename next to it is a variable.
- Renaming an asset rewrites the reference as a path under the **assets root**, so the
  template keeps working; a mapper prefix in front of it is left alone.

![Ctrl+B on a control tag landing on the factory method that builds the component]({{ '/assets/img/screens/S31-goto-control.png' | relative_url }})

## Find Usages (`Alt+F7`)

Run Find Usages on a `{block}`, `{define}`, variable, component or filter to get every
reference across the project – for blocks and defines that means every `{include}`
caller, in every file.

![Find Usages on a define listing every include of it across the project]({{ '/assets/img/screens/S22-find-usages-block.png' | relative_url }})

It works on a **template parameter** too, and on an optional one that is where it earns
its keep. The result lists every use of the parameter – the reads inside the template and
the `{include}` arguments that supply it – which is a *narrower* set than "everywhere this
template is included": callers that rely on the default simply are not in it. So the
question it answers is "who actually sets this?", not "where is this template used?".

![Find Usages on an optional template parameter, listing the reads and the include arguments that pass it]({{ '/assets/img/screens/S35-find-usages-parameter.png' | relative_url }})

[More screenshots]({{ site.baseurl }}/screenshots.html#navigation)

## Rename (`Shift+F6`)

- **Variables** rename in-place (no dialog) and are **scope-aware**: renaming an
  outer `{var $item}` does not touch a `{foreach … as $item}` body that shadows it.
- **Blocks & defines** rename **atomically across the whole project** – every
  `{include}` caller is updated in one operation.
- **Include argument keys** rename together with all callers.

## Quick fixes

- **Create `{define}`** – when an `{include #foo}` points at a missing block, a
  one-click quick fix creates the `{define foo}` in the target file.
- **Did you mean…?** – misspelled tags, components and filters offer a
  Levenshtein-based suggestion to the closest valid name.
