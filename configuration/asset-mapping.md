---
layout: default
title: Asset mappers
parent: Configuration
nav_order: 8
---

# Asset mappers

`{asset 'images:logo.gif'}` names a **mapper** before the colon, not a directory. Which
root that mapper stands for is what Nette's `assets: mapping` describes, and Latte+ reads
that section straight out of your project's NEON configuration – so the missing-asset
check, `Ctrl+B` and completion all agree with where your files actually are.

```neon
assets:
    mapping:
        default:
            path: %wwwDir%/assets
        images:
            path: %wwwDir%/images
```

With this configuration `images:logo.gif` is checked against `www/images/logo.gif`, and a
name no mapper declares – `{asset 'imgs:logo.gif'}` – is reported as **the mapper being
wrong**, rather than sending you to look for a file that was never meant to exist.

## Where the configuration is looked for

The same walk as [presenter mapping](./presenter-mapping.html): Latte+ goes **up** from
the template you are editing, and on each level reads the `.neon` files sitting directly
in that directory, in its `config/` subdirectory and in its `app/config/` subdirectory.
Nearer configuration wins, and within one level `local.neon` is read first because Nette
includes it last.

The short form is understood too, where a mapper is a bare path instead of a block:

```neon
assets:
    mapping:
        images: %wwwDir%/images
```

## DI parameters are expanded – and checked

Unlike a presenter mask, an asset path has to end up at a real directory, so the
parameters in it are resolved rather than skipped:

| Parameter | How it is resolved |
|---|---|
| `%rootDir%` | The project root. |
| `%wwwDir%` | The directory holding the entry script (`index.php`), which is where Nette takes it from – **not** `%rootDir%/www` by convention. A project whose entry script is `public/index.php` gets `public/`. Where there is no entry script at all, a conventional directory that exists on disk is used instead. |
| `%appDir%` | The directory holding `Bootstrap.php`. |

Anything else leaves the path unresolved, and an unresolved path means **no verdict**:
the reference is left unchecked rather than measured against a guess. That is deliberate —
a missing message costs one unchecked reference, a wrong one sends you looking for a file
that was never meant to be there.

## The override

**Settings → Languages & Frameworks → Latte+ → Asset mappers** shows what Latte+ read,
each mapper with the directory its path expands to, and says plainly when an expansion
could not be checked against the disk. That top half is usually all you need: it answers
"does the plugin see my configuration?" without any guesswork.

The table below it is for the case where the answer is no – a path built from a parameter
that points nowhere the plugin can follow, or a mapper registered in PHP rather than in
NEON. One row per mapper, and the directory may be absolute or relative to the project.

| Mapper name | Directory |
|---|---|
| `images` | `www/images` |

Entries here **replace** the configuration rather than adding to it. The override exists
for a configuration that cannot be trusted, so merging the two would put back the half
that was wrong. Leave the table empty – the normal case – and the project's own NEON is
used.

## What changes once the mappers are read

- `{asset 'images:logo.gif'}` is checked against the right directory instead of being
  skipped, so a typo in the filename is caught.
- A mapper name your configuration does not declare is reported as such.
- `Ctrl+B` opens the file the reference names, through a mapper as readily as without one.
- Completion offers the mapper names, and once you type a mapper's colon, the files under
  **that** mapper.

The optional forms stay quiet about a missing file, since the runtime hands back null
instead of throwing. They still report an unknown mapper, which throws either way.

The marker goes on the **name**, not into the reference — `{asset? 'logo.png'}` and
`n:asset?="logo.png"`. Written inside the string it is simply the first character of a
path: `{asset '?logo.png'}` looks for a file called `?logo.png`, which is almost never
what was meant, so it is reported as missing.
