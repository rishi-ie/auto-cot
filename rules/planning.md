# Rule: Planning

## When to use
Task involves: building something new, project setup, roadmap creation, feature planning, architecture design, or any complex multi-step work.

## Trigger keywords
build, create, plan, design, architect, implement, roadmap, setup, initialize, establish, structure

## Reasoning steps

### 1. Define the goal
State the desired end state clearly and concretely.
- "A working X that does Y"
- "A system that achieves Z under conditions A, B, C"
- Avoid vague goals like "improve performance"

### 2. Identify constraints
What are the boundaries?
- Time limits (deadline, time budget)
- Resource limits (team size, budget, skills)
- Technical constraints (existing stack, APIs, integrations)
- Quality constraints (performance targets, error rates)

### 3. Break down
Decompose the goal into the smallest possible units.
- Use hobbit-style decomposition: goal → atomic leaves
- Identify dependencies between units
- Find which units can run in parallel

### 4. Identify unknowns
What don't you know yet?
- Technology decisions not made
- Requirements not confirmed
- External dependencies (APIs, third parties)
- Risks that need validation

### 5. Validate unknowns first
Tackle the highest-risk unknowns early.
- Build prototypes for uncertain components
- Get requirements signed off before full build
- Test integrations before depending on them

### 6. Order for delivery
Arrange units for early partial delivery.
- MVP first: what is the smallest thing that demonstrates value?
- Core functionality before polish
- Infrastructure that enables other work first

### 7. Define milestones
What are the checkpoint states?
- Milestone 1: [definition]
- Milestone 2: [definition]
- Each milestone should be verifiable independently

### 8. Plan for iteration
What will change? How will you adapt?
- Requirements will shift — build for changeability
- Tests will reveal misunderstandings — expect rework
- New information will arrive — stay flexible

## Red flags
- Starting to build before the goal is clear
- Building things that don't directly serve the goal
- Ignoring risks instead of addressing them
- Planning in too much detail for distant milestones
- Over-engineering for requirements that don't exist yet

## Exit criteria
- [ ] Goal stated concretely
- [ ] Constraints documented
- [ ] Work decomposed to atomic units
- [ ] Dependencies mapped
- [ ] Unknowns identified and addressed
- [ ] Milestones defined with clear acceptance criteria