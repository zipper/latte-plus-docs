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
- A filter your own project registers jumps to the method behind it – from a
  `Latte\Extension`, from an `addFilter()` call, from a factory array or from the
  Latte+ settings – and so do PHP functions. A filter built into Latte has no
  declaration in your project to open.
- A variable that a `{varType}` types and a presenter fills offers **two** targets: the
  `{varType}` line first, the `$this->template->… = …` assignment second.
- A signal link resolves segment by segment: in `{link commentList:save!}`,
  `commentList` opens the `createComponentCommentList()` factory and `save!` opens the
  `handleSave()` that will run.
- `{control gallery:thumb}` opens the `renderThumb()` a component in that position can
  really reach, rather than any method of that name in the project.
- A path assembled from pieces (`{include $dir . '/parts/item.latte'}`) is followed by
  the part that is spelled out: the run of string literals that **ends** the expression
  is a real suffix of the path, so it resolves relatively or through an alias. It works
  in `{include}`, `{embed}`, `{layout}`, `{import}`, `{sandbox}` and after `from`.
- An `{include}` argument key jumps to the matching `{parameters}` / `{default}`
  declaration in the target template.
- An asset reference opens the file it names – in `{asset}`, `{preload}` and `n:asset`
  alike, and through a named mapper (`images:logo.gif`) as readily as a plain path.
  Intermediate directories are navigable too, and the mapper prefix opens the directory it
  stands for – even when the filename next to it is a variable.
- Renaming an asset rewrites the reference as a path under the **assets root**, so the
  template keeps working; a mapper prefix in front of it is left alone.

![Ctrl+B on a control tag landing on the factory method that builds the component]({{ '/assets/img/screens/S31-goto-control.png' | relative_url }})

![Go to declaration on a typed variable, offering the varType line and the presenter method that sets the value]({{ '/assets/img/screens/S37-goto-typed-variable.png' | relative_url }})

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

Results are named rather than filed under "Unclassified": a usage from a template reads
as **Latte template**, and on a variable the window separates **Value read in Latte
template** from **Value written in Latte template** – so you see which line puts a
value in and which lines only take one out.

![Find Usages on an optional template parameter, listing the reads and the include arguments that pass it]({{ '/assets/img/screens/S35-find-usages-parameter.png' | relative_url }})

[More screenshots]({{ site.baseurl }}/screenshots.html#navigation)

## PHP that only a template uses

A `createComponentX()` factory exists because some template writes `{control x}`, and
the two share no text – so PhpStorm used to grey the method out as unused, and acting
on that greying deleted live code. Latte+ puts `.latte` files back into the scope the
unused check searches, and indexes the names the framework builds rather than writes:
`{control x}`, `{control x:m}`, `{form x}`, `n:name` and `{link P:a}` all keep their
factory, render, action and handle methods alive. This holds in the editor and in a
whole-project **Inspect Code** run alike.

The same references work in both directions: `Alt+F7` on the method lists the template
that reaches it, and `Shift+F6` rewrites the template along with the declaration.

![Three component factories a template names, live, next to a fourth that no template names and stays greyed out as unused]({{ '/assets/img/screens/S36-kept-alive-by-template.png' | relative_url }})

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
