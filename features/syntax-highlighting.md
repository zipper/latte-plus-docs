---
layout: default
title: Syntax & editor visuals
parent: Features
nav_order: 1
---

# Syntax & editor visuals
{: .no_toc }

1. TOC
{:toc}

---

## Syntax highlighting

Every Latte 3.x construct is highlighted: tags, keywords, `n:attributes`, variables,
filters, functions, strings and comments — and every one has its own
[configurable color](../configuration/colors-code-style.html).

The HTML around your Latte tags is highlighted natively too — including inside
`n:attributes` (see [Embedded languages](./embedded-languages.html)).

## Configurable n:attribute colors

`n:if`, `n:class`, `n:foreach` and friends are rendered with their own color so they
stand out from (or blend with) regular HTML attributes — your choice. Configure it
under **Settings → Editor → Color Scheme → Latte**. This is something most Latte
plugins don't expose.

## Sticky lines

As you scroll through a long template, the enclosing Latte tags — `{if}`,
`{foreach}`, `{block}`, `{define}`, `{switch}` and 27+ other paired tags — stay
pinned to the top of the editor, just like sticky lines work for PHP or JavaScript.
You always know which block you're inside.

## Paired-tag highlighting & brace matching

Place the caret on an opening tag and its matching closing tag is highlighted, and
vice-versa. Clause tags such as `{else}`, `{elseif}`, `{case}` highlight as part of
their enclosing group. Braces `{ }`, `( )` and `[ ]` are matched as you'd expect.

## Code folding

Paired tags and large blocks can be collapsed. Folding regions follow the Latte
structure, not just brace counting.

## Structure view & breadcrumbs

- **Structure view** (`Alt+7`) shows the hierarchy of Latte tags in the file.
- **Breadcrumbs** above the editor show the path of nested tags at the caret, so you
  can navigate up the structure with a click.
