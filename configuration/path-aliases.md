---
layout: default
title: Path aliases
parent: Configuration
nav_order: 1
---

# Path aliases

Projects often reference templates with a shorthand prefix instead of a full relative
path. Path aliases tell Latte+ how to resolve those prefixes so that completion,
navigation and the *missing file* inspection all work.

## What it does

Under **Settings → Languages & Frameworks → Latte+ → Path Aliases** you define one or
more aliases. Each alias is a prefix with a description and **two independent lists of
folders**: the path-based one answers a reference that carries a slash or a file
extension (`{include '~Home/default.latte'}`, `{include '~Gallery.latte'}`), the
name-based one is searched recursively for a reference with neither
(`{include '~gallery'}`), which is how a component is named. Leave a list empty and
that kind of reference simply is not resolved for this alias.

| Prefix | Path-based folders | Name-based folders |
|---|---|---|
| `@layout` | `app/Presentation/@layout` | – |
| `~` | `app/Presentation`, `app/Components` | `app/Components` |

With the aliases above, both of these resolve correctly:

```latte
{include '@layout/base.latte'}
{include '~Home/default.latte'}
```

![The Path Aliases settings page with one alias and its search paths]({{ '/assets/img/screens/S26-settings-path-aliases.png' | relative_url }})

[More screenshots]({{ site.baseurl }}/screenshots.html#configuration)

## Completion

Alias-prefixed paths complete with **fuzzy camel-hump matching**: typing
`{include '~PaAvail'}` matches `PartialAvailability` across path segments, so you can
reach deeply nested templates with a few characters.

A handful of camel-hump letters is enough, and each suggestion shows which search path
it was found in.

![Fuzzy completion of an include path, the suggestions showing the search path each one came from]({{ '/assets/img/screens/S11-alias-completion.png' | relative_url }})

## Tips

- Point an alias at multiple search paths to let one prefix span several roots.
- Aliases are resolved for every include syntax, not just string literals.
