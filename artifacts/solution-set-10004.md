# Spec-Driven Development Discovery Kit

## What is Spec-Driven Development?

Spec-Driven Development (SDD) flips the script on traditional software development. Instead of writing specifications to guide code, **specifications become executable** — they directly generate working implementations.

**The Power Inversion:**
- **Traditional:** Specs serve code (specs get outdated, code is truth)
- **SDD:** Code serves specs (specs are truth, code is generated)

This eliminates the gap between intent and implementation by making specifications precise enough to generate working systems.

---

## Core Philosophy

### The Three Pillars

1. **Specifications as Source of Truth** — The spec is the primary artifact, code is its expression
2. **Executable Precision** — Specs must be complete and unambiguous enough to generate working systems
3. **Continuous Evolution** — Specs evolve based on real-world feedback, code regenerates automatically

### When SDD Makes Sense

✅ **Good for:**
- Complex systems with many integrations
- Projects requiring frequent pivots
- Teams wanting rapid iteration
- Systems needing consistent architecture

❌ **Not ideal for:**
- Simple, one-off scripts
- Highly experimental research code
- Projects where requirements are completely unknown

---

## Discovery Framework

### Phase 1: Idea to Specification

#### The Specification Discovery Prompt

Use this conversational prompt to transform vague ideas into structured specifications:

```
I have an idea for [BRIEF DESCRIPTION]. Help me turn this into a complete specification by:

1. Asking clarifying questions about user needs and goals
2. Identifying edge cases I might have missed  
3. Defining precise acceptance criteria
4. Highlighting any assumptions I'm making
5. Suggesting what research might be needed

Please ask me questions one theme at a time, and help me think through each aspect thoroughly before moving to the next.
```

#### The Five Essential Questions

For any feature, these questions must have clear answers:

1. **WHO** needs this and why? (User personas and motivations)
2. **WHAT** exactly should happen? (Specific behaviors and outcomes)
3. **WHEN** does this occur? (Triggers, timing, sequences)
4. **WHERE** does this fit? (System boundaries and context)
5. **HOW MUCH** is acceptable? (Performance, scale, error rates)

### Phase 2: Specification Quality Gates

#### Completeness Checklist

Before moving to implementation planning, verify:

- [ ] No [NEEDS CLARIFICATION] markers remain
- [ ] Every user story has acceptance criteria
- [ ] Success is measurable (specific metrics defined)
- [ ] Failure scenarios are documented
- [ ] Dependencies and constraints are explicit
- [ ] Non-functional requirements are specified

#### The Ambiguity Test

Read your specification aloud to someone unfamiliar with the project. If they ask "what do you mean by..." or "how should I handle...", your spec needs more precision.

### Phase 3: Implementation Planning Discovery

#### The Technical Translation Prompt

Use this to convert business requirements into technical plans:

```
Given this specification: [PASTE SPEC]

Help me create an implementation plan by:

1. Identifying the core technical challenges
2. Suggesting appropriate technologies and why
3. Breaking down into implementable phases
4. Highlighting risks and mitigation strategies
5. Creating a clear success criteria for each phase

Focus on simplicity and proven approaches over clever solutions.
```

#### Architecture Decision Framework

For each major technical decision, document:

- **Decision:** What we're choosing
- **Context:** Why this decision is needed
- **Options:** What alternatives we considered
- **Rationale:** Why this option is best
- **Consequences:** What this enables/constrains

---

## Quality Guidelines

### The Simplicity Principle

Always start with the simplest solution that could work:

- ✅ Use existing tools and frameworks directly
- ✅ Minimize the number of moving parts
- ✅ Choose boring, proven technologies
- ❌ Build custom abstractions prematurely
- ❌ Add features "for the future"
- ❌ Over-engineer for scale you don't have

### The Test-First Mindset

Before writing any code, define:

1. **What success looks like** (acceptance tests)
2. **How you'll verify it works** (integration tests)
3. **What could go wrong** (error scenarios)
4. **How you'll know if it breaks** (monitoring)

### The Research Integration Pattern

For every major technical choice, gather:

- **Library comparisons** (features, performance, community)
- **Integration examples** (how it works with your stack)
- **Organizational fit** (team skills, existing standards)
- **Operational impact** (deployment, monitoring, maintenance)

---

## Practical Templates

### Feature Specification Template

```markdown
# Feature: [Name]

## User Story
As a [persona], I want to [action] so that [benefit].

## Acceptance Criteria
Given [context]
When [action]
Then [expected outcome]

## Success Metrics
- [Measurable outcome 1]
- [Measurable outcome 2]

## Edge Cases
- [Scenario 1] → [Expected behavior]
- [Scenario 2] → [Expected behavior]

## Constraints
- Technical: [limitations]
- Business: [requirements]
- Timeline: [deadlines]

## Research Needed
- [Question 1]
- [Question 2]

## Notes
[Additional context, assumptions, related decisions]
```

### Implementation Plan Template

```markdown
# Implementation Plan: [Feature]

## Overview
[2-3 sentence summary of approach]

## Technical Decisions
| Decision | Options Considered | Choice | Rationale |
|----------|-------------------|--------|-----------|
| Database | PostgreSQL, MongoDB | PostgreSQL | [reasoning] |
| API Style | REST, GraphQL | REST | [reasoning] |

## Implementation Phases
### Phase 1: [Name]
- **Goal:** [what this achieves]
- **Tasks:** [concrete deliverables]
- **Success:** [how you know it's done]

### Phase 2: [Name]
- **Goal:** [what this achieves]
- **Tasks:** [concrete deliverables]  
- **Success:** [how you know it's done]

## Risks & Mitigations
- **Risk:** [potential problem]
  - **Mitigation:** [how to address]

## Quality Gates
- [ ] Tests are written and failing
- [ ] Integration contracts defined
- [ ] Performance criteria established
- [ ] Error handling specified
```

---

## Workflow Patterns

### The Specification-First Workflow

1. **Ideate** → Use discovery prompts to explore the problem
2. **Specify** → Create detailed, unambiguous specifications
3. **Research** → Gather technical context and options
4. **Plan** → Design implementation approach
5. **Generate** → Create code from specifications
6. **Validate** → Test against acceptance criteria
7. **Evolve** → Update specs based on real-world feedback

### The Feedback Loop

- **Real-world usage** informs specification updates
- **Performance metrics** become new requirements
- **User feedback** drives acceptance criteria changes
- **Technical debt** becomes architectural constraints

### The Pivot Pattern

When requirements change:
1. Update the specification first
2. Identify affected implementation plans
3. Regenerate code from updated specs
4. Test against new acceptance criteria

This transforms pivots from manual rewrites into systematic regenerations.

---

## Success Indicators

### You're Doing SDD Right When:

- ✅ Changing requirements feels manageable, not catastrophic
- ✅ New team members understand the system by reading specs
- ✅ Code generation feels reliable and predictable
- ✅ Technical decisions have clear traceability to requirements
- ✅ Testing scenarios come naturally from specifications

### Red Flags to Watch For:

- ❌ Specifications are vague or full of "TBD"
- ❌ Code doesn't match what specifications describe
- ❌ Implementation plans assume too much context
- ❌ Research findings don't influence technical decisions
- ❌ Acceptance criteria can't be automatically tested

---

## Getting Started Checklist

### Week 1: Foundation
- [ ] Choose a small, well-understood feature to start with
- [ ] Write your first specification using the discovery prompts
- [ ] Create an implementation plan following the template
- [ ] Generate and validate initial code

### Week 2: Process Refinement  
- [ ] Add feedback loops (metrics, user testing)
- [ ] Update specifications based on real usage
- [ ] Practice the pivot pattern with a requirements change
- [ ] Document what works and what doesn't

### Week 3: Team Integration
- [ ] Share specifications with team for review
- [ ] Establish specification review processes
- [ ] Create shared research knowledge base
- [ ] Define team-specific quality gates

### Month 2 and Beyond
- [ ] Expand to larger, more complex features
- [ ] Build library of reusable specification patterns
- [ ] Integrate with existing development workflows
- [ ] Measure impact on development velocity and quality

---

## Common Questions

**Q: How is this different from traditional documentation?**
A: Traditional docs describe what was built. SDD specs define what will be built and generate it directly. The spec is executable, not just descriptive.

**Q: Do I need special tools?**
A: No. You can practice SDD with any AI assistant, text editor, and version control system. The methodology is more important than the tools.

**Q: What if specifications become too complex?**
A: Break them down. If a spec is hard to understand, it's hard to implement correctly. Use the simplicity principle and phase-based delivery.

**Q: How do I handle rapidly changing requirements?**
A: This is SDD's strength. Update the specification, and implementation follows. The key is maintaining specs as the single source of truth.

**Q: Can this work for existing projects?**
A: Yes, but start with new features or major refactors. Gradually expand SDD practices as you prove their value on your team.

---

*Remember: SDD is not about replacing developers—it's about amplifying human capability by automating the mechanical translation from specification to implementation while keeping creativity, critical thinking, and problem-solving at the center of development work.*
