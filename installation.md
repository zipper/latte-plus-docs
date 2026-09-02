---
layout: default
title: Installation
nav_order: 2
---

# Installation
{: .no_toc }

1. TOC
{:toc}

---

## Requirements

| Requirement | Details |
|---|---|
| **IDE** | PhpStorm **2023.3** or newer (build `233+`). Latte+ depends on the bundled PHP support, so it requires PhpStorm specifically (not a plain IntelliJ IDEA without the PHP plugin). |
| **Latte** | Designed for **Latte 3.x**. Older 2.x syntax is parsed, but the feature set targets Latte 3. |
| **PHP** | Type inference understands **PHP 8.1+** features – enums, readonly properties, intersection/union types, `list<T>` generics. |

## Install from JetBrains Marketplace

The recommended way:

1. Open **Settings / Preferences** → **Plugins**.
2. Switch to the **Marketplace** tab.
3. Search for **`Latte+`**.
4. Click **Install** and restart the IDE when prompted.

> **Already using another Latte plugin?**
> Disable or uninstall it before installing Latte+. Running two plugins that register
> the `.latte` file type at the same time leads to conflicting highlighting and
> duplicated inspections. See [Comparison](./comparison.html) for the differences, and
> [Reliability](./reliability.html) for what a clean template looks like.

## Install from disk

If you received a plugin `.zip` directly, or want a specific build:

1. Download the Latte+ plugin `.zip`.
2. Open **Settings / Preferences** → **Plugins**.
3. Click the **⚙ gear icon** → **Install Plugin from Disk…**.
4. Select the downloaded `.zip` and restart the IDE.

## Verifying the installation

After restarting, open any `.latte` file. You should see:

- Colored Latte tags, `n:attributes` and filters.
- Autocomplete after typing `{` inside a template.
- The **Latte+** settings page under
  **Settings → Languages & Frameworks → Latte+**.

If `.latte` files still open as plain text or HTML, check
**Settings → Editor → File Types** and make sure the `*.latte` pattern is mapped to
the **Latte** file type.

## Updating

Marketplace installs update automatically through PhpStorm's plugin updater. You can
also check manually via **Settings → Plugins → Installed → Latte+ → Update**.
