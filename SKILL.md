# Auto-CoT

Automatic Chain of Thought evolution. Monitors an agent's reasoning in real time, learns from task outcomes, and evolves the reasoning rules over time.

## What it does

1. **Observes** — hooks into the agent's message stream and reads CoT outputs as they happen
2. **Tracks** — logs reasoning patterns, task outcomes, and efficiency metrics continuously
3. **Evolves** — generates, refines, and replaces reasoning rules based on what works
4. **Replays** — applies learned patterns to new tasks automatically

It runs both proactively (every N reasoning steps) and reactively (on task failure or near-miss).

## What it produces

**Reasoning rules** — markdown files that encode a set of reasoning rules for a specific task type. Each rule is a set of step-by-step guidelines the agent follows when handling that class of task.

Example rule: `rules/debugging.md`
```markdown
# Rule: Debugging Tasks

## When to use
Task involves: bugs, errors, exceptions, crashes, broken tests, or unexpected behavior.

## Reasoning steps
1. **Reproduce** — isolate the exact conditions that trigger the issue
2. **Hypothesize** — form a specific testable hypothesis about the cause
3. **Probe** — add logging, breakpoints, or assertions to test the hypothesis
4. **Narrow** — if wrong, use the new info to form the next hypothesis
5. **Fix** — implement the minimal change that resolves the issue
6. **Verify** — confirm the fix works and nothing else broke

## Red flags
- Vague hypotheses ("it probably has to do with X")
- Changing multiple things at once
- No reproduction step before diving into code
- Fixing symptoms instead of root cause

## Exit criteria
- Exact bug condition reproduced
- Root cause identified and documented
- Fix verified with the original failing case
- No regressions in related functionality
```

## Directory structure

```
auto-cot/
├── SKILL.md           ← this skill
└── rules/
    ├── default.md     ← fallback rules for unmatched tasks
    ├── debugging.md   ← reasoning rules for debugging
    ├── planning.md    ← reasoning rules for planning
    ├── research.md    ← reasoning rules for research
    └── ...            ← add new rules as needed
```

## How it works

### Observer
Monitors the agent's message stream continuously. Reads:
- Task description and intent
- Reasoning chain (CoT) outputs
- Tool calls and results
- Final outcome (success/failure/efficiency)

### Rule matching
When a task arrives, the skill matches it to the most relevant rule:
- Keyword matching on task description
- Task-type tags
- Falls back to `default.md` if no match

### Evolution triggers
Rules evolve when:
- Task **fails** — extract what went wrong, update the rule
- Task **succeeds inefficiently** — extract what was slow, optimize the rule
- Task **succeeds well** — extract the winning pattern, promote it
- Every **N reasoning steps** — continuous check and refine

### Learning and replay
The learner extracts successful reasoning patterns from completed tasks:
- Stores patterns in the matching rule file
- On new tasks, replays the most similar successful pattern
- New patterns that solve novel task types → new rule files

## Skill commands

| Command | Description |
|---------|-------------|
| `autocot observe` | Monitor current task reasoning |
| `autocot match <task>` | Show which rule matches a task |
| `autocot evolve` | Run evolution cycle on all rules |
| `autocot learn <task>` | Extract patterns from completed task |
| `autocot replay <task>` | Apply best matching pattern to task |
| `autocot status` | Show rule coverage and evolution state |
| `autocot add-rule <name>` | Create a new rule for a task type |

## Notes for agents

- When the user says "debug this", "figure out why X is broken", etc. — auto-cot activates
- When the agent is doing complex reasoning, auto-cot observes and may inject guidance
- Rules are markdown — human-readable and editable
- New rules can be added at any time by adding a `.md` file to `rules/`
- The skill is always running in the background, watching reasoning chains
- If a task type has no rule, use `default.md` and flag it as needing a new rule
- After every task, ask: "Did the reasoning rule work? What should change?"