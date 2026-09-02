---
layout: default
title: Configuration
nav_order: 5
has_children: true
permalink: /configuration/
---

# Configuration

Latte+ works out of the box, but a few settings let you teach it about your project's
conventions and tune the editor to your taste.

Settings live in two places:

- **Project settings** – **Settings → Languages & Frameworks → Latte+**
  (path aliases, implicit variables, `{php}` tag mode, presenter mapping, asset
  mappers, custom extensions).
- **Editor settings** – **Settings → Editor → Color Scheme → Latte** and
  **Settings → Code Style → Latte** (plus **Code Style → HTML**, which owns the
  alignment of wrapped HTML attributes).

| Page | What you configure |
|---|---|
| [Path aliases](./path-aliases.html) | Custom `{include}` path prefixes that map to search directories. |
| [Implicit variables](./implicit-variables.html) | Variables (and their types) available to every template. |
| [`{php}` tag mode](./php-mode.html) | Strict vs. permissive interpretation of `{php}` / `{do}`. |
| [Presenter mapping](./presenter-mapping.html) | Where Latte+ reads `application: mapping` from, and how to set it by hand. |
| [Asset mappers](./asset-mapping.html) | Where Latte+ reads `assets: mapping` from, and how to set it by hand. |
| [Custom extensions](./custom-extensions.html) | Register your own tags, filters, functions and `n:attributes`. |
| [Custom tag signatures](./custom-tag-signatures.html) | Describe a custom tag's argument shape for completion, hints and inspections. |
| [Colors & code style](./colors-code-style.html) | Token colors and code style. |
| [Indentation](./indentation.html) | Which settings drive structural indent vs. attribute alignment, and why `.editorconfig` wins. |
