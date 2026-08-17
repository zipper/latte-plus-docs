---
layout: default
title: Screenshots
nav_order: 3
---

# Screenshots
{: .no_toc }

A tour of Latte+ in the editor. If you are deciding whether the plugin is worth
installing, this page is the fastest way to see what it actually does – every feature
described elsewhere in this documentation is shown here on real code.

All shots were taken in PhpStorm 2025.3 with the default dark theme, on a small Nette
blog with a real `vendor/` directory – so completion, hover, inspections and Find Usages
behave exactly as they do in a project of your own, not in a stripped-down demo.

1. TOC
{:toc}

---

## Syntax and the editor

### Latte and HTML in one file

Latte tags, PHP expressions, filters and `n:attributes` coloured next to the HTML they
are embedded in. Nothing is underlined and the inspection widget in the top-right corner
is green – valid templates stay clean.

![Latte and HTML highlighted together]({{ '/assets/img/screens/S01-syntax-highlighting.png' | relative_url }})

### Sticky lines

Scrolled into a nested loop: the enclosing scopes stay pinned at the top of the editor.
Latte tags and HTML elements mix freely, so `{block}`, `<article>`, `<ul>`, `{foreach}`
and `<li>` are all visible at once.

![Sticky lines pinned above the viewport]({{ '/assets/img/screens/S02-sticky-lines.png' | relative_url }})

### Paired-tag highlighting and breadcrumbs

The caret sits on `{/foreach}` and the matching `{foreach}` 14 lines above lights up with
it. The breadcrumb bar under the editor shows the full tag path at the caret.

![A closing tag highlighted together with its opening tag]({{ '/assets/img/screens/S03-paired-tag-breadcrumbs.png' | relative_url }})

### Code folding

The whole `{foreach}` collapsed into a single line. Folding regions follow the Latte tag
structure, not brace counting.

![A collapsed foreach block]({{ '/assets/img/screens/S04-code-folding.png' | relative_url }})

---

## Embedded languages

### JavaScript and CSS inside a template

Both languages fully highlighted, Latte expressions inside them still coloured as Latte,
and the JavaScript object literal on line 12 is not mistaken for a Latte tag.

![JavaScript and CSS embedded in a template]({{ '/assets/img/screens/S23-embedded-js-css.png' | relative_url }})

### PHP inside `{php}` and `{do}`

The bodies of `{php}` and `{do}` are a real injected PHP fragment – the status bar says
so – which is why `ceil()` gets a parameter-name hint like any other PHP call.

![PHP highlighted inside php and do tags]({{ '/assets/img/screens/S24-php-completion.png' | relative_url }})

---

## Completion

### Latte tags

Typing `{fo` offers the matching tags, each with the closing tag it will insert.

![Tag completion after an opening brace]({{ '/assets/img/screens/S06-tag-completion.png' | relative_url }})

### PHP members, with types

The item type comes from `{varType list<App\Model\Image> $gallery}` at the top of the
file – the loop variable inherits the collection's item type, so `$image->` completes
with the declaring class and the type of every member.

![Member completion with inferred types]({{ '/assets/img/screens/S07-member-completion.png' | relative_url }})

### Variables

File-local variables, implicit Nette ones and variables declared as custom implicit
variables in the settings, side by side – each with its origin and its type.

![Variable completion]({{ '/assets/img/screens/S08-variable-completion.png' | relative_url }})

### Filters

Filters complete after `|`, with their full parameter signature.

![Filter completion]({{ '/assets/img/screens/S09-filter-completion.png' | relative_url }})

### Block names from another file

`{include #` offers the blocks defined in the referenced file, and both suggestions carry
the file they come from.

![Block name completion across files]({{ '/assets/img/screens/S10-block-completion.png' | relative_url }})

### Paths through an alias, fuzzy

`PaGal` matches `~Parts/Gallery.latte` – the prefix is split across the directory
separator, and the alias resolves to a real directory shown on the right.

![Fuzzy path completion through an alias]({{ '/assets/img/screens/S11-alias-completion.png' | relative_url }})

### n:attributes

Inside an HTML tag, typing `n:` lists every valid `n:attribute`.

![n:attribute completion]({{ '/assets/img/screens/S12-nattr-completion.png' | relative_url }})

### Closing tags

After `{/`, the innermost unclosed tag is offered first.

![Closing tag completion]({{ '/assets/img/screens/S13-close-tag-completion.png' | relative_url }})

### Components from their factory methods

`commentForm` and `commentList` come from the `createComponent*` methods on the
presenter.

![Component completion]({{ '/assets/img/screens/S25-control-completion.png' | relative_url }})

---

## Documentation and parameters

### Tag documentation

`Ctrl+Q` on a tag shows its syntax, what it does and a link to the matching page on the
official Latte documentation.

![Quick documentation for a tag]({{ '/assets/img/screens/S14-quick-doc-tag.png' | relative_url }})

### Argument documentation from the target template

Type, declaring file and default value are read from the `{parameters}` of the included
template, and the argument is marked optional because it has a default.

![Quick documentation for an include argument]({{ '/assets/img/screens/S15-quick-doc-argkey.png' | relative_url }})

### Parameter info

`Ctrl+P` on an `{include}` shows the full parameter list of the target template, with the
current argument highlighted.

![Parameter info for an include]({{ '/assets/img/screens/S16-param-info.png' | relative_url }})

### Inlay hints

Positional arguments read as named ones: filter parameters, and the arguments of a
`{control}`.

![Inlay hints in front of positional arguments]({{ '/assets/img/screens/S17-inlay-hints.png' | relative_url }})

---

## Inspections and quick fixes

### Several problems at once

A misspelled `n:attribute`, an unknown property, an unknown filter, a block that is never
defined, a template that does not exist and a component with no factory – each reported
by its own inspection.

![Inspections in a single template]({{ '/assets/img/screens/S18-inspections.png' | relative_url }})

### Did you mean?

The misspelled `n:clas` offers the correction directly, with a preview of the result – or
you can register the name as a custom extension instead.

![Quick fix for a misspelled n:attribute]({{ '/assets/img/screens/S19-quickfix-nattr.png' | relative_url }})

### Missing template

An `{include}` pointing at a file that does not exist offers to create it.

![Quick fix creating a missing template]({{ '/assets/img/screens/S20-quickfix-create-file.png' | relative_url }})

---

## Navigation

### From a link to the code behind it

`Ctrl+B` on an `n:href` destination lands in the presenter that serves it.

![Go to declaration from a link]({{ '/assets/img/screens/S21-goto-link-target.png' | relative_url }})

### Find usages of a block across the project

Find Usages on a `{define}` lists every `{include}` that pulls it in, anywhere in the
project – here across three templates in two different directories.

![Find usages of a block]({{ '/assets/img/screens/S22-find-usages-block.png' | relative_url }})

---

## Configuration

### Path aliases

An alias maps a prefix – here `~` – to the directories it may resolve to, separately for
path-based and name-based references.

![Path alias settings]({{ '/assets/img/screens/S26-settings-path-aliases.png' | relative_url }})

### Custom extensions

Tags, filters, functions and `n:attributes` found in the project's own
`Latte\Extension` classes, with the argument signature of the `{icon}` tag and a link
back to the class that registers it.

![Custom extension settings]({{ '/assets/img/screens/S27-settings-custom-extensions.png' | relative_url }})

---

Short clips of the interactive features – live paired-tag rename, cross-file parameter
rename and smart indentation – will be added here later.
