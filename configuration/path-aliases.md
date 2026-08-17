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
more aliases. Each alias is a **prefix** mapped to one or more **search paths**.

| Prefix | Search paths |
|---|---|
| `@layout` | `app/Presentation/@layout` |
| `~` | `app/Presentation`, `app/Components` |

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

## Tips

- Point an alias at multiple search paths to let one prefix span several roots.
- Aliases are resolved for every include syntax, not just string literals.
