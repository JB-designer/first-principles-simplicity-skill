# First Principles + Simplicity

An agent skill for any task (code, debugging, product, design, research, writing). It makes the AI reason from the real goal and verified facts, and build only the complexity that earns its place.

> **Solve the actual problem with the minimum sufficient complexity.**

## Two concepts, one order

They are related but not equivalent.

- **First principles** works on the *problem*: what is actually true and required. It does not promise a simple answer.
- **Simplicity** works on the *solution*: the least complex option that meets the requirement. It does not promise a correct answer.

First principles sets what "sufficient" means. Simplicity then chooses the least complex option inside that boundary. When they conflict, first principles wins.

## What makes it different

- **Default and proportional.** The skill reasons internally and needs no report, template or files. A small question gets a small amount of reasoning.
- **Preserve before transforming.** If an original value already meets the need, it is used as-is. Translating, renaming, normalizing and inferring each need a reason.
- **Mechanism over exceptions.** For open-ended inputs it uses one fallback ladder: known case, then the raw value, then an honest generic fallback. No endless dictionaries.
- **Exceptions need evidence.** Each edge case is weighed on frequency, impact, cost, coverage and reversibility.
- **Explicit stop.** Once the goal is met and verified, work stops.
- **Limits in both directions.** It names the cases where more complexity is required: unreliable data, severe failure costs, stated uniformity, and simple rules that have demonstrably failed.
- **Counts the agent's own costs.** Extra searches, tool calls and re-checks need a reason too.

## The loop

| Step | Mainly | Question |
|------|--------|----------|
| Goal | First principles | What must be true when this is done? |
| Facts | First principles | What already exists and is reliable? What is observed, and what is assumed? |
| Necessity | First principles | What fails if this step is left out? |
| Minimum sufficient solution | Simplicity | What is the smallest mechanism that meets the need? |
| Verify | Both | Where does it really break? Was anything cut that the facts require? |
| Stop | Simplicity | Is the goal met? Then stop. |

## Example

> **Task:** let users pick the phone column when importing a spreadsheet.
>
> **Over-built:** a synonym dictionary mapping `cust_tel`, `phone_no`, `tel1`... to "Customer phone", maintained for every new file.
>
> **With this skill:** the header `cust_tel` already tells users which column it is. Show it as-is, and use a generic label only when there is no header.

## Installation

**Claude Code**

```bash
git clone https://github.com/JB-designer/first-principles-simplicity-skill ~/.claude/skills/first-principles-simplicity
```

For one project only, clone into `.claude/skills/first-principles-simplicity` instead.

**Claude apps (claude.ai / desktop):** download `first-principles-simplicity.zip` from [Releases](https://github.com/JB-designer/first-principles-simplicity-skill/releases), then upload it in the Skills section of Settings.

**Other agents:** the skill is a single Markdown file ([`SKILL.md`](SKILL.md)). Use it as a skill or paste it into a system prompt.

## Make it the default

A skill loads only when its description matches the task. To apply it to every task, add this line to your project instructions, `CLAUDE.md` or `AGENTS.md`:

```text
Before thinking through any problem, invoke the `first-principles-simplicity` skill. Pass this rule into every subagent brief.
```

## Limitations

- It is a thinking discipline, not an engineering process. It does not replace testing or review.
- It is built for everyday decisions, not for deliberate "rebuild from zero" innovation exercises.
- Its benefits are design goals and have not been benchmarked yet.

## License

[MIT](LICENSE)
