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
filters, functions, strings and comments – and every one has its own
[configurable color](../configuration/colors-code-style.html).

The HTML around your Latte tags is highlighted natively too – including inside
`n:attributes` (see [Embedded languages](./embedded-languages.html)).

![Latte tags, PHP expressions, filters and HTML coloured side by side in one template]({{ '/assets/img/screens/S01-syntax-highlighting.png' | relative_url }})

[More screenshots]({{ site.baseurl }}/screenshots.html#syntax-and-the-editor)

## Configurable n:attribute colors

`n:if`, `n:class`, `n:foreach` and friends are rendered with their own color so they
stand out from (or blend with) regular HTML attributes – your choice. Configure it
under **Settings → Editor → Color Scheme → Latte**. This is something most Latte
plugins don't expose.

## Sticky lines

As you scroll through a long template, the enclosing Latte tags – `{if}`,
`{foreach}`, `{block}`, `{define}`, `{switch}` and the other paired tags – stay
pinned to the top of the editor, just like sticky lines work for PHP or JavaScript.
You always know which block you're inside.

Scrolled deep into a gallery loop, the block, the `{foreach}` and the HTML that
opened the current context stay readable at the top of the editor.

![A block, a foreach and the surrounding HTML pinned to the top of the editor while the body is scrolled]({{ '/assets/img/screens/S02-sticky-lines.png' | relative_url }})

## Paired-tag highlighting & brace matching

Place the caret on an opening tag and its matching closing tag is highlighted, and
vice-versa. Clause tags such as `{else}`, `{elseif}`, `{case}` highlight as part of
their enclosing group. Braces `{ }`, `( )` and `[ ]` are matched as you'd expect.

With the caret on `{/foreach}`, its opening tag lights up too – and the breadcrumbs
at the bottom of the editor spell out the full path of tags leading to the caret.

![The caret on a closing foreach tag, its opening tag highlighted and breadcrumbs showing the nesting path]({{ '/assets/img/screens/S03-paired-tag-breadcrumbs.png' | relative_url }})

## Code folding

Paired tags and large blocks can be collapsed. Folding regions follow the Latte
structure, not just brace counting, and a collapsed block keeps its opening tag visible
so you still know what you folded away.

![A collapsed foreach block showing its opening tag as the placeholder]({{ '/assets/img/screens/S04-code-folding.png' | relative_url }})

## Structure view & breadcrumbs

- **Structure view** (`Alt+7`, or `Ctrl+F12` for the same tree as a popup) shows the
  file's Latte tags nested the way they are nested in the template – blocks,
  `{define}`, `{snippet}`, `{embed}`, `{include}`, `{layout}` and the control-flow
  tags – and the toolbar can sort them by name. HTML elements and `n:attributes` are
  not part of the tree; it is a map of the template's Latte structure.
- **Breadcrumbs** above the editor show the path of nested tags at the caret, so you
  can navigate up the structure with a click.

![The structure panel listing a template's Latte tags, the if inside a foreach inside a block selected]({{ '/assets/img/screens/S05-structure-view.png' | relative_url }})
