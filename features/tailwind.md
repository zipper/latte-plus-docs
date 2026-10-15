---
layout: default
title: Tailwind CSS
parent: Features
nav_order: 8.5
---

# Tailwind CSS
{: .no_toc }

1. TOC
{:toc}

---

Latte+ does not ship its own list of Tailwind utilities. They come from the **Tailwind
CSS** plugin that is bundled with PhpStorm, and Latte+ makes sure its suggestions reach
the class names of a Latte template.

## What you get

- **Utilities in `class` and `n:class`.** The Tailwind popup works in a plain `class`
  value and in the class names of `n:class` – the strings, bare names and the branches
  that Latte renders as classes (see
  [CSS classes in `n:class`](./completion.html#css-classes-in-nclass)). The popup stays
  open across the dashes of a name, so `text-red-` keeps filtering.
- **Your own classes first.** Classes defined in your project's stylesheets sort above
  the Tailwind utilities. A name both of them offer appears once, as your CSS class.
- **The same insert.** A picked utility is followed by a space, exactly like a project
  class, unless a space, a comma or the closing quote already comes next.
- **Arbitrary values.** In a `class` value and in `n:class`, `[` and `(` are paired and
  their closer is stepped over, so `w-[12px]` and `px-(--gap)` type naturally.
- **Long class lists.** In a string of `n:class`, Enter splits the list into a new string
  on the next line – see [Smart Enter](./smart-editing.html#smart-enter).

## Requirements

The Tailwind CSS plugin itself decides whether it runs. According to the PhpStorm
documentation it needs:

- Node.js, ideally configured as the project's Node.js runtime,
- the bundled **CSS** and **Tailwind CSS** plugins enabled,
- `tailwindcss` as a dependency in `package.json`, installed in `node_modules`,
- a CSS file with `@import "tailwindcss"` (Tailwind CSS 4) or a `tailwind.config.js`
  (Tailwind CSS 3).

## Recommended settings

The Tailwind CSS plugin is configured in JSON under
**Settings → Languages & Frameworks → Style Sheets → Tailwind CSS**. Two entries make it
work well with Latte. Keep the rest of the configuration as it is and add or merge these:

```json
{
  "includeLanguages": {
    "latte": "html"
  },
  "experimental": {
    "classRegex": [
      ["classList\\.(?:add|remove|toggle)\\(([^)]*)\\)", "[\"'`]([^\"'`]*)[\"'`]"],
      ["\\b\\w*[cC]lass\\w*\\s*:\\s*([^,}]*)", "[\"'`]([^\"'`]*)[\"'`]"],
      ["n:class=\"([^\"]*)\"", "[\"'`]([^\"'`]*)[\"'`]"],
      ["\\$\\w*[cC]lass\\w*\\s*=\\s*(\\[[^}]*\\]|[^\\n]*)", "[\"'`]([^\"'`]*)[\"'`]"],
      ["\\w*[cC]lass\\w*\\s*=>\\s*(\\[[^}]*\\]|[^,}]*)", "[\"'`]([^\"'`]*)[\"'`]"]
    ]
  }
}
```

**`includeLanguages`** tells Tailwind to treat `.latte` files as HTML, so it suggests
utilities in their `class` attributes at all.

**`experimental.classRegex`** names further places that hold class names. Each entry is
a pair: the first expression finds the place, the second picks the quoted strings in it.

1. `classList.add('…')`, `.remove()` and `.toggle()` – classes switched from JavaScript.
2. `class: '…'` – a named argument such as `{include card, class: 'p-4'}`, or any name
   ending in `class`/`Class` (`buttonClass: …`).
3. `n:class="…"` – the strings inside `n:class`, which Tailwind does not know as a class
   attribute.
4. `$class = '…'` – an assignment such as `{var $class = 'flex gap-2'}` or
   `{var $wrapperClass = ['…', '…']}`.
5. `'class' => '…'` – an array item such as `['class' => 'mt-4']`, passed to
   `{include}` or built in PHP.
