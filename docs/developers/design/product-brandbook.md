---
title: "Brand book"
parent: Design
grand_parent: Developers
nav_order: 1
---

# Brand book

This guide is the human-readable website edition of the [source specification](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/product-brandbook.md). It preserves the design rules and their identifiers; implementation status and measurements are those recorded in the source, not a fresh audit.
> Product brand book · Version 1.0 · 2026-10-28

## Reading this guide

Use the first sections to understand the product promise and explain it consistently. Use the visual identity section when designing a publication, presentation or other branded material. The approved marks and colours are supplied as assets, with rules for size, placement and use.

## 1. Brand at a glance

Atlas brings a workshop approach to knowledge work: the expert and the agent develop the result together, even when the initial brief is incomplete. The identity supports that purpose, from product communications to the smallest use of the mark.

- **Product:** Atlas (Atlas Suite where the suite context needs emphasis).
- **Positioning:** The Light Factory.
- **Category:** Human–AI co-creation platform for bespoke knowledge work.
- **Primary tagline:** **Industrial power. Bespoke thinking.**
- **Core promise:** Bring the productivity and capabilities of agentic AI to expert-led work without forcing evolving, individual problems into predefined workflows.
- **Personality line (optional):** *Not all factories belong to the Dark Side.*

**Canonical short description**

> Atlas is the Light Factory: a human–AI co-creation platform for bespoke knowledge work. It equips experts with agentic tools to explore, design, build, evaluate and refine unique outcomes—even when the requirements are incomplete or change along the way.

**One-sentence pitch**

> Atlas brings industrial-scale AI capability to work that does not come with a finished blueprint.

## 2. The founding distinction

A **Dark Factory** is the useful metaphor for autonomous production against an established specification: well-defined inputs, predictable processes, repeatable outputs and minimal human intervention. It excels when the problem is sufficiently understood and the result can be specified in advance.

A **Light Factory** is designed for a different production problem: work in which the expert's judgment, the requirements and the artifact develop together. Its inputs may be vague; experiments may change the objective; revision is normal; and success is judged by fitness for purpose, quality and distinctiveness—not simply compliance with an initial instruction.

**Neither paradigm is universally superior.** Dark factories are appropriate for repeatable work; light factories are appropriate for bespoke, exploratory and evolving work. A light factory can use autonomous agents extensively. The difference is not “manual versus automated,” but **what the production system is optimized for**.

### Tailoring analogy

Mass-produced clothing begins with standardized patterns and is optimized for reproducibility, variety and low unit cost. Bespoke tailoring begins with an individual, an intent and expert craftsmanship. Fittings, material choices, adjustments and changing preferences are part of the work. The tailor still uses excellent tools; those tools increase capability rather than replace the tailor's judgment.

Atlas aims to provide the equivalent of the best modern workshop for bespoke knowledge work.

| Dimension | Dark Factory | Light Factory |
| --- | --- | --- |
| Starting point | Defined specification | Intent, context, partial requirements |
| Process | Repeatable execution | Exploration, construction, evaluation, revision |
| Human contribution | Objectives, governance, exceptions | Domain expertise, direction, interpretation, judgment |
| AI contribution | Autonomous task execution | Research, proposals, construction, analysis, delegated execution |
| Change | Managed deviation | Expected source of learning |
| Quality | Conformance and reliability | Fitness for purpose, craft and uniqueness |
| Economics | Cost per repeatable output | Cost and time to a satisfactory bespoke outcome |
| Scaling | Replicate a process | Amplify expert capability |

**Key thesis:** In bespoke work, requirements discovery and production are intertwined. Rework is not necessarily waste; it can be how the right result is discovered.

## 3. What Atlas believes

1. **Expert agency is an asset.** Human judgment is not an inefficiency to engineer away.
2. **Automation is a tool, not the goal.** Delegate well-understood work freely; retain meaningful human control where judgment matters.
3. **Iteration is first-class.** “Keep this, change that,” “compare alternatives,” and “revise the assumption” must be ordinary workflows.
4. **Work outlives the conversation.** Shared, inspectable, persistent artifacts make collaboration cumulative rather than ephemeral.
5. **Structure makes creativity scalable.** Models, maps, plans, sources, relationships and reusable tools make complex bespoke work manageable.
6. **Transparency supports craftsmanship.** People must be able to inspect, understand, correct and redirect outcomes.
7. **A good tool adapts to the task.** The task should not have to become generic just to fit the tool.

### Working modes

- **Co-create:** Human and AI explore and shape an outcome together.
- **Delegate:** AI independently executes bounded tasks where autonomy is useful.
- **Review and refine:** Humans assess outputs, revise assumptions and redirect work.

These modes can alternate within the same project. Do not describe Atlas as requiring human approval for every action.

## 4. Product identity and architecture

**Atlas** remains the name of the product and technical ecosystem. **The Light Factory** names its philosophy and market positioning, not a replacement product or a new repository.

The original Atlas metaphor remains useful: Atlas carries the complexity of infrastructure and interconnected work, leaving people free to focus on the decisions and creations that matter.

Illustrative capabilities and their contribution to the promise:

- **Workspace:** Persistent, shared artifacts and revisions.
- **Planner:** Structured plans and timelines that remain editable as assumptions evolve.
- **Edition apps:** each brings its own area (models, maps, learning, ...) under the same principles.
- **Other specialized applications:** Task-appropriate instruments within the same collaborative environment.

These are descriptions of product intent, not a guarantee that every workflow is fully implemented or production-ready. Verify current capability before making feature claims.

**Do not rename** repositories, packages, namespaces, MCP servers, app IDs, APIs, environment variables or existing technical identifiers solely for this branding exercise. Keep app names functional.

### Independence from the AI provider

Atlas keeps project artifacts and their working context separate from the agent that created them. A person can use compatible clients across model providers and subscriptions, then inspect and refine the same work in Atlas. This is a practical consequence of persistent artifacts and expert agency, rather than a promise of universal client support or shared subscription allowances. See the [co-creation articles]({{ site.baseurl }}{% link docs/articles/index.md %}).

## 5. Audience and value proposition

**Primary:** Professionals, researchers, designers, architects, analysts, developers and creators whose work is complex, distinctive or difficult to specify completely in advance.

**Secondary:** Teams creating unique client deliverables, knowledge structures, plans, research and prototypes; technical leaders evaluating AI beyond repetitive task automation.

**Value:** Faster movement from incomplete intent to high-quality, individual outcomes; lower cost of exploration and revision; reusable knowledge and artifacts; meaningful expert control.

**Avoid promising:** “Zero human effort,” “fully autonomous creativity,” “perfect outputs,” or proven industrial-scale economics without measurement.

## 6. Messaging hierarchy

### Formal / executive

**Atlas — The Light Factory**

**Industrial power. Bespoke thinking.**

> Atlas equips professionals with agentic AI tools for bespoke work: exploring ambiguous problems, creating structured artifacts and refining outcomes as requirements evolve.

Emphasize expert productivity, differentiated outcomes, iterative quality, governance and persistent work.

### Technical / practitioner

> Dark factories execute what is already understood. Atlas helps experts discover, design and build what is not yet fully specified.

Emphasize tools, shared state, editable artifacts, bounded autonomy, inspection, correction and integration. Do not claim that all Dark Factory systems reject ambiguous input; describe their optimization target, not a universal limitation.

### Playful / developer

> **Not all factories belong to the Dark Side.**

Or, once the Dark Factory analogy is already established:

> **The Dark Side has factories. We keep the lights on.**

The reference is a wink, not the argument. Use it sparingly in a talk opener, documentation sidebar or closing slide. Avoid lightsabers, Jedi/Sith identities, franchise logos, quotations or visual imitation in the core brand. The metaphor must work without knowledge of Star Wars.

### Additional approved lines

- **AI for work without a finished blueprint.**
- **Tools for the work that changes as you create it.**
- **Automation where useful. Craftsmanship where it matters.**

Use “mass-production efficiency, bespoke quality” only as an aspiration, not a verified performance claim.

## 7. Voice and editorial rules

**Voice:** Precise, confident, technically credible, inventive but restrained. Explain the production problem before introducing slogans.

**Prefer:** “co-create,” “explore,” “refine,” “inspect,” “delegate,” “bespoke,” “expert-led,” “shared artifacts,” “evolving requirements,” “fit for purpose.”

**Avoid:** “AI magic,” “replace professionals,” “humans always in the loop,” “one-click perfection,” “fully automated everything,” and language implying Dark Factory approaches are bad or obsolete.

**Do:** State what Atlas enables and where its approach fits.

**Don't:** Conflate light with mere visibility, imply that human involvement always means synchronous supervision, or reduce the message to Star Wars references.

Use **Atlas** as the noun; **the Light Factory** as the descriptor. Preferred first mention: **Atlas — The Light Factory**. Subsequent mentions: **Atlas**. “Light Factory” may stand alone when contrasting production paradigms.

## 8. Visual direction (guidance, not a UI redesign)

The visual metaphor is a **well-lit professional workshop**, not a sci-fi battle or a literal lightbulb factory. Show people and capable tools producing and revising meaningful work: plans, models, maps, prototypes and structured knowledge.

- **Imagery:** Drafting surfaces, workshops, precise instruments, layered diagrams, evolving artifacts, material texture and purposeful light.
- **Contrast:** Dark/light may illustrate two production paradigms; never suggest dark means evil.
- **Composition:** Show iterations, annotations, choices and transformations—not only a polished final result.
- **Typography:** Clear, technical, restrained. Favor readability over spectacle.
- **Color and logos:** Section 12 defines them. How the product UI uses them is defined in [ui_design.md]({{ site.baseurl }}{% link docs/developers/design/ui_design.md %}), not here.
- **Accessibility:** Maintain readable contrast and never communicate meaning through color alone.

This document governs **product positioning, the visual identity (section 12) and marketing language**. [ui_design.md]({{ site.baseurl }}{% link docs/developers/design/ui_design.md %}) governs the **product UI**, and takes every brand colour it uses from section 12. If they disagree, this document wins for the logo and the brand hexes, and `ui_design.md` wins for everything in the product UI.

## 9. Recommended applications

**GitHub description:** “Atlas — The Light Factory. Agentic AI tools for bespoke, expert-led knowledge work.”

**README opening:**
> # Atlas — The Light Factory
>
> **Industrial power. Bespoke thinking.**
>
> Atlas is a human–AI co-creation platform for work that evolves as you build it. Its specialized tools help experts explore, construct, inspect and refine persistent artifacts without forcing unique problems into predefined workflows.

**Presentation narrative:**
1. Dark factories excel at repeatable, specified work.
2. Much valuable professional work is bespoke and evolves through discovery.
3. Atlas is the Light Factory: agentic capability paired with expert judgment and reusable tools.
4. Demonstrate an artifact evolving through multiple human–AI iterations.
5. Explain the productivity opportunity and the current maturity of the platform.

**Demo test:** Show an initially incomplete request, a constructed artifact, a human correction that changes the direction, and a successful revision preserving valuable earlier work. That demonstrates the brand more convincingly than a slogan.

## 10. Success criteria and boundaries

Evaluate Light Factory workflows by **quality of the resulting artifact, time to satisfactory outcome, cost of meaningful iteration, expert productivity, ability to preserve/reuse work, and user control**. These are proposed measures, not measured product claims.

Some tasks developed collaboratively may become stable enough for Dark Factory-style automation. That is a successful progression, not a contradiction. Atlas need not compete with autonomous execution engines on every repetitive workflow.

## 11. Brand governance

This is the canonical positioning reference for Atlas communications. Future marketing copy, product descriptions and presentations should remain consistent with the principles here. Section 12 is the one source of the logo and the brand palette. The product UI is governed by [ui_design.md]({{ site.baseurl }}{% link docs/developers/design/ui_design.md %}).

**Decision:** Keep Atlas. Adopt “The Light Factory” as positioning. Use the Dark Factory contrast as the primary conceptual explanation and Force references only as understated secondary personality. Prioritize product development over technical renaming.

## 12. Approved visual identity (2026-10-28)

The following assets are the approved masters. Open the linked SVG files for reuse; do not recreate the logo from a screenshot.

[Horizontal logo (SVG)]({{ site.baseurl }}{% link docs/developers/design/assets/atlas-logo-aubergine-coral.svg %}) · [Symbol (SVG)]({{ site.baseurl }}{% link docs/developers/design/assets/atlas-symbol-aubergine-coral.svg %})

The approved direction combines the **open A-shaped symbol and curved underline** from the earlier forest-green concept with the **aubergine and coral** palette. It replaces the blue angular logo direction, which was judged too similar to Atlassian.

This section is the one source of the Atlas logo and the brand palette. Other documents name these colours; they do not redefine them.

### 12.1 Logo assets

![Atlas approved horizontal logo]({{ site.baseurl }}{% link docs/developers/design/assets/atlas-logo-aubergine-coral.svg %})

![Atlas approved symbol]({{ site.baseurl }}{% link docs/developers/design/assets/atlas-symbol-aubergine-coral.svg %})

| Asset | File | Use it for |
| --- | --- | --- |
| Horizontal logo | `assets/atlas-logo-aubergine-coral.svg` | Covers, publications, slides, the launcher's start page |
| Symbol | `assets/atlas-symbol-aubergine-coral.svg` | Favicon, app icons, the product's AppBar and tab strip, avatars |

- **Minimum size:** the symbol 16px high; the horizontal logo 120px wide. Below 24px, draw the symbol flat: the gradient replaced by Aubergine, the underline Coral.
- **Clear space:** a quarter of the symbol's height on every side. Nothing else enters it.
- **Grounds:** White or Mist. On Graphite or any other dark ground, place the mark on a White tile; never put it directly on a dark colour.
- **Never:** recolour, stretch, rotate, outline or add effects (shadow, glow, bevel); set the wordmark in another typeface; use the superseded blue marks.

### 12.2 Brand palette

| Name | Hex | Role |
| --- | --- | --- |
| Aubergine | `#5B21B6` | The main brand colour: the symbol, headlines and accents |
| Coral | `#F97366` | The curved underline and rare highlights. Decorative: it is too light for text on white (2.7:1) |
| Graphite | `#1F2328` | Text and dark chrome |
| White | `#FFFFFF` | The working canvas |
| Mist | `#F6F7F9` | Secondary surfaces |
| Border | `#E2E2E2` | Rules and boundaries |

The symbol's gradient runs `#7C3AED` → Aubergine → `#32134F`. These two extra stops belong to the logo files only; they are not palette colours.

<div class="design-swatches" style="display:grid;grid-template-columns:repeat(auto-fit,minmax(130px,1fr));gap:12px;margin:20px 0;"><div style="padding:16px;border:1px solid #e2e2e2;border-radius:6px;background:#5B21B6;color:#fff"><strong>Aubergine</strong><br>#5B21B6</div><div style="padding:16px;border:1px solid #e2e2e2;border-radius:6px;background:#F97366;color:#1F2328"><strong>Coral</strong><br>#F97366</div><div style="padding:16px;border:1px solid #e2e2e2;border-radius:6px;background:#1F2328;color:#fff"><strong>Graphite</strong><br>#1F2328</div><div style="padding:16px;border:1px solid #e2e2e2;border-radius:6px;background:#FFFFFF;color:#1F2328"><strong>White</strong><br>#FFFFFF</div><div style="padding:16px;border:1px solid #e2e2e2;border-radius:6px;background:#F6F7F9;color:#1F2328"><strong>Mist</strong><br>#F6F7F9</div><div style="padding:16px;border:1px solid #e2e2e2;border-radius:6px;background:#E2E2E2;color:#1F2328"><strong>Border</strong><br>#E2E2E2</div></div>

### 12.3 In publications and marketing

Graphite text on White or Mist. Aubergine for headlines, the symbol and accents. Coral for the curved underline and at most one other highlight per page or slide. Status colours (red, amber, green) are not brand colours; when a publication needs them, use the product's (see `ui_design.md`).

### 12.4 In the product

The product UI shares the neutrals exactly (White, Mist, Border, Graphite) and uses Aubergine and Coral **only as identity colours**: the mark, and a few places on the launcher. Interaction, status and data colours are UI-only. [ui_design.md]({{ site.baseurl }}{% link docs/developers/design/ui_design.md %}) (Color) lists every UI colour and its one job. The product stays white-first: do not recolour semantic icons, status or content surfaces for brand consistency.

**Superseded concepts:** Earlier blue variants are archived in the repository for provenance only. They are not part of the active brand book, must not be used in slides or UI, and are intentionally not illustrated here.


## Historical design assets

The following files are retained for comparison and provenance only. They are **not approved alternatives** to the current identity and should not be used in new product UI or publications.

- [Blue and amber logo]({{ site.baseurl }}{% link docs/developers/design/assets/atlas-logo-blue-amber.svg %})
- [Blue and light logo]({{ site.baseurl }}{% link docs/developers/design/assets/atlas-logo-blue-light.svg %})
- [Blue and teal logo]({{ site.baseurl }}{% link docs/developers/design/assets/atlas-logo-blue-teal.svg %})
- [Historical palette comparison]({{ site.baseurl }}{% link docs/developers/design/assets/atlas-palette-comparison.svg %})
