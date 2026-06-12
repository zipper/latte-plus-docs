---
layout: default
title: Implicit variables
parent: Configuration
nav_order: 2
---

# Implicit variables

Some variables are available in every template even though they're never declared in
the template itself — Nette injects them (`$presenter`, `$user`, `$basePath`, …), or
your application adds its own globals via a template factory.

## What it does

Under **Settings → Languages & Frameworks → Latte+ → Implicit Variables** you list
those variables and (optionally) their fully-qualified types.

| Variable | Type |
|---|---|
| `$presenter` | `Nette\Application\UI\Presenter` |
| `$user` | `Nette\Security\User` |
| `$basePath` | `string` |
| `$currentLocale` | `App\Localization\Locale` |

## Why it matters

Once declared, these variables:

- stop being reported by the **undefined variable** inspection,
- offer **type-aware completion** on their members (`$user->isLoggedIn()`),
- participate in [type flow](../features/type-inference.html) like any other typed
  variable.

This keeps templates clean of boilerplate `{varType}` lines for variables that are
genuinely global to your application.
