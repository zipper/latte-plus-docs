---
layout: default
title: Release notes
nav_order: 11
---

# Release notes

## 1.0.2 - 2026-09-17

### Added

- Typing `|` in a Latte expression opens the filter list right away, instead of
  only on Ctrl+Space. It stays quiet where a pipe is not a filter: on the second
  pipe of `||`, inside a string literal, in `{php}` and `{do}`, and outside a
  Latte tag.
- Template path completion opens in the target of a `from` clause, so
  `{include 'inner' from '…'}` offers files the way every other path slot does.
  A path built from an expression behaves like any other path slot too, so
  `{include 'target' . $…}` offers variables.
- The arguments of `{_'Hello %s', $…}` and `{translate}` offer variables.
- A `from` clause written behind a file target is reported for what it is. Latte
  compiles that shape into an exception; until now the line was flagged with a
  misleading "file not found".
- Two include targets that compile but are almost certainly typos are reported
  as weak warnings: `{include from 'x.latte'}`, which includes a block literally
  named `from` and passes the path as its argument, and
  `{include block 'parts/box.latte' from 'x.latte'}`, which asks for a block
  whose name reads like a file path.
- A named argument whose key a custom tag does not declare is named as such.
  Tags whose signature ends in a variadic or attribute tail keep accepting
  foreign keys silently.
- The value of a named argument is checked against the declared type of the
  parameter it fills, the same way a positional argument already was.
- Quick documentation renders for a class member or type name whose class the
  project holds several identical copies of, instead of showing nothing.

### Changed

- All Latte+ inspections now live under the **Latte** group in
  Settings | Editor | Inspections, in new subgroups for components and controls,
  forms, and links and snippets, instead of a separate top-level group beside
  Latte.
- The quick fixes that offer to open Latte+ settings, and Ctrl+B on an implicit
  variable that no PHP class matches, now open the settings dialog on the page
  they name instead of on whichever page it showed last.
- The table of registered extensions lists each name once even when several
  classes register it, and the rescan notification reports the same number of
  tags the table then shows.
- The unknown-tag report only suggests a replacement close enough to the name
  you typed, so a tag with no plausible neighbour is reported without a
  misleading "Did you mean …?" and without a quick fix that would rewrite the
  line into an unrelated tag.
- Completion stays out of the places Latte itself rejects: behind a finished
  argument value, behind a filtered path, inside the quotes of a path literal,
  and behind the first argument written without a comma. Typing `$` in those
  places no longer reopens the popup either.
- A required parameter that nothing fills is reported even when an optional
  parameter happens to occupy its position, so a call can no longer look
  complete by argument count alone.
- The identity comparison report says "always true" when the operator is `!==`,
  instead of claiming the branch is always false, and the inspection is now
  called "Identity comparison between unrelated types".
- The arithmetic report names the class that made it fire, instead of naming a
  string operand the check never objects to.
- When a missing block has a close-enough neighbour, the rename fix is the first
  Alt+Enter offer, ahead of creating the block the typo named.

### Fixed

- A broken `{include}` no longer switches the plugin off for the rest of the
  template. Until now a single half-typed argument cost every tag below it its
  highlighting, inspections, completion and Ctrl+B. A half-typed block target,
  such as `{include #}` or `{include block #title from 'x'}`, is reported on the
  spot for the same reason.
- A closing tag with no opening tag, such as a stray `{/if}`, no longer destroys
  the structure of everything below it, and it is reported with a readable
  message instead of an internal parser dump.
- Error messages on a broken tag name the token Latte could not use, instead of
  claiming a closing `}` is missing when it is written right there.
- Include shapes that Latte accepts are no longer reported as syntax errors,
  among them `{include from 'x.latte'}`, `{include #from}` (with or without a
  space after the marker) and `{include block from 'x.latte'}`.
- An include target that a marker settles as a block name is no longer treated
  as a file: `{include 'title' from 'x.latte'}` no longer offers file paths,
  reports a missing file, or is rewritten when a template moves. Conversely,
  block completion no longer steps aside when a block name happens to read like
  a path.
- A path or a block name that starts with a string literal and continues with a
  concatenation, such as `{block 'x-' . $key}` or
  `{include '~x' . \ucfirst($s)}`, is no longer a syntax error.
- `false` is accepted as a type everywhere Latte accepts it: `{varType false $x}`,
  `?false`, `false[]`, `array{ok: false}`, `list<false>`, `{parameters}`,
  `{var}`, `{default}`, `{define}`, closure parameters and `{templateType}`. The
  rejection did not stop at a squiggle: the tag stopped parsing, so the variable
  it declared was reported as undefined everywhere below.
- `{$x|null}` is no longer accepted as a filter chain; Latte refuses it too.
- `n:class` accepts a named item written as `key: value`, the same shape
  `n:attr` has always accepted.
- A method call inside string interpolation, such as `"faq-{$faq->getFaqId()}"`,
  no longer breaks the whole tag and silences every other check in it.
- Tags are no longer reported as unknown when the package that registers them
  does so the Latte 2 way, from an installed Composer package rather than from
  project sources. Component factories that an installed package brings in are
  recognised too, so `{control …}` in a layout that belongs to no presenter is
  no longer reported as undeclared.
- Inspections apply the rules of the Latte major version the project actually
  runs, so `{php}` with a semicolon, several statements, or a leading `if` is no
  longer reported on a Latte 2 project.
- Custom tags called with named arguments are no longer reported as missing
  arguments, and a ternary passed to a custom tag is no longer counted as three
  arguments. An argument is matched to the parameter it really fills, by name or
  by position, so a named argument no longer gets a file path offer, a Ctrl+B
  underline, a missing-file warning, or a rewrite when a file moves.
- The value of a named argument in `{include}`, `{embed}` and `{sandbox}` is no
  longer treated as a template path.
- Form fields added to a container held in a local variable are found, so a
  template addressing that container no longer reports every one of its fields
  as undeclared. A shared partial that receives `$form` as an include argument is
  no longer told the variable is used outside a form.
- A template header that names a class which does not exist draws a single
  report on the class, instead of a further report on every use of the variable
  below it.
- The `translate` filter accepts the extra placeholder arguments the translator
  takes, so `{='key'|translate, [name: $x]}` is no longer reported as passing too
  many arguments.
- A function call is worth what it returns: `{if trim($text) !== ''}` was
  reported as two types that can never be identical, because the call was read
  as a class named after the function. A function whose return type cannot be
  read, such as `count()`, no longer produces a "got mixed" mismatch.
- A method named like a PHP global function (`link`, `key`, `next`, `date`,
  `current`, …) is read as the method, so `parse_url($presenter->link(…))` is no
  longer reported as passing a bool where a string is expected.
- `{if $pos !== false}` over an `int|false` is no longer reported: a written-out
  `false` or `true` is read as a branch of the same family as `bool`. A bareword
  that resolves to no class no longer counts as a type either, and a first-class
  callable such as `strlen(...)` is no longer typed by the function's return
  value.
- A ternary passed as an argument is typed by its branches instead of by its
  condition, so `array_merge($hasSubmenu ? […] : […], …)` is no longer reported
  as receiving a bool. The else arm of a ternary null check is narrowed, so
  `$place === null ? null : $place->personalPickupId` no longer asks for `?->`
  in the arm that runs when the value is not null.
- Printing a value that cannot be printed is reported for a call written as a
  bare name (`{= makeList()}`), which used to be silent while the parenthesised
  spelling was not. Reading an offset off a scalar asks about the value being
  indexed rather than the result.
- `n:ifcontent`, `{elseif}` bodies and a condition joined with `&&` inside an
  n:attribute are recognised as ways a template already handles an empty value,
  so hints no longer appear on code that handles it.
- A `<head>` that pulls its content in through `{embed}` is no longer reported
  as missing a `<title>`.
- A link that spells its action the way the URL does, in kebab case, is no
  longer reported as an action that does not exist.
- A presenter is no longer reported as missing when several presenters in the
  project answer to the same name, as they normally do in a monorepo.
- When a project holds several copies of the same class, a template resolves to
  the copy installed next to it, the way the same expression resolves in a PHP
  file; where no single copy can be chosen, go-to-declaration offers them all
  and the class is no longer reported as undefined.
- A comma inside a call or an array literal is no longer mistaken for a tag
  argument separator: `{embed 'target.latte', a: implode(' ', $…)}` offers the
  caller's variables and PHP functions again. Variables are offered inside an
  initializer's nested commas as well (`{var $arr = ['x', $…]}` and the same
  shapes in `{default}`, `{varType}`, `{parameters}` and `{capture}`), and the
  colons of `{varType array{name: string} $…}` no longer turn a declaration slot
  into an expression slot.
- Typing `$` between apostrophes no longer opens a popup that can only answer
  "No suggestions"; a single-quoted string does not interpolate. Double quotes
  keep their popup.
- Reformat Code and the re-indent that runs after a paste no longer fail on a
  template the parser could not read whole, which used to leave the pasted lines
  unindented.
- An array or a bracket opened in the argument list of a multi-line tag keeps
  its indent level on every line below it, not just the first, across Enter,
  Tab, End and Backspace; a closing bracket returns to the level of the line
  that opened it, and a trailing comma above the tag's closing brace keeps the
  caret at argument level. A bracket written inside a string literal no longer
  moves a closing bracket to the wrong level.
- Tab, End and Backspace no longer copy the previous line's column when no
  indent rule applies, so a mis-indented line no longer spreads its indent to
  everything typed below it. Switching the HTML "Align attributes" option off
  now applies to them as well.
- Backspace in the indent of a line with a blank line above it removes the blank
  line and keeps the indent, instead of joining the lines and losing the indent
  in a single press.
- Inspection messages no longer show their internal escaping, such as
  `use ''?->''` or `Duplicate '{'else'}' clause`.

## 1.0.1 - 2026-09-04

- First public release of Latte+.
