# Rule: Default (Fallback)

## When to use
Any task that doesn't match a specific rule category. Every rule file should be a specialization — this is the safety net.

## Reasoning steps
1. **Understand** — restate the task in your own words. What is the expected outcome?
2. **Decompose** — break it into the smallest possible steps
3. **Prioritize** — which steps are blockers? Which can run in parallel?
4. **Execute** — run one step at a time, verify before moving on
5. **Verify** — does the output match the expected outcome?
6. **Iterate** — if not, adjust and try again

## Red flags
- Starting without fully understanding the task
- Skipping verification steps
- Assuming the task is done before checking
- Ignoring error messages

## Exit criteria
- Expected outcome achieved
- No unexplained state changes
- All related systems verified working