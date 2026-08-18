---
layout: default
title: PHP mode
parent: Configuration
nav_order: 3
---

# PHP mode

The `{php}` and `{do}` tags can carry either a single expression or full raw PHP,
depending on whether your project enables Latte's `RawPhpExtension`. Latte+ needs to
know which, so it can inject and validate the PHP correctly (see
[Embedded languages](../features/embedded-languages.html)).

With the right mode in place, both tags are treated as PHP – highlighted, completed and
checked – and you get no spurious errors about what the tag is allowed to contain.

![PHP tags whose bodies are highlighted and checked as PHP, with an argument hint on a function call]({{ '/assets/img/screens/S24-php-completion.png' | relative_url }})

## The two modes

| Mode | Allowed in `{php}` / `{do}` |
|---|---|
| **Strict** | A single expression. No statement separators (`;`). This is Latte's default. |
| **Permissive** | Full multi-statement raw PHP. Requires `RawPhpExtension`. |

## Automatic detection

By default Latte+ figures out the mode automatically, in this order:

1. A per-file marker, if present.
2. Your **PHP Mode** project setting (below).
3. The presenter/template factory chain.
4. A project-wide scan for `RawPhpExtension` registration.

## Overriding the mode

Under **Settings → Languages & Frameworks → Latte+ → PHP Mode** you can force the
behavior instead of relying on detection:

- **Auto** (default) – use the detection above.
- **Force strict** – always treat `{php}` / `{do}` as a single expression.
- **Force permissive** – always allow full raw PHP.

Set this if your project's configuration can't be auto-detected, or to silence
spurious PHP-tag inspections in an unusual setup.
