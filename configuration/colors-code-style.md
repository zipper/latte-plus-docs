---
layout: default
title: Colors & code style
parent: Configuration
nav_order: 5
---

# Colors & code style
{: .no_toc }

1. TOC
{:toc}

---

## Token colors

Under **Settings → Editor → Color Scheme → Latte** every Latte token type has its own
configurable color and font style, including:

- Latte keywords and tags
- `n:attributes` (a separate, independently configurable color)
- Variables, functions and filters
- PHP expressions inside tags
- Strings and comments

A live preview shows your changes against sample Latte code, and the colors are saved
per color scheme so your light and dark themes can differ.

![Latte token colour settings with a live preview]({{ '/assets/img/screens/S28-settings-colors.png' | relative_url }})

[More screenshots]({{ site.baseurl }}/screenshots.html#token-colors)

## Code style & formatter

Under **Settings → Code Style → Latte** you control how the formatter
(`Ctrl+Alt+L`) reflows templates:

| Option | Effect | Default |
|---|---|---|
| **Spaces within parentheses** | `( $foo )` vs `($foo)` | off |
| **Spaces within brackets** | `[ 1, 2 ]` vs `[1, 2]` | off |
| **Single-line paired tags** | Comma-separated list of paired tags kept on one line (e.g. `first,last,sep,translate,label`) | `first,last,sep,translate,label` |
| **Indent inside top-level block** | Indent the body of a top-level `{block}` that has no `{/block}` | off |

Tabs and indent size are configured in the same panel (`Use tab character`, `Indent`),
and per-file overrides (detected indents) are respected. Which of them applies to what
– and why wrapped HTML attributes follow your **HTML** code style instead – is
explained in [Indentation](./indentation.html).
