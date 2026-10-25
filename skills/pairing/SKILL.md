---
name: pairing
description: Use when the human partner wants to type the code themselves and have Claude direct the work so they learn while implementing. Triggers include "let's work on this together", "tell me which file to go to and what to write", "I want to implement it myself", "teach me as we go", "pair on this", "I'll drive", "give me hints, not answers", or an explicit /pairing invocation at the start of or during an implementation.
---

# Pairing

## Overview

The partner drives: they type, run and read. Claude navigates: one step per turn, each step the smallest change the partner can verify by running one command. The learning happens when they see each red and green themselves, so a batched plan defeats the purpose even when it would be faster.

**Keyboard rule.** Claude does not edit, write or run anything in the repository while pairing. Reading files to prepare a step is fine. If the partner hands a step over explicitly ("you do this one"), do it, say so, and return to navigating.

## One turn is one step

Every turn has these parts, in this order:

1. **Read-back** (one or two sentences). What their pasted output says and whether it is the expected red or green. If it is not, this turn is about that, and the next step waits.
2. **Where.** One file path and the spot in it: function name, line, or "new file".
3. **What.** The exact code, complete, in one block. No `...`, no "similar to the previous one".
4. **Why** (one to three points). The decisions in this code the partner could not infer: why this file, why this shape, what breaks otherwise. Not a tour of the feature.
5. **Run.** One command, the exact output expected, and what a wrong result would look like and mean.
6. Stop. The next step waits for their paste.

A step that needs two files to compile is two steps, unless the second file is a one-line import.

## Hint mode

When the partner asks to try first ("let me write it", "just give me a hint"), replace **What** with the goal of the edit: what the code must do, the names it must use, and the run that proves it. Withhold the code. If their attempt fails, read the error back and give the next smallest hint. Show the full code only when they ask for it or after two failed attempts on the same step. Stay in hint mode until they leave it.

## When a plan already exists

Follow the plan's task order. A plan step that bundles several edits is split at the keyboard into one edit per turn. Quote the plan's "why" instead of re-deriving it.

## Pressure

"We're short on time" shrinks the step to the smallest verifiable unit. It never grows the step to "here is everything". Offer to stop at the next green checkpoint instead of offering to take over.

## Pausing and resuming

When the partner stops, end at a green checkpoint with a recap they can paste into the next session:

- **Done:** the steps completed, one line each
- **State:** the command that should still be green, and its expected output
- **Next:** the next step's Where, ready to start

When a session starts from a recap, ask them to run the State command first, and do not continue until it is green.

## Quick reference

| Partner says | Claude does |
| --- | --- |
| "Done, output below" | Read the output first, state expected or unexpected, then the next step |
| "What did I do wrong?" | Point at file and line, show how the error text leads there, they type the fix |
| "Can we speed this up?" | Fewer words in Why, same one-step turn |
| "Just do this part" | Do it, say so, resume navigating |
| "Let me try first" | Hint mode: goal and run, no code |
| "What is X?" (a concept) | Short answer with a small example, then repeat the same step |
| "Let's stop here" | Pausing recap at the last green |
| Their code differs from the step | Say if the difference matters; if it does not, keep theirs |
| Nothing about the result | Ask for the output; never assume green |

## Common mistakes

- **The whole plan in one message under time pressure.** Observed in baseline testing: the agent wrote the test, the run, and both fixes in one reply, then asked the partner to "start with step 1". Every answer was already on screen.
- **Next step before reading their output.** A typo in step one then compounds into step two.
- **Explaining what the code does instead of why it is shaped this way.** The partner can read code; they cannot read the decision.
