---
layout: default
title: Custom tag signatures
parent: Configuration
nav_order: 5
---

# Custom tag signatures
{: .no_toc }

A small DSL that teaches the IDE the argument shape of your project's custom tags.
{: .fs-6 .fw-300 }

1. TOC
{:toc}

---

Latte+ can learn about project-specific tags registered through a `Latte\Extension`.
Auto-detection covers the **name** and the **paired / unpaired** flag (derived from
`yield` inside the parser callback), but the **argument signature cannot be derived
statically** – Latte's parser callbacks are imperative generators, not method calls
with a declared parameter list.

You supply the signature manually in **Settings → Languages & Frameworks → Latte+ →
Custom Extensions → Tags**, and the plugin uses it for:

- completion templates with Tab-navigable placeholders,
- hover documentation,
- inlay parameter-name hints,
- missing-arg / too-many-arg inspections.

## Quick example

For a tag invoked as

```latte
{imageTag $product, '300x200', alt: 'cover', data-test: $name}
```

you write the signature

```
image: expr, size: string, ...attrs: attrs
```

Result:

| Spec param | Matches | Notes |
|------------|---------|-------|
| `image: expr` | `$product` | Positional #1 – the name `image` is a placeholder label, **not** a call-site key. |
| `size: string` | `'300x200'` | Positional #2. |
| `...attrs: attrs` | `alt: 'cover', data-test: $name` | Variadic rest-bucket – extra `key: value` pairs are folded here. |

## Grammar

```
signature    := [ tolerantFlag ] params
tolerantFlag := 'tolerant!' [ ',' ]
params       := param ( ',' param )*
param        := [ '...' ] [ '?' ] NAME ':' TYPE [ '=' default ]
```

Surrounding parentheses are optional – `(image: expr, size: string)` and
`image: expr, size: string` parse the same way.

## Parameter forms

| Form | Meaning | Example |
|------|---------|---------|
| `name: type` | Required positional argument | `image: expr` |
| `?name: type` | Optional positional argument | `?alt: string` |
| `name: type = default` | Optional with a default value | `decimals: int = 2` |
| `...name: type` | Variadic rest-bucket (any number of trailing args) | `...attrs: attrs` |

A variadic param implies tolerant mode automatically – missing-arg inspections
skip the signature.

## Signature-level flag

Prefix `tolerant!` at the start of the signature to opt every required slot out of
missing-arg validation. Useful when the tag has dispatch shapes a static spec can't
capture cleanly.

```
tolerant! image: expr, size: string
```

## Types

| Type | Meaning |
|------|---------|
| `string` | Plain (possibly quoted) string literal. |
| `expr` | Arbitrary PHP expression. |
| `path` | File path – receives a path reference for Ctrl+B navigation. |
| `identifier` | Bare identifier (block name, variable name, …). |
| `identifier_or_expr` | Either a bare identifier (`{block foo}`) or a PHP expression (`{block $name}`). Completion / inspection picks the path based on the first non-space char. |
| `form-control-name` | Validated Nette form control name. |
| `attrs` | Free `key: value` HTML attribute bucket (`alt:`, `data-foo:`, `aria-*:`, …). |
| `bool` | Boolean literal expected (`true` / `false`). |
| `int` | Integer literal expected. |
| `float` | Float / double literal expected. |
| `mixed` | Anything – no validation, no completion driver. Default when auto-detect can't derive a more specific type. |

Each type accepts a few aliases so older saved signatures keep working:

| Canonical | Aliases |
|-----------|---------|
| `expr` | `expression` |
| `path` | `file-path`, `filepath` |
| `identifier` | `ident`, `id` |
| `identifier_or_expr` | `ident_or_expr`, `id_or_expr` |
| `form-control-name` | `formcontrolname`, `form-control`, `formcontrol` |
| `attrs` | `attr`, `attributes` |
| `bool` | `boolean` |
| `int` | `integer` |
| `float` | `double`, `number`, `numeric` |
| `mixed` | `any` |

Type matching is case-insensitive and treats `-` and `_` the same, so `form-control-name`,
`FORM_CONTROL_NAME` and `formcontrolname` all resolve to the same type. An unrecognised
type falls back to `mixed`.

## Validation

Latte+ checks two things against the signature as you type, for both the braced
(`{pdSrcset …}`) and the attribute (`n:srcset="…"`) form:

- **Argument count (arity).** Fewer arguments than the required positional params is
  reported as a missing-argument error; more positional arguments than the signature
  declares (when there is no `...` variadic / `attrs` tail) is reported too. A trailing
  `...` param or the `tolerant!` flag relaxes the count check.
- **Literal shape.** When an argument is written as a *literal* – a quoted string, a
  number, `true`/`false`/`null` – its shape is checked against the declared type (e.g.
  a number literal in a `string` slot, or a string literal in an `int` slot).

**What is intentionally not checked: the runtime type of a variable or expression.**
A `$variable`, a member access (`$x->y`), a call (`foo()`) or any other non-literal
argument is treated as compatible with every declared type – Latte+ does not infer its
PHP type for this check. So `image: string` does **not** flag `n:srcset="$maybeBool"`;
the value's type is opaque here, and the same applies to `{pdSrcset $maybeBool}`.

This is a deliberate trade-off, not an oversight. A custom tag / `n:*` body is parsed as
a flat positional token stream so that it tolerates whitespace-separated, hyphenated
barewords like `image-xs` – which means there is no structured expression tree to run
full type inference on (the same root cause as the bare-string member-access entry on
the [Known limitations](../limitations.html) page). Filters and `{include}` / `{embed}`
parameters, which *do* expose a structured expression, get full argument-type
validation.

## Why positional, not named

Latte's tag macros are called like PHP functions – arguments are positional. PHP 8 /
Latte 3 named-argument syntax (`{include 'foo.latte', showHeader: true}`) is supported
by a handful of built-in tags (`include`, `embed`, `extends`), but **custom user tags
use positional calls**. The spec's `name:` therefore plays the same role as a parameter
name in a PHP function declaration: it's the placeholder label / inlay-hint text, not a
call-site key.

If your tag accepts a free `key: value` tail (an HTML attribute splat), declare it with
`...name: attrs` – only there does the call site become `key: value`-shaped.

## What is not supported

- **Multiple variadic slots** – at most one `...` param per signature; later ones are
  silently ignored.
- **Multi-level grouping** (`(...) | (...)`) – pick one shape and use `tolerant!` if
  dispatch varies.
- **Named-argument calls on custom tags** – not how Latte parses them.

## Tip: live-template completion

When you type the tag prefix and accept it from the popup, Latte+ inserts a **live
template** with one Tab-stop per signature param. The default placeholder text per type:

| Type | Default placeholder |
|------|---------------------|
| `expr` / `mixed` | `$name` |
| `string` | `'name'` |
| `path` | `'path/to/file.latte'` |
| `identifier` / `identifier_or_expr` / `form-control-name` | the param name |
| `bool` | `false` |
| `int` | `0` |
| `float` | `0.0` |
| `attrs` | _(empty – fill in your own `key: value` pairs)_ |

Tab moves the caret to the next placeholder. The final Tab-stop lands inside the paired
block (or after the closing `}` for unpaired tags).
