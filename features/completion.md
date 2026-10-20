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

An attribute that Latte compiles without a value is inserted bare, without an empty
`=""` to delete afterwards; `n:block` and `n:snippet` keep the pre-filled name slot,
because a name is what you are about to type there anyway.

![n:attribute completion inside an HTML tag]({{ '/assets/img/screens/S12-nattr-completion.png' | relative_url }})

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
to open the other template to remember them. Behind `from`, that means the blocks of the
named template and of its own `{import}` and `{layout}` – the ones Latte reads there –
and none from the template you are editing.

![Block name completion in an include, listing the blocks defined by the referenced template]({{ '/assets/img/screens/S10-block-completion.png' | relative_url }})

## Include arguments

After the template path, completion offers the parameters the target template declares
in its `{parameters}` line – with the type of each one and whether it is required or
optional – so you can pass arguments correctly without opening the other file.

![Completion of the arguments an included template declares, each with its type and whether it is required]({{ '/assets/img/screens/S29-include-args-completion.png' | relative_url }})

## Assets

Inside `{asset '…'}`, `{preload '…'}` and `n:asset` the popup opens on its own and offers
the files under your assets root, together with the **named mappers** your `assets:`
configuration declares. Type a mapper's colon and it opens again, now listing what lives
under *that* mapper rather than everything under the default root – so what you pick is
what will resolve.

Accepting a mapper leaves the popup open, because a mapper is never the whole reference:
the file under it still has to be named.

In `n:asset` the offer stops where Latte stops accepting. A variable is offered at the
start of the value, where it is legal on its own, but not after a mapper – there it has
to be braced (`images:{$name}`), and completing the bare spelling would build a template
that does not compile.

## Classes & functions

PHP class names complete inside `{varType}`, `{templateType}` and `instanceof`
expressions; PHP functions complete inside any Latte expression. Short names match
and the fully-qualified name is inserted automatically.

`true`, `false` and `null` are inserted as the literals they are, not as a call.

## HTML attribute values

Inside `n:attr="…"` and regular attributes, the HTML5 schema provides value
completion just as it would in a plain HTML file.

As in a plain HTML file, the popup in a `class` value stays open while you type a dashed
name such as `text-red-500`.

Latte tags complete inside an attribute value too – in `class`, `href` and `id` as
readily as anywhere else, and with text already typed in front of them.

![Latte tag completion inside the value of a class attribute, offered after text already typed in front of it]({{ '/assets/img/screens/S38-tag-in-attribute-value.png' | relative_url }})

## CSS classes in `n:class`

`n:class` gets the class names a plain `class` attribute gets. Wherever Latte renders a
value as a class name – a word in a string, a bare name, either branch of a ternary, the
value of a `key => value` or `key: value` pair, the right side of `??` – completion
offers the classes defined in your project's stylesheets, at the top of the list:

```latte
<a n:class="'btn btn-primary', $active ? 'is-active' : 'is-idle'">
```

A condition, an array item or a function argument is not a class name, so nothing is
offered there.

![Completion inside n:class, the project's own classes listed above the Tailwind utilities]({{ '/assets/img/screens/S40-nclass-completion.png' | relative_url }})

- The popup opens by itself at a class name and stays open across the dashes of the
  name, so `text-red-500` filters as you type. Classes from other sources, such as a
  Tailwind language server, appear alongside, each name only once – see
  [Tailwind CSS](./tailwind.html) for the settings that bring Tailwind into `n:class`.
- A picked class is followed by a space, unless a space, a comma or the closing quote
  already comes next. On a bare name, a class Latte would not render literally is
  inserted in quotes.
- At a class name only class names are offered. In an empty bare slot, where a variable
  or a call is just as likely, `Ctrl+Space` offers the variables and functions too, below
  the classes; a `$` brings the variables back anywhere.
- Each class name is also a reference to its CSS rule: `Ctrl+B` opens the rule, Find
  Usages and Rename on the rule reach the template, and a rule used only from `n:class`
  is no longer reported as unused.
