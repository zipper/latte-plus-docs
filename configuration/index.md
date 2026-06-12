---
layout: default
title: Configuration
nav_order: 4
has_children: true
permalink: /configuration/
---

# Configuration

Latte+ works out of the box, but a few settings let you teach it about your project's
conventions and tune the editor to your taste.

Settings live in two places:

- **Project settings** — **Settings → Languages & Frameworks → Latte+**
  (path aliases, implicit variables, PHP mode, custom extensions).
- **Editor settings** — **Settings → Editor → Color Scheme → Latte** and
  **Settings → Code Style → Latte**.

| Page | What you configure |
|---|---|
| [Path aliases](./path-aliases.html) | Custom `{include}` path prefixes that map to search directories. |
| [Implicit variables](./implicit-variables.html) | Variables (and their types) available to every template. |
| [PHP mode](./php-mode.html) | Strict vs. permissive interpretation of `{php}` / `{do}`. |
| [Custom extensions](./custom-extensions.html) | Register your own tags, filters, functions and `n:attributes`. |
| [Colors & code style](./colors-code-style.html) | Token colors and formatter options. |
