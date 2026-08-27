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

- Link destinations (`Presenter:action`) are resolved and validated – in `{link}` and
  `{plink}`, in `n:href`, and in `{ifCurrent}`. An action the target presenter does not
  have is reported too, not just an unknown presenter.
- Route parameters and the target action's signature are surfaced as
  [parameter info](./documentation-hints.html).
- Your project's own `application: mapping` is read from its NEON configuration, so
  custom prefixes, modules without a `Module` suffix, presenters kept outside the main
  namespace and several mapping roots in one project all resolve – see
  [presenter mapping]({{ '/configuration/presenter-mapping.html' | relative_url }}).
  Once a mapping is read, completion offers exactly the destinations it can route.
- Without such a configuration the layouts the Nette skeletons ship are recognised
  anyway: `app/Presentation` with each presenter in a directory of its own name (the
  current default) and modules as plain directories, the older `app/UI`, and the
  classic `app/Presenters` with `*Module` directories. What that costs is in
  [known limitations]({{ '/limitations.html' | relative_url }}).

`Ctrl+B` on a destination offers both places it stands for: the template that renders
the action and the presenter method behind it.

![Go to declaration on a link destination offering the target template and the presenter render method]({{ '/assets/img/screens/S21-goto-link-target.png' | relative_url }})

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
