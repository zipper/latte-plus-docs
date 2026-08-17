---
layout: default
title: Documentation & hints
parent: Features
nav_order: 9
---

# Documentation & hints
{: .no_toc }

1. TOC
{:toc}

---

## Hover documentation (`Ctrl+Q`)

Hover or press `Ctrl+Q` on almost anything for inline documentation:

- **Latte tags** – description, syntax and a usage example, with a link to the
  relevant page on [latte.nette.org](https://latte.nette.org/).
- **`n:attributes`** – what the attribute does and where it applies.
- **Filters** – signature, parameters and description.
- **PHP functions, components and variables** – the underlying PHPDoc and signature.

## Parameter info (`Ctrl+P`)

Parameter hints appear as you fill in arguments for `{include}`, `{control}`,
`{cache}`, `{link}`, filters and custom tags – showing parameter names, types and
defaults.

## Inlay hints

Inline hints render directly in the editor to reduce guesswork:

- **Filter parameter names** before filter arguments – `{$n|number: 2, ',', ' '}`
  shows `decimals:`, `decPoint:`, `thousandsSep:`.
- **Function parameter names** before PHP function arguments.

![Inlay hints naming the positional arguments of filters and of a control tag]({{ '/assets/img/screens/S17-inlay-hints.png' | relative_url }})

> Some type-related inlay hints are currently disabled while a mid-typing edge case
> is resolved – see [Known limitations](../limitations.html).

[More screenshots]({{ site.baseurl }}/screenshots.html#documentation-and-parameters)
