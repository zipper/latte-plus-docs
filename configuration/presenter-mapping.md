---
layout: default
title: Presenter mapping
parent: Configuration
nav_order: 7
---

# Presenter mapping

A Latte link destination (`Admin:Product:edit`) is a presenter name, not a class name.
Turning one into the other is what Nette's `application: mapping` describes, and Latte+
reads that section straight out of your project's NEON configuration – so completion,
`Ctrl+B` and the undefined-link inspection agree with what your application actually
routes, whatever your namespaces look like.

```neon
application:
    mapping:
        *: App\Presentation\*\*Presenter
        Api: App\Api\*Presenter
```

With this configuration `Front:Home` is `App\Presentation\Front\HomePresenter`, and
`Api:Users` is `App\Api\UsersPresenter` – two layouts no class-name convention could
have guessed.

## Where the configuration is looked for

Latte+ walks **up** from the template you are editing. On each level it reads the
`.neon` files sitting directly in that directory, in its `config/` subdirectory and in
its `app/config/` subdirectory. That covers the layouts real projects use:

| Layout | Configuration read |
|---|---|
| Nette skeleton | `config/common.neon`, `config/local.neon` |
| Older skeletons | `app/config/config.neon` |
| Monorepo package | a `.neon` next to the package sources, with no `config/` at all |

Nearer configuration wins over configuration farther up, and within one level
`local.neon` (and `*.local.neon`) is read first, because Nette includes it last. The
first file to define a mapping key keeps it.

`includes:` is not followed – every `.neon` in the chain is read instead, which covers
the same ground without chasing include trees. DI parameters are not expanded, so a
mask built out of them (`%appDir%\*Presenter`) is skipped rather than guessed at.

## What changes once the mapping is read

- Presenters your layout places unusually are offered in `{link}`, `{plink}` and
  `n:href` – for example `Front:Home` in a flat modular layout, whose module directory
  carries no `Module` suffix and whose presenter is not in a directory of its own name.
- Names that a convention would guess wrongly are corrected. In a flat modular project
  `App\Presentation\Admin\AdminPresenter` is `Admin:Admin`, not `Admin`.
- A presenter your mapping cannot route is **not** offered any more, because a link to
  it would fail at runtime – even Nette could not resolve it.
- Editing the configuration takes effect on the next lookup, before you save it, so
  completion and `Ctrl+B` follow the change straight away. Warnings already painted in a
  template are re-evaluated when you save, or whenever the editor next inspects it.

Projects without a mapping Latte+ can read are unaffected: destinations keep being
matched against the layouts the Nette skeletons ship. See
[known limitations]({{ '/limitations.html' | relative_url }}) for what that costs.

## Setting the mapping by hand

**Settings → Languages & Frameworks → Latte+ → Presenter mapping** shows which mapping
was found for the file you are editing and which configuration file it came from. Start
there – it usually answers the question you opened the page for.

The table below it is an override, for a project whose mapping cannot be read from
NEON: a mask assembled from DI parameters, or a mapping built in PHP. One row per
mapping key, with the class mask in the same form the NEON file uses, and `*` as the
catch-all. Leave the table empty to keep the detection, which is what almost every
project wants.
