---
layout: default
title: Latte version
parent: Configuration
nav_order: 3.5
---

# Latte version

A few things Latte refuses depend on its version – a tag crossing an HTML comment, a
hand-written `|escape`, `|noescape` inside a comment. Latte+ checks those only when it
knows which version your project runs, and stays silent rather than guess when it does
not (see [inspections](../features/inspections.html)).

## Automatic detection

Latte+ reads the version of `latte/latte` from `composer.lock`, or from the constraint in
`composer.json` when there is no lock file:

1. In the project root.
2. When the root names no Latte version, in directories up to three levels below it –
   a monorepo with the Nette application in a subdirectory, say. `vendor`,
   `node_modules`, `build`, hidden and excluded directories are skipped.

When several files are found, they have to agree on the major version: the lowest
pinned minor among them is used. Two different majors are a conflict, and the version
stays unknown until you choose it by hand.

A change to a composer file is picked up as soon as it lands on disk, also when it
comes from outside the IDE – after `composer update` in a terminal, for instance.

## Choosing the version by hand

**Settings → Languages & Frameworks → Latte+ → Latte runtime** has a **Latte version**
section:

- **Auto (detect from composer.lock or composer.json)** – the default.
- **Latte 2**
- **Latte 3**

Under the choice, the page says what Auto detects and from which file – or why it
detects nothing, including which versions are in conflict. With a manual choice it still
shows what Auto would pick, so you can tell when the override is no longer needed. A
change applies to open templates straight away.

![The Latte runtime settings page with the Latte version detected from composer.lock and the php tag mode below]({{ '/assets/img/screens/S43-settings-latte-runtime.png' | relative_url }})

The same page holds the [`{php}` tag mode](./php-mode.html).
