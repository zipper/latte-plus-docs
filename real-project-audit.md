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

<span class="label label-blue">Latte+ 1.0.1</span> <span class="label label-green">373 templates</span> <span class="label label-purple">Latte 2.11.7</span> <span class="label">monorepo</span>

So Latte+ and another Latte plugin were pointed at the same real project, in two sandboxes
of the same PhpStorm, and every report either of them made – error, warning and weak
warning alike – was collected and classified.

**The project.** A monorepo: two Nette applications plus a shared template library that
has no PHP counterpart of its own. **373 `.latte` files**, **Latte 2.11.7** on Nette 3.1.
Templates are included across application boundaries and the library's templates are
reached through a path alias – the kind of layout that makes a plugin either resolve
things properly or start guessing.

<div class="la-note" markdown="1">
**This is Latte+ 1.0.1, measured on a Latte 2 project.**

Latte+ targets **Latte 3**, and Latte 2 support is deliberately strong rather than an
afterthought – but of the two, a Latte 2 codebase is the harder test, which is why it was
picked. The numbers below are the first public release on its *worse* footing. They are a
snapshot, not a ceiling: the audit is repeated after each round of fixes, and a companion
audit on a Latte 3 project will follow.
</div>

## The yardstick: what Latte compiles is not an error

<div class="la-etalon">
  <div class="la-figure">373<span>/373</span></div>
  <div>
    <p><b>Every template compiles with the project's own Latte, without a single failure.</b></p>
    <p>The compile run used the same macro set the application registers, the project's own
    template loader for the path alias, and the container's <code>strictTypes</code> setting.
    Production traffic confirms it from the other side.</p>
    <p>So <b>none</b> of the 41 errors Latte+ raised, and none of the 58 the other plugin
    raised, can be a compile error. Runtime problems – a property that isn't there, a dead
    action – are not ruled out by this; compilation evaluates nothing.</p>
  </div>
</div>

## The headline numbers

<div class="la-score">
  <div>
    <span class="la-k">Reports in total</span>
    <span class="la-pair"><b class="us">292</b><i>/</i><b class="them">1161</b></span>
    <span class="la-cap">Latte+ / the other plugin – it is 4&times; louder</span>
  </div>
  <div>
    <span class="la-k">Files with a report</span>
    <span class="la-pair"><b class="us">99</b><i>/</i><b class="them">203</b></span>
    <span class="la-cap">out of 373; 162 files stayed clean under both</span>
  </div>
  <div>
    <span class="la-k">Files with 10+ reports</span>
    <span class="la-pair"><b class="us">2</b><i>/</i><b class="them">32</b></span>
    <span class="la-cap">worst file: 21 reports against 44</span>
  </div>
  <div>
    <span class="la-k">Files marked with an error</span>
    <span class="la-pair"><b class="us">28</b><i>/</i><b class="them">40</b></span>
    <span class="la-cap">a red file in the project tree – on code that compiles</span>
  </div>
</div>

Not every report costs the same. A grey weak warning is ignorable. A red error on working
code is expensive twice over: the file turns red in the project tree, and after the second
false one you stop reading the true ones. And a file carrying twenty warnings doesn't get
read – it gets its inspection switched off, useful reports and all. That is why the last
two tiles weigh more than the first.

Of the 292 Latte+ reports, 57 come from the platform's own HTML and CSS support; 235 are
Latte+'s own. Of those, **167 are demonstrably false**, **31 are genuine finds**, and the
rest could not be decided from the template alone because the invariant lives in PHP. For
the other plugin, its four largest groups – 744 reports, 64 % of its output – are false
according to verified samples.

## By area

<div class="la-legend">
  <span><i class="sw e"></i> error</span>
  <span><i class="sw w"></i> warning</span>
  <span><i class="sw k"></i> weak warning</span>
</div>

<div class="la-areas">

  <div class="la-area">
    <div class="la-name"><b>Types &amp; nullability</b><span>Nullable access, members that don't exist, printing a value that may be null.</span></div>
    <div class="la-bars">
      <div class="la-row"><span class="who">Latte+</span><span class="track"><i class="w" style="width:9.3%"></i><i class="k" style="width:6.5%"></i></span><span class="tot us">80</span></div>
      <div class="la-row"><span class="who">other</span><span class="track"><i class="e" style="width:4.1%"></i><i class="w" style="width:78.3%"></i><i class="k" style="width:17.5%"></i></span><span class="tot them">508</span></div>
    </div>
    <span class="la-verdict v-us">Latte+</span>
  </div>

  <div class="la-area">
    <div class="la-name"><b>Variables &amp; scope</b><span>Where a variable came from – an include argument, <code>{capture}</code>, <code>{var}</code>.</span></div>
    <div class="la-bars">
      <div class="la-row"><span class="who">Latte+</span><span class="track"><i class="w" style="width:3.5%"></i></span><span class="tot us">18</span></div>
      <div class="la-row"><span class="who">other</span><span class="track"><i class="w" style="width:51.8%"></i><i class="k" style="width:3.9%"></i></span><span class="tot them">283</span></div>
    </div>
    <span class="la-verdict v-us">Latte+</span>
  </div>

  <div class="la-area">
    <div class="la-name"><b>Presenters, actions, links</b><span><code>n:href</code> targets, whether an action exists, finding the presenter in a monorepo.</span></div>
    <div class="la-bars">
      <div class="la-row"><span class="who">Latte+</span><span class="track"><i class="w" style="width:1.8%"></i></span><span class="tot us">9</span></div>
      <div class="la-row"><span class="who">other</span><span class="track"><i class="w" style="width:41.9%"></i></span><span class="tot them">213</span></div>
    </div>
    <span class="la-verdict v-us">Latte+</span>
  </div>

  <div class="la-area">
    <div class="la-name"><b>Components &amp; factories</b><span>Whether a <code>createComponent*</code> exists for a <code>{control}</code>.</span></div>
    <div class="la-bars">
      <div class="la-row"><span class="who">Latte+</span><span class="track"><i class="e" style="width:0.2%"></i><i class="w" style="width:1.6%"></i></span><span class="tot us">9</span></div>
      <div class="la-row"><span class="who">other</span><span class="track"><i class="w" style="width:7.1%"></i></span><span class="tot them">36</span></div>
    </div>
    <span class="la-verdict v-us">Latte+</span>
  </div>

  <div class="la-area">
    <div class="la-name"><b>Custom tags &amp; filters</b><span>Recognising macros and filters the project or its dependencies register.</span></div>
    <div class="la-bars">
      <div class="la-row"><span class="who">Latte+</span><span class="track"><i class="e" style="width:4.1%"></i><i class="w" style="width:2.2%"></i></span><span class="tot us">32</span></div>
      <div class="la-row"><span class="who">other</span><span class="track"><i class="e" style="width:6.7%"></i><i class="w" style="width:3%"></i></span><span class="tot them">49</span></div>
    </div>
    <span class="la-verdict v-us">Latte+, just</span>
  </div>

  <div class="la-area">
    <div class="la-name"><b>Forms</b><span>Form fields and containers, read from the PHP factory.</span></div>
    <div class="la-bars">
      <div class="la-row"><span class="who">Latte+</span><span class="track"><i class="w" style="width:9.4%"></i><i class="k" style="width:0.2%"></i></span><span class="tot us">49</span></div>
      <div class="la-row"><span class="who">other</span><span class="track"></span><span class="tot them">0</span></div>
    </div>
    <span class="la-verdict v-us">only Latte+</span>
  </div>

  <div class="la-area">
    <div class="la-name"><b>HTML validity</b><span>Unclosed tags, forbidden attributes – reported by the platform, but only as well as the injection allows.</span></div>
    <div class="la-bars">
      <div class="la-row"><span class="who">Latte+</span><span class="track"><i class="e" style="width:1.6%"></i><i class="w" style="width:9.6%"></i></span><span class="tot us">57</span></div>
      <div class="la-row"><span class="who">other</span><span class="track"><i class="w" style="width:9.3%"></i></span><span class="tot them">47</span></div>
    </div>
    <span class="la-verdict v-us">Latte+</span>
  </div>

  <div class="la-area">
    <div class="la-name"><b>Blocks, embed, include</b><span>A block handed from the calling template into an <code>{embed}</code> slot.</span></div>
    <div class="la-bars">
      <div class="la-row"><span class="who">Latte+</span><span class="track"><i class="e" style="width:0.4%"></i><i class="w" style="width:3.5%"></i></span><span class="tot us">20</span></div>
      <div class="la-row"><span class="who">other</span><span class="track"><i class="w" style="width:4.3%"></i></span><span class="tot them">22</span></div>
    </div>
    <span class="la-verdict v-draw">a draw</span>
  </div>

  <div class="la-area">
    <div class="la-name"><b>Syntax &amp; parsing</b><span>Handling the spellings Latte accepts – dynamic names, <code>{php}</code>, n:attributes.</span></div>
    <div class="la-bars">
      <div class="la-row"><span class="who">Latte+</span><span class="track"><i class="e" style="width:1.8%"></i><i class="k" style="width:1.8%"></i></span><span class="tot us">18</span></div>
      <div class="la-row"><span class="who">other</span><span class="track"><i class="e" style="width:0.6%"></i></span><span class="tot them">3</span></div>
    </div>
    <span class="la-verdict v-them">the other one</span>
  </div>

</div>

<div class="la-tally">
  <span class="t-us"><b>7</b> areas to Latte+</span>
  <span class="t-draw"><b>1</b> draw</span>
  <span class="t-them"><b>1</b> to the other plugin</span>
</div>

<p class="la-foot">Bar length is the share of the loudest measurement in the run (508 reports). A verdict
weighs severity and how many reports survive classification, not the count alone.</p>

## What drives the gap

| Area | The difference | Latte+ | Other |
|---|---|:--:|:--:|
| Variables & scope | `{capture $x}` isn't treated as a declaration, so the declaring line itself is reported – and then every use of the variable below it. | 0 | 263 |
| Presenters & actions | An action that has a template but no `action*()` method is an ordinary Nette pattern. The template is there; the method isn't, so it is reported. | 0 | 170 |
| Types & nullability | Methods a presenter inherits from a trait inside `vendor/` aren't seen, and `$form['x']` isn't typed. Separately, `?->` is reported as *use of the mark on a non-nullable type* on a member that genuinely is nullable – 21 of those, all errors. | 22 | 247 |
| Custom tags & filters | Without scanning, a project's own tag and filter stay unknown until they are restated in the plugin's own XML configuration. Latte+ finds them itself. | 0 | 13 |
| Custom tags & filters | An image macro from a vendor package, registered the Latte 2 way. **Both plugins fail here.** | 21 err | 21 err |
| Forms | Field checking the other plugin doesn't do at all. Ours produced 48 false reports here – see below. | 49 | 0 |
| HTML validity | Seven stray `</span>` in one template. Latte+ reports them; the other plugin makes 29 other reports on that same file and misses the real fault. | 7 err | 0 |
| Blocks & embed | A block passed into an `{embed}` slot is found by neither plugin, on the same lines. A shared limitation. | 18 | 22 |
| Unused variables | 20 reports, of which the sampled one is used 53 lines further down. Latte+ raised none on this project. | 0 | 20 |
| Syntax | `{var $arr[] = expr}` flagged as an error. Latte accepts it – measured: `{var $ids[] = 5}{var $ids[] = 6}` renders `[5, 6]` – and an error on working code is worse than saying nothing. Latte+ stays quiet. | 0 | err |
| Syntax | Dynamic names Latte 2 accepts: `{block 'x-' . $key}`, `{include '~x' . ucfirst($s)}`, `n:ifset="#block"`, `n:class="key: value"`. **Here it is us who report them and the other plugin that doesn't.** | 9 err | 0 |

## What Latte+ still gets wrong

167 of our own reports are demonstrably false. They are not spread thinly across the
project; they sit in a handful of places, each with a cause you can point at:

- **Form fields – 48.** Fields added to a container that is held in a local PHP variable
  aren't traced yet. That is the price of having the inspection at all – it is the one
  area where the other plugin is silent because it does not look.
- **A vendor image macro – 21 errors.** Registered the Latte 2 way from inside `vendor/`;
  neither plugin finds it.
- **Blocks handed into `{embed}` slots – 18.**
- **Dynamic names – 9 errors.** The spellings listed in the last table row above.
- **Latte 3 rules meeting a Latte 2 template – 9 weak warnings.** A `;` inside `{php}`,
  and the `if` keyword inside it. Both are legal in Latte 2; neither restriction existed
  before Latte 3.

The 31 genuine finds were the other side of the run: a duplicated named argument, a
property that doesn't exist on the type, a `{varType}` pointing at the wrong class, a link
to a route that had been removed.

## Where the other plugin does better

- **Syntax.** Nine of our errors and nine of our weak warnings land on code Latte compiles
  happily, against three reports from the other plugin in the whole area. It is quieter and
  more accurate here – and this is the most expensive category of report there is. Most of
  it traces back to Latte 3 rules meeting Latte 2 templates, which is exactly what this
  project was chosen to expose.
- **A more specific message** when a filter's input doesn't match: it names the type it
  actually inferred, where our wording stays general.
- **Checking HTML anchor targets** (~25 reports) – an inspection Latte+ doesn't have. On
  this project it was noise, because the anchors point at targets JavaScript creates at
  runtime, but the capability is real.

## The short version

Seven of the nine areas go to Latte+, one is a draw and one goes the other way – on a
quarter of the reports, half the affected files and a sixteenth of the densely flagged
ones.

The decisive difference isn't the count, it is **what the noise is made of**. The other
plugin doesn't recognise ordinary Nette and Latte patterns – `{capture}`, an action without
a method, a trait in `vendor/` – so its noise grows with the size of a project and with how
idiomatically it is written. Latte+'s noise sits at the edges of specific features, and
every group above has a locatable cause. That is why these numbers are expected to move,
and why this page will be measured again.

[Back to the capability comparison](./comparison.html){: .btn .btn-outline }

---

<p class="la-foot"><b>Method.</b> Both plugins were driven through the same sweep in two sandboxes of the
same PhpStorm, opening each of the 373 files and reading everything the IDE reported, weak
warnings included. The compile baseline ran the project's own Latte over all 373 templates
with the application's macro set and template loader. Every Latte+ report – all 235 that
remain after subtracting the platform's own – was classified individually; for the other
plugin, samples of its largest groups were classified rather than all 1161.</p>

<style>
.la-note { background: #f0f6fb; border: 1px solid #cfe0ec; border-left: 4px solid #3b7ea8; padding: 0.9rem 1.1rem; margin: 1.5rem 0; }
.la-note p { margin: 0.35rem 0 0; }
.la-note p:first-child { margin-top: 0; }

.la-etalon { display: flex; gap: 1.25rem; align-items: flex-start; background: #f3f8f5; border: 1px solid #cfe3d7; border-left: 4px solid #2c7355; padding: 1.1rem 1.25rem; margin: 1.25rem 0; }
.la-etalon .la-figure { font-size: 2.4rem; font-weight: 700; line-height: 1; color: #2c7355; font-variant-numeric: tabular-nums; white-space: nowrap; }
.la-etalon .la-figure span { font-size: 1.1rem; color: #7d8994; }
.la-etalon p { margin: 0 0 0.4rem; font-size: 0.9rem; line-height: 1.5; }
.la-etalon p:last-child { margin-bottom: 0; }

.la-score { display: grid; grid-template-columns: repeat(2, 1fr); gap: 1px; background: #dae1e7; border: 1px solid #dae1e7; margin: 1.25rem 0; }
@media (max-width: 30rem) { .la-score { grid-template-columns: 1fr; } }
@media (min-width: 80rem) { .la-score { grid-template-columns: repeat(4, 1fr); } }
.la-score > div { background: #fff; padding: 0.9rem 1rem; display: flex; flex-direction: column; gap: 0.25rem; }
.la-score .la-k { font-size: 0.7rem; letter-spacing: 0.09em; text-transform: uppercase; color: #7d8994; }
.la-score .la-pair { display: flex; align-items: baseline; gap: 0.5rem; font-variant-numeric: tabular-nums; }
.la-score .la-pair b { font-size: 1.7rem; line-height: 1; }
.la-score .la-pair i { color: #9aa6b0; font-style: normal; }
.la-score .la-cap { font-size: 0.8rem; color: #55636f; line-height: 1.4; }
.la-score .us, .la-row .us { color: #2f6f8f; }
.la-score .them, .la-row .them { color: #a8622c; }

.la-legend { display: flex; flex-wrap: wrap; gap: 0.3rem 1.2rem; font-size: 0.78rem; color: #55636f; margin: 0.75rem 0; }
.la-legend span { display: inline-flex; align-items: center; gap: 0.35rem; }
.sw { width: 0.7rem; height: 0.7rem; border-radius: 2px; display: inline-block; }
.sw.e, .la-row .track .e { background: #bf3a2e; }
.sw.w, .la-row .track .w { background: #a8761a; }
.sw.k, .la-row .track .k { background: #8b98a1; }

.la-areas { display: flex; flex-direction: column; gap: 1px; background: #dae1e7; border: 1px solid #dae1e7; margin: 0.75rem 0 0.5rem; container-type: inline-size; }
.la-area { background: #fff; display: grid; grid-template-columns: minmax(9rem, 1fr) minmax(11rem, 1.6fr) auto; gap: 0.5rem 1rem; padding: 0.9rem 1rem 0.9rem 0.75rem; align-items: center; border-left: 4px solid transparent; }
@container (max-width: 40rem) { .la-area { grid-template-columns: 1fr; align-items: start; } }
.la-area:has(.v-us) { border-left-color: #2f6f8f; }
.la-area:has(.v-them) { border-left-color: #a8622c; }
.la-area:has(.v-draw) { border-left-color: #b6c0c8; }
@media (max-width: 900px) { .la-area { grid-template-columns: 1fr; align-items: start; } }
.la-area .la-name { display: flex; flex-direction: column; gap: 0.15rem; }
.la-area .la-name b { font-size: 0.98rem; }
.la-area .la-name span { font-size: 0.82rem; color: #55636f; line-height: 1.45; }
.la-bars { display: flex; flex-direction: column; gap: 0.4rem; min-width: 0; }
.la-row { display: grid; grid-template-columns: 3.4rem 1fr 2.6rem; gap: 0.5rem; align-items: center; }
.la-row .who { font-size: 0.72rem; color: #55636f; }
.la-row .track { height: 0.8rem; background: #eceff2; display: flex; overflow: hidden; }
.la-row .track i { display: block; height: 100%; }
.la-row .tot { font-size: 0.82rem; font-variant-numeric: tabular-nums; text-align: right; font-weight: 600; }
.la-verdict { font-size: 0.7rem; font-weight: 600; letter-spacing: 0.05em; text-transform: uppercase; padding: 0.25rem 0.5rem; border: 1px solid currentColor; white-space: nowrap; justify-self: start; }
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
