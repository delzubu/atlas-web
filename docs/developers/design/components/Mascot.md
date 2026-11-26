---
title: Mascot
parent: Component reference
grand_parent: Design
nav_order: 15
nav_exclude: true
---

# Mascot

[Component reference]({{ site.baseurl }}{% link docs/developers/design/components/index.md %}) · [Source specification](https://github.com/delzubu/atlas-core/blob/4a656981ba21c6139a3d3750e0cb76f8d706284a/docs/design/components/Mascot.md)

A character (a penguin, a pink shark, a scientist) that keeps a person company and comments on what they are doing: it welcomes, encourages,
reacts to a result, waits with them. It is a **suite component** (`packages/app-suite/src/mascot`): any tool may show one. The contract is
`docs/specs/mascots.md`; the first user is Study.

## Where it sits
- **In the content region, in the flow**, never floating over text, never covering a control, and there is **at most one** on a screen. It has three forms:
  - **Scene**: the whole stage, 8:3, at most 640 px wide, its height follows. For screens where the person is not working: welcome and progress (about 44% wide,
    beside the lead text), a result (above the score card). Below 360 px wide it becomes a strip.
  - **Rail**: a narrow card (260 px) beside the work, sticky: the character on its ground and its line below; no props, decor or pointing. For a test, where
    the question must keep the screen: the page gives the rail a column when its content is at least 1150 px wide.
  - **Strip**: a small avatar and the line in one row, about 56 px tall, no backdrop. The fallback where a rail has no room (and while a result is pending).
  A work screen never uses a scene: the question and its pictures keep the space.
- The stage is picture, not chrome: a thin border and the radius of a card; the character's colour is used only as the bubble's outline.
- It is **offered by authors** (a `.mascot` folder names one) and **hidden by the person**: a small × in its corner, remembered per browser, and a palette
  command in the app. Hiding it loses nothing.

## What it says
- **One short sentence**, kind and literal, in the content's language (English and German first). It follows the suite's voice: verb-first where it
  instructs, no "Oops", no exclamation marks in a row, no mocking. Humour belongs to the character, never to the person's mistakes.
- It says only what is **true**. It never shows a score that is not final, never guesses which answers are right while a test is open, and never
  implies a result the app has not reached. A person's agent may leave a short remark with a result; the remark replaces the rule line.
- Its words are **text** (`role="status"`, polite); the pictures are decoration (`aria-hidden`). Nothing is conveyed by the picture alone.

## Motion and light
- Walking, jumping and bobbing are short CSS movements of the character layer. `prefers-reduced-motion` turns all of them off.
- There is no autoplay audio, no looping animation that cannot be stopped, no flashing. A picture never changes while the person reads it: the choice is
  fixed per event and attempt.

## Pieces that are not made yet
A picture that does not exist is drawn as a labelled placeholder (a body as a rounded badge in the character's colour, an object as a dashed box), so a pack with
planned art is usable and the gaps are visible. The mascot gallery lists every placeholder and every fallback.

## Mascot gallery
A tool for authors and owners (palette: "Show the mascot gallery"): the pictures of a character by tag, which intents have pictures, and twelve reference
scenes (a heart held, snow gear, pointing at a table and a pyramid, a teacher, tired, cheering, thinking of a cat with a headdress, stern, happy, sad, a jump)
on any backdrop, with the notes the engine made. It is how new art is checked.
