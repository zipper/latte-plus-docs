---
layout: default
title: Release notes
nav_order: 11
---

# Release notes

## 1.1.0 - 2026-10-23

### Added

- CSS classes in `n:class`. Wherever Latte renders a value of `n:class` as a
  class name (a word in a string, a bare name, either branch of a ternary, the
  value of a `key => value` or `key: value` item, the right side of `??`),
  completion offers the classes defined in your project's stylesheets, the same
  ones a plain `class` attribute gets, at the top of the list. Classes from
  other sources the IDE knows, such as the utilities of the Tailwind CSS plugin,
  are offered alongside, each name only once. The popup opens by itself at a
  class name and stays open across the dashes of a name such as `text-red-500`.
  A picked class is followed by a space unless a space, a comma or the closing
  quote already comes next, and on a bare name a class Latte would not render
  literally is inserted in quotes. At a class name only class names are
  offered; in an empty bare slot Ctrl+Space adds the variables and functions
  below the classes, and a `$` brings the variables back anywhere.
- A class name in `n:class` is a reference to its CSS rule: Ctrl+B opens the
  rule in your stylesheet, Find Usages on the rule lists the `n:class` strings
  that use the class, renaming the rule renames the class in the template too,
  and a rule used only from `n:class` is no longer reported as unused.
- Enter in a string of `n:class` splits the class list: it closes the string,
  adds a comma and opens a new string with the same quote on the next line,
  aligned with the first item. The classes after the caret move into the new
  string. It works in a plain item, in the value of a `key => '…'` or
  `key: '…'` item, in the `else` branch of a ternary and on the right of `??`;
  everywhere else Enter only breaks the line.
- In a `class` value and in the strings of `n:class`, `[` and `(` are paired and
  their `]` and `)` stepped over, so class names with brackets, such as the
  arbitrary values `w-[12px]` or `px-(--gap)`, type naturally. A plain `.html`
  file keeps the platform's behaviour.
- A class picked from completion in a quoted `class` attribute of a Latte
  template is followed by a space, whichever source offered it, unless a space
  or the closing quote already comes next. A class picked in the middle of the
  list therefore does not run into the one after it.
- Finishing a closing tag indents the body of the tag it closed, the way Reformat
  Code would: Latte tags (`{/block}`, `{/if}`, …) and HTML tags (`</div>`, …),
  whether the closer is completed automatically, typed by hand, typed over an
  existing one or picked from completion, also with several carets at once.
  Pasted markup no longer stays on column 0 once you close it. Only the indent
  of the lines changes, the text in them stays as it is. After typing a closer,
  the first Ctrl+Z takes back just the indent. Elements over 300 lines and files
  over 30 000 characters are left alone. It can be switched off in Settings >
  Editor > General > Smart Keys.
- Ctrl+B on `{control reviews}` offers the template the component renders
  first and the `createComponentReviews()` factory second. The template is read
  from the component's PHP: `setFile()`, `render()` with a path, a getter a
  subclass overrides, or a base class building the path from the class name. A
  factory typed with an abstract class or an interface stands for its concrete
  subclasses. In `{control gallery:thumb}` each part leads to its own place, the
  way `Class::method` does in PHP: `gallery` opens just the factory, while
  `thumb` offers the template of `renderThumb()` and the method itself, falling
  back to the template of `render()` when `renderThumb()` names none.
- A `Multiplier` is looked through: `{control parts-1}` and
  `{control "parts-{$id}"}` address the component the multiplier creates, so
  Ctrl+B, render method completion, Ctrl+P, Ctrl+Q and the checks work on that
  child class. The class comes from the callback's return type, or from the one
  class every `return` in it creates. `{control parts}` still means the
  multiplier itself.
- A template belongs also to the class whose PHP sets it with `setFile()`,
  `render()` with a path or `setLayout()`, and a partial with no owner of its
  own takes the owners of the templates that render it through `{include}`,
  `{embed}`, `{include … from}`, `{layout}`, `{extends}` or `{import}`.
  Components, forms, signals and relative links in such a shared partial get
  the completion, navigation and checks they get in a view, and a report names
  every owner when there are several.
  A component renames from such a template together with its factory.
- Where a component, a form, a signal or a presenter has several candidate
  declarations and nothing decides between them (a partial nothing renders, a
  template several classes could own, two presenters of the same name), Ctrl+B
  offers every declaration, templates first, instead of leading nowhere.
  Ctrl+Q documents the first of them and names the others in a "Declared by …"
  row, or "One of …" for presenters. In a template nothing renders, a name that
  exactly one method in the project declares is followed directly by Ctrl+B,
  completion, Ctrl+Q and the parameter hints.
- A Navigation section on the Latte+ settings page chooses what Ctrl+B offers
  on a name with both a template and PHP code behind it, such as a link
  destination or a `{control}` tag: both with the template first (the default),
  the template only, or the PHP code only. Where only one kind exists, Ctrl+B
  goes there whatever is chosen. The setting applies to every project.
- Find Usages on a template file lists every place whose Ctrl+B leads to it:
  `{control}` tags for the template of a component (including the fallback to
  the template of `render()`, `Multiplier` children, subclasses and traits),
  and `{link}`, `{plink}`, `n:href`, `{ifCurrent}` and `n:ifCurrent` for the
  template of a presenter action. The PHP strings that name the template, such
  as `setFile(__DIR__ . '/detail.latte')`, are grouped as "Template path in
  PHP" instead of "Unclassified". Renaming or deleting the template still
  changes only the PHP that names it.
- `{ifCurrent}` and `n:ifCurrent` take the destination and arguments of a link
  and get what `{link}` gets: destination completion, Ctrl+B to the template
  and the presenter method, parameter info and inlay hints, rename, and the
  check of the destination, now in `n:ifCurrent` too. On a Latte 3 project a new
  inspection reports the shapes Latte 3 refuses: an unquoted `*` in the
  destination (`Product:*`), a second argument without a comma, and a filter in
  `n:ifCurrent`. A filter in the `{ifCurrent}` tag is refused by every version
  and reported whatever the version.
- The Latte version can be chosen by hand. The settings page that held the
  `{php}` tag mode is now called Latte runtime and has a Latte version section
  (Auto, Latte 2, Latte 3) that says what Auto detects and from which file, or
  which versions are in conflict. When the project root names no Latte version,
  Auto now also looks for it up to three directory levels below the root, such
  as in a monorepo, and it picks up a changed composer file as soon as it lands
  on disk, also after `composer update` outside the IDE. A changed choice
  applies to open templates straight away.
- A new inspection, Unchecked nette/assets reference, marks with a weak warning
  an asset reference Latte+ cannot check: no assets root and no `default`
  mapper, a mapper path with a DI parameter that cannot be resolved, or a prefix
  no `assets:` mapping declares. Before, such a reference stayed as quiet as one
  that was checked and fine. It and the report of an undeclared mapper offer
  the quick fix "Open Latte+ asset mapper settings", which opens the settings
  prefilled with the mappers read from the configuration the template uses,
  with the reported mapper selected. A configured mapper whose directory does
  not exist is reported, naming the directory.
- New checks in templates no presenter or component renders: `{control x:m}` is
  checked against the class of every `createComponentX()` factory in the
  project and reported when each of them gives a known class and none has the
  render method, and a signal link such as `{link x:save!}` is reported when no
  factory creates `x`. When two presenters share the name a link gives and
  nothing decides between them, an action neither of them has is reported once,
  naming both.

### Changed

- A block behind `from` (`{include foo from 'x.latte'}`) is looked up in that
  template and in its own `{import}` and `{layout}` only, the way Latte reads
  it, never in the template you are editing or its layout. Block completion
  behind `from` offers just those blocks.
- A declared `default` asset mapper whose path cannot be checked no longer
  falls back to the conventional `www/assets` directory: the reference is
  marked as unchecked instead, and completion and Ctrl+B no longer use the
  conventional directory in its place either.
- In a monorepo that installs the same package into several of its packages,
  Ctrl+B and the checks use the copy of a class that belongs to the template's
  own package, the one PhpStorm itself picks, instead of whichever copy was
  found first. A template outside every package is offered all copies.
- With "Align attributes" off, the lines of a multi-line attribute value are
  your own text, as in an HTML file: Reformat Code and Auto-Indent Lines leave
  them where they are, and Enter, Tab, End and Backspace copy the whitespace of
  the line above instead of computing a column, for `n:` attributes and plain
  ones alike. Enter right after a Latte block tag inside the value
  (`class="a {if $x}`) still adds a level.
- Enter, Tab, End and Backspace now compute the indent of a line the same way
  everywhere, so they agree with each other on every line of a multi-line Latte
  tag, of an HTML tag header (a `>` on its own line goes under the tag), below a
  closing tag at the start of a line and below a line typed too deep or too
  shallow.
- With "Indent inside a top-level block" on, Auto-Indent Lines (Ctrl+Alt+I)
  indents the body of an unclosed top-level `{block}` the way Reformat Code
  does, instead of leaving it on column 0.
- With "Indent inside a top-level block" on, a `<style>` element in the body of
  an unclosed top-level `{block}` is formatted the same way as in a closed
  block.
- With "Indent inside a top-level block" on and an unclosed top-level `{block}`
  opened inside HTML elements, Reformat Code keeps the closing tags of those
  elements under their opening tags instead of moving them with the body.
- With "Indent inside a top-level block" on, Reformat Code no longer rewrites
  the inner lines of a multi-line `{* … *}` in the body of an unclosed
  top-level `{block}` to one space before `*`. The comment moves as a whole and
  keeps its own alignment, as it does everywhere else, and Auto-Indent Lines
  gives the same result.
- Pressing Enter in front of the `}` of a Latte tag now looks only at the end
  of the line before the caret, on the first line of the tag as on any later
  one: after a binary operator or a comma the `}` moves one continuation indent
  in, so the expression goes on there; otherwise the tag is finished and the
  `}` goes back under the tag, also when more text follows it (`}</p>`).
  Before, the first line always moved it in (`{if $a|}`).
- Inside the value of a plain HTML attribute, pressing Enter after a `(` or `[`
  at the end of a line, or after a Latte tag left open behind an operator
  (`class="{if $a &&`), puts the new line under the start of the value or, with
  "Align attributes" off, at the indent of the line above, as Tab does. Before,
  it moved one level or one continuation indent in.

### Fixed

- A named argument called like a PHP keyword (`{include card, list: 1}`,
  `match:`, `fn:`, `new:`, `use:`, `as:`, `in:`, `block:` …) is no longer
  reported as a syntax error, is highlighted as a name and binds to the
  parameter of that name in your own tags. `true`, `false`, `null` and `array`
  in front of `:` are reported, as Latte refuses them.
- `key => value` arguments are read the way Latte reads them: accepted in tag
  arguments, arrays, `n:attr` and `n:class` with the keys Latte takes, and
  reported in a call or a filter, which take no `=>`. A hyphenated name is
  joined only without spaces, so `a - b: 1` is an error as in Latte, and
  hyphenated keys in front of `=>` are highlighted as the parser reads them.
- The template name behind `from` is read the way Latte reads it, a `:` inside
  it included, so `{include foo from: 'x.latte'}` means the block `foo` from a
  template named `:` and is reported as missing.
- An asset path written without quotes, `{asset logo.svg}`, is checked like a
  quoted one, and Ctrl+B opens the file. `{asset img/logo.svg}` is a division
  to Latte, not a path, and is left alone.
- `%rootDir%`, `%wwwDir%` and `%appDir%` in the asset configuration come from
  the root of the application the configuration belongs to, so a Nette
  application in a subdirectory of a monorepo no longer has every asset
  reference reported as using a parameter Latte+ cannot resolve.
- Custom extensions: a change in the settings or a new scan applies to open
  templates without waiting for an edit. A discovered `n:attribute` with no
  known signature accepts any value instead of reporting "Wrong attribute
  value"; give it the signature `()` to have a value rejected. An override no
  longer loses the class Ctrl+B leads to and the signature shared with the
  `n:` form, and overrides saved without them get them back on the next scan. A
  tag or a contextual filter no longer gets its description from Latte's own
  classes, and such descriptions are cleared from overrides. A contextual
  filter is no longer reported as requiring an argument or refusing a `string`
  input. The hover of an overridden filter says it is an override.
- Typing a variable name without `$` and picking it from completion inserts the
  `$` as well.
- Renaming a `createComponentX()` factory also renames quoted component names
  such as `{control "parts-1"}` and `{control 'parts-{$id}'}`, and Find Usages
  lists them.
- A signal link to a component whose factory has no readable return type, and
  the child of a component that is not a `Multiplier` (`{control form-rating}`),
  are no longer judged against a class they do not have. Ctrl+B there leads to
  the factory.
- The Ctrl+Q title of `{control x-y:m}` quotes the name as the template writes
  it instead of leaving out the part after the dash.
- Picking a presenter in `n:href` or `n:ifCurrent` opens the list of its
  actions every time, as in `{link}`; before, it often stayed closed. Ctrl+Space
  on a partly typed action (`Order:de`) offers the actions of that presenter in
  every link destination, `isLinkCurrent()` included.
- Brackets and quotes are no longer paired in the element content and in plain
  attributes next to an `n:` attribute, and the quote that opens a block name in
  an `{include}` inside an `n:` value opens its completion again. The quote the
  attribute value itself is written in is never paired and steps over the
  closing quote at the end of the value.
- In a plain attribute value such as `class="text-red-"`, typing a dash no
  longer closes the completion popup, as in an HTML file.
- Wrapping a selection in `{` inside an `n:` attribute value no longer ends with
  an error, and neither does typing a space there or typing on with a
  completion list from a language server open.
- A value printed inside markup written in a `<script>` or `<style>` element is
  checked by the rules of that element, as Latte escapes it.
- Reformat Code puts JavaScript or CSS between `{if}` and `{/if}` (or in a
  `{case}` branch) inside `<script>` and `<style>` one level deeper than the
  tag and no longer adds a blank line next to a Latte tag in CSS. The HTML
  option "Use HTML indents within style and script tags" applies in a `.latte`
  file as in an `.html` one, so JavaScript and CSS lines are no longer indented
  with spaces where the HTML file uses tabs.
- On a line that starts inside a multi-line string of a Latte tag, Enter writes
  no indent and Tab types an indent at the caret, since the whitespace there is
  part of the string.
- With "Indent inside a top-level block" on, repeated Reformat Code no longer
  moves lines in the body of an unclosed top-level `{block}` further right on
  every run: HTML comments, `<pre>`, `<textarea>`, `{syntax off}` and inline
  elements spanning several lines stay where the first Reformat put them.
- An element right after a paired open tag (`{if $a}<div>`, `{foreach}<div>`)
  gets its final indent on the first Reformat Code instead of the second.
- Reformat Code indents a call, a `new`, a parenthesised expression or filter
  arguments broken over several lines inside a Latte tag one level below the
  line holding the `(`, the way Enter, Tab and End do. Until now it flattened
  them to the level of the tag's arguments, or to the column of the tag.
- Reformat Code, Auto-Indent Lines and the indent given to a body when its
  closing tag is finished move a multi-line HTML comment as a whole, as in an
  HTML file: the lines inside it and the line with `-->` follow the line with
  `<!--` and keep their indent relative to it. Until now only the first line
  moved and the comment fell apart. Latte tags inside the comment are moved
  along with it and no longer formatted on their own.
- A multi-line Latte comment `{* … *}` moves as a whole in the same way, inside
  HTML elements and in the body of Latte tags. Until now only the line with
  `{*` moved.
- With "Indent inside a top-level block" on, the body of an unclosed top-level
  `{block}` whose opener is followed by an HTML comment on the same line gets
  its indent on Reformat Code instead of staying on column 0.
- Reformat Code, Auto-Indent Lines and the indent given to a body when its
  closing tag is finished no longer re-indent Latte tags inside `<pre>`,
  `<textarea>`, `<span>` and the other elements listed in HTML "Keep white
  spaces inside": the body of `{if}` or `{foreach}`, the lines of a multi-line
  tag and a Latte line right after another one keep their whitespace, which in
  `<pre>` and `<textarea>` is what the page shows and the form sends. Plain HTML
  content there was already kept.
- Reformat Code no longer pushes the rest of the file one level right after a
  closed Latte tag such as `{block}`, `{if}` or `{foreach}` whose body leaves an
  element open: `{if}` and `{else}` branches that each open their own `<div>`
  closed once below `{/if}`, or an `<li>` or `<p>` without its end tag. The body
  gets one level, the closing tag stays under the opening one and what follows
  keeps its indent. Until now every such tag moved everything below it one
  level further. Auto-Indent Lines, Enter and the indent given to a body when
  its closing tag is finished follow the same layout.
- Reformat Code keeps every continuation line of a multi-line Latte tag
  (`{if $a &&` / `$b}`, `{var}`, `{=…}`, `{foreach}` …) one continuation indent
  below the tag, as the editor writes it, instead of moving it back under the
  tag. Enter, Tab and End agree with it on the third and later lines, in
  `{=…}` / `{$…}` and in the body of a multi-line `{elseif}`.
- Reformat Code indents the body of a `{case}` or `{default}` branch one level
  deeper than the branch, the way the editor writes it when you press Enter
  after `{case …}`, instead of putting it on the column of `{case}`. Enter,
  Tab, Backspace and End on the later lines of a branch now keep the same
  indent.
- Reformat Code no longer adds an empty line before `</script>` each time it
  runs when the last line of the script is a Latte tag, such as the `{/if}` of
  an `{if}…{/if}` or a `{var}`.
- Reformat Code no longer re-indents a Latte tag written over several lines
  inside the value of a plain HTML attribute (`class="{=…}"`, `title`,
  `data-*`): the lines of the value stay as you typed them, as in an HTML file.
  Until now they moved to the column of the line holding the element.

## 1.0.3 - 2026-10-02

### Added

- Tag completion offers `{else}` inside `{try}`, `{first}`, `{last}`, `{sep}`
  and `{ifchanged}`.
- On a Latte 3 project, a Latte tag on the other side of an HTML comment
  boundary than its pair is reported, because Latte 3 compiles a comment as a
  fragment of its own: a closer inside a comment whose opener is outside, a
  clause or a `{case}` inside a comment of a pair outside it, and a pair opened
  inside a comment and closed outside it. Hidden conditional comments such as
  `<!--[if mso]>` are checked the same way. A comment with no `-->` is reported
  where it starts. Latte 2 compiles all of these shapes, so nothing is reported
  there, nor on a project whose Latte version is unknown.
- An `{include}` whose `#` marker is followed by a space and the word `from`,
  with nothing behind it, gets a weak warning that a block name between the
  marker and `from` is probably missing. Latte reads that line as a block named
  `from`, which is almost never what a half-written from-clause meant.
- The name in `n:block`, `n:define` and `n:snippetArea` is highlighted as a
  block declaration, the same way it is in the braced tag.

### Changed

- On a Latte 3 project, closing-tag completion offers only tags opened on the
  same side of an HTML comment boundary as the caret.
- `{case}` and `{default}` have to stand directly inside `{switch}`; one nested
  in another tag of the switch body is reported, and so is `{default}` with
  arguments inside a switch, where it is the clause and not the variable tag.
- Custom paired tags are matched by name, so a closing tag of a custom pair
  with no opener is reported as unexpected, and an opener with no closer as
  unclosed. A tag registered as unpaired no longer waits for a closer.
- `{define name($x)}` is read the way Latte reads it: the parenthesis makes the
  name a function call, so `$x` is not a parameter and is reported as
  undefined. Declare parameters as `{define name, $x}` or `{define name $x}`.
- A dynamic snippet name that Latte can only know at run time gets the advice
  to wrap it in `{snippetArea}` in `n:snippet` too, and when it reads an array
  item such as `{snippet $a['k']}`.
- The `|noescape` check inside HTML comments (Latte 3.1 and newer) also covers
  hidden conditional comments, no longer counts the text behind the first
  `-->` as part of the comment, and stays silent in text, JavaScript, CSS and
  iCal templates, where `<!--` is ordinary text.
- Backspace on an empty line inside `{first}`, `{last}`, `{sep}` and
  `{ifchanged}` unindents to the level of the tag, as it does in `{foreach}`,
  because an `{else}` may follow.
- Quick documentation of `{else}` lists the tags that really take it, now
  including `{try}`, `{first}`, `{last}`, `{sep}` and `{ifchanged}`, and no
  longer `{switch}`.

### Fixed

- `{else}` inside `{first}`, `{last}`, `{sep}` and `{ifchanged}` is no longer
  reported as an error, and the rest of the template is checked again. A second
  `{else}` in these tags is reported as a duplicate, like in `{if}`.
- A paired tag written whole inside an HTML comment, such as
  `<!-- {if $a}…{/if} -->`, no longer breaks the rest of the template, and a
  comment that holds a closing tag or a clause no longer stops the plugin from
  checking everything below it.
- `{else}`, `{elseif}` or `{elseifset}` with no tag around them is reported as
  an unexpected tag, the way Latte reports it, instead of with an internal
  parser message that also switched off the rest of the template.
- `{rollback}` is accepted anywhere inside `{try}`, also nested in another tag,
  and in an element with `n:try`. Completion inserts it at the indent of the
  surrounding code instead of at the column of `{try}`.
- `{l}`, `{r}` and `{}` inside an HTML comment are read as literal braces, not
  reported as unknown tags.
- The word `from` in the arguments of an `{include}` no longer breaks the tag:
  a call such as `Foo::from($id)`, an array key or an argument named `from` is
  an ordinary name again. Until now the tag failed to parse and every tag below
  it lost its colours and checks. `from` separates a block from its template
  only between the target and the first argument, which is where Latte reads
  it.
- A member read whose name is computed in braces, such as `$item->{$name}`,
  `$item?->{$name}` or `Foo::{$name}`, is no longer a syntax error.
- A block name may be any expression Latte accepts, behind the `block` keyword
  and the `#` marker in `{include}` and `{embed}`, and in `{block}`,
  `{define}`, `{snippet}`, `n:define`, `n:block` and `n:snippet`: a property, an
  array item, a class constant, a call or an operator, such as
  `{include block $item->name from 'x.latte'}`, `{define Foo::BAR}` or
  `{snippet $a + $b}`. These were syntax errors, which on a paired tag took its
  body and the rest of the template with it.
- `{include block}` and `{include block, 5}` include the block named `block`,
  as in Latte, instead of being a syntax error. The name is checked for a
  missing block and Ctrl+B follows it.
- A block name glued with `-` or `/`, such as `{block a/b}` or
  `{include block a/b}`, is one name: it is no longer a syntax error, it is
  painted as one name, Ctrl+B and the missing-block check work over the whole
  of it, and block completion offers it whole. `{include #content-2}` now
  includes the block `content-2` instead of `content` with `-2` as an argument.
  An unmarked `{include a/b}` stays a file, as in Latte.
- Holding Ctrl over a block name glued from several parts underlines the whole
  name instead of one part at a time.
- A block named with a Latte keyword is highlighted and checked like any other:
  `{include block from from 'x.latte'}` and `{include #from}` now report a block
  that does not exist and navigate to one that does. The `#` marker followed by
  a space and `from` is painted as a block name too, and Ctrl+B follows it.
- A block included from a template that only run time can tell, such as
  `{include inner from $paths['layout']}`, is no longer reported as missing,
  and the "Did you mean" fix no longer suggests the very name it reports.
- A block name built by concatenation behind the `block` keyword, the `#`
  marker or a from-clause no longer offers Ctrl+B to a file on its pieces.
- With the Czech language pack, every inspection, settings page, quick fix and
  notification is in Czech, instead of mixing in English wherever a
  translation was missing. The Czech missing-asset message shows the directory
  that was searched instead of always naming `www/assets/`.
- PHP class names in the settings texts keep their backslashes, so
  `Latte\Extension` no longer shows as `LatteExtension`.
- The notification and the preview of the "Add to custom Latte extensions"
  quick fix spell the attribute category as `n:attribute`, without a space.

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
