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

![Latte and HTML highlighted together]({{ '/assets/img/screens/S01-syntax-highlighting.png' | relative_url }})

### Sticky lines

Deep inside a nested loop you still see where you are: the five enclosing lines stay
pinned at the top, Latte tags and HTML elements alike. No scrolling up to find out which
`{block}` and which `<ul>` you are in.

![Sticky lines pinned above the viewport]({{ '/assets/img/screens/S02-sticky-lines.png' | relative_url }})

### Paired-tag highlighting and breadcrumbs

Put the caret on a `{/foreach}` and the `{foreach}` it closes lights up 14 lines above,
while the bar under the editor spells out the whole path of tags around the caret.
Working out what a closing tag belongs to stops being guesswork.

![A closing tag highlighted together with its opening tag]({{ '/assets/img/screens/S03-paired-tag-breadcrumbs.png' | relative_url }})

### Code folding

Collapse a `{foreach}` you are not working on and the rest of the template fits on one
screen. Folding follows the tag structure, so blocks collapse at their real boundaries.

![A collapsed foreach block]({{ '/assets/img/screens/S04-code-folding.png' | relative_url }})

### Structure view

`Alt+7` turns the template into a map of its Latte tags: blocks, `{define}`,
`{snippet}`, `{embed}`, `{include}` and the control-flow tags, nested the way they are
nested in the file. The node at the caret is selected, so the panel and the breadcrumbs
under the editor tell you the same thing from two directions. Clicking a node jumps
there, the toolbar sorts by name, and `Ctrl+F12` shows the same tree as a popup.

![The structure panel listing a template's Latte tags, the if inside a foreach inside a block selected]({{ '/assets/img/screens/S05-structure-view.png' | relative_url }})

---

## Embedded languages

### JavaScript and CSS inside a template

Script and style blocks keep the support you would get in a `.js` or `.css` file, and
Latte expressions inside them are still recognised. A JavaScript object literal is not
mistaken for a Latte tag, so nothing turns red just because you typed `{`.

![JavaScript and CSS embedded in a template]({{ '/assets/img/screens/S23-embedded-js-css.png' | relative_url }})

### PHP inside `{php}` and `{do}`

The body of `{php}` and `{do}` is handled as real PHP – the status bar says so – so
completion, inspections and navigation work there as they do in a `.php` file. PHP calls
elsewhere in the template are annotated with parameter names, here `num:`.

![PHP highlighted inside php and do tags]({{ '/assets/img/screens/S24-php-completion.png' | relative_url }})

---

## Completion

### Latte tags

Start a tag and Latte+ offers every valid one, each showing the closing tag it will
insert for you.

![Tag completion after an opening brace]({{ '/assets/img/screens/S06-tag-completion.png' | relative_url }})

### Latte tags inside an attribute value

A tag completes inside an attribute value as readily as anywhere else, here in `class`
with `btn ` already typed in front of it. Tags your own project registers are in the
list next to the built-in ones, each with the arguments it takes.

![Latte tag completion inside an attribute value]({{ '/assets/img/screens/S38-tag-in-attribute-value.png' | relative_url }})

### PHP members, with types

`$image->` completes against the item type of the collection you are looping over, read
from the `{varType}` at the top of the file. Every suggestion carries the class it comes
from and its type, so you don't switch to the PHP class to check a name.

![Member completion with inferred types]({{ '/assets/img/screens/S07-member-completion.png' | relative_url }})

### Variables

Everything in scope in one list: variables from the template itself, the ones Nette
injects, and any you added in the settings – each labelled with where it comes from and
what type it has.

![Variable completion]({{ '/assets/img/screens/S08-variable-completion.png' | relative_url }})

### Filters

Filters complete after `|` with the arguments they take, so you can pick one and fill it
in without opening the documentation. The letters you type are matched anywhere in the
name, so `tr` also finds `stripTags`.

![Filter completion]({{ '/assets/img/screens/S09-filter-completion.png' | relative_url }})

### Block names from another file

`{include #` lists the blocks the referenced file really defines, each with the file it
came from – no jumping to the other template to recall a name.

![Block name completion across files]({{ '/assets/img/screens/S10-block-completion.png' | relative_url }})

### Paths through an alias, fuzzy

Type a few letters of the file you want – `PaGal` finds `~Parts/Gallery.latte`, even
across the directory separator – and each suggestion shows the real folder the alias
resolves to.

![Fuzzy path completion through an alias]({{ '/assets/img/screens/S11-alias-completion.png' | relative_url }})

### n:attributes

Typing `n:if` narrows the list to the attributes that match, `inner-` variants included,
so you pick the one you meant instead of recalling the full set.

![n:attribute completion]({{ '/assets/img/screens/S12-nattr-completion.png' | relative_url }})

### Closing tags

A paired tag inserted from completion already comes with its closing tag, so normally you
write nothing. When the closing tag is missing – you deleted it, or the code came from
somewhere else – `{/` fills in the nearest open tag on its own, and completion inside it
lets you pick a different one: the tags open at that position, innermost first.

![Closing tag completion]({{ '/assets/img/screens/S13-close-tag-completion.png' | relative_url }})

### Components from their factory methods

`{control comm` offers the components the presenter really has – they are read from its
`createComponent*` methods, so a mistyped name doesn't survive until you load the page.

![Component completion]({{ '/assets/img/screens/S25-control-completion.png' | relative_url }})

### Arguments of the included template

Put a comma after the template path and you get the parameters that template declares:
`images` is required and expects a `list<App\Model\Image>`, `columns` is optional and
takes an `int`. Both the names and the types come from the `{parameters}` line of the
file you are including, so you can fill an `{include}` in correctly – and see what you
must not forget – without opening the other template at all.

![Completion of the arguments an included template declares]({{ '/assets/img/screens/S29-include-args-completion.png' | relative_url }})

### Form fields inside `{form}`

`{input }` offers the fields the form factory actually builds, each with the method that
created it – `email` from `addEmail`, `frequency` from `addSelect`. PHP classes are
offered next to them on purpose: a field name may also be a constant, as in
`{input Form::FIELD_EMAIL}`.

![Completion of form field names inside a form tag]({{ '/assets/img/screens/S30-form-input-completion.png' | relative_url }})

---

## Documentation and parameters

### Tag documentation

`Ctrl+Q` on a tag explains what the tag does, shows its syntax on an example and links
straight to the matching page of the official Latte documentation.

![Quick documentation for a tag]({{ '/assets/img/screens/S14-quick-doc-tag.png' | relative_url }})

### Argument documentation from the target template

`Ctrl+Q` on an argument answers what you would otherwise open the other file for: its
type, where it is declared, its default value, and that it is optional.

![Quick documentation for an include argument]({{ '/assets/img/screens/S15-quick-doc-argkey.png' | relative_url }})

### Parameter info

`Ctrl+P` shows the full parameter list of the template you are including, with types and
defaults, while you are still typing the arguments.

![Parameter info for an include]({{ '/assets/img/screens/S16-param-info.png' | relative_url }})

### Inlay hints

Positional arguments are labelled with the name they fill – in filters, in `{control}`
and in links alike – so `120`, `5` and the value behind `Article:detail` read as
`length`, `limit` and `id` without counting commas.

![Inlay hints in front of positional arguments]({{ '/assets/img/screens/S17-inlay-hints.png' | relative_url }})

---

## Inspections and quick fixes

### Several problems at once

Six mistakes in one template, each named and located: a misspelled `n:attribute`, a
filter that isn't registered, a file that isn't there, an unknown property, a block
nobody defines and a component with no factory. Every one of them is something you would
otherwise meet as an error page in the browser.

![Inspections in a single template]({{ '/assets/img/screens/S18-inspections.png' | relative_url }})

### Did you mean?

The correction is offered right where the typo is, with a preview of the result. If the
name is not a typo but something your project defines, you can register it instead and
the report goes away.

![Quick fix for a misspelled n:attribute]({{ '/assets/img/screens/S19-quickfix-nattr.png' | relative_url }})

### Missing template

An `{include}` pointing at a file that does not exist offers to create the file on the
spot, so you can keep writing the template you are in.

![Quick fix creating a missing template]({{ '/assets/img/screens/S20-quickfix-create-file.png' | relative_url }})

The fix asks for a name and offers the directory the reference itself names, so the new
template lands where the `{include}` is looking – and opens ready to write in:

![Creating the missing template, start to finish]({{ '/assets/video/V06-create-missing-template.gif' | relative_url }})

### A form name that does not exist

`<form n:name="…">` is checked against the forms the presenter really builds, so a typo
is reported where you wrote it and the fix renames it to the closest name that exists.
`Ctrl+Q` on the fix shows what it will do before you accept it.

![Quick fix for a misspelled form name]({{ '/assets/img/screens/S39-form-name-quickfix.png' | relative_url }})

---

## Navigation

### From a link to the code behind it

`Ctrl+B` on a link destination offers both places it lives – the template that renders
the action, listed first, and the presenter method behind it – so a link takes you to
either one in a single step, without opening the presenter to find the template.

![Go to declaration from a link]({{ '/assets/img/screens/S21-goto-link-target.png' | relative_url }})

### From a component tag to the component

`Ctrl+B` on `{control commentList}` lands on the factory method that builds that
component, instead of you searching the presenter for the right `createComponent*`.

![Go to declaration from a control tag]({{ '/assets/img/screens/S31-goto-control.png' | relative_url }})

### From a typed variable to both of its sources

A variable that a `{varType}` types and a presenter fills has two places worth opening,
and `Ctrl+B` offers both: the `{varType}` line that gives it a type, and the presenter
method that puts the value in, named in full so you know where you are going.

![Go to declaration on a typed variable]({{ '/assets/img/screens/S37-goto-typed-variable.png' | relative_url }})

### Find usages of a block across the project

Find Usages on a `{define}` collects every `{include}` that pulls the block in, anywhere
in the project – so you know what you are about to break before you change it.

![Find usages of a block]({{ '/assets/img/screens/S22-find-usages-block.png' | relative_url }})

### Find usages of a template parameter

On an optional parameter, Find Usages lists every use of it – the reads inside the
template and the `{include}` arguments that pass it. Callers that leave the default
alone are not in the list, so you get the answer to "who actually sets this?" instead
of "where is this template used?".

![Find usages of an optional template parameter]({{ '/assets/img/screens/S35-find-usages-parameter.png' | relative_url }})

### PHP that only a template uses

A `createComponentX()` factory is never called by name in PHP, because the framework
builds the name out of what the template writes. Latte+ indexes those names, so the
three factories this template reaches read as live code, while the fourth one, which no
template names, is greyed out as unused.

![Component factories kept alive by a template, next to one that no template names]({{ '/assets/img/screens/S36-kept-alive-by-template.png' | relative_url }})

---

## Editing

### Tabs for structure, spaces for alignment

Whitespace rendering shows what the editor actually writes into a multi-line tag: tabs
carry the indent, spaces only pad the alignment of the wrapped attributes. Enter, Tab
and Reformat all produce the same result, so a template keeps one consistent shape no
matter how you got there.

![Tab indentation and space alignment shown with whitespace rendering]({{ '/assets/img/screens/S32-smart-tabs.png' | relative_url }})

Indenting is a movement, so here it is as one. A template with no indentation at all is
straightened by a single Reformat; a class added inside a multi-line `n:class` lines up
with the list it joins; joining two attributes leaves exactly one space between them; and
`{else}` snaps back onto its `{if}` as it is accepted. Whitespace rendering is on
throughout, so every tab and every space is visible:

![Auto-indent, smart tabs and a clause snapping back onto its tag]({{ '/assets/video/V04-smart-indent.gif' | relative_url }})

---

## Configuration

### Path aliases

Tell Latte+ which prefix your project uses – here `~` – and which folders it stands for,
and every `{include}`, `{layout}` or `{embed}` written with it resolves, completes and
navigates. Path-based and name-based references can search different folders.

![Path alias settings]({{ '/assets/img/screens/S26-settings-path-aliases.png' | relative_url }})

### Implicit variables

Variables your framework hands to every template are known without declaring them.
The Nette ones are built in, `$iterator` only counts inside a `foreach`, and you can add
your own project-wide variables or switch a built-in one off.

![Implicit variable settings]({{ '/assets/img/screens/S33-settings-implicit-variables.png' | relative_url }})

### Custom extensions

Tags, filters, functions and `n:attributes` from your project's own Latte extensions are
found automatically and treated like the built-in ones. Give a tag a signature and a
description and both show up in completion and documentation.

![Custom extension settings]({{ '/assets/img/screens/S27-settings-custom-extensions.png' | relative_url }})

### Signatures for your own tags

Describe a tag once – here `{icon}` takes a required name and an optional size – and the
editor treats it like a built-in tag. Each argument is labelled with the parameter it
fills, optional ones are marked with `?`, and the call that leaves the required argument
out is reported like any other problem.

![A custom tag with its arguments labelled and a missing argument reported]({{ '/assets/img/screens/S34-custom-tag-signature.png' | relative_url }})

### Token colors

Every kind of Latte token has its own colour setting, with a live preview underneath.
`n:attributes` are configured separately from HTML attributes, so `n:href` can either
stand out or blend in.

![Latte token colour settings]({{ '/assets/img/screens/S28-settings-colors.png' | relative_url }})

---

Short clips of the interactive features – live paired-tag rename, cross-file parameter
rename and smart indentation – will be added here later.
