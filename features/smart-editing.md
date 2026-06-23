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

## Smart Enter

Pressing Enter is context-aware across 10+ situations, including:

- Indenting the body after an opening paired tag.
- Aligning a closing tag with its opener.
- Expanding a multi-line Latte tag and aligning its arguments.

## Smart Backspace

A universal un-indent rule removes a full indent step at a time and preserves tag
alignment, instead of deleting a single space.

## Smart Tab, End & Space

- **Tab** on an under-indented line jumps to the expected indent level.
- **End** on a whitespace-only line moves to the expected indent.
- **Space** merges lines and trims context-appropriately.

## Comment toggling

`Ctrl+/` toggles a Latte `{* … *}` comment around the selection or line.

## Formatter

A dedicated formatter (`Ctrl+Alt+L`) reflows Latte templates. Its behavior is
configurable under **Settings → Code Style → Latte** – see
[Colors & code style](../configuration/colors-code-style.html) for the options
(spaces in parentheses/brackets, single-line paired tags, block indentation).
