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

A few letters narrow the list, and each paired tag shows the closing tag that will be
inserted along with it.

![Tag completion after an opening brace, paired tags showing the closing tag that comes with them]({{ '/assets/img/screens/S06-tag-completion.png' | relative_url }})

## n:attributes

Inside an HTML tag, completion offers the 28 valid `n:attributes`, correctly
distinguishing flag attributes (`n:ifcontent`) from value attributes (`n:href`), and
offering the `inner-` / `tag-` prefixed variants where they apply.

## Variables

`$variable` completion is **scope-aware** and **type-aware**. Variables declared with
`{var}`, `{varType}`, `{parameters}`, `{capture}`, `{foreach … as}`, `{for}` and
implicit Nette variables are all offered, respecting lexical scope and shadowing.

Every suggestion carries its type and where it comes from – a declaration in this file,
an implicit Nette variable, or one you configured yourself.

![Variable completion listing each variable with its type and origin, including implicit and custom ones]({{ '/assets/img/screens/S08-variable-completion.png' | relative_url }})

## PHP members

After `$object->` or `$array[`, Latte+ completes methods, properties and array keys
using its [PHP type inference](./type-inference.html). Chains like
`$product->category->name` resolve step by step.

![Member completion on a foreach variable, each item showing its declaring class and type]({{ '/assets/img/screens/S07-member-completion.png' | relative_url }})

[More screenshots]({{ site.baseurl }}/screenshots.html#completion)

## Filters

After `|`, completion offers 50+ built-in Latte/Nette filters plus any
[custom filters](../configuration/custom-extensions.html) discovered in your project,
each with documentation and parameter hints.

The list is matched anywhere in the name, and every filter shows the arguments it
accepts right in the popup.

![Filter completion after a pipe, each filter listed with the arguments it accepts]({{ '/assets/img/screens/S09-filter-completion.png' | relative_url }})

## Block & include names

All five `{include}` syntaxes are supported, with auto-triggering completion of block
names – including blocks defined in other files. Path completion resolves template
files and honors your [path aliases](../configuration/path-aliases.html) with fuzzy
camel-hump matching (e.g. `PaAvail` matches `PartialAvailability`).

Once the file is named, the block names it defines are offered too, so you don't have
to open the other template to remember them.

![Block name completion in an include, listing the blocks defined by the referenced template]({{ '/assets/img/screens/S10-block-completion.png' | relative_url }})

## Include arguments

After the template path, completion offers the parameters the target template declares
in its `{parameters}` line – with the type of each one and whether it is required or
optional – so you can pass arguments correctly without opening the other file.

![Completion of the arguments an included template declares, each with its type and whether it is required]({{ '/assets/img/screens/S29-include-args-completion.png' | relative_url }})

## Classes & functions

PHP class names complete inside `{varType}`, `{templateType}` and `instanceof`
expressions; PHP functions complete inside any Latte expression. Short names match
and the fully-qualified name is inserted automatically.

## HTML attribute values

Inside `n:attr="…"` and regular attributes, the HTML5 schema provides value
completion just as it would in a plain HTML file.
