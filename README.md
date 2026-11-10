# First Principles + Simplicity

An agent skill that makes **first-principles reasoning and simplicity** the default way an AI works on any task: code, debugging, product decisions, design, research, writing, and workflows.

> **Solve the actual problem with the minimum sufficient complexity.**

- *First principles* decides **what is true and what is needed**: the goal, the verified facts, the real constraints.
- *Simplicity* decides **how much to build**: the smallest mechanism that meets that need.

### Two related concepts, not one

The two are connected but not equivalent:

- **First principles** works on the *problem*. It strips away analogy and convention to find what is actually true and required. It does not promise a simple answer. Some problems, reasoned from their facts, genuinely need a complex system.
- **Simplicity** works on the *solution*. Among the options that meet the requirement, it picks the one with the least complexity. It does not promise a correct answer. A short solution to the wrong problem is still wrong.

The skill puts them in a fixed order. First principles defines what "sufficient" means, and simplicity chooses the least complex option inside that boundary. When they conflict, first principles wins. If the facts require more complexity, it is added, and simplicity then governs how it is built, not whether it exists.

---

## Why this skill

AI agents are good at producing plausible solutions. Their recurring failure is a different one: they keep adding parsers, mappings, exception branches, lookups and abstraction layers without first asking whether any of it is needed. This skill is built to catch that tendency.

What it does:

- **Runs by default, in proportion.** It is a reasoning discipline, not a ceremony. A one-line question gets a few seconds of it. There is no mandatory report, template or file output, and the agent shows only the reasoning that changes the decision.
- **Preserve before transforming.** Translating, renaming, normalizing, parsing and inferring each have to justify themselves. If the original value is accurate, safe and good enough, it is used as-is.
- **Generalize the mechanism, not every exception.** For open-ended inputs (many sources, vendors, languages, formats), it uses one acquisition mechanism plus a fallback ladder: known case, then the raw value, then an honest generic fallback. It does not grow a dictionary with no end. A system's generality is judged by how it behaves on inputs it has never seen.
- **Admits exceptions by evidence, not imagination.** Each edge case is weighed on five factors: frequency, impact, cost, coverage and reversibility. Hypothetical cases don't get to dictate the architecture.
- **Has an explicit stop condition.** Once the goal is met and verified, the agent stops. No extra optimizing, research, exceptions or documents just to be complete.
- **Sets limits in both directions.** "Minimum" never overrides "sufficient". The skill names the situations where more complexity is required: unreliable data, a raw form that won't work in the target environment, severe failure costs (money, security, privacy, data integrity), uniformity as a stated requirement, and simple rules that have demonstrably failed.
- **Counts the agent's own costs.** Extra fetches, tool calls, sub-tasks, review rounds and re-checks of settled facts cost tokens, time and failure surface. The same burden of proof applies to them.
- **Is honest about evidence.** Observed facts are kept separate from assumptions, and estimates are labelled as estimates.

---

## The loop

**Goal → Facts → Necessity → Minimum sufficient solution → Verify → Stop**

| Step | Mainly | Question |
|------|--------|----------|
| Goal | First principles | What must actually be true when this is done? (Outcome, not implementation.) |
| Facts | First principles | What reliable information and capability already exist? What is observed, and what is assumed? |
| Necessity | First principles | What concrete thing fails if this step is left out? "Do nothing" is a valid option. |
| Minimum sufficient solution | Simplicity | Use what exists as-is, then native capability, then a direct relationship, then a small rule set, then a mechanism with a fallback. Build a new framework only when all of these fail. |
| Verify | Both | What is the simplest realistic input that breaks this approach? Was anything cut that the facts require? |
| Stop | Simplicity | Is the goal met and verified? If yes, stop. |

The skill also lists **warning signs** that send the agent back to the goal. Examples: each fix needs another workaround, several fixes built on the same assumption have failed, or a rule list has no natural end.

---

## Example

> **Task:** "Let users pick the phone column when importing a spreadsheet."
>
> **Typical over-built answer:** build a synonym dictionary that maps `cust_tel`, `phone_no`, `mobile`, `tel1`... to "Customer phone", then maintain it for every new file.
>
> **With this skill:** the goal is that users can recognize the phone column. The header `cust_tel` already does that. Show the header as-is, keep any existing mapping for names it already knows, and fall back to a generic label only when there is no header. No new workflow is created.

More short examples are in [`SKILL.md`](SKILL.md): data imports, timestamps, form validation, and a slow-report debugging case.

---

## Installation

### Claude Code

Personal (all projects):

```bash
git clone https://github.com/JB-designer/first-principles-simplicity-skill ~/.claude/skills/first-principles-simplicity
```

Project only:

```bash
git clone https://github.com/JB-designer/first-principles-simplicity-skill .claude/skills/first-principles-simplicity
```

### Claude apps (claude.ai / desktop)

Zip the folder that contains `SKILL.md` and upload it in the Skills section of Settings.

### Other agents

The skill is a single Markdown file. Any agent that reads `SKILL.md`-style skills, or accepts a system or project prompt, can use it.

---

## Making it truly default

A skill is only loaded when its description matches the task. To have it applied to **every** task, add one standing line to your project instructions, `CLAUDE.md`, or `AGENTS.md`:

```text
Before thinking through any problem (analysis, debugging, design, product decisions, plans), invoke the `first-principles-simplicity` skill. Pass this rule into every subagent brief.
```

---

## Usage

Once installed and referenced as above, it works automatically. To call it explicitly:

```
/first-principles-simplicity Should we add a caching layer here?
/first-principles-simplicity Review this import plan before I build it.
```

---

## Structure

```
.
├── SKILL.md    # The skill
└── README.md   # This file
```

---

## Limitations

- It is a thinking layer, not an engineering process. It does not replace testing, code review or release procedures.
- It is built for everyday decisions, not for deliberate "rebuild everything from zero" innovation exercises.
- It does not create an audit trail of its reasoning. If a major decision needs a written record, ask for one.
- Its benefits (fewer steps, lower token use, equal or better correctness) are design goals, not yet benchmarked.
