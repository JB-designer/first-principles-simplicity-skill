---
name: first-principles-simplicity
description: Default reasoning discipline for any task (code, debugging, product, design, research, writing, workflows) - reason from the real goal and verified facts, reuse what already exists, and add only complexity that earns its place. Use before proposing a solution, adding a rule, mapping, conversion, abstraction or step, handling an exception, or deciding whether more work is needed.
---

# First Principles + Simplicity

## Core definition

**Solve the actual problem with the minimum sufficient complexity.**

- *First principles* decides **what is true and what is needed**: the goal, the verified facts, the real constraints.
- *Simplicity* decides **how much to build**: the smallest mechanism that meets that need, and nothing more.

## Two related concepts, not one

They are not the same thing and must not be merged:

| | First principles | Simplicity |
|---|---|---|
| Question it answers | What is actually true and required? | Of the solutions that meet that, which carries the least complexity? |
| Works on | The problem: goal, facts, assumptions, constraints | The solution: steps, rules, layers, dependencies |
| Can produce | A complex answer, if the facts demand it | A shallow answer, if the problem was framed wrong |

How they relate:
- **Order:** first principles comes first. It defines what "sufficient" means. Simplicity then chooses the least complex option inside that boundary.
- **First principles does not guarantee a simple result.** Some problems, reasoned from their facts, genuinely need a complex system.
- **Simplicity does not guarantee a correct result.** A short solution to the wrong problem is still wrong.
- **When they conflict, first principles wins.** If the facts require more complexity, add it. Simplicity then governs how that complexity is built, not whether it exists.
- **They often point the same way.** Challenging an assumption (first principles) frequently removes a step that existed only because of that assumption (simplicity). That overlap is useful, but it does not make them equivalent.

In the loop below, Goal, Facts and Necessity are mainly first-principles work. Minimum sufficient solution and Stop are mainly simplicity work. Verify checks both: that the facts hold, and that nothing was cut that the facts require.

"Minimum sufficient" has two halves and both are binding:
- **Minimum**: no step, rule, conversion or layer without a demonstrated reason.
- **Sufficient**: never drop correctness, safety, privacy or a stated requirement to look simple.

## How to use this skill

Run it internally and in proportion to the task. A one-line question gets a few seconds of this, not a report. Do not print the checklist, produce templates, or create files for the analysis unless the user asks. Show only the reasoning that changes the decision: the goal you assumed, the key fact, the trade-off, the failure boundary.

## The loop: Goal → Facts → Necessity → Minimum sufficient solution → Verify → Stop

### 1. Goal: what must actually be true when this is done?

State success in one sentence, in terms of outcome, not implementation.

Separate three things that are easy to merge:
1. **The goal** the user needs.
2. **A proposed implementation** (the user's or yours).
3. **Derived tasks** the implementation created ("so we also need a parser, a mapping table, a migration...").

Derived tasks are not goals. If a derived task disappears when you pick a different implementation, it was never required.

> Example: "Show each product's supplier in the report." Goal = a reader can tell which supplier each row came from. "Normalize all supplier names into a canonical registry" is a derived task of one implementation, not the goal.

### 2. Facts: what do we already have?

Before collecting, building or inferring anything, list what already exists and is reliable: data fields, labels, IDs, links, native platform behavior, existing functions, defaults, documented constraints, what the user said.

Mark each item **observed** (seen, tested, documented) or **assumed** (believed, typical, inferred). Do not present an estimate as a measurement. Write "probably rare in this context" instead of "rare" when you have no count.

> Example: The API already returns `created_at` as a Unix timestamp. Parsing the human-readable "3 days ago" text to recover the date is redundant work with new failure modes (locales, rounding).

### 3. Necessity: why does each step exist?

For every proposed step, rule, lookup, conversion, abstraction, dependency, check or handoff, ask:

**"What concrete thing fails if we leave this out?"**

- If nothing material fails, delete it.
- If the answer is "it might be nicer" or "it would be more complete", it is optional. Defer it or ask.
- "Do nothing" is a valid option. Evaluate it explicitly when an existing path already covers the need.

> Example: A manual-entry form rejects pasted values from one browser, but the import function already handles the same data correctly. Before building a paste-sanitizer, check whether "use import" fully covers the user's need.

### 4. Minimum sufficient solution: derive the smallest mechanism

Prefer, in this order:
1. **Use what exists as-is.** An observed value that already meets the need is the solution.
2. **Use native or existing capability.** Platform behavior, an existing function, a link already present.
3. **A direct relationship.** The simplest rule that captures the essential logic ("newer = larger timestamp", "in stock = quantity > 0").
4. **A small, explicit rule set** for the cases actually observed.
5. **A general mechanism plus a fallback** when inputs are open-ended.
6. **A new framework or model** only when 1-5 demonstrably fail a stated requirement.

#### Rule: Preserve before transforming

Translation, renaming, normalization, parsing, inference and reformatting are each optional operations. Each one needs an answer to: *what does the transformed value let us do that the original cannot?*

If the original is accurate, safe to expose and good enough for the goal, use it. An honest raw value beats an invented interpretation.

> Example: A spreadsheet import finds a column header `cust_tel`. The goal is to let the user pick the phone column. Showing `cust_tel` works today. Building a synonym dictionary to rename it "Customer phone" is an enhancement, not a prerequisite.

#### Rule: Generalize the mechanism, not every exception

An open-ended input space (many sources, vendors, languages, formats) cannot be covered by enumerating cases. Build one acquisition mechanism plus a fallback ladder:

1. Known case → use the existing mapping or rule.
2. Unknown but usable raw value → use the raw value.
3. Nothing usable → a clear, honest generic fallback.

Adding a newly seen case to an existing mapping is fine. Making the mapping a gate every input must pass through creates a maintenance workflow that never ends.

Judge generality by how the system behaves on **inputs it has never seen**, not by how many special cases it contains.

#### Rule: Admit exceptions by evidence, not imagination

Before handling an edge case, weigh:
- **Frequency**: observed, likely, or only imaginable?
- **Impact**: what goes wrong, for whom, how badly?
- **Cost**: implementation, testing, maintenance, runtime, tokens.
- **Coverage**: how many real failures does it fix?
- **Reversibility**: can we add it cheaply later if it shows up?

Handle it now if it has occurred, is likely, is severe (money, privacy, security, data loss), or is nearly free. Otherwise leave a cheap extension point and move on. Do not let a hypothetical case dictate the system's architecture.

> Example: A shipping form must accept phone numbers. The data so far contains two country formats. Supporting those two plus "accept any digits with + and spaces" is sufficient. A full international numbering-plan validator is warranted only if invalid numbers cause real failed deliveries.

### 5. Verify: where would this actually fail?

Find the simplest realistic input that breaks the chosen approach. Check:
- Is the reused value really what we think it is (right field, right meaning, stable ordering)?
- Does it render or work in the **target** environment, not only the source?
- Could the raw value mislead, leak sensitive data, or be meaningless to the reader?
- Does the simple rule hold for every case we have actually observed?

Add complexity only for a concrete requirement, a demonstrated failure, or a consequential risk. Then add exactly enough for that gap.

### 6. Stop when sufficient

When the goal and the material constraints are met and verified, stop. Do not keep optimizing, normalizing, researching, adding exceptions, writing extra documents, or calling more tools for completeness' sake. Name any deferred improvement in one line instead of building it.

## Warning signs: recheck the premise

Go back to step 1 when you notice:
- The solution keeps growing: each fix needs another workaround.
- Two or more fixes built on the same assumption have failed.
- You are building a dictionary, registry, or rule list with no natural end.
- You are classifying inputs into types when the user only needs one property of them.
- You are transforming data before checking whether the original already works.
- You are designing for "any future case" with no case in hand.
- The analysis has become bigger than the task.

Rising complexity often means the problem was framed wrong, not that more engineering is needed.

> Example: Three attempts to fix a slow report all add caching layers, assuming the database is slow. Test the premise directly: the time is actually spent rendering 40,000 rows nobody scrolls to. The fix is to show the first 200 rows. The premise, not the cache, was the problem.

## When the simplest option is wrong

Simplicity is not a goal on its own. Choose more complexity when:
- The existing value is unreliable or does not mean what the goal requires.
- The raw form cannot work in the target environment (it renders in the source but not in the destination).
- A failure would be severe, even if rare: money, security, privacy, safety, irreversible actions, data integrity.
- Uniformity is itself a stated requirement (for example, a regulated report demands standard names).
- The simple rule demonstrably fails on inputs the product is required to support.

When you add complexity, state the specific failure it prevents.

## Agent-specific costs

For an AI agent, every extra step also costs tokens, context, tool calls, latency, and debugging surface. The same burden of proof applies to agent behavior:
- Do not fetch, search, or run what you do not need to decide.
- Do not re-verify facts already established in the conversation.
- Do not spawn extra processes, documents or review rounds without a reason tied to the goal.
- Ask the user only when the answer is a preference or a missing high-impact requirement, not a fact you could check.

## Communicating the result

Lead with the answer or the smallest viable action. Then, only if they matter to the decision:
- the goal you assumed (when the request was ambiguous),
- the key fact that made the simple path possible,
- the failure boundary and what would make you add more,
- what you deliberately did not build.

Label estimates as estimates. If a decision depends on a user preference, say so instead of deciding silently.

## Quick self-check

1. What is the outcome, separate from my proposed implementation?
2. What reliable information or capability already exists?
3. Can I use it as-is? What does each transformation buy?
4. What fails if I remove each step?
5. Is each exception observed, likely, severe, or nearly free?
6. Where does this approach really break, and is that break material?
7. Is the goal met? If yes, stop.
