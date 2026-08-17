---
layout: default
title: Nette integration
parent: Features
nav_order: 7
---

# Nette integration
{: .no_toc }

1. TOC
{:toc}

---

Latte is the template engine of the [Nette Framework](https://nette.org/), and
Latte+ understands the Nette-specific tags that connect templates to presenters,
components, routing and forms.

## `{control}`

```latte
{control reviews}
{control productGallery:thumbnails}
```

- Completion after `{control ` lists the `createComponentX()` factories from the
  presenter/component inheritance chain.
- After `:` it lists the component's `render*` methods.
- Hover (`Ctrl+Q`) shows the factory/render method signature and PHPDoc.
- `Ctrl+B` jumps to the `createComponentX()` factory – or, after `:`, to the `renderX`
  method; Find Usages lists every `{control x}`.
- A did-you-mean quick fix suggests the closest component when the name is misspelled.

![Component completion listing the createComponent factories of the presenter]({{ '/assets/img/screens/S25-control-completion.png' | relative_url }})

[More screenshots]({{ site.baseurl }}/screenshots.html#completion)

## `{link}` and `n:href`

```latte
<a n:href="Product:detail $product->id">{$product->name}</a>
```

- Link destinations (`Presenter:action`) are resolved and validated.
- Route parameters and the target action's signature are surfaced as
  [parameter info](./documentation-hints.html).

## `{form}` and form fields

Form tags, field names, containers and the owning form are recognized, so field
references are validated and navigable. Inside a `{form}`, `{input }` completes the
fields the form factory builds, each with the method that created it – and PHP classes
alongside them, because a field name may be a constant.

![Completion of form field names inside a form tag]({{ '/assets/img/screens/S30-form-input-completion.png' | relative_url }})

## `{snippet}` and AJAX

`{snippet}` / `{snippetArea}` names are recognized and validated, including snippets
spread across files.

## Layouts, `{include}` and `{embed}`

The layout/`{extends}` hierarchy is understood, so blocks defined in a child template
resolve against the parent, and `{include}` / `{embed}` targets are navigable across
the project (see [Navigation & refactoring](./navigation-refactoring.html)).
