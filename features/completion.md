---
layout: default
title: Code completion
parent: Features
nav_order: 2
---

# Code completion
{: .no_toc }

1. TOC
{:toc}

---

Latte+ offers context-aware completion everywhere you type, and many completions
**trigger automatically** – you don't have to press `Ctrl+Space`. Auto-popup fires
after `{`, after `'` in include paths, after `#` for block names, and more.

## Tags

Type `{` and Latte+ suggests all valid Latte tags (60+), with documentation and a
sensible insertion (closing tag inserted for paired tags, caret placed inside).

## n:attributes

Inside an HTML tag, completion offers the 28 valid `n:attributes`, correctly
distinguishing flag attributes (`n:ifcontent`) from value attributes (`n:href`), and
offering the `inner-` / `tag-` prefixed variants where they apply.

## Variables

`$variable` completion is **scope-aware** and **type-aware**. Variables declared with
`{var}`, `{varType}`, `{parameters}`, `{capture}`, `{foreach … as}`, `{for}` and
implicit Nette variables are all offered, respecting lexical scope and shadowing.

## PHP members

After `$object->` or `$array[`, Latte+ completes methods, properties and array keys
using its [PHP type inference](./type-inference.html). Chains like
`$product->category->name` resolve step by step.

## Filters

After `|`, completion offers 50+ built-in Latte/Nette filters plus any
[custom filters](../configuration/custom-extensions.html) discovered in your project,
each with documentation and parameter hints.

## Block & include names

All five `{include}` syntaxes are supported, with auto-triggering completion of block
names – including blocks defined in other files. Path completion resolves template
files and honors your [path aliases](../configuration/path-aliases.html) with fuzzy
camel-hump matching (e.g. `PaAvail` matches `PartialAvailability`).

## Classes & functions

PHP class names complete inside `{varType}`, `{templateType}` and `instanceof`
expressions; PHP functions complete inside any Latte expression. Short names match
and the fully-qualified name is inserted automatically.

## HTML attribute values

Inside `n:attr="…"` and regular attributes, the HTML5 schema provides value
completion just as it would in a plain HTML file.
