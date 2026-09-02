---
layout: default
title: Colors & code style
parent: Configuration
nav_order: 8
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

## Code style

**Settings → Code Style → Latte** has two tabs. **Tabs and Indents** carries the usual
indent settings, plus one Latte-specific switch; **Paired Tag Insert** decides which
paired tags completion inserts on a single line instead of opening a body for you.

| Setting | Where | Effect |
|---|---|---|
| **Indent content inside top-level `{block}`** | Tabs and Indents | Indents the body of a top-level `{block}` that has no `{/block}`. Off by default. |
| Two lists of paired tags | Paired Tag Insert | A tag in the **Single-line** list is inserted as `{first}…{/first}` on one line; everything in the **3-line** list opens a body. Single-line by default: `first`, `last`, `sep`, `translate`, `label`. |

Inserting a paired tag in the middle of a line always produces the single-line form,
whichever list the tag is in.

Reformatting a template (`Ctrl+Alt+L`) sets the indentation of the Latte tags
themselves; the HTML around them goes through PhpStorm's own HTML formatter, so your
**HTML** code style decides how markup and wrapped attributes come out. Which setting
drives what is explained in [Indentation](./indentation.html).
