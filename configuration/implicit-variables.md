---
layout: default
title: Implicit variables
parent: Configuration
nav_order: 2
---

# Implicit variables
{: .no_toc }

1. TOC
{:toc}

---

Some variables are available in every template even though they're never declared in
the template itself – Nette injects them at runtime. Latte+ already knows the common
Nette ones out of the box, and you can add your own application globals.

## Built-in variables

Latte+ ships with these implicit variables, so they get type-aware completion and the
optional *undefined variable* inspection leaves them alone – no configuration needed:

| Variable | Type | Available |
|---|---|---|
| `$presenter` | `Nette\Application\UI\Presenter` | everywhere |
| `$control` | `Nette\Application\UI\Control` | everywhere |
| `$user` | `Nette\Security\User` | everywhere |
| `$flashes` | `stdClass[]` | everywhere |
| `$basePath` | `string` | everywhere |
| `$baseUrl` | `string` | everywhere |
| `$this` | `Latte\Runtime\Template` | everywhere |
| `$form` | `Nette\Application\UI\Form` | inside `{form}` or `<form n:name=…>` |
| `$formContainer` | `Nette\Forms\Container` | inside `{formContainer}` / `n:formContainer` |
| `$iterator` | `Latte\Runtime\CachingIterator\|Latte\Essential\CachingIterator` | inside `{foreach}` / `n:foreach` |

A few notes:

- **Scope-bound variables** (`$form`, `$formContainer`, `$iterator`) only appear in
  completion and only resolve **inside their tag** – `$iterator->isFirst()` is
  offered within a `{foreach}` but not outside it.
- **`$this`** is narrowed to your own template subclass when a template declares
  `{templateType App\…}`, so its members complete against your class.
- **`$this->global`** is recognized as a runtime container (the Application bridge
  assigns it). Accessing it won't be flagged as an undefined member; its individual
  properties come from your registered Latte extensions and can't be enumerated
  statically.

## Adding your own

If your application exposes extra globals to every template (e.g. via a template
factory), declare them under
**Settings → Languages & Frameworks → Latte+ → Implicit Variables**. For each entry
you provide a **name**, an optional **type** (fully-qualified class name), and whether
it's limited to a `{foreach}` scope.

![Implicit variable settings]({{ '/assets/img/screens/S33-settings-implicit-variables.png' | relative_url }})

[More screenshots]({{ site.baseurl }}/screenshots.html#implicit-variables)

| Variable | Type |
|---|---|
| `$currentLocale` | `App\Localization\Locale` |
| `$settings` | `App\Model\Settings` |

Once added, these behave exactly like the built-ins:

- they are not reported by the **undefined variable** inspection, where you have it on,
- they offer **type-aware completion** on their members (`$currentLocale->code`),
- they take part in [type flow](../features/type-inference.html) like any typed
  variable.

## Overriding or disabling built-ins

In the same settings page you can:

- **Disable** a built-in you don't use, or
- **Override** one with a different type, for a project whose framework hands over
  something of its own. The built-in `$iterator` already covers both the Latte 2.x and
  the Latte 3.x class, so that case needs no override.

A custom entry that shares a name with a built-in takes precedence.
