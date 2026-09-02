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
  have is reported too, not just an unknown presenter. A destination that starts with a
  slash is a URL, not a presenter path, so `{link //Product:detail}` is left alone
  instead of being reported as an unknown presenter. An inner slash stays checked –
  `Front:product/detail` is a legitimate path segment.
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

A signal (`{link save!}`) counts only when the handler is one Nette can actually
dispatch – public, non-static, non-abstract – so completion no longer offers the
inherited `invalidLink!`, and a link to a `protected` handler is reported instead of
failing at runtime. In a template that no presenter or component owns, a signal still
gets the one question that survives the missing owner: does any class in the project
declare that handler at all? That answer is deliberately coarser – with no owner there
is no handler list to suggest a spelling from, so it names no candidate and offers no
fix.

### One destination, two places to land

`Ctrl+B` on a link destination offers **two** targets, and the action's template is the
**first** of them:

```latte
<a n:href="Product:detail $product->id">{$product->name}</a>
```

1. **`Product/detail.latte`** – the template that renders the action.
2. **`ProductPresenter::renderDetail()`** (or `actionDetail()`) – the method behind it.

The same holds in `{link}` and `{plink}`, so the template you are about to edit is a
keystroke away from the link that leads to it.

![Go to declaration on a link destination offering the target template and the presenter render method]({{ '/assets/img/screens/S21-goto-link-target.png' | relative_url }})

## `{form}` and form fields

Form tags, field names, containers and the owning form are recognized, so field
references are validated and navigable. Inside a `{form}`, `{input }` completes the
fields the form factory builds, each with the method that created it – and PHP classes
alongside them, because a field name may be a constant.

An unknown form, an unknown field and a render method that does not exist are reported
rather than passed over, and the same check runs on `<form n:name="…">` and
`<input n:name="…">` as inside `{form}`. A misspelled name comes with a quick fix that
renames it to the closest one the factory really builds.

![Completion of form field names inside a form tag]({{ '/assets/img/screens/S30-form-input-completion.png' | relative_url }})

## `{snippet}` and AJAX

`{snippet}` and `{snippetArea}` names are recognised and checked within the template
that declares them. A snippet redrawn from PHP by name is not linked back to the
template yet.

## Layouts, `{include}` and `{embed}`

The layout/`{extends}` hierarchy is understood, so blocks defined in a child template
resolve against the parent, and `{include}` / `{embed}` targets are navigable across
the project (see [Navigation & refactoring](./navigation-refactoring.html)).
