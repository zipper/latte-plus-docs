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

A `.latte` file is rarely *just* Latte — it's HTML with PHP expressions, plus
JavaScript and CSS. Latte+ gives each embedded language native IDE support while
keeping the Latte parser intact.

## HTML

Latte+ maintains a **real HTML tree** alongside the Latte tree (the "dual PSI"
model). HTML completion, inspections and `n:attributes` all work natively, and you
won't get false *"tag start is not closed"* errors from Latte tags that span HTML
boundaries.

## PHP inside `{php}` and `{do}`

PHP tags get **full PhpStorm language injection** — completion, inspections,
navigation and refactoring all work inside the PHP body:

```latte
{php $total = array_sum($prices)}
{do $logger->info('rendered')}
```

Latte+ detects the appropriate mode automatically:

- **Strict mode** (the default for `{do}` and basic `{php}`) — a single expression,
  no statement separators.
- **Permissive mode** (when `RawPhpExtension` is enabled) — full multi-statement raw
  PHP.

The mode is auto-detected from a file marker → your
[PHP mode setting](../configuration/php-mode.html) → the presenter chain → a project
scan, and can be overridden in settings. Type flow carries from `{php}` assignments
into later Latte expressions (see [type inference](./type-inference.html)).

## JavaScript & CSS

JavaScript inside `<script>`, CSS inside `<style>`, and values in inline `style=""`
and event `on*=""` attributes behave just like they do in a plain HTML file —
completion, inspections and formatting included.

Crucially, JS object literals `{ }` and CSS rule blocks `{ }` **do not** confuse the
Latte parser. The `n:syntax="double"` and `n:syntax="off"` switches are fully
respected, so you can opt parts of a template out of Latte interpolation when your
JS/CSS needs literal braces.
