---
layout: default
title: Smart editing
parent: Features
nav_order: 6
---

# Smart editing
{: .no_toc }

1. TOC
{:toc}

---

Latte+ makes day-to-day editing of templates feel native, with smart keys tuned for
Latte's mix of tags, HTML and indentation.

## Auto-pairing

- Typing `{` inserts the closing `}` and opens the tag-completion popup.
- Quotes (`'`, `"`) and brackets (`(`, `[`) pair automatically, with context
  awareness so they don't get in your way inside strings.
- Bracket pairs auto-expand when you press Enter between them.

## Live paired-tag rename

Rename a paired tag the way you rename `<div>` in HTML: start typing over the name in
the opening tag and **the closing tag follows along, character by character**. No
selection, no refactoring dialog, no completion popup needed.

```latte
{block foo}…{/block}     →  type "embed" over "block"  →  {embed foo}…{/embed}
```

- Works **in both directions** – editing the name in `{/block}` updates the opener too.
- A **single Ctrl+Z** reverts both halves, because the mirrored edit is part of the same
  typing command.
- Survives half-typed names (`{bl`, `{blo`) that are not valid tags yet.
- Picks the right counterpart with nested same-name tags
  (`{block a}{block b}{/block}{/block}`).
- Only the tag name changes, so an argument on the closing tag (`{/cache 'key'}`) and
  `{{double}}` syntax stay intact.
- Honours the IDE-wide **Simultaneous `<tag></tag>` editing** setting
  (Settings → Editor → Smart Keys) – no separate option to learn.

Known limitation: clauses are not renamed with their tag. Turning `{if}` into
`{foreach}` updates `{/if}`, but leaves `{elseif}` in place – see
[Limitations](../limitations.html).

## Closing-tag completion

Typing `{/` closes the nearest unclosed paired tag automatically, including multi-line
openers such as:

```latte
{embed 'Parts/Item.latte',
	link: $link,
	class: ''}
{/   ← becomes {/embed}
```

Inside an existing `{/…}` you can also invoke completion (Ctrl+Space) to pick a name –
the popup offers the tags actually open at that position, innermost first, and inserts
just the closing tag.

![A completion popup after a closing brace listing the tags open at that position, the innermost one first]({{ '/assets/img/screens/S13-close-tag-completion.png' | relative_url }})

## Smart Enter

Pressing Enter is context-aware across 10+ situations, including:

- Indenting the body after an opening paired tag.
- Aligning a closing tag with its opener.
- Expanding a multi-line Latte tag and aligning its arguments.

## Smart Tab, End & Backspace

- **Tab** on an under-indented line jumps to the expected indent level
  ([screenshot]({{ site.baseurl }}/screenshots.html#editing)).
- **End** on a whitespace-only line moves to the expected indent.
- **Backspace** in a line's indentation takes it to the indent the context calls for,
  in one press rather than space by space; where the indent is already right, it joins
  the line with the one above instead. It also deletes an auto-inserted pair
  (`{}`, `()`, `[]`, `''`, `""`) as one character.

Turn whitespace rendering on and the two roles are easy to tell apart: tabs carry the
indentation of the nested markup, spaces align the wrapped `n:attribute` values under
the first one.

![A template with whitespace rendered, showing tab indentation and space alignment of wrapped attribute values]({{ '/assets/img/screens/S32-smart-tabs.png' | relative_url }})

## Comment toggling

`Ctrl+/` toggles a Latte `{* … *}` comment around the selection or line.

## Formatter

`Ctrl+Alt+L` re-indents the Latte side of a template rather than rewriting it: there
is no Latte spacing model, so nothing gets re-spaced inside a tag. The HTML around the
tags is formatted by PhpStorm's own HTML formatter, which is why wrapped attributes
follow your HTML code style – see
[Indentation](../configuration/indentation.html) for which settings drive what.
