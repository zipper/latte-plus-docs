---
layout: default
title: Real-project audit
parent: Comparison
nav_order: 1
---

# Audit on a real project

Feature tables say what a plugin *can* do. They say nothing about how loud it is on a
codebase nobody wrote as a demo.
{: .fs-6 .fw-300 }

<span class="label label-blue">Latte+ 1.0.2</span> <span class="label label-green">378 templates</span> <span class="label label-purple">Latte 2.11.7</span> <span class="label">monorepo</span> <span class="label label-yellow">zero manual setup</span>

So Latte+ and another Latte plugin were pointed at the same real project, in two sandboxes
of the same PhpStorm, and every report either of them made – error, warning and weak
warning alike – was collected and classified.

**The project.** A monorepo: two Nette applications plus a shared template library that
has no PHP counterpart of its own. **378 `.latte` files**, **Latte 2.11.7** on Nette 3.1.
Templates are included across application boundaries and the library's templates are
reached through a path alias – the kind of layout that makes a plugin either resolve
things properly or start guessing.

<div class="la-note" markdown="1">
**This is Latte+ 1.0.2, measured on a Latte 2 project.**

Latte+ targets **Latte 3**, and Latte 2 support is deliberately strong rather than an
afterthought – but of the two, a Latte 2 codebase is the harder test, which is why it was
picked. The numbers below are the second round, after a first pass of fixes, on the
plugin's *worse* footing. They are a snapshot, not a ceiling: the audit is repeated after
each round of fixes, and a companion audit on a Latte 3 project will follow.
</div>

<div class="la-note la-note-warn" markdown="1">
**Measured out of the box.**

Both plugins let you declare a project's own tags and filters by hand. Latte+ ran with that
list empty – it finds registrations by scanning the code. The other plugin ran with part of
its project configuration filled in by hand, which is the route it needs, since it does not
read registrations from code.

One thing was configured on the Latte+ side: the project's `~` path alias, which is how its
template library is addressed. It is left out of every number on this page, for both
plugins. Other Latte plugins have nowhere to declare such an alias: the only thing you can do
about the reports is switch the check off. With it on, the other plugin raises **1849 errors** on
references that resolve perfectly well at runtime. Latte+ has the equivalent check, on by default
and at error severity too, and the alias declaration is what keeps it quiet – so those references
are left out on both sides, and everything else is compared as measured.
</div>

## How to read these numbers

A report is not automatically a good thing or a bad thing, so the two kinds are kept apart
everywhere on this page:

<div class="la-howto">
  <div class="la-howto-row">
    <span class="la-dir la-dir-down">lower is better</span>
    <p><b>Noise</b> – reports over code that is correct. A missing check is a debt that holds
    nobody up; a report over valid code holds up everyone who opens the file.</p>
  </div>
  <div class="la-howto-row">
    <span class="la-dir la-dir-up">higher is better</span>
    <p><b>Findings</b> – reports that point at a real defect. This is the whole reason to run an
    inspection, and it is counted as <i>defects located</i>, not as reports emitted.</p>
  </div>
</div>

Severity matters as much as the count. A grey weak warning is ignorable. A red error on
working code is expensive twice over: the file turns red in the project tree, and after the
second false one you stop reading the true ones. A file carrying twenty warnings doesn't get
read at all – it gets its inspection switched off, useful reports and all.

## The yardstick: what Latte compiles is not an error

<div class="la-etalon">
  <div class="la-figure">378<span>/378</span></div>
  <div>
    <p><b>Every template compiles with the project's own Latte, without a single failure.</b></p>
    <p>The compile run used the same macro set the application registers, the project's own
    template loader for the path alias, and the container's <code>strictTypes</code> setting.</p>
    <p>So <b>none</b> of the 11 errors Latte+ raised, and none of the 31 the other plugin
    raised, can be a compile error. This says nothing about runtime, in either direction:
    compilation evaluates nothing, and it does not check the PHP it generates either.</p>
  </div>
</div>

## The noise

<span class="la-dir la-dir-down">lower is better</span>

<div class="la-score">
  <div>
    <span class="la-k">Reports in total</span>
    <span class="la-pair"><b class="us">223</b><i>/</i><b class="them">1230</b></span>
    <span class="la-cap">Latte+ / the other plugin – it is 5.5&times; louder</span>
  </div>
  <div>
    <span class="la-k">Files with a report</span>
    <span class="la-pair"><b class="us">93</b><i>/</i><b class="them">204</b></span>
    <span class="la-cap">out of 378; 173 files stayed clean under both</span>
  </div>
  <div>
    <span class="la-k">Files with 10+ reports</span>
    <span class="la-pair"><b class="us">1</b><i>/</i><b class="them">32</b></span>
    <span class="la-cap">worst file: 11 reports against 84</span>
  </div>
  <div>
    <span class="la-k">Files marked with an error</span>
    <span class="la-pair"><b class="us">5</b><i>/</i><b class="them">16</b></span>
    <span class="la-cap">a red file in the project tree; 7 of Latte+'s 11 errors are real faults</span>
  </div>
</div>

Of the 223 Latte+ reports, 50 come from the platform's own HTML and CSS support; **173 are
Latte+'s own**. Every one of those was classified individually: **125 are demonstrably
false**, **34 point at a real defect**, and **14 could not be decided** from the template
alone, because the invariant lives in PHP.

Two thirds of the false ones come from a single check. Latte+'s undefined-variable
inspection ships **disabled**; it was switched on for this run, and on this project all
**83** of its reports are false. The other opt-in check, unused `{define}`, adds 7 reports of
which 6 are false. With both back in their shipped state the Latte+ figures read 133 reports,
83 of them the plugin's own, of which 36 are false – and no file reaches ten reports at all,
since the one file that does is made up entirely of the undefined-variable check.

For the other plugin, its four largest groups – `Undefined variable`, `Unknown action`,
`Undefined method` and `Print probably nullable type` – account for **853 reports, 69 % of
its output**, and verified samples put their causes on the false side: an element taken out of a
generic collection loses its type, `{capture $x}` is not treated as a declaration, and an action
with a template but no `action*()` method is reported as unknown – all three ordinary in a Nette
codebase.

## What it actually found

<span class="la-dir la-dir-up">higher is better</span>

Latte+'s own checks located **34 real defects**, and the platform's HTML inspection raised
**8 more** through the injection Latte+ provides – stray closing tags in two templates – for
**42** in total. The 34 are not equal, so they are listed by what actually happens:

<div class="la-tiers">
  <div class="la-tier">
    <span class="la-tier-n">11</span>
    <p><b>Latent faults.</b> Seven are a nullable value dereferenced with no guard and one more
    feeds it to a filter that won't take it; one is a method the type does not have, called inside
    a branch; two are links to an action that was removed. On today's data none of them fire,
    because the data is complete – which is exactly why nobody would find them by using the site.
    The day a field comes back empty they surface far from the template that caused them.</p>
  </div>
  <div class="la-tier">
    <span class="la-tier-n">14</span>
    <p><b>Silently empty output.</b> One of them names a property the model does not declare and
    has no <code>__get</code> for – the neighbouring lines address that same model correctly – so
    what that element prints is nothing. The other thirteen print a nullable value with nothing to
    fall back on.</p>
  </div>
  <div class="la-tier">
    <span class="la-tier-n">6</span>
    <p><b>Broken editor support, no runtime effect.</b> A <code>{varType}</code> pointing at a
    class that does not exist – copied from a skeleton – or at a base class missing the method the
    template calls. <code>{varType}</code> generates no code, so nothing breaks at runtime; what
    breaks is completion, navigation and type checking for that whole file.</p>
  </div>
  <div class="la-tier">
    <span class="la-tier-n">3</span>
    <p><b>Code that contradicts itself.</b> A named argument passed twice in one call, so one of
    the two values is dead, and a <code>{define}</code> nothing includes.</p>
  </div>
</div>

**Does the other plugin find them too?** On the same 34 defects, it reports the same fault on
27, is silent on 4 – a duplicated `link:` argument, a `{define}` nobody uses, and two `{plink}`
calls pointing at an action that was removed – and on 3 more it reports something else on that
line: an undefined variable where the fault is a duplicated argument, an undefined property on a
property that exists. Of the eight stray closing tags, it reports one.

**And the other way round: it located 11 defects Latte+ says nothing about.** Six are links to
an in-page anchor whose `id` exists nowhere in the repository; five are declarations nothing
reads, one of them spending two link-building calls per render on a value no one looks at. Both
come from checks Latte+ does not have at all, and all eleven sit in library and styleguide
templates.

Counted the same way on both sides – every real fault the IDE surfaces with that plugin
installed, including the ones the platform's own inspections raise – the pool comes to 53
defects:

<div class="la-score la-score-2">
  <div>
    <span class="la-k">Defects located</span>
    <span class="la-pair"><b class="us">42</b><i>/</i><b class="them">39</b></span>
    <span class="la-cap">Latte+ / the other plugin – 28 of them found by both, 14 only by Latte+, 11 only by the other</span>
  </div>
  <div>
    <span class="la-k">Reports spent on them</span>
    <span class="la-pair"><b class="us">223</b><i>/</i><b class="them">1230</b></span>
    <span class="la-cap">what you read through to get there</span>
  </div>
</div>

So on locating real defects the two are level. The difference is what comes with them.

## By area

Each row reads *how many of that plugin's reports here were real*, and then that as a share. A higher share means less to read past to get to something; the bar is the volume it took.

<div class="la-legend">
  <span><i class="sw e"></i> error</span>
  <span><i class="sw w"></i> warning</span>
  <span><i class="sw k"></i> weak warning</span>
  <span class="la-legend-note">bar length = share of 292 reports, the second-loudest measurement; the outlined bar is the 508 one, on a scale of its own</span>
</div>

<div class="la-areas">

  <div class="la-area">
    <div class="la-name"><b>Types &amp; nullability</b><span>Nullable access, members that don't exist, printing a value that may be null. <em>Two thirds of everything either plugin found is here.</em></span></div>
    <div class="la-bars">
      <div class="la-row"><span class="who">Latte+</span><span class="mix"><b>28</b> real of 45</span><span class="snr">62&thinsp;%</span><span class="track"><i class="w" style="width:9.9%"></i><i class="k" style="width:5.5%"></i></span></div>
      <div class="la-row"><span class="who">other</span><span class="mix"><b>26</b> real of 508</span><span class="snr">5&thinsp;%</span><span class="track over"><i class="e" style="width:4.1%"></i><i class="w" style="width:78.3%"></i><i class="k" style="width:17.5%"></i></span></div>
    </div>
    <span class="la-verdict v-us">Latte+</span>
  </div>

  <div class="la-area">
    <div class="la-name"><b>Variables &amp; scope</b><span>Where a variable came from – an include argument, <code>{capture}</code>, <code>{var}</code>. <em>Latte+'s check here is opt-in, was switched on for this run, and every one of its 83 reports is false. The other plugin found five unused declarations Latte+ has no check for.</em></span></div>
    <div class="la-bars">
      <div class="la-row"><span class="who">Latte+</span><span class="mix"><b>0</b> real of 83</span><span class="snr">0&thinsp;%</span><span class="track"><i class="k" style="width:28.4%"></i></span></div>
      <div class="la-row"><span class="who">other</span><span class="mix"><b>5</b> real of 292</span><span class="snr">2&thinsp;%</span><span class="track"><i class="w" style="width:90.8%"></i><i class="k" style="width:9.2%"></i></span></div>
    </div>
    <span class="la-verdict v-draw">Draw</span>
  </div>

  <div class="la-area">
    <div class="la-name"><b>Presenters, actions, links</b><span><code>n:href</code> targets, whether an action exists, finding the presenter in a monorepo. <em>Both Latte+ reports are findings – a link to an action that was removed – and there are no others.</em></span></div>
    <div class="la-bars">
      <div class="la-row"><span class="who">Latte+</span><span class="mix"><b>2</b> real of 2</span><span class="snr">100&thinsp;%</span><span class="track"><i class="w" style="width:0.7%"></i></span></div>
      <div class="la-row"><span class="who">other</span><span class="mix"><b>0</b> real of 272</span><span class="snr">0&thinsp;%</span><span class="track"><i class="w" style="width:93.2%"></i></span></div>
    </div>
    <span class="la-verdict v-us">Latte+</span>
  </div>

  <div class="la-area">
    <div class="la-name"><b>Components &amp; factories</b><span>Whether a <code>createComponent*</code> exists for a <code>{control}</code>. <em>Nothing to find here, and Latte+ says nothing; all 36 reports on the other side were checked and none holds.</em></span></div>
    <div class="la-bars">
      <div class="la-row"><span class="who">Latte+</span><span class="mix"><b>0</b> real of 0</span><span class="snr">&ndash;</span><span class="track"></span></div>
      <div class="la-row"><span class="who">other</span><span class="mix"><b>0</b> real of 36</span><span class="snr">0&thinsp;%</span><span class="track"><i class="w" style="width:12.3%"></i></span></div>
    </div>
    <span class="la-verdict v-us">Latte+</span>
  </div>

  <div class="la-area">
    <div class="la-name"><b>Custom tags &amp; filters</b><span>Recognising macros and filters the project or its dependencies register. <em>Neither plugin reports an unknown tag any more – Latte+ scans for them, the other plugin was told about them by hand.</em></span></div>
    <div class="la-bars">
      <div class="la-row"><span class="who">Latte+</span><span class="mix"><b>2</b> real of 9</span><span class="snr">22&thinsp;%</span><span class="track"><i class="w" style="width:3.1%"></i></span></div>
      <div class="la-row"><span class="who">other</span><span class="mix"><b>1</b> real of 16</span><span class="snr">6&thinsp;%</span><span class="track"><i class="e" style="width:0.3%"></i><i class="w" style="width:5.1%"></i></span></div>
    </div>
    <span class="la-verdict v-us">Latte+, just</span>
  </div>

  <div class="la-area">
    <div class="la-name"><b>Forms</b><span>Form fields and containers, read from the PHP factory. <em>Only Latte+ looks here at all – 48 false reports last round, one now – but this round it turned up nothing either.</em></span></div>
    <div class="la-bars">
      <div class="la-row"><span class="who">Latte+</span><span class="mix"><b>0</b> real of 2</span><span class="snr">0&thinsp;%</span><span class="track"><i class="w" style="width:0.3%"></i><i class="k" style="width:0.3%"></i></span></div>
      <div class="la-row"><span class="who">other</span><span class="mix"><b>0</b> real of 0</span><span class="snr">&ndash;</span><span class="track"></span></div>
    </div>
    <span class="la-verdict v-draw">Draw</span>
  </div>

  <div class="la-area">
    <div class="la-name"><b>HTML validity</b><span>Unclosed tags, forbidden attributes, anchor targets – raised by the platform, but only as well as the injection allows. <em>The two sets barely overlap: stray closing tags on one side, dead anchors on the other.</em></span></div>
    <div class="la-bars">
      <div class="la-row"><span class="who">Latte+</span><span class="mix"><b>8</b> real of 50</span><span class="snr">16&thinsp;%</span><span class="track"><i class="e" style="width:2.4%"></i><i class="w" style="width:14.7%"></i></span></div>
      <div class="la-row"><span class="who">other</span><span class="mix"><b>7</b> real of 72</span><span class="snr">10&thinsp;%</span><span class="track"><i class="w" style="width:24.7%"></i></span></div>
    </div>
    <span class="la-verdict v-us">Latte+, just</span>
  </div>

  <div class="la-area">
    <div class="la-name"><b>Blocks, embed, include</b><span>A block handed from the calling template into an <code>{embed}</code> slot – missed by both, on the same lines.</span></div>
    <div class="la-bars">
      <div class="la-row"><span class="who">Latte+</span><span class="mix"><b>2</b> real of 29</span><span class="snr">7&thinsp;%</span><span class="track"><i class="e" style="width:0.3%"></i><i class="w" style="width:7.2%"></i><i class="k" style="width:2.4%"></i></span></div>
      <div class="la-row"><span class="who">other</span><span class="mix"><b>0</b> real of 31</span><span class="snr">0&thinsp;%</span><span class="track"><i class="e" style="width:2.1%"></i><i class="w" style="width:8.6%"></i></span></div>
    </div>
    <span class="la-verdict v-us">Latte+, just</span>
  </div>

  <div class="la-area">
    <div class="la-name"><b>Syntax &amp; parsing</b><span>Handling the spellings Latte accepts – dynamic names, <code>{php}</code>, n:attributes. <em>Three reports each, false on both sides, and nothing found by either.</em></span></div>
    <div class="la-bars">
      <div class="la-row"><span class="who">Latte+</span><span class="mix"><b>0</b> real of 3</span><span class="snr">0&thinsp;%</span><span class="track"><i class="e" style="width:1.0%"></i></span></div>
      <div class="la-row"><span class="who">other</span><span class="mix"><b>0</b> real of 3</span><span class="snr">0&thinsp;%</span><span class="track"><i class="e" style="width:1.0%"></i></span></div>
    </div>
    <span class="la-verdict v-draw">Draw</span>
  </div>

</div>

<div class="la-tally">
  <span class="t-us"><b>6</b> areas to Latte+</span>
  <span class="t-draw"><b>3</b> draws</span>
  <span class="t-them"><b>0</b> to the other plugin</span>
</div>

<p class="la-foot">Across every area together, <b>42 of Latte+'s 223 reports were real – 19&thinsp;% – against 39 of
1230, or 3&thinsp;%</b>. An area goes to the plugin that found more for fewer reports; where that is not
clear-cut, it is a draw. <i>Variables &amp; scope</i> and <i>Forms</i> are draws for the same reason from
opposite directions: one side is quieter, the other found something, and neither combination wins.
Latte+'s counts come from classifying all 173 of its own reports; the other plugin's come from the groups
checked in full plus the faults both plugins report, so they are a floor.</p>

## What drives the gap

| Area | The difference | Latte+ | Other |
|---|---|:--:|:--:|
| Variables & scope | `{capture $x}` isn't treated as a declaration, so the declaring line itself is reported – and then every use of the variable below it. | 0 | 265 |
| Presenters & actions | An action that has a template but no `action*()` method is an ordinary Nette pattern. The template is there; the method isn't, so it is reported. | 0 | 252 |
| Types & nullability | An element taken out of a generic collection loses its type – on its own that accounts for well over half the group – and methods a presenter inherits from a trait inside `vendor/` aren't seen either. Two reports even call `$this->hasBlock(…)`, a method every Latte template has, undefined. | 0 | 508 |
| Components & factories | Factories that arrive with an installed package are counted now, so a `{control}` backed by one is no longer reported. | 0 | 36 |
| Custom tags & filters | Reports of an *unknown tag*: none on either side now. Latte+ finds the project's tags by scanning; the other plugin was told about them by hand, three names typed into its settings. Last round, unconfigured, it raised 33 errors here. | 0 | 0 |
| Forms | Fields added to a container held in a local PHP variable used to come out with no container path, so a template addressing the container found none of them: 48 warnings, every one on a field that exists. Fixed; one shape still misses. | 1 | 0 |
| HTML validity | Seven stray `</span>` in one template. Latte+ reports them; the other plugin makes 29 other reports on that same file and misses the real fault. | 7 err | 0 |
| Blocks & embed | A block passed into an `{embed}` slot is found by neither plugin, on the same lines. | 21 | 25 |
| Blocks & embed | A bareword in `{embed course}` is a block name, not a path. Reported as a missing file. | 0 | 6 err |
| Syntax | `n:ifset="#block"` and `n:ifset="block $key"` – Latte 2 accepts both; Latte+ parses the attribute as a plain PHP expression and stops at the `#`. | 3 err | 0 |
| Syntax | `{var $arr[] = expr}` flagged as an error. Latte accepts it – measured: `{var $ids[] = 5}{var $ids[] = 6}` renders `[5, 6]`. | 0 | 3 err |

<p class="la-foot">The two columns are the whole area's reports, not only the ones the described cause explains;
where a cause covers part of a group, the row says so.</p>

## What Latte+ still gets wrong

125 of its own reports are false. They sit in a handful of places, each with a cause you can
point at:

- **A variable the PHP side hands over in a shape the plugin doesn't read – 83.** All of the
  opt-in undefined-variable check's output, in four groups: `$this->getTemplate()->add('x', …)`
  (this project uses `add()` 108 times and `$this->template->x = …` never), `renderToString($path,
  ['x' => …])`, a variable inherited through an argument-less `{include}` – Latte 2 passes the
  caller's whole scope – and a named `{include}` argument that every call site does pass. Two
  smaller faults ride along: the check fires inside `isset()`, and its *did you mean* suggestion
  was wrong in all 14 cases where it appeared, twice proposing the loop variable being declared
  on that very line.
- **Blocks handed into `{embed}` slots – 21.** Every one of the 16 templates is embedded and the
  caller does define the block.
- **A `{define}` consumed by a `{block}` in the layout – 6.** Latte 2 puts `{define}` in the same
  layer as `{block}`, so template inheritance reaches it; the check only looks for an `{include}`.
- **A filter's declared input type against weak mode – 6.** The project runs without
  `strict_types`, so a numeric string reaching `number` is coerced, and two more sit behind a
  guard the filter inspection doesn't read.
- **`n:ifset` with a block reference – 3 errors.** The spellings in the table above.
- **The rest – 6.** A form container addressed through the tag form, `$presenter->getRequest()`
  treated as nullable inside a template that only renders during a request, a guard that passes
  through an intermediate boolean, and `count()`'s return type resolving to the literal `\count`.

Thirteen of the 14 undecidable ones are the same shape: the value is genuinely nullable and what
rules null out lives in the wiring between two PHP methods, which no amount of reading the
template can recover. The fourteenth is a hint rather than a complaint – a library template with
no PHP of its own gets told that its `$form` has no known owner, and offered the annotation that
would give it one.

## Where the other plugin does better

Last round it was ahead on syntax, which was the most expensive category on the list – nine
Latte+ errors on code Latte compiles happily. That area is now three reports each, false on both
sides. What it is ahead on this round is **two checks Latte+ does not have**, and both of them
earned their keep:

- **Anchor targets** (24 reports, **6 real**) – links to an in-page anchor whose `id` appears
  nowhere in the repository. This is the platform's own inspection, and Latte+ suppresses it inside
  templates; targeted suppression is awkward because of n:attributes, but the cost of the blanket
  version is now measured, and it is six faults.
- **Unused variable** (27 reports, **5 real**) – a declaration nothing reads. Latte+ reports an
  unused `{define}` and an undefined variable; an unused *variable* it does not check at all. One of
  the five spends two `$presenter->link()` calls per render computing a value nobody looks at.
And that is the list. A third candidate did not survive checking: its message for a filter input
mismatch was thought to be the more specific one, but both plugins name the type they inferred and
the type they expected.

One candidate did not hold up, and it is instructive. On a homepage control the other plugin
reports three variables as **unused** where they are assigned, and the same three as
**undefined** nineteen lines later where they are read:

```latte
{foreach $mainNode->getChildren() as $child}
    {var $url = …}          {* reported: unused variable *}
{/foreach}

{embed '~button', link: $url ?? $presenter->link(…)}   {* reported: undefined variable *}
```

Both claims cannot be true at once, and neither is: the variable is assigned and it is read. The
pair is a fingerprint of treating `{foreach}` as a scope that closes at `{/foreach}`, which Latte 2
does not do – it compiles to a plain PHP loop, so the value survives it. Whether taking the last
iteration's value is what the template wants is a question about its author's intent, which no
plugin can read, so this is counted as neither a finding nor a fault. Latte+ reports nothing on
this file.

## The short version

**On finding real defects the two are level** – 42 against 39 out of a pool of 53, with 28 of
them found by both. Six of the nine areas go to Latte+ and three are draws.

**On what it costs to get there they are not close at all.** The same project yields 223 reports
against 1230; 93 flagged files against 204; one file with ten or more reports against 32. The
other plugin doesn't recognise ordinary Nette and Latte patterns – `{capture}`, an action without
a method, a trait in `vendor/`, `$this->hasBlock()` – so its noise grows with the size of a
project and with how idiomatically it is written. Latte+'s noise sits at the edges of specific
features, and every group above has a locatable cause: two thirds of it is one check that ships
switched off, and the areas fixed since the last round went from 49 reports to 2, from 32 to 9,
and from 18 to 3.

Both directions are on the list. The gaps this round exposed – an unused variable, an anchor that
points nowhere – are checks worth having, and the false reports that remain each have a cause with
a name. That is why these numbers are expected to move, and why this page will be measured again.

[Back to the capability comparison](./comparison.html){: .btn .btn-outline }

---

<p class="la-foot"><b>Method.</b> Both plugins were driven through the same sweep in two sandboxes of the
same PhpStorm, opening each of the 378 files and reading everything the IDE reported, weak
warnings included – re-reading each file until two consecutive reads agreed, because the checks
that need the PHP index answer later than the rest and a first non-empty answer can be partial.
Both of Latte+'s opt-in checks were enabled. The compile baseline ran the project's own Latte over
all 378 templates with the application's macro set and template loader. Every Latte+ report – all
173 that remain after subtracting the platform's own – was classified individually against the
project's PHP. For the other plugin, the question asked was the one that matters: on every line
where Latte+ is silent, is it right? Every group small enough to finish was checked in full,
including all 24 of its anchor reports and all 245 of its `Unknown action` reports that land on a
line where Latte+ is silent; its four largest groups were otherwise sampled, covering the families
that make up most of each. So its 11 finds are a verified list, while its false count is a floor
rather than a total. Reports on the project's `~`
path alias are excluded on both sides.</p>

<style>
.la-note { background: #f0f6fb; border: 1px solid #cfe0ec; border-left: 4px solid #3b7ea8; padding: 0.9rem 1.1rem; margin: 1.5rem 0; }
.la-note p { margin: 0.35rem 0 0; }
.la-note p:first-child { margin-top: 0; }
.la-note-warn { background: #fdf6ec; border-color: #ecdcc2; border-left-color: #a8761a; }

.la-dir { display: inline-block; font-size: 0.7rem; font-weight: 600; letter-spacing: 0.06em; text-transform: uppercase; padding: 0.2rem 0.45rem; border: 1px solid currentColor; white-space: nowrap; }
.la-dir-down { color: #2c7355; background: #eaf3ee; }
.la-dir-up { color: #2f6f8f; background: #e7f0f6; }

.la-howto { display: flex; flex-direction: column; gap: 1px; background: #dae1e7; border: 1px solid #dae1e7; margin: 1rem 0 1.25rem; }
.la-howto-row { background: #fff; display: grid; grid-template-columns: 9rem 1fr; gap: 0.9rem; padding: 0.8rem 1rem; align-items: start; }
@media (max-width: 30rem) { .la-howto-row { grid-template-columns: 1fr; } }
.la-howto-row p { margin: 0; font-size: 0.9rem; line-height: 1.5; }

.la-tiers { display: flex; flex-direction: column; gap: 1px; background: #dae1e7; border: 1px solid #dae1e7; margin: 1rem 0 1.25rem; }
.la-tier { background: #fff; display: grid; grid-template-columns: 3.2rem 1fr; gap: 0.9rem; padding: 0.8rem 1rem; align-items: start; }
.la-tier-n { font-size: 1.6rem; font-weight: 700; line-height: 1; color: #2f6f8f; font-variant-numeric: tabular-nums; text-align: right; }
.la-tier p { margin: 0; font-size: 0.9rem; line-height: 1.5; }

.la-etalon { display: flex; gap: 1.25rem; align-items: flex-start; background: #f3f8f5; border: 1px solid #cfe3d7; border-left: 4px solid #2c7355; padding: 1.1rem 1.25rem; margin: 1.25rem 0; }
.la-etalon .la-figure { font-size: 2.4rem; font-weight: 700; line-height: 1; color: #2c7355; font-variant-numeric: tabular-nums; white-space: nowrap; }
.la-etalon .la-figure span { font-size: 1.1rem; color: #7d8994; }
.la-etalon p { margin: 0 0 0.4rem; font-size: 0.9rem; line-height: 1.5; }
.la-etalon p:last-child { margin-bottom: 0; }

.la-score { display: grid; grid-template-columns: repeat(2, 1fr); gap: 1px; background: #dae1e7; border: 1px solid #dae1e7; margin: 0.75rem 0 1.25rem; }
@media (max-width: 30rem) { .la-score { grid-template-columns: 1fr; } }
@media (min-width: 80rem) { .la-score { grid-template-columns: repeat(4, 1fr); } }
@media (min-width: 80rem) { .la-score.la-score-2 { grid-template-columns: repeat(2, 1fr); } }
.la-score > div { background: #fff; padding: 0.9rem 1rem; display: flex; flex-direction: column; gap: 0.25rem; }
.la-score .la-k { font-size: 0.7rem; letter-spacing: 0.09em; text-transform: uppercase; color: #7d8994; }
.la-score .la-pair { display: flex; align-items: baseline; gap: 0.5rem; font-variant-numeric: tabular-nums; }
.la-score .la-pair b { font-size: 1.7rem; line-height: 1; }
.la-score .la-pair i { color: #9aa6b0; font-style: normal; }
.la-score .la-cap { font-size: 0.8rem; color: #55636f; line-height: 1.4; }
.la-score .us { color: #2f6f8f; }
.la-score .them { color: #a8622c; }

.la-legend { display: flex; flex-wrap: wrap; gap: 0.3rem 1.2rem; font-size: 0.78rem; color: #55636f; margin: 0.75rem 0; align-items: center; }
.la-legend span { display: inline-flex; align-items: center; gap: 0.35rem; }
.la-legend .la-dir { font-size: 0.62rem; padding: 0.1rem 0.3rem; }
.la-legend-note { color: #7d8994; }
.sw { width: 0.7rem; height: 0.7rem; border-radius: 2px; display: inline-block; }
.sw.e, .la-row .track .e { background: #bf3a2e; }
.sw.w, .la-row .track .w { background: #a8761a; }
.sw.k, .la-row .track .k { background: #8b98a1; }

.la-areas { display: flex; flex-direction: column; gap: 1px; background: #dae1e7; border: 1px solid #dae1e7; margin: 0.75rem 0 0.5rem; container-type: inline-size; }
.la-area { background: #fff; display: grid; grid-template-columns: minmax(9rem, 1fr) minmax(11rem, 1.6fr) 7.5rem; gap: 0.5rem 1rem; padding: 0.9rem 1rem 0.9rem 0.75rem; align-items: center; border-left: 4px solid transparent; }
@container (max-width: 40rem) { .la-area { grid-template-columns: 1fr; align-items: start; } }
.la-area:has(.v-us) { border-left-color: #2f6f8f; }
.la-area:has(.v-them) { border-left-color: #a8622c; }
.la-area:has(.v-draw) { border-left-color: #b6c0c8; }
@media (max-width: 900px) { .la-area { grid-template-columns: 1fr; align-items: start; } }
.la-area .la-name { display: flex; flex-direction: column; gap: 0.15rem; }
.la-area .la-name b { font-size: 0.98rem; }
.la-area .la-name span { font-size: 0.82rem; color: #55636f; line-height: 1.45; }
.la-bars { display: flex; flex-direction: column; gap: 0.4rem; min-width: 0; }
.la-row { display: grid; grid-template-columns: 3.4rem 7.4rem 2.6rem 1fr; gap: 0.5rem; align-items: center; }
.la-row .who { font-size: 0.72rem; color: #55636f; }
.la-row .mix { font-size: 0.78rem; color: #55636f; white-space: nowrap; font-variant-numeric: tabular-nums; }
.la-row .mix b { font-size: 0.9rem; color: #2f3a44; }
.la-row .snr { font-size: 0.82rem; font-weight: 700; font-variant-numeric: tabular-nums; text-align: right; color: #2f3a44; }
.la-row .track { height: 0.8rem; background: #eceff2; display: flex; overflow: hidden; }
.la-row .track i { display: block; height: 100%; }
.la-row .track.over { outline: 1px dashed #a8622c; outline-offset: 1px; }
.la-verdict { font-size: 0.7rem; font-weight: 600; letter-spacing: 0.04em; text-transform: uppercase; padding: 0.25rem 0.35rem; border: 1px solid currentColor; white-space: nowrap; justify-self: stretch; text-align: center; }
@container (max-width: 40rem) { .la-verdict { justify-self: start; padding-inline: 0.6rem; } }
.v-us { color: #2f6f8f; background: #e7f0f6; }
.v-them { color: #a8622c; background: #f6eade; }
.v-draw { color: #55636f; background: #eff2f4; }

.la-tally { display: flex; flex-wrap: wrap; gap: 1px; background: #dae1e7; border: 1px solid #dae1e7; margin: 0 0 0.75rem; }
.la-tally span { flex: 1 1 8rem; background: #fff; padding: 0.55rem 0.85rem; font-size: 0.82rem; color: #55636f; border-top: 3px solid; }
.la-tally b { font-size: 1.15rem; margin-right: 0.3rem; }
.la-tally .t-us { border-top-color: #2f6f8f; } .la-tally .t-us b { color: #2f6f8f; }
.la-tally .t-them { border-top-color: #a8622c; } .la-tally .t-them b { color: #a8622c; }
.la-tally .t-draw { border-top-color: #b6c0c8; } .la-tally .t-draw b { color: #55636f; }

.la-foot { font-size: 0.82rem; color: #55636f; line-height: 1.55; }
</style>
