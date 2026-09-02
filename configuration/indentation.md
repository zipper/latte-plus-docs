---
layout: default
title: Indentation
parent: Configuration
nav_order: 9
---

# Indentation
{: .no_toc }

1. TOC
{:toc}

---

A `.latte` file mixes Latte tags with HTML, so two code style sections have a say in
how a line ends up indented. Knowing which one owns what saves a lot of guessing when
the result isn't what you expected.

## Structure vs. alignment

Latte+ splits indentation into two separate questions:

| What | Controlled by |
|---|---|
| **Structural indentation** of the `.latte` file – the indent level of every line, no matter whether it starts with a Latte tag or an HTML tag | **Settings → Code Style → Latte** (`Use tab character`, `Indent`) |
| **Alignment of wrapped HTML attributes**, and the continuation indent used when they are not aligned | **Settings → Code Style → HTML** (`Smart tabs`, `Align attributes`, `Continuation indent`) |

### Why structure has a single source

To the IDE, a `.latte` file is one file with one base language. Structural indentation
therefore has exactly one owner: the **Latte** code style. If Latte tags took their
indent from the Latte settings while HTML tags took theirs from the HTML settings, a
single template could end up with two different indent widths – or a mix of tabs and
spaces – depending on what each line happens to start with.

So even this HTML-only fragment is indented according to your Latte settings:

```latte
{block content}
	<ul>
		<li n:foreach="$items as $item">{$item->name}</li>
	</ul>
{/block}
```

### Why alignment follows HTML

Alignment is a property of a concrete construct, not of the file as a whole. Wrapping
attributes over several lines is an **HTML** construct, so it follows your HTML code
style – also inside `.latte`:

```latte
<a href="{$link}"
   n:class="$active ? 'is-active'">
	{$label}
</a>
```

Continuation indent works the same way: it belongs to whoever owns the tag being
continued.

| Tag being continued | Continuation indent taken from |
|---|---|
| HTML tag (`<a href=… n:class=…>`) | **HTML** code style |
| Latte tag (`{parameters …}`, `{if …}`) | **Latte** code style |

The Latte panel deliberately has **no `Smart tabs` option**: Latte tag arguments are
not aligned, and the alignment of HTML attributes is governed by the HTML settings
above.

## Keep the Latte and HTML settings in sync

Because both sections contribute to the same file, they should agree with each other.

Use the **same `Use tab character` and the same indent size** in
**Code Style → Latte** and **Code Style → HTML**. Otherwise structural indentation and
attribute continuation lines are measured in different units, and the outcome will look
wrong no matter which of the two settings you change.

> If you indent with tabs, `Smart tabs` in **Code Style → HTML** is what keeps
> attribute alignment usable: the structural indent stays tabs, and only the alignment
> padding on top of it uses spaces.

With whitespace rendering turned on you can see the two apart – dashes are the
structural tabs, dots the alignment padding:

![Tab indentation and space alignment shown with whitespace rendering]({{ '/assets/img/screens/S32-smart-tabs.png' | relative_url }})

[More screenshots]({{ site.baseurl }}/screenshots.html#editing)

## The same model for every action

All indentation-producing actions resolve the indent through the model above, so they
agree with each other by design:

- **Enter** – a new line lands at the indent the formatter would give it.
- **Tab** / **Shift+Tab** on a selection – indent or unindent a block.
- **Ctrl+Alt+L** (Reformat Code).
- **Ctrl+Alt+I** (Auto-Indent Lines).

In other words, pressing Enter puts the line exactly where reformatting the file would
put it. If one of these actions surprises you, the cause is almost always a setting
(or an `.editorconfig` file, below), not the individual action.

## `.editorconfig` overrides your code style

> **Watch out for `.editorconfig`.**
> If your project contains one, its keys **take precedence over
> Settings → Code Style**. Switching to spaces or turning `Smart tabs` off in Settings
> then has no visible effect at all.

The keys that matter for templates are `indent_style`, `indent_size` and
`ij_smart_tabs`. A block like this one wins over anything you configure in Settings:

```ini
root = true

[*]
indent_style = tab

[{*.html,*.latte}]
ij_smart_tabs = true
```

Two things make this easy to miss:

- `.editorconfig` files **cascade**. The one that affects your template may sit in a
  parent directory, not in the project root, and several of them can apply at once
  (up to the nearest `root = true`).
- After you edit or delete an `.editorconfig` file, the change **may not be picked up
  immediately**. Reopen the template – or the project – before concluding that it made
  no difference.

> Before you go looking for an indentation bug, check for `.editorconfig` in the
> project and in its parent directories. It is by far the most common reason why a code
> style change appears to be ignored.
