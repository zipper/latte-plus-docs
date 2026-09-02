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

A variable that a `{varType}` types and a presenter fills is the interesting case here.
On such a variable the hover adds a **`Set by Class::method().`** line under the type,
so the declaration and the source of the value are one popup apart. The same wording
names the second target when you jump to the declaration instead.

![The two declarations offered for a typed variable, the second naming the method that sets the value]({{ '/assets/img/screens/S37-goto-typed-variable.png' | relative_url }})

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
- **Include argument types** – the type each `{include}` or `{embed}` argument is
  declared with in the template you are calling.
- **Custom Latte tag parameters** – the positional argument names from the signature
  you configured for a project-custom tag, such as `{imageTag}` or `{webpack}`.

![Inlay hints naming the positional arguments of filters, a control tag and a link]({{ '/assets/img/screens/S17-inlay-hints.png' | relative_url }})

> One gap worth knowing: while you are still typing an argument
> (`{include 'x.latte', name:`), neither the inlay hint nor `Ctrl+Q` names that
> argument's type – both wait until the value is there.

[More screenshots]({{ site.baseurl }}/screenshots.html#documentation-and-parameters)
