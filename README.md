# Auto-CoT

Automatic Chain of Thought evolution. A skill that monitors an agent's reasoning in real time, learns from task outcomes, and evolves reasoning rules over time.

## What it does

Auto-CoT runs alongside an agent and:

1. **Observes** — hooks into the agent's message stream, reading CoT outputs as they happen
2. **Tracks** — logs reasoning patterns, task types, and outcomes continuously, not just on failure
3. **Evolves** — generates, refines, and replaces reasoning rules based on what works
4. **Replays** — applies learned patterns to new tasks automatically

It is both **proactive** (checks reasoning every N steps) and **reactive** (responds to task failure, near-miss, and success).

## How it works

The skill maintains a set of **reasoning rules** — markdown files that encode how to approach specific task types. Each rule is a set of step-by-step guidelines the agent follows.

When a task arrives, auto-cot:
1. Matches the task to the most relevant rule (keyword/tag matching, falls back to `default.md`)
2. The agent follows the rule's reasoning steps
3. After each task, auto-cot evaluates: did the rule work? What should change?
4. Rules evolve: new patterns are synthesized, failed approaches are deprecated, successful ones are promoted

## Rules

Each rule is a markdown file in `rules/`. The skill ships with:

| Rule | When to use |
|------|-------------|
| `default.md` | Any task without a specific rule |
| `debugging.md` | Bugs, errors, exceptions, crashes, broken tests |
| `planning.md` | Building new things, roadmaps, architecture, feature planning |
| `research.md` | Finding information, literature review, fact-checking |
| `writing.md` | Prose, documentation, emails, reports, technical writing |

### Rule format

Every rule follows this structure:

```markdown
# Rule: <Task Type>

## When to use
<What triggers this rule>

## Reasoning steps
<Ordered steps the agent follows>

## Red flags
<What to watch for that signals wrong reasoning>

## Exit criteria
<What must be true before the task is done>
```

### Example: debugging.md

```markdown
## Reasoning steps
1. **Reproduce** — isolate the exact conditions that trigger the issue
2. **Hypothesize** — form a specific, testable hypothesis
3. **Probe** — test the hypothesis with logging or breakpoints
4. **Narrow** — if wrong, use the new info for the next hypothesis
5. **Fix** — implement the minimal change that resolves it
6. **Verify** — confirm fix works and nothing else broke
```

## Proactive vs Reactive

| Mode | Trigger | Action |
|------|---------|--------|
| **Proactive** | Every N reasoning steps | Check if reasoning is on track, offer correction |
| **Reactive** | Task fails | Extract failure pattern, update rule |
| **Reactive** | Task succeeds inefficiently | Optimize the reasoning steps |
| **Reactive** | Task succeeds well | Promote pattern to rule or improve existing |

## For Agents

Add this skill to any agent that does complex reasoning. When:
- The agent encounters a new task type → create a new rule
- The agent follows a rule and it works → log the success pattern
- The agent follows a rule and it fails → extract what went wrong
- The agent discovers a better reasoning approach → update the rule

### Activating auto-cot

When the skill is loaded:
- It observes automatically on every reasoning step
- Use `autocot match <task>` to see which rule matches
- Use `autocot status` to see rule coverage and evolution state
- Use `autocot add-rule <name>` to create a new rule for a task type

### Adding rules

Add a new rule by creating a `.md` file in `rules/`:

```markdown
# Rule: <Task Type>

## When to use
<keywords that trigger this rule>

## Reasoning steps
1. <step>
2. <step>

## Red flags
- <warning>

## Exit criteria
- [ ] <criterion>
```

## Directory structure

```
auto-cot/
├── SKILL.md           ← the skill definition
├── README.md          ← this file
└── rules/
    ├── default.md
    ├── debugging.md
    ├── planning.md
    ├── research.md
    └── writing.md
```

## Contributing

Add new rules:
1. Create `rules/<type>.md` following the format
2. Include: when-to-use, reasoning steps, red flags, exit criteria
3. Make rules specific — generic rules don't help
4. Test against real tasks and refine based on outcomes

---

*Built by [rishi-ie](https://github.com/rishi-ie) | Part of the rishi-ie portfolio*
