---
title: Causal archetype recipes
nav_exclude: true
permalink: /docs/apps/causal/archetype-recipes/
---

# Causal archetype recipes

Build these models and check the feedback loops and scenario results in Causal.

## Limits to growth
{: #limits-to-growth }

**You meet it when** something grows nicely and then stalls without anyone changing what they do: a product spreading by
word of mouth until the market is saturated, a team that grows until coordination eats its output, a service that wins
customers until it runs out of capacity.

**The shape:** a reinforcing loop drives growth, and a balancing loop, usually delayed or on something that can run out, slows it.

### Build it

1. **The growth loop.** Variables for the thing that grows (*Adopters*, a stock) and what makes it grow (*Adoption rate*,
   a flow). Link *Adopters → Adoption rate* (+: word of mouth) and *Adoption rate → Adopters* (+). That is the R loop.
2. **The limit.** Ask what the growth uses up: *Potential adopters*. Mark it limited.
   Link *Potential adopters → Adoption rate* (+: fewer people left to convince means fewer converts) and
   *Adoption rate → Potential adopters* (−). That is the B loop, and it shares *Adoption rate* with the growth loop.
3. **The outcome** (*Revenue*, from *Adopters*) and the lever someone would pull (*Advertising → Adoption rate*).

### What Causal shows

One R loop, one B loop and the *limits to growth* candidate, with the *Potential adopters* variable as the limit. The lever
*Advertising* feeds the growth loop; what-if shows it helping, and the loops show why it will stop helping.

### What to do about it

Don't push the growth loop harder. Find the limit early and ease it, or accept it, before it binds: widen the market,
add capacity, change what the growth depends on.

### Check yourself

- A balancing loop that is not delayed and not through a *limited* variable is not recognised as a limit. Mark the real limit.
- A limit that sits **outside** the model (the market is fixed) must still be a variable the growth reduces.

## Fixes that fail
{: #fixes-that-fail }

**You meet it when** a remedy relieves the problem at once and the problem is back worse: cutting maintenance to save cost,
pushing a release through by skipping tests, discounting to hit this quarter's number.

**The shape:** the fix works through a balancing loop (problem → fix → problem), and a *delayed* reinforcing loop through the same fix
makes the problem bigger.

### Build it

1. **The problem**, as a variable with *Better when* set: *Cost overrun* (outcome, lower is
   better). Set it: fixes that fail needs to know which direction is an improvement.
2. **The fix**, as a policy lever: *Cut preventive maintenance*. Link
   *Cost overrun → Cut maintenance* (+: the pressure to cut) and *Cut maintenance → Cost overrun* (−: the saving shows at once).
   That is the quick balancing loop.
3. **The side effect**, with its delay. *Cut maintenance → Equipment breakdowns* (+), **delayed**, strong: worn equipment fails months
   later. Then *Breakdowns → Repair cost* (+) and *Repair cost → Cost overrun* (+). That is the delayed reinforcing loop back
   into the problem.

### What Causal shows

An immediate B loop and a delayed R loop that share the stretch *Cost overrun → Cut maintenance*, and the
*fixes that fail* candidate with *problem* and *fix* named, at high confidence (the fix is a lever and the problem has
*Better when*). In what-if, switching the fix on is *better before worse* for the overrun.

### What to do about it

Look for the fix's delayed side effect, and treat the cause, not the symptom; or counter the side effect before it arrives.
Ask what the fix does to the thing that causes the problem.

### Check yourself

- Without a delay on the side effect, the pattern is not recognised: the side effect and the fix would be one balancing loop.
- If the fix does not improve the problem when *Better when* is set (the quick path moves it the wrong way), Causal treats
  it as an investment, not a fix.

## Shifting the burden
{: #shifting-the-burden }

**You meet it when** a team relies on a quick symptomatic relief and never gets to the fundamental solution: overtime instead of
hiring, a workaround instead of a fix, a tool that hides the problem.

**The shape:** two balancing loops act on one problem. The symptomatic fix is quick; the fundamental solution is slow. The quick fix
also takes away the pressure for the slow one.

### Build it

1. **The problem**: *Delivery backlog*, an outcome, lower is better.
2. **The quick fix.** *Backlog → Overtime* (+) and *Overtime → Backlog* (−). Balancing, immediate.
3. **The slow solution.** *Backlog → Perceived need to hire* (+), *Perceived need → Skilled staff* (+, **delayed**: hiring and
   training take months) and *Skilled staff → Backlog* (−). Balancing, delayed. It shares the problem with the quick loop.
4. **The side effect that makes it a trap.** *Overtime → Perceived need to hire* (−): overtime hides the need to hire.
   This is what makes Causal confident.

### What Causal shows

Two B loops on *Delivery backlog*, one immediate and one delayed, and the *shifting the burden* candidate naming the problem,
the symptomatic fix (*Overtime*) and the fundamental solution (*Skilled staff*), at high confidence because the quick fix weakens
the slow one. The same loops also fit *fixes that fail* and *limits to growth* at lower confidence; read the highest first.

### What to do about it

Invest in the fundamental solution and limit the symptomatic fix, especially when the fix weakens the solution. Ask what would
have to be true for the slow solution to start *now*.

### Check yourself

- Without the − link from the quick path into the slow loop, the pattern still shows, with low confidence: the model does not
  say that the fix makes things worse in the long run. Is that what you meant?
- Make sure both loops are on the **same** problem variable.

## Escalation
{: #escalation }

**You meet it when** each side's move raises the other's: an arms race, a price war, two departments that each add controls
because the other did.

**The shape:** a reinforcing loop across two peers, with each reacting to the other.

### Build it

1. **Two peer subsystems** (neither inside the other): *Iron League* and *Furyondy*, each with *Perceived threat* and *Military*.
2. **Each reacts to what it perceives.** *Threat → Military* (+) in each.
3. **Each causes the other's perception.** *Iron.Military → Furyondy.Threat* (+) and *Furyondy.Military → Iron.Threat* (+).

With many pairs, make the cross-links a connection type ("rival of", symmetric) with the link
template *from.Military → to.Threat*, and connect the rivals. Every pair then gets the links.

### What Causal shows

One R loop that **crosses peers** and an *escalation* candidate with the two *parties*, high confidence when two links cross.

### What to do about it

Find a way for one side to step back unilaterally, or agree a joint limit. Look for the link in the loop that either side can
cut or delay.

### Check yourself

- If one of the subsystems contains the other, the loop crosses *levels*, not peers, and is not escalation.
- All peer-crossing links must be +. A − link means one side calms the other and the loop balances instead.

## Eroding goals
{: #eroding-goals }

**You meet it when** a standard slips a little at a time because it is easier to lower than to meet: a quality target
adjusted after every miss, a deadline moved, a service level "recalibrated".

**The shape:** a balancing loop closes a gap, and one of the ways it closes the gap is by lowering the goal.

### Build it

1. **The goal**, named so: *Quality target*, an outcome, higher is better. Put the actual figure in its description.
2. **The condition**: *Actual quality*. The **gap** is *Quality gap*: *Target → Gap* (+) and *Actual quality → Gap* (−).
3. **The two responses to the gap.** The slow honest one: *Gap → Corrective effort* (+) and *Corrective effort → Actual quality* (+,
   **delayed**). The quick dishonest one: *Gap → Pressure to lower the target* (+) and *Pressure → Target* (−).

### What Causal shows

Balancing loops through *Quality target* and an *eroding goals* candidate (always low confidence: it is a name-based
match). The same loops also resemble *shifting the burden*: lowering the target is the quick symptomatic fix.

### What to do about it

Hold the goal fixed, and act on the actual condition. Make the goal visible and changes to it deliberate.

### Check yourself

- A variable must have *goal* or *target* in its name or id for Causal to look for the pattern.
- The link into the goal must be negative (the gap lowers it).

## Success to the successful
{: #success-to-the-successful }

**You meet it when** two parties compete for one resource, and whoever starts ahead gets more of it and so pulls further
ahead: two products sharing a budget, two teams sharing the best people, two suppliers sharing orders.

**The shape:** a reinforcing loop through two siblings, where each one's success takes from the other.

### Build it

1. **Two sibling subsystems** inside one parent (*Product A* and *Product B* inside *Company*), each with *Success* (an outcome)
   and *Resources*. Subsystems are needed here: the pattern is about peers sharing a parent.
2. **Each one's own loop.** *Resources → Success* (+) and *Success → Resources* (+): budget follows success.
3. **The competition.** *A.Success → B.Resources* (−) and *B.Success → A.Resources* (−): the budget is fixed, so what one gets the other does not.

### What Causal shows

R loops through both products, and a *success to the successful* candidate with a *winner* and a *loser*. In what-if, boost one and
the other falls.

### What to do about it

Decouple the allocation from past success, or set an explicit allocation rule (a minimum for each, a review at fixed intervals).

### Check yourself

- Causal recognises the form above, where one loop passes through both siblings. The textbook form in which both draw on a single
  allocation variable on the parent forms separate loops; model the cross-links as shown until that form is recognised.
- The loop must contain a − link; without competition the two loops are independent growth.

## Tragedy of the commons
{: #tragedy-of-the-commons }

**You meet it when** several users each help themselves to a shared resource, and the combined demand ruins it for all: a shared
engineering team, a common budget, a shared cluster, a fishery.

**The shape:** siblings each grow on their own, while their combined demand on a shared parent variable comes back to every one of them.

### Build it

1. **Two templates**: *Organisation* (the parent) and *Team* (the child). A template is natural here: the teams are alike.
2. **A shared resource as a pool**: use the *add pool* macro on the organisation template with *Support load* as the teams'
   **demand** and *Capacity* as what each team **receives**. It adds the shared resource, the summed demand, what is available,
   and the link that gives each team back its share.
3. **Each team's own appetite.** On the team template: *Capacity → Support load* (+): the more capacity a team has, the more it uses.
4. **Two teams** inside an organisation.

### What Causal shows

Balancing loops that run through the roll-up from each team into the organisation's shared demand and back, and a
*tragedy of the commons* candidate naming the *commons* and its *users*.

### What to do about it

Make the shared resource's state visible to each user, and govern its use from the parent: quotas, charging, a shared
view of the load.

### Check yourself

- The users must be **siblings**, and their demand must reach the parent through a roll-up
  (the pool macro does this).
- Confidence is high when at least two users also have a reinforcing loop on their own activity.
