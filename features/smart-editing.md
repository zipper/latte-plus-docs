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
- Inside an `n:attribute` value, brackets and quotes pair by the rules of the Latte
  expression there; in the element content and in plain attributes next to it they are
  left alone, as in HTML. The quote the attribute value itself is written in is never
  paired – at the end of the value it steps over the closing quote.
- In a `class` value and in the strings of `n:class`, `[` and `(` are paired and their
  `]` and `)` stepped over, so Tailwind arbitrary values such as `w-[12px]` or
  `px-(--gap)` type naturally. A plain `.html`
  file keeps the platform's behaviour.

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
  (Settings → Editor → General → Smart Keys) – no separate option to learn.

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

On a Latte 3 project the popup respects HTML comments: inside `<!-- … -->` it offers only
the tags opened in that same comment, and outside every comment it leaves out tags opened
inside one, because Latte 3 cannot pair a closer across that boundary.

![A completion popup after a closing brace listing the tags open at that position, the innermost one first]({{ '/assets/img/screens/S13-close-tag-completion.png' | relative_url }})

## Closing a tag indents its body

Paste a chunk of markup and it usually lands on column 0. Finish the closing tag around
it and Latte+ gives every line from the opening tag down to the closer the indent
Reformat Code would give it:

```latte
{if $showGallery}
<div class="gallery">
<img n:foreach="$photos as $photo" src="{$photo->url}">
</div>
{/   ← becomes {/if}, and the three lines above move one level in
```

- It works for Latte closers (`{/block}`, `{/if}`, …) and HTML ones (`</div>`, …),
  whether the closer is completed automatically, typed by hand, typed over an existing
  one or picked from completion – and with several carets at once.
- Only the indent of the lines changes; the text inside them stays as it is.
- The first **Ctrl+Z** takes back just the indent, leaving the closer you typed.
- Elements over 300 lines and files over 30 000 characters are left alone.
- It can be switched off under **Settings → Editor → General → Smart Keys** –
  *Latte: indent the body of a tag when typing its closing tag*.

## Smart Enter

Pressing Enter is context-aware across 10+ situations, including:

- Indenting the body after an opening paired tag.
- Aligning a closing tag with its opener.
- Expanding a multi-line Latte tag and aligning its arguments.
- Deciding where the `}` of a multi-line tag goes by the end of the line before the
  caret: after a binary operator or a comma it moves one continuation indent in, so the
  expression goes on there; otherwise the tag is finished and the `}` goes back under
  the tag, also when more text follows it (`}</p>`).
- Splitting a class list. In a string of `n:class`, Enter closes the string, adds a
  comma and opens a new string with the same quote on the next line, aligned with the
  first item. Pressed right before the closing `'` of `'btn btn-primary'`, it gives:

  ```latte
  <div n:class="'btn btn-primary',
                ''">
  ```

  with the caret inside the new `''`. The same works in the value of a `key => '…'` or
  `key: '…'` item, in the `else` branch of a ternary and on the right of `??`. Everywhere
  else – the `then` branch of a ternary, parentheses, an array, a function argument –
  Enter only breaks the line.
- Leaving a line that starts inside a multi-line string literal alone: its leading
  whitespace is part of the string, so Enter writes no indent there.

Enter, Tab, End and Backspace all ask the same question – where does this line belong? –
so they agree with each other on every line of a multi-line tag, an HTML tag header and
a line typed too deep or too shallow.

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

Reformat Code and Auto-Indent Lines (`Ctrl+Alt+I`) lay out a few shapes the way the
editor writes them while you type:

- The continuation lines of a multi-line tag (`{if $a &&` / `$b}`, `{var}`, `{=…}`,
  `{foreach}`) stay one continuation indent below the tag.
- A call, a `new`, a parenthesised expression or filter arguments broken over several
  lines inside a tag sit one level below the line holding the `(`.
- The body of a `{case}` or `{default}` branch goes one level deeper than the branch.
- A multi-line HTML comment or `{* … *}` comment moves as a whole and keeps its own
  alignment, as in an HTML file.
- Inside `<pre>`, `<textarea>`, `<span>` and the other elements listed in the HTML
  **Keep white spaces inside** option, Latte tags keep their whitespace, as the
  surrounding HTML does – in `<pre>` and `<textarea>` that whitespace is what the page
  shows and the form sends.
- A Latte tag written over several lines inside a plain HTML attribute value
  (`class="{=…}"`, `title`, `data-*`) keeps its lines as typed, as the value text does
  in an HTML file.
