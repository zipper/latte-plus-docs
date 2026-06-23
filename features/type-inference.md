---
layout: default
title: PHP type inference
parent: Features
nav_order: 3
---

# PHP type inference
{: .no_toc }

1. TOC
{:toc}

---

Accurate type inference is what makes completion, navigation and inspections useful
rather than noisy. Latte+ works out the type of every variable from several sources,
so member completion (`$obj->…`), navigation and type checks all know what they're
looking at.

## Declared types

The strongest signal comes from explicit declarations:

```latte
{varType App\Model\Product $product}
{templateType App\Presentation\Home\HomeTemplate}
{var int $count = 0}
{parameters App\Model\Order $order, bool $editable = false}
```

After any of these, `$product->`, `$order->` and `$count` get fully typed completion
and inspection.

## Inferred types

When no explicit type is given, Latte+ infers from context:

- **`foreach` item types** – iterating `iterable<App\Image>` or `App\Image[]` types
  the loop variable as `App\Image`.
- **Assignments** – `{var $u = $order->getCustomer()}` carries the return type onto `$u`.
- **`{capture}`**, **`{for}`**, ternaries and array literals all contribute types.
- **`list<T>` generics** – `{varType list<App\Tag> $tags}` is understood and the item
  type flows into the loop. (Some other plugins flag `list<…>` as an error.)

## Implicit / framework variables

Variables that Nette injects into every template – and any you declare as
[implicit variables](../configuration/implicit-variables.html) in settings – are
typed and available without an explicit `{varType}`.

## Cross-tag type flow

Types propagate across tags within a template. A variable created in one tag is typed
in the next:

```latte
{var $cart = $order->getCart()}
{* $cart->getItems() now completes and is type-checked *}
{foreach $cart->getItems() as $item}
    {$item->product->name}   {* fully resolved chain *}
{/foreach}
```

Type flow even reaches into [`{php}` / `{do}` tags](./embedded-languages.html):
a variable assigned inside a `{php}` block is typed in later Latte expressions.
