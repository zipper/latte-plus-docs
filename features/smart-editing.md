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

## Smart Enter

Pressing Enter is context-aware across 10+ situations, including:

- Indenting the body after an opening paired tag.
- Aligning a closing tag with its opener.
- Expanding a multi-line Latte tag and aligning its arguments.

## Smart Backspace

A universal un-indent rule removes a full indent step at a time and preserves tag
alignment, instead of deleting a single space.

## Smart Tab, End & Space

- **Tab** on an under-indented line jumps to the expected indent level
  ([screenshot]({{ site.baseurl }}/screenshots.html#editing)).
- **End** on a whitespace-only line moves to the expected indent.
- **Space** merges lines and trims context-appropriately.

## Comment toggling

`Ctrl+/` toggles a Latte `{* … *}` comment around the selection or line.

## Formatter

A dedicated formatter (`Ctrl+Alt+L`) reflows Latte templates. Its behavior is
configurable under **Settings → Code Style → Latte** – see
[Colors & code style](../configuration/colors-code-style.html) for the options
(spaces in parentheses/brackets, single-line paired tags, block indentation).

Reformatting, `Ctrl+Alt+I`, Enter and Tab all derive the indent from the same model, so
they never disagree – see [Indentation](../configuration/indentation.html) for which
settings drive what.
