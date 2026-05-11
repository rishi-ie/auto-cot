# Rule: Debugging

## When to use
Task involves: bugs, errors, exceptions, crashes, broken tests, unexpected behavior, or any "it doesn't work" situation.

## Trigger keywords
debug, fix, error, exception, bug, crash, broken, failing, not working, wrong output, incorrect, unexpected

## Reasoning steps

### 1. Reproduce
Isolate the exact conditions that trigger the issue.
- What input causes the failure?
- What is the expected vs actual output?
- Can you create a minimal reproduction case?

### 2. Hypothesize
Form a specific, testable hypothesis about the root cause.
- "The issue occurs because X returns null when Y is true"
- Avoid vague guesses: "it probably has to do with X"
- One hypothesis at a time

### 3. Probe
Add targeted instrumentation to test the hypothesis.
- Logging at the suspected failure point
- Breakpoints to inspect state
- Assertions to confirm assumptions
- Read existing logs before adding new code

### 4. Narrow
If hypothesis is wrong, use the new information to form the next one.
- The error message tells you WHERE, not necessarily WHY
- Check the call stack, variable state, return values
- Trace backwards from the error to the source

### 5. Fix
Implement the minimal change that resolves the root cause.
- Don't over-engineer — fix only what is broken
- If refactoring is needed, do it separately after the bug is fixed
- Prefer the simplest fix that passes all tests

### 6. Verify
Confirm the fix works and nothing else broke.
- Run the exact reproduction case
- Run related tests
- Check edge cases around the fix
- Document the root cause for future reference

## Red flags
- Changing multiple things at once
- No reproduction step before diving in
- Fixing symptoms instead of root cause
- Assuming the test suite is sufficient
- Not verifying the fix manually

## Exit criteria
- [ ] Exact bug condition reproduced
- [ ] Root cause identified and documented
- [ ] Fix implemented and verified
- [ ] Original failing case now passes
- [ ] No regressions in related functionality

## Pattern library

### Null/undefined errors
1. Find where the null/undefined occurs
2. Trace back to where it should have been set
3. Check all paths that lead to that variable
4. Add null check at the earliest point possible

### Type errors
1. Identify the expected vs actual type
2. Find where the value was created/transformed
3. Check all paths that could produce wrong type
4. Add type guard or validation at boundary

### Async/timing errors
1. Confirm it is a timing issue (not logic)
2. Find the race condition or missing await
3. Add explicit sequencing or synchronization
4. Test under stress conditions

### Logic errors
1. Write a test that fails with the wrong output
2. Trace the execution path manually
3. Find where the decision diverges from intent
4. Fix the condition, not the outcome