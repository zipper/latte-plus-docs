---
layout: default
title: Embedded languages
parent: Features
nav_order: 8
---

# Embedded languages
{: .no_toc }

1. TOC
{:toc}

---

A `.latte` file is rarely *just* Latte – it's HTML with PHP expressions, plus
JavaScript and CSS. Latte+ gives each embedded language native IDE support without
breaking the Latte syntax around them.

## HTML

HTML completion, inspections and `n:attributes` all work natively inside your
templates, and you won't get false *"tag start is not closed"* errors from Latte tags
that wrap or span HTML elements.

PhpStorm's own HTML inspections keep running in a `.latte` file: a misspelled tag or
attribute name, a missing `alt` or `lang`, an unbound namespace prefix or a stray closing
tag are reported exactly as they would be in a plain `.html` file. They are quieted only
at the spot where a Latte tag or an `n:attribute` is the thing the HTML parser cannot
read – never across the whole file.

## PHP inside `{php}` and `{do}`

PHP tags get full IDE support – completion, inspections, navigation and refactoring
all work inside the PHP body:

```latte
{php $total = array_sum($prices)}
{do $logger->info('rendered')}
```

The body is edited as real PHP – highlighted, completed and inspected, with argument
hints on function calls, exactly as in a `.php` file.

![PHP tags whose bodies are highlighted as PHP, with an argument hint on a function call]({{ '/assets/img/screens/S24-php-completion.png' | relative_url }})

Latte+ detects the appropriate mode automatically:

- **Strict mode** (the default for `{do}` and basic `{php}`) – a single expression,
  no statement separators.
- **Permissive mode** (when `RawPhpExtension` is enabled) – full multi-statement raw
  PHP.

The mode is auto-detected from a file marker → your
[PHP mode setting](../configuration/php-mode.html) → the presenter chain → a project
scan, and can be overridden in settings. Type flow carries from `{php}` assignments
into later Latte expressions (see [type inference](./type-inference.html)).

## JavaScript & CSS

JavaScript inside `<script>`, CSS inside `<style>`, and values in inline `style=""`
and event `on*=""` attributes behave just like they do in a plain HTML file –
completion, inspections and formatting included.

Crucially, JS object literals `{ }` and CSS rule blocks `{ }` are **not** mistaken
for Latte tags. The `n:syntax="double"` and `n:syntax="off"` switches are fully
respected, so you can opt parts of a template out of Latte interpolation when your
JS/CSS needs literal braces.

![A template with a style block and a script block, both highlighted, with Latte expressions inside them]({{ '/assets/img/screens/S23-embedded-js-css.png' | relative_url }})

[More screenshots]({{ site.baseurl }}/screenshots.html#embedded-languages)
