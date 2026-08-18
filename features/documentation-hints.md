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

On a tag you get its syntax, a short description of what it does and a link straight to
the matching page of the Latte documentation.

![Quick documentation of a foreach tag with its syntax, description and a link to the Latte docs]({{ '/assets/img/screens/S14-quick-doc-tag.png' | relative_url }})

## Parameter info (`Ctrl+P`)

Parameter hints appear as you fill in arguments for `{include}`, `{control}`,
`{cache}`, `{link}`, filters and custom tags – showing parameter names, types and
defaults.

Filling in an `{include}`, you see the parameters the target template expects without
opening it.

![Parameter info above an include tag listing the parameters the target template expects]({{ '/assets/img/screens/S16-param-info.png' | relative_url }})

## Inlay hints

Inline hints render directly in the editor to reduce guesswork:

- **Filter parameter names** before filter arguments – `{$n|number: 2, ',', ' '}`
  shows `decimals:`, `decPoint:`, `thousandsSep:`.
- **Component and link arguments** – the values you pass to `{control}`, `{link}`
  and `n:href` are labelled with the parameter they fill.
- **Function parameter names** before PHP function arguments.

![Inlay hints naming the positional arguments of filters, a control tag and a link]({{ '/assets/img/screens/S17-inlay-hints.png' | relative_url }})

> Some type-related inlay hints are currently disabled while a mid-typing edge case
> is resolved – see [Known limitations](../limitations.html).

[More screenshots]({{ site.baseurl }}/screenshots.html#documentation-and-parameters)
