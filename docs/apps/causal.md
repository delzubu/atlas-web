---
title: Causal
parent: Work
grand_parent: Apps
nav_order: 1
---

# Causal

Use Causal to model how variables influence each other, including effects that take time to appear. Inspect feedback loops and compare scenarios to explore what could change when you adjust a policy lever.

## How to use Causal

1. Add quantities as variables and mark the outcomes you care about. Add policy levers for the actions someone can choose.
2. Connect the variables with signed links. Add a delay and document the reasoning where the effect takes time or is uncertain.
3. Inspect feedback loops and archetypes. Define a scenario, run it in **Impact**, and compare it with alternatives.

Causal is qualitative. It helps you reason about direction, feedback and timing using your own assumptions. Review which assumptions drive a result before using it to choose an intervention.

## Build a first model

Use price and demand as variables, sales as the outcome and a discount as the lever.

1. **Add the variables.** Press **Add variable** in the navigator toolbar (or Ctrl K, *Add variable*). Name each one as a
   noun that can rise or fall: *Sales*, *Price*, *Customer demand*. Tick **Outcome** on *Sales* and set **Better when** to
   *Higher*.
2. **Add a policy lever**: *Discount the price*. A lever is something someone can choose to do.
3. **Link them.** Press **Add link** (or `L`). Choose *Price* as the source and *Customer demand* as the target, and set the
   effect to **− opposite**: when the price goes up, demand goes down. Add *Customer demand → Sales* (+) and
   *Discount → Price* (−).
4. **Close the loop.** Add *Sales → Price* (+) and set its **Timing** to *Delayed*: strong sales let you raise the price, but
   only after a while.
5. **Read the loops.** Open the **Loops** tab of the inspector. You have one balancing loop (**B**) that acts late.
6. **Ask what if.** Add a scenario with the lever *Discount the price* switched on and run it on the **Impact** tab. Sales
   go up at once, and the delayed link then pushes them back: Causal flags the pattern *better before worse*.

## Build a model that answers a question

A good model is small, is built to answer one question, and says where it is unsure. This is the order that works, whether
you build by hand, with an agent, or in a workshop. (It follows the practice of causal loop diagramming from system
dynamics: Kim's and Sterman's guidelines and the group model building scripts.)

### The four steps, on the diagram

On the canvas the work is four moves, the ones taught in every causal loop diagram course. Each is one gesture in the
editor; drawing a diagram shows them in order.

1. **Select the variable names.** Double-click the canvas and type a noun: *Purchasing of healthy food*. Names are plain text on the diagram (naming).
2. **Draw the links.** Drag from a variable's handle to another variable. The arrow points from cause to effect.
3. **Set the polarities.** Click the **+** or **−** at the arrowhead to flip it. Mark a delayed link with **‖** (click it).
4. **Label the loop.** The editor puts an **R** or **B** badge in the middle of every loop. Double-click it and name the behaviour it produces:
   *Health motivation*, *Social support*.

The nine steps below say what to think about on the way.

### 1. Frame the question

Write down, before drawing anything:

- the **question**: "Why did margin fall for two quarters after we automated, and should we automate the other projects?";
- the **boundary**: what is inside the model and what is an outside push;
- the **reference mode**: the behaviour over time you want to explain, in a sentence or a sketch: *margin fell for two quarters,
  then recovered*.

Put them in the model's description. The finished model's loops must be able to produce the reference mode. If they cannot,
a link, a delay or a variable is missing. This is the test of the whole model.

### 2. List the variables

Write the quantities that matter, as nouns, in their positive sense (naming). For each, decide the
kind (stock, flow or quantity) and *Better when*. Mark the one to three
outcomes. Mark anything that can run out as limited.

Then the policy levers (things someone can decide) and the *outside pushes* (things that
happen to you).

### 3. Draw the links

Colour the links by theme when the diagram has more than one story in it.

For each pair you think is connected, apply the test: *if the source goes up, all else equal, does the target go up or
down?* Then:

- one link carries **one mechanism**. If the honest answer is "it depends", add the variable that explains it;
- a lever's cost and its benefit are **two links**;
- set the strength and write the reason in the note;
- give each link a confidence, and evidence where you have it.

### 4. Mark the delays

Mark delayed the links whose effect arrives much later than the others around them. Be honest,
not generous: too few delays hide the patterns that matter most (worse before better, fixes that fail), and too many invent them.

### 5. Close the loops on purpose

For each outcome ask:

- **What reacts when it changes?** That is a balancing loop (a goal being sought).
- **What does its success feed?** That is a reinforcing loop.
- **What limits it?** And what side effects come later?

The archetypes are good starting shapes for these questions, and the recipes
show each one built from scratch. A model with an outcome and no loop is usually missing the feedback that explains it.

### 6. Add structure only when it repeats

Build the first version flat. Add subsystems when you have peers or
levels, templates when the same structure repeats, roll-ups
when something aggregates, and connections between peers that react to each other.

### 7. Define scenarios

One scenario per option under decision, including "do nothing" as the baseline if the
question needs one. Use *Withdrawn* for stopping an existing practice, and external pushes for what happens to you.

### 8. Validate, then read the loops against the reference mode

Open **Issues** and fix errors, then warnings (validation). Open **Loops**: do the loops through
the outcomes explain the reference mode? Name them after the behaviour they produce. Confirm each archetype candidate with the people
who know the situation. If the story the loops tell is not the story people tell, one of you is wrong, and that is the useful
result.

### 9. Compare the options, and say what you do not know

Run the scenarios (what-if), compare them and rank the
levers. Then write the conclusion with its limits:

- which loops dominate the outcomes, and which way they push;
- for each option: the outcome verdicts, the timing pattern (worse before better needs a way to survive the dip), and the side effects elsewhere;
- which low-confidence links the conclusion depends on, and how to check them;
- what the model leaves out. It has **no quantities and no time axis**. Which loop dominates changes over time, so
  when a decision turns on magnitudes or timing, the next step is a simulation model, not more links.

### When you work with an agent

An agent can build and read the model through the same operations as the editor (the *Causal* skill carries this method).
Ask it to state the question and the reference mode first, to build in small batches you can review, and to tell you which
links it was unsure about. Someone may be editing the same model: the editor reloads after an agent's change.

## Build common feedback patterns

Follow the [archetype recipes]({{ site.baseurl }}{% link docs/apps/causal/archetype-recipes.md %}) to build and compare seven feedback patterns.

## Playbooks supporting work

[Prepare a steering committee update]({{ site.baseurl }}{% link docs/playbooks/work.md %}#prepare-steerco-update) helps turn model findings into issues and decisions for review. Bring the relevant scenario results as inputs.
