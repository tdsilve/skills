---
name: code-orientation
description: Apply SoC, DRY, KISS, YAGNI when writing or refactoring code. Use on any coding task — features, fixes, scripts, reviews. Output code only; no principle commentary unless asked.
---

# Code Orientation

These four principles orient how code gets written. They are not a checklist to narrate — apply them silently, the way an experienced engineer would, and let the resulting code speak for itself. Don't add comments, summaries, or callouts explaining that SoC/DRY/KISS/YAGNI were followed; just write the code that follows them.

## Core Principles

**SoC — Separation of Concerns.** Each unit of code (function, class, module) should have one reason to change. If a function is fetching data, transforming it, AND formatting it for display, that's three concerns wearing one coat — split them. The test: can you name what this function does in one short phrase without using "and"? If not, split it.

**DRY — Don't Repeat Yourself.** Extract duplicated logic only when the duplication is real (same logic, same reason to change together) and the resulting abstraction stays simple. Two call sites that happen to look similar today but represent different concerns are not duplication — they're coincidence, and extracting them couples things that should stay independent. Wait for a third real occurrence, or a clear case where two copies will rot out of sync, before reaching for an abstraction.

**KISS — Keep It Simple.** Prefer the direct, idiomatic solution for the language and context over a clever or generalized one. If a plain loop reads more clearly than a chain of higher-order functions, use the loop. If a single function does the job, don't wrap it in a class. Idiomatic beats impressive.

**YAGNI — You Aren't Gonna Need It.** Don't add config options, extension points, abstract base classes, feature flags, or speculative parameters for needs that don't exist yet. Build for the requirement in front of you. It is cheaper to add an abstraction later, once a second real use case shows up, than to carry one now that may never be used.

## Resolving conflicts between principles

These principles sometimes pull in different directions — most often DRY (which wants to extract) against KISS/YAGNI (which want to leave things alone). When that happens, **simplicity wins**: don't extract a helper, base class, or config layer for a single current use case just because the logic looks reusable in theory. Duplication that is small, stable, and easy to read beats an abstraction built ahead of need. Revisit the decision when a real second or third use case actually appears — that's the trigger DRY is waiting for, not foresight.

SoC is mostly compatible with the others and rarely needs trading off: splitting unrelated responsibilities tends to make code simpler to read, not more complex. Apply it readily.

## How to apply this while coding

- Before writing a function or module, ask what its one job is. If the request bundles multiple jobs ("fetch the file, parse it, and email a summary"), write separate units for each rather than one block that does all three.
- While writing, default to the most ordinary, idiomatic shape for the language at hand — the version another engineer would write without thinking twice. Reach for a less common pattern only when it removes real complexity, not when it's just a way to seem thorough.
- Before adding a parameter, flag, config option, or extension hook, check whether the current request actually needs it. If it's there for a "might want this later" reason, leave it out.
- Before extracting a shared helper, check whether there are genuinely two-or-more current call sites with the same logic and the same reason to change. If there's only one call site, or the call sites differ in ways that would force the helper to grow conditionals, don't extract — write each one plainly instead.
- When in doubt between a simpler version and a more "complete" one, write the simpler version. It's always cheaper to extend later than to unwind speculative structure now.

## What not to do

- Don't produce a written explanation, checklist, or comment block listing which principles were applied — the code should reflect them implicitly.
- Don't pre-emptively split a small, cohesive function just to "demonstrate" SoC if it genuinely has one job.
- Don't leave obvious duplication unaddressed when a third occurrence makes the pattern clear and the abstraction would be simple.
- Don't build generic, configurable, or pluggable structures for a single concrete use case.