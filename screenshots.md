---
layout: default
title: Screenshots
nav_order: 3
---

# Screenshots
{: .no_toc }

A tour of Latte+ in the editor. If you are deciding whether the plugin is worth
installing, this page is the fastest way to see what it actually does – every feature
described elsewhere in this documentation is shown here on real code.

All shots were taken in PhpStorm 2025.3 with the default dark theme, on a small Nette
blog with a real `vendor/` directory – so completion, hover, inspections and Find Usages
behave exactly as they do in a project of your own, not in a stripped-down demo.

1. TOC
{:toc}

---

## Syntax and the editor

### Latte and HTML in one file

Tags, PHP expressions and filters get their own colours, and the HTML around them keeps
the colouring you already know. The check mark in the top-right corner says the whole
template is clean – there is no set of phantom errors to learn to ignore.

<img src="{{ '/assets/img/screens/S01-syntax-highlighting.png' | relative_url }}" alt="Latte and HTML highlighted together" loading="lazy" decoding="async">

### Sticky lines

Deep inside a nested loop you still see where you are: the five enclosing lines stay
pinned at the top, Latte tags and HTML elements alike. No scrolling up to find out which
`{block}` and which `<ul>` you are in.

<img src="{{ '/assets/img/screens/S02-sticky-lines.png' | relative_url }}" alt="Sticky lines pinned above the viewport" loading="lazy" decoding="async">

A still shows the result; what it cannot show is the swap. As you scroll, the pinned rows
are exchanged for the ones that now enclose you, Latte tags and HTML elements mixed:

<video class="clip" width="1600" height="1000" autoplay loop muted playsinline
       preload="metadata" style="max-width:100%;height:auto" aria-label="Sticky lines exchanged while scrolling">
  <source src="{{ '/assets/video/V05-sticky-lines.webm' | relative_url }}" type="video/webm">
  <source src="{{ '/assets/video/V05-sticky-lines.mp4' | relative_url }}" type="video/mp4">
</video>

### Paired-tag highlighting and breadcrumbs

Put the caret on a `{/foreach}` and the `{foreach}` it closes lights up 14 lines above,
while the bar under the editor spells out the whole path of tags around the caret.
Working out what a closing tag belongs to stops being guesswork.

<img src="{{ '/assets/img/screens/S03-paired-tag-breadcrumbs.png' | relative_url }}" alt="A closing tag highlighted together with its opening tag" loading="lazy" decoding="async">

The pair is live, not just highlighted: rename one half and the other follows as you
type, in either direction, with no refactoring dialog in the way.

<video class="clip" width="1600" height="1000" autoplay loop muted playsinline
       preload="metadata" style="max-width:100%;height:auto" aria-label="Renaming a paired tag, the other half following along">
  <source src="{{ '/assets/video/V01-live-tag-rename.webm' | relative_url }}" type="video/webm">
  <source src="{{ '/assets/video/V01-live-tag-rename.mp4' | relative_url }}" type="video/mp4">
</video>

### Code folding

Collapse a `{foreach}` you are not working on and the rest of the template fits on one
screen. Folding follows the tag structure, so blocks collapse at their real boundaries.

<img src="{{ '/assets/img/screens/S04-code-folding.png' | relative_url }}" alt="A collapsed foreach block" loading="lazy" decoding="async">

### Structure view

`Alt+7` turns the template into a map of its Latte tags: blocks, `{define}`,
`{snippet}`, `{embed}`, `{include}` and the control-flow tags, nested the way they are
nested in the file. The node at the caret is selected, so the panel and the breadcrumbs
under the editor tell you the same thing from two directions. Clicking a node jumps
there, the toolbar sorts by name, and `Ctrl+F12` shows the same tree as a popup.

<img src="{{ '/assets/img/screens/S05-structure-view.png' | relative_url }}" alt="The structure panel listing a template's Latte tags, the if inside a foreach inside a block selected" loading="lazy" decoding="async">

---

## Embedded languages

### JavaScript and CSS inside a template

Script and style blocks keep the support you would get in a `.js` or `.css` file, and
Latte expressions inside them are still recognised. A JavaScript object literal is not
mistaken for a Latte tag, so nothing turns red just because you typed `{`.

<img src="{{ '/assets/img/screens/S23-embedded-js-css.png' | relative_url }}" alt="JavaScript and CSS embedded in a template" loading="lazy" decoding="async">

### PHP inside `{php}` and `{do}`

The body of `{php}` and `{do}` is handled as real PHP – the status bar says so – so
completion, inspections and navigation work there as they do in a `.php` file. PHP calls
elsewhere in the template are annotated with parameter names, here `num:`.

<img src="{{ '/assets/img/screens/S24-php-completion.png' | relative_url }}" alt="PHP highlighted inside php and do tags" loading="lazy" decoding="async">

---

## Completion

### Latte tags

Start a tag and Latte+ offers every valid one, each showing the closing tag it will
insert for you.

<img src="{{ '/assets/img/screens/S06-tag-completion.png' | relative_url }}" alt="Tag completion after an opening brace" loading="lazy" decoding="async">

The same thing while a tag is actually being written: the list opens on `{` without being
asked, a paired tag arrives as its three-line shape with the caret already inside, and the
member offered next comes from the type the loop variable really has:

<video class="clip" width="1600" height="1000" autoplay loop muted playsinline
       preload="metadata" style="max-width:100%;height:auto" aria-label="Writing a foreach from the opening brace to a typed member">
  <source src="{{ '/assets/video/V02-write-foreach.webm' | relative_url }}" type="video/webm">
  <source src="{{ '/assets/video/V02-write-foreach.mp4' | relative_url }}" type="video/mp4">
</video>

### Latte tags inside an attribute value

A tag completes inside an attribute value as readily as anywhere else, here in `class`
with `btn ` already typed in front of it. Tags your own project registers are in the
list next to the built-in ones, each with the arguments it takes.

<img src="{{ '/assets/img/screens/S38-tag-in-attribute-value.png' | relative_url }}" alt="Latte tag completion inside an attribute value" loading="lazy" decoding="async">

### PHP members, with types

`$image->` completes against the item type of the collection you are looping over, read
from the `{varType}` at the top of the file. Every suggestion carries the class it comes
from and its type, so you don't switch to the PHP class to check a name.

<img src="{{ '/assets/img/screens/S07-member-completion.png' | relative_url }}" alt="Member completion with inferred types" loading="lazy" decoding="async">

### Variables

Everything in scope in one list: variables from the template itself, the ones Nette
injects, and any you added in the settings – each labelled with where it comes from and
what type it has.

<img src="{{ '/assets/img/screens/S08-variable-completion.png' | relative_url }}" alt="Variable completion" loading="lazy" decoding="async">

### Filters

Filters complete after `|` with the arguments they take, so you can pick one and fill it
in without opening the documentation. The letters you type are matched anywhere in the
name, so `tr` also finds `stripTags`.

<img src="{{ '/assets/img/screens/S09-filter-completion.png' | relative_url }}" alt="Filter completion" loading="lazy" decoding="async">

### Block names from another file

`{include #` lists the blocks the referenced file really defines, each with the file it
came from – no jumping to the other template to recall a name.

<img src="{{ '/assets/img/screens/S10-block-completion.png' | relative_url }}" alt="Block name completion across files" loading="lazy" decoding="async">

### Paths through an alias, fuzzy

Type a few letters of the file you want – `PaGal` finds `~Parts/Gallery.latte`, even
across the directory separator – and each suggestion shows the real folder the alias
resolves to.

<img src="{{ '/assets/img/screens/S11-alias-completion.png' | relative_url }}" alt="Fuzzy path completion through an alias" loading="lazy" decoding="async">

### n:attributes

Typing `n:if` narrows the list to the attributes that match, `inner-` variants included,
so you pick the one you meant instead of recalling the full set.

<img src="{{ '/assets/img/screens/S12-nattr-completion.png' | relative_url }}" alt="n:attribute completion" loading="lazy" decoding="async">

### Closing tags

A paired tag inserted from completion already comes with its closing tag, so normally you
write nothing. When the closing tag is missing – you deleted it, or the code came from
somewhere else – `{/` fills in the nearest open tag on its own, and completion inside it
lets you pick a different one: the tags open at that position, innermost first.

<img src="{{ '/assets/img/screens/S13-close-tag-completion.png' | relative_url }}" alt="Closing tag completion" loading="lazy" decoding="async">

### Components from their factory methods

`{control comm` offers the components the presenter really has – they are read from its
`createComponent*` methods, so a mistyped name doesn't survive until you load the page.

<img src="{{ '/assets/img/screens/S25-control-completion.png' | relative_url }}" alt="Component completion" loading="lazy" decoding="async">

### Arguments of the included template

Put a comma after the template path and you get the parameters that template declares:
`images` is required and expects a `list<App\Model\Image>`, `columns` is optional and
takes an `int`. Both the names and the types come from the `{parameters}` line of the
file you are including, so you can fill an `{include}` in correctly – and see what you
must not forget – without opening the other template at all.

<img src="{{ '/assets/img/screens/S29-include-args-completion.png' | relative_url }}" alt="Completion of the arguments an included template declares" loading="lazy" decoding="async">

### Form fields inside `{form}`

`{input }` offers the fields the form factory actually builds, each with the method that
created it – `email` from `addEmail`, `frequency` from `addSelect`. PHP classes are
offered next to them on purpose: a field name may also be a constant, as in
`{input Form::FIELD_EMAIL}`.

<img src="{{ '/assets/img/screens/S30-form-input-completion.png' | relative_url }}" alt="Completion of form field names inside a form tag" loading="lazy" decoding="async">

---

## Documentation and parameters

### Tag documentation

`Ctrl+Q` on a tag explains what the tag does, shows its syntax on an example and links
straight to the matching page of the official Latte documentation.

<img src="{{ '/assets/img/screens/S14-quick-doc-tag.png' | relative_url }}" alt="Quick documentation for a tag" loading="lazy" decoding="async">

### Argument documentation from the target template

`Ctrl+Q` on an argument answers what you would otherwise open the other file for: its
type, where it is declared, its default value, and that it is optional.

<img src="{{ '/assets/img/screens/S15-quick-doc-argkey.png' | relative_url }}" alt="Quick documentation for an include argument" loading="lazy" decoding="async">

### Parameter info

`Ctrl+P` shows the full parameter list of the template you are including, with types and
defaults, while you are still typing the arguments.

<img src="{{ '/assets/img/screens/S16-param-info.png' | relative_url }}" alt="Parameter info for an include" loading="lazy" decoding="async">

### Inlay hints

Positional arguments are labelled with the name they fill – in filters, in `{control}`
and in links alike – so `120`, `5` and the value behind `Article:detail` read as
`length`, `limit` and `id` without counting commas.

<img src="{{ '/assets/img/screens/S17-inlay-hints.png' | relative_url }}" alt="Inlay hints in front of positional arguments" loading="lazy" decoding="async">

---

## Inspections and quick fixes

### Several problems at once

Six mistakes in one template, each named and located: a misspelled `n:attribute`, a
filter that isn't registered, a file that isn't there, an unknown property, a block
nobody defines and a component with no factory. Every one of them is something you would
otherwise meet as an error page in the browser.

<img src="{{ '/assets/img/screens/S18-inspections.png' | relative_url }}" alt="Inspections in a single template" loading="lazy" decoding="async">

### Did you mean?

The correction is offered right where the typo is, with a preview of the result. If the
name is not a typo but something your project defines, you can register it instead and
the report goes away.

<img src="{{ '/assets/img/screens/S19-quickfix-nattr.png' | relative_url }}" alt="Quick fix for a misspelled n:attribute" loading="lazy" decoding="async">

### Missing template

An `{include}` pointing at a file that does not exist offers to create the file on the
spot, so you can keep writing the template you are in.

<img src="{{ '/assets/img/screens/S20-quickfix-create-file.png' | relative_url }}" alt="Quick fix creating a missing template" loading="lazy" decoding="async">

The fix asks for a name and offers the directory the reference itself names, so the new
template lands where the `{include}` is looking – and opens ready to write in:

<video class="clip" width="1600" height="1000" autoplay loop muted playsinline
       preload="metadata" style="max-width:100%;height:auto" aria-label="Creating the missing template, start to finish">
  <source src="{{ '/assets/video/V06-create-missing-template.webm' | relative_url }}" type="video/webm">
  <source src="{{ '/assets/video/V06-create-missing-template.mp4' | relative_url }}" type="video/mp4">
</video>

### A form name that does not exist

`<form n:name="…">` is checked against the forms the presenter really builds, so a typo
is reported where you wrote it and the fix renames it to the closest name that exists.
`Ctrl+Q` on the fix shows what it will do before you accept it.

<img src="{{ '/assets/img/screens/S39-form-name-quickfix.png' | relative_url }}" alt="Quick fix for a misspelled form name" loading="lazy" decoding="async">

---

## Navigation

### From a link to the code behind it

`Ctrl+B` on a link destination offers both places it lives – the template that renders
the action, listed first, and the presenter method behind it – so a link takes you to
either one in a single step, without opening the presenter to find the template.

<img src="{{ '/assets/img/screens/S21-goto-link-target.png' | relative_url }}" alt="Go to declaration from a link" loading="lazy" decoding="async">

### From a component tag to the component

`Ctrl+B` on `{control commentList}` lands on the factory method that builds that
component, instead of you searching the presenter for the right `createComponent*`.

<img src="{{ '/assets/img/screens/S31-goto-control.png' | relative_url }}" alt="Go to declaration from a control tag" loading="lazy" decoding="async">

### From a typed variable to both of its sources

A variable that a `{varType}` types and a presenter fills has two places worth opening,
and `Ctrl+B` offers both: the `{varType}` line that gives it a type, and the presenter
method that puts the value in, named in full so you know where you are going.

<img src="{{ '/assets/img/screens/S37-goto-typed-variable.png' | relative_url }}" alt="Go to declaration on a typed variable" loading="lazy" decoding="async">

### Find usages of a block across the project

Find Usages on a `{define}` collects every `{include}` that pulls the block in, anywhere
in the project – so you know what you are about to break before you change it.

<img src="{{ '/assets/img/screens/S22-find-usages-block.png' | relative_url }}" alt="Find usages of a block" loading="lazy" decoding="async">

### Find usages of a template parameter

On an optional parameter, Find Usages lists every use of it – the reads inside the
template and the `{include}` arguments that pass it. Callers that leave the default
alone are not in the list, so you get the answer to "who actually sets this?" instead
of "where is this template used?".

<img src="{{ '/assets/img/screens/S35-find-usages-parameter.png' | relative_url }}" alt="Find usages of an optional template parameter" loading="lazy" decoding="async">

Because the usages are known, the parameter can be renamed rather than found and edited by
hand: the declaration in `{parameters}` and the argument passed from another template change
together.

<video class="clip" width="1600" height="1000" autoplay loop muted playsinline
       preload="metadata" style="max-width:100%;height:auto" aria-label="Renaming a template parameter across two files">
  <source src="{{ '/assets/video/V03-rename-across-files.webm' | relative_url }}" type="video/webm">
  <source src="{{ '/assets/video/V03-rename-across-files.mp4' | relative_url }}" type="video/mp4">
</video>

### PHP that only a template uses

A `createComponentX()` factory is never called by name in PHP, because the framework
builds the name out of what the template writes. Latte+ indexes those names, so the
three factories this template reaches read as live code, while the fourth one, which no
template names, is greyed out as unused.

<img src="{{ '/assets/img/screens/S36-kept-alive-by-template.png' | relative_url }}" alt="Component factories kept alive by a template, next to one that no template names" loading="lazy" decoding="async">

---

## Editing

### Tabs for structure, spaces for alignment

Whitespace rendering shows what the editor actually writes into a multi-line tag: tabs
carry the indent, spaces only pad the alignment of the wrapped attributes. Enter, Tab
and Reformat all produce the same result, so a template keeps one consistent shape no
matter how you got there.

<img src="{{ '/assets/img/screens/S32-smart-tabs.png' | relative_url }}" alt="Tab indentation and space alignment shown with whitespace rendering" loading="lazy" decoding="async">

Indenting is a movement, so here it is as one. A template with no indentation at all is
straightened by a single Reformat; a class added inside a multi-line `n:class` lines up
with the list it joins; joining two attributes leaves exactly one space between them; and
`{else}` snaps back onto its `{if}` as it is accepted. Whitespace rendering is on
throughout, so every tab and every space is visible:

<video class="clip" width="1600" height="1000" autoplay loop muted playsinline
       preload="metadata" style="max-width:100%;height:auto" aria-label="Auto-indent, smart tabs and a clause snapping back onto its tag">
  <source src="{{ '/assets/video/V04-smart-indent.webm' | relative_url }}" type="video/webm">
  <source src="{{ '/assets/video/V04-smart-indent.mp4' | relative_url }}" type="video/mp4">
</video>

---

## Configuration

### Path aliases

Tell Latte+ which prefix your project uses – here `~` – and which folders it stands for,
and every `{include}`, `{layout}` or `{embed}` written with it resolves, completes and
navigates. Path-based and name-based references can search different folders.

<img src="{{ '/assets/img/screens/S26-settings-path-aliases.png' | relative_url }}" alt="Path alias settings" loading="lazy" decoding="async">

### Implicit variables

Variables your framework hands to every template are known without declaring them.
The Nette ones are built in, `$iterator` only counts inside a `foreach`, and you can add
your own project-wide variables or switch a built-in one off.

<img src="{{ '/assets/img/screens/S33-settings-implicit-variables.png' | relative_url }}" alt="Implicit variable settings" loading="lazy" decoding="async">

### Custom extensions

Tags, filters, functions and `n:attributes` from your project's own Latte extensions are
found automatically and treated like the built-in ones. Give a tag a signature and a
description and both show up in completion and documentation.

<img src="{{ '/assets/img/screens/S27-settings-custom-extensions.png' | relative_url }}" alt="Custom extension settings" loading="lazy" decoding="async">

### Signatures for your own tags

Describe a tag once – here `{icon}` takes a required name and an optional size – and the
editor treats it like a built-in tag. Each argument is labelled with the parameter it
fills, optional ones are marked with `?`, and the call that leaves the required argument
out is reported like any other problem.

<img src="{{ '/assets/img/screens/S34-custom-tag-signature.png' | relative_url }}" alt="A custom tag with its arguments labelled and a missing argument reported" loading="lazy" decoding="async">

### Token colors

Every kind of Latte token has its own colour setting, with a live preview underneath.
`n:attributes` are configured separately from HTML attributes, so `n:href` can either
stand out or blend in.

<img src="{{ '/assets/img/screens/S28-settings-colors.png' | relative_url }}" alt="Latte token colour settings" loading="lazy" decoding="async">

---

Short clips of the interactive features – live paired-tag rename, cross-file parameter
rename and smart indentation – will be added here later.
