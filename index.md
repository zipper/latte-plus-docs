---
layout: default
title: Home
nav_order: 1
description: "Latte+ — the most comprehensive Latte template language plugin for PhpStorm."
permalink: /
---

# Latte+
{: .fs-9 }

Full Latte template language support for PhpStorm — autocomplete, type inference,
refactoring and Nette integration that actually understands your templates.
{: .fs-6 .fw-300 }

[Get started](./installation.html){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[See the features](./features/){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## What is Latte+?

**Latte+** is a PhpStorm plugin that brings first-class IDE support for the
[Latte](https://latte.nette.org/) templating engine and the
[Nette Framework](https://nette.org/). It understands both the Latte syntax **and**
the HTML, PHP, JavaScript and CSS embedded inside your `.latte` files at the same
time — so completion, navigation, inspections and refactoring work the way you'd
expect from a native language.

It targets **Latte 3.x**, **PHP 8.1+** and **PhpStorm 2025.3+**.

## Why Latte+ stands out

Latte+ goes well beyond syntax coloring. These are the things you won't find — or
won't find as complete — in other Latte plugins:

| Killer feature | What it gives you |
|---|---|
| **PHP type-aware autocomplete** | `{varType}`, `{templateType}`, `foreach` item types and `$obj->method()` chains are understood, so completion on your variables actually knows their types. |
| **Native HTML support** | HTML completion, inspections and `n:attributes` work right inside your templates — without the false "tag not closed" errors you get elsewhere. |
| **Full support inside `{php}` / `{do}`** | Completion, inspections and refactoring work inside PHP tags, with automatic strict/permissive mode detection. |
| **Cross-file blocks & includes** | Ctrl-click an `{include}` to jump to its `{block}`/`{define}`, Find Usages across the whole project, and rename atomically across every caller. |
| **Nette integration** | `{control}` factory completion, `{link}` target resolution, `{form}` field awareness and `{snippet}` support. |
| **Sticky lines for Latte tags** | The enclosing `{if}` / `{foreach}` / `{block}` stays pinned to the top of the editor as you scroll. |
| **Embedded JS & CSS** | JavaScript and CSS inside `<script>`, `<style>`, `style=""` and `on*=""` behave as in a plain HTML file — Latte `{...}` never breaks them. |
| **Quiet, reliable inspections** | Valid templates stay clean — Latte+ accepts everything the Latte engine accepts, so you're not buried under false errors. |

[Read the full feature reference](./features/){: .btn .btn-outline }

## A quick taste

```latte
{varType App\Model\Product $product}

<article n:class="card, $product->isNew ? card--new">
    <h1>{$product->name}</h1>

    {* type-aware completion on $product->… and on the filter below *}
    <p class="price">{$product->price|number: 2, ',', ' '} Kč</p>

    {foreach $product->images as $image}
        {* $image is inferred from the collection's item type *}
        <img src="{$image->url}" alt="{$image->alt}">
    {/foreach}

    {control reviews}  {* completes from createComponentReviews() *}
</article>
```

## Where to next?

- **[Installation](./installation.html)** — requirements and how to install.
- **[Features](./features/)** — the complete, categorized reference.
- **[Configuration](./configuration/)** — path aliases, implicit variables, PHP mode, colors.
- **[Comparison](./comparison.html)** — how Latte+ compares to other Latte plugins.
- **[Known limitations](./limitations.html)** — honest list of current edge cases.
- **[Support](./support.html)** — reporting bugs and requesting features.
