# LLM-Guided Spec-Driven Development (SDD) Pipelines

This guide expands on Spec-Driven Development (SDD) by providing a comprehensive set of **chatbot LLM pipelines**, **workflows**, **contexts**, and **steps** to leverage an LLM (like Grok) for creating, updating, validating, and verifying key artifacts. These are designed to be accessible, requiring only conversational interaction with the LLM—no code libraries, terminals, or external tools needed. The LLM acts as a collaborative partner, generating and refining artifacts based on user inputs.

The core **four-phase workflow** structures the process:
1. **Specify**: Define the "what" and "why" in a stable specification.
2. **Plan**: Translate specs into a high-level "how" with technical decisions.
3. **Tasks**: Break the plan into executable steps.
4. **Implement**: Generate or guide the actual code/implementation.

This structured approach provides clarity to AI coding agents (or human developers), reducing "vibe-coding" (ad-hoc, unstructured development) and ensuring reliable, high-quality outcomes. **Benefits** include separating stable intent ("what") from flexible execution ("how"), embedding security/compliance early, and enabling rapid iterations. **Applications** span new projects (0-to-1 builds), feature additions to existing codebases, and modernizing legacy systems (e.g., refactoring monolithic apps into modular ones). Ultimately, this shifts the **future of development** from "code as the source of truth" to "intent as the source of truth," where specifications evolve as living documents, and implementations are regenerated as needed.

Each phase includes:
- **Context**: Guidelines and principles for the phase.
- **Workflow**: High-level sequence of steps.
- **Pipelines**: LLM interaction patterns (e.g., iterative prompting).
- **Key Prompts**: Ready-to-use templates for creating, updating, validating, and verifying artifacts. These prompts are self-contained, incorporating SDD principles like clarity markers ([NEEDS CLARIFICATION]), checklists, and constitutional compliance (e.g., simplicity, test-first).

Use these by copying the prompts into your LLM conversation, providing context from previous artifacts, and iterating based on responses.

---

## Phase 1: Specify (Define Intent and Requirements)

### Context
- Focus on the "what" and "why" without "how." Specifications are natural-language documents capturing user needs, stories, and criteria.
- Embed security (e.g., data encryption), compliance (e.g., GDPR), and non-functional requirements (e.g., performance) from the start.
- Use [NEEDS CLARIFICATION] for ambiguities to prevent assumptions.
- Artifacts: Feature Specification Document (using the template from previous response).
- Guidelines: Keep specs stable and high-level; update only for intent changes, not implementation tweaks. Validate for completeness and testability.

### Workflow
1. Start with a vague idea or user prompt.
2. Iteratively refine through questions and clarifications.
3. Generate the spec document.
4. Validate and verify against principles.
5. Update as needed based on feedback.

### Pipelines
- **Creation Pipeline**: User provides initial idea → LLM generates draft → User reviews/answers clarifications → LLM finalizes.
- **Update Pipeline**: User provides existing spec + changes → LLM incorporates updates → Validates no unintended shifts.
- **Validation/Verification Pipeline**: LLM self-checks or user-prompts checks against checklists.

### Key Prompts

#### Create a Specification
```
You are an expert in Spec-Driven Development. Create a Feature Specification Document based on this idea: [Describe the feature or project idea, e.g., "Build a real-time chat system for team collaboration"].

Use this template:
# Feature Specification: [Feature Name]

## Overview
- **Purpose**: What problem does this feature solve, and for whom?
- **Success Criteria**: How will we measure success? (e.g., user adoption, performance metrics)
- **Scope**: What is included and excluded?

## User Stories
- **Story 1**: As a [user type], I want [action] so that [benefit].
  - Acceptance Criteria: [List measurable criteria]
  - [NEEDS CLARIFICATION: If any ambiguity, e.g., "Auth method not specified - email/password?"]

## Non-Functional Requirements
- **Performance**: [e.g., <2s response]
- **Security**: [e.g., Encrypt messages]
- **Compliance**: [e.g., GDPR data handling]
- **Other**: [e.g., Accessibility]

## Constraints
- **Organizational/Technical**: [e.g., Use existing auth system]

## Edge Cases
- [List scenarios and handling]

Principles to follow:
- Focus on WHAT and WHY, not HOW (no tech stacks).
- Mark ambiguities with [NEEDS CLARIFICATION].
- Ensure requirements are testable and unambiguous.
- Bake in security/compliance from the start.
- Keep it concise yet complete.

Output the full document.
```

#### Update a Specification
```
Here is the existing Feature Specification: [Paste current spec document].

Apply these changes: [Describe updates, e.g., "Add support for file sharing; update performance to <1s; resolve all [NEEDS CLARIFICATION] with: email auth"].

Update the document while:
- Maintaining separation of "what" from "how."
- Ensuring no speculative features are added.
- Re-validating all sections for clarity and testability.
- Preserving version history (add a new entry under ## Version History).

Output the updated document.
```

#### Validate a Specification
```
Validate this Feature Specification: [Paste spec document].

Use this checklist:
- [ ] No [NEEDS CLARIFICATION] markers remain.
- [ ] All requirements are specific, measurable, and testable.
- [ ] Security and compliance are embedded (e.g., encryption, data privacy).
- [ ] Scope is clear; no "how" details leaked in.
- [ ] Edge cases cover failures and extremes.
- [ ] Aligns with SDD principles: Intent as truth, simplicity over speculation.

Provide a report: Mark checklist items as passed/failed, explain issues, and suggest fixes.
```

#### Verify a Specification
```
Verify this Feature Specification against real-world context: [Paste spec] + [Additional context, e.g., "For a legacy system modernization: Integrate with old SQL database; ensure backward compatibility"].

Check for:
- Consistency with constraints (e.g., legacy integration).
- Gaps in user stories or non-functionals.
- Potential conflicts (e.g., performance vs. compliance).
- Overall feasibility without assuming implementation.

Output a verification summary: Strengths, weaknesses, and recommended refinements.
```

---

## Phase 2: Plan (Translate to Technical Decisions)

### Context
- Bridge spec to "how" with high-level architecture, choices, and rationale.
- Enforce "constitutional" principles: Library-first, test-first, simplicity (e.g., ≤3 projects, no over-abstraction).
- Artifacts: Implementation Plan Document (using template from previous response).
- Guidelines: Trace every decision back to spec requirements. Include data models, APIs, and test scenarios. Avoid code snippets—keep high-level.

### Workflow
1. Input the finalized spec.
2. Analyze for technical implications.
3. Generate plan with rationale.
4. Validate against principles.
5. Update for changes in spec or new insights.

### Pipelines
- **Creation Pipeline**: LLM reads spec → Generates plan → Iterates on gates/checks.
- **Update Pipeline**: Incorporate spec updates or feedback (e.g., from production metrics).
- **Validation/Verification Pipeline**: Check gates (e.g., simplicity) and traceability.

### Key Prompts

#### Create an Implementation Plan
```
Based on this Feature Specification: [Paste spec document].

Generate an Implementation Plan using this template:
# Implementation Plan: [Feature Name]

## Overview
- **Linked Specification**: [Reference]
- **Purpose**: Summary
- **Key Deliverables**: [e.g., APIs, models]

## Architecture
- **Components**: [High-level list]
- **Data Models**: [Entities/relationships]
- **User Flows**: [Sequences]
- **APIs/Contracts**: [Endpoints/events]

## Technology Choices
- **Tools**: [e.g., WebSocket for real-time]
- **Rationale**: [Link to spec requirements]

## Test Scenarios
- **Contract/Integration/E2E/Unit**: [Outline]

## Quickstart Guide
- **Validation Scenarios**: [Key tests]

## Risks/Mitigations
- [List]

Principles:
- Enforce constitution: Library-first (start as standalone lib), test-first (scenarios before impl), simplicity (≤3 projects, no wrappers).
- Use Phase -1 Gates: Simplicity [ ], Anti-Abstraction [ ], Integration-First [ ].
- Trace all decisions to spec.
- Keep high-level; no code.

Output the full document.
```

#### Update an Implementation Plan
```
Existing Implementation Plan: [Paste plan].

Linked Spec Updates: [Paste updated spec or describe changes, e.g., "Added file sharing; tightened security"].

Update the plan:
- Regenerate affected sections (e.g., architecture, tests).
- Re-check gates and principles.
- Add to version history.
- Ensure security/compliance remains baked in.

Output the updated document.
```

#### Validate an Implementation Plan
```
Validate this Implementation Plan: [Paste plan] against its linked Spec: [Paste spec].

Checklist:
- [ ] All spec requirements traced to plan elements.
- [ ] Gates passed (or justified exceptions).
- [ ] Test scenarios cover acceptance criteria.
- [ ] Simplicity enforced (no over-engineering).
- [ ] Security/compliance integrated (e.g., auth in APIs).

Report: Passed/failed items, explanations, fixes.
```

#### Verify an Implementation Plan
```
Verify this Plan: [Paste plan] for [Application context, e.g., "Adding to existing codebase: Must integrate with legacy API"].

Check:
- Compatibility with existing systems.
- No conflicts with principles (e.g., modularity).
- Feasibility for modernization/new features.
- Gaps in risks or mitigations.

Output summary: Verified elements, issues, refinements.
```

---

## Phase 3: Tasks (Break into Executable Steps)

### Context
- Derive granular, parallelizable tasks from the plan.
- Mark independent tasks [P] for parallel execution.
- Artifacts: Tasks Document (e.g., markdown list with dependencies).
- Guidelines: Focus on sequence, prerequisites, and deliverables. Include test creation first.

### Workflow
1. Input plan and related docs (e.g., data models).
2. Derive tasks.
3. Organize by order/parallel groups.
4. Validate completeness.
5. Update for plan changes.

### Pipelines
- **Creation Pipeline**: Analyze plan → Generate task list.
- **Update Pipeline**: Adjust for refinements.
- **Validation/Verification Pipeline**: Ensure coverage and logic.

### Key Prompts

#### Create Tasks
```
From this Implementation Plan: [Paste plan]. (Include if available: Data models, contracts, research).

Generate a Tasks Document:
# Tasks: [Feature Name]

## Task List
1. [Task description] - Prerequisite: [None/Prev task] - Deliverable: [e.g., Test file] [P if parallel]
2. ...

## Parallel Groups
- Group 1: Tasks 1,3 [P]

Principles:
- Test-first: Start with contract/integration tests.
- Derive from plan elements (e.g., one task per API).
- Mark [P] for independents.
- Ensure full coverage of plan.

Output the document.
```

#### Update Tasks
```
Existing Tasks: [Paste tasks].

Plan Updates: [Describe or paste changes, e.g., "Added new API endpoint"].

Update tasks:
- Add/remove/reorder as needed.
- Re-mark parallels.
- Maintain test-first order.

Output updated document.
```

#### Validate Tasks
```
Validate these Tasks: [Paste tasks] against Plan: [Paste plan].

Checklist:
- [ ] Full coverage of plan components.
- [ ] Test tasks first.
- [ ] Logical sequencing (prereqs clear).
- [ ] Parallels marked accurately.
- [ ] Security tasks included (e.g., auth implementation).

Report: Issues and suggestions.
```

#### Verify Tasks
```
Verify Tasks: [Paste tasks] for [Context, e.g., "New project: No existing code"].

Check:
- Feasibility (e.g., no assumed deps).
- Completeness for applications (e.g., legacy migration steps).
- No gaps in error handling or compliance.

Summary: Strengths, fixes.
```

---

## Phase 4: Implement (Generate or Guide Execution)

### Context
- Use tasks to generate code or guide human/AI implementation.
- Regenerate code as specs/plans evolve.
- Artifacts: Code snippets, full implementations, or guidance docs.
- Guidelines: Follow test-driven: Write tests first, then code to pass them. Ensure modularity and simplicity.

### Workflow
1. Input tasks.
2. Generate tests, then code.
3. Validate output.
4. Update for iterations (e.g., bug fixes via spec updates).

### Pipelines
- **Creation Pipeline**: Per-task generation → Integrate.
- **Update Pipeline**: Regenerate affected parts.
- **Validation/Verification Pipeline**: Run checks (e.g., via LLM simulation or user review).

### Key Prompts

#### Create Implementation
```
Based on these Tasks: [Paste tasks]. Start with Task 1: [Describe task].

Generate implementation:
- For test tasks: Write test code/specs.
- For code tasks: Write code to pass tests.
- Language/Framework: [Specify, e.g., Python/Flask].
- Principles: Library-first, CLI interfaces, simplicity.

Output: Code block with explanations.
```

#### Update Implementation
```
Existing Implementation: [Paste code].

Task/Plan Updates: [Describe, e.g., "Fix bug in message delivery; add encryption"].

Update code:
- Regenerate affected sections.
- Ensure tests still pass (describe changes).
- Link back to updated plan/spec.

Output updated code.
```

#### Validate Implementation
```
Validate this Implementation: [Paste code] against Tasks: [Paste tasks] and Plan: [Paste plan].

Checklist:
- [ ] Passes all test scenarios.
- [ ] Adheres to principles (e.g., no over-abstraction).
- [ ] Security/compliance implemented.
- [ ] Modular and maintainable.

Report: Compliance, issues, fixes.
```

#### Verify Implementation
```
Verify Implementation: [Paste code] for [Context, e.g., "Modernizing legacy: Check integration with old DB"].

Check:
- Functionality matches spec intent.
- No technical debt (e.g., simple structure).
- Ready for production (e.g., error handling).

Summary: Verification results, recommendations.
```

---

## Overall Guidelines for Using These Pipelines
- **Iteration**: Treat SDD as a loop—feedback from implementation updates specs.
- **Collaboration**: Share artifacts in tools like Notion/Google Docs for team review.
- **Scaling**: For large projects, branch specs (e.g., "v2-experimental") to explore alternatives.
- **AI Amplification**: Use LLM for all phases to amplify creativity, not replace it.
- **Avoid Vibe-Coding**: Always reference artifacts to ground decisions.

This setup enables reliable development for any application, emphasizing intent over code. If you provide a specific feature idea, I can demonstrate by generating artifacts!
