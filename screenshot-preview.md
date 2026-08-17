---
layout: default
title: Screenshot preview
nav_exclude: true
search_exclude: true
---

# Screenshot preview
{: .no_toc }

A working page for reviewing the screenshot set before the shots are placed into the
documentation pages and the Marketplace listing. It is deliberately kept out of the
navigation and out of search.

All shots were taken in PhpStorm 2025.3 with the default dark theme, on the
[demo project]({{ site.baseurl }}/) built for this purpose - a small Nette blog with a real
`vendor/`, so component completion, hover and inspections behave as they do in a real
codebase. Captured at 1600 x 1000 and exported to 1280 x 800, which is the ratio the
Marketplace expects from every image in a listing.

1. TOC
{:toc}

---

## Syntax and the editor

### Latte and HTML in one file

Latte tags, PHP expressions, filters and HTML coloured side by side. `n:class` carries the
same colour as a plain `class` attribute. Nothing is underlined and the right-hand margin is
clean - that is the point of this shot.

![Latte and HTML highlighted together]({{ '/assets/img/screens/S01-syntax-highlighting.png' | relative_url }})

### Sticky lines

Scrolled into a nested loop: the enclosing scopes stay pinned at the top of the editor.
Latte tags and HTML elements are mixed, so `{block}`, `<article>`, `<ul>`, `{foreach}` and
`<li>` are all visible at once.

![Sticky lines pinned above the viewport]({{ '/assets/img/screens/S02-sticky-lines.png' | relative_url }})

### Inlay hints

Positional arguments read as named ones: filter parameters, and the `{control}` argument.

![Inlay hints in front of positional arguments]({{ '/assets/img/screens/S17-inlay-hints.png' | relative_url }})

### JavaScript and CSS inside a template

Both languages fully highlighted, Latte expressions inside them still coloured as Latte,
and no false errors around the holes.

![JavaScript and CSS embedded in a template]({{ '/assets/img/screens/S23-embedded-js-css.png' | relative_url }})

---

## Completion

### Latte tags

![Tag completion after an opening brace]({{ '/assets/img/screens/S06-tag-completion.png' | relative_url }})

### PHP members, with types

The item type comes from `{varType list<App\Model\Image> $gallery}` two lines above - the
loop variable inherits the collection's item type.

![Member completion with inferred types]({{ '/assets/img/screens/S07-member-completion.png' | relative_url }})

### Variables

File-local variables and implicit ones side by side, each with its type.

![Variable completion]({{ '/assets/img/screens/S08-variable-completion.png' | relative_url }})

### Filters

![Filter completion]({{ '/assets/img/screens/S09-filter-completion.png' | relative_url }})

### Block names from another file

Both suggestions carry the file they come from.

![Block name completion across files]({{ '/assets/img/screens/S10-block-completion.png' | relative_url }})

### Paths through an alias, fuzzy

`PaGal` matches `~Parts/Gallery.latte` - the prefix is split across the directory separator.

![Fuzzy path completion through an alias]({{ '/assets/img/screens/S11-alias-completion.png' | relative_url }})

### n:attributes

![n:attribute completion]({{ '/assets/img/screens/S12-nattr-completion.png' | relative_url }})

### Closing tags

![Closing tag completion]({{ '/assets/img/screens/S13-close-tag-completion.png' | relative_url }})

### Components from their factory methods

`commentList` and `commentForm` come from `createComponent*` methods on the presenter.

![Component completion]({{ '/assets/img/screens/S25-control-completion.png' | relative_url }})

### PHP inside a Latte tag

![PHP completion inside a do tag]({{ '/assets/img/screens/S24-php-completion.png' | relative_url }})

---

## Documentation and parameters

### Tag documentation

![Quick documentation for a tag]({{ '/assets/img/screens/S14-quick-doc-tag.png' | relative_url }})

### Argument documentation from the target template

Type and default value are read from the `{parameters}` of the included file.

![Quick documentation for an include argument]({{ '/assets/img/screens/S15-quick-doc-argkey.png' | relative_url }})

### Parameter info

![Parameter info for an include]({{ '/assets/img/screens/S16-param-info.png' | relative_url }})

---

## Inspections and quick fixes

### Several problems at once

![Inspections in a single template]({{ '/assets/img/screens/S18-inspections.png' | relative_url }})

### Did you mean?

A misspelled `n:attribute` offers the correction directly.

![Quick fix for a misspelled n:attribute]({{ '/assets/img/screens/S19-quickfix-nattr.png' | relative_url }})

### Missing template

![Quick fix creating a missing template]({{ '/assets/img/screens/S20-quickfix-create-file.png' | relative_url }})

---

## Navigation

### From a link to both the template and the method

`Ctrl+B` on a link offers the target template *and* the presenter method behind it.

![Go to declaration from a link]({{ '/assets/img/screens/S21-goto-link-target.png' | relative_url }})

### Find usages of a block across the project

![Find usages of a block]({{ '/assets/img/screens/S22-find-usages-block.png' | relative_url }})

---

## Still missing

- Configuration screens: path aliases, custom extensions, token colours.
- The clips: live paired-tag rename, writing a `{foreach}` from scratch, cross-file
  parameter rename, smart indentation, sticky lines while scrolling.
