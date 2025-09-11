This is the core of modern, AI-augmented software engineering. Here is a comprehensive framework of LLM pipelines, workflows, and precise prompts to systematically create, update, and validate all key artifacts across the four-phase Spec-Driven Development workflow.

### **The Four-Phase LLM Pipeline for Spec-Driven Development**

This framework treats the LLM not as a coder, but as a **Specification Engineer, Architect, and Product Manager** that you are guiding through a structured, iterative process.

---

### **Phase 1: SPECIFY - The "What & Why" Pipeline**
**Goal:** Transform a vague idea into a pristine, unambiguous Product Requirements Document (PRD).
**LLM Persona:** Product Manager / Business Analyst

#### **Workflow & Context Guidelines:**
1.  **Provide Ample Context:** Always start by giving the LLM the necessary background.
2.  **Iterate:** The first output will be a draft. Use follow-up prompts to refine, challenge, and expand.
3.  **Demand Clarification Flags:** Force the LLM to be honest about uncertainties.

#### **Core Prompts:**

**1. Initial Draft Creation Prompt:**
```markdown
**Role:** You are an expert Product Manager. Your task is to create a first draft of a Product Requirements Document (PRD) based on my idea.

**Context:** We are building a [type of application, e.g., SaaS platform, mobile app]. The overall goal of the project is [high-level project goal]. Our target users are [user persona 1] and [user persona 2].

**Instruction:** Transform the following feature idea into a structured PRD using the template below. Crucially, you MUST mark every ambiguity or missing detail with `[NEEDS CLARIFICATION: ...]`. Do not assume anything.

**Feature Idea:** "[Your vague feature idea goes here. e.g., We need a way for users to collaborate on documents in real-time.]"

**PRD Template to Use:**
```
# Feature: [Feature ID] - [Feature Name]

## 1. Background & Motivation
*   **Problem:** What user problem does this solve?
*   **Goal:** What is the strategic goal of this feature?

## 2. User Stories & Acceptance Criteria
*   **As a** [User Role], **I want** [Goal], **so that** [Reason/Benefit].
    *   **Acceptance Criteria:**
        *   Given [initial state], When [action], Then [expected outcome].
        *   [Scenario 2]
*   [User Story 2...]

## 3. [NEEDS CLARIFICATION] Section
*   List all ambiguities here explicitly.
```
```

**2. Specification Refinement & Challenge Prompt:**
*(After reviewing the first draft)*
```markdown
This is a good start. Now, act as a critical product advisor. Review this draft PRD and perform the following tasks:

1.  **Challenge Assumptions:** Identify any implicit assumptions I might be making and question them.
2.  **Identify Edge Cases:** Propose 5-10 edge cases or error states we haven't considered (e.g., network failure, empty states, invalid inputs, concurrent edits).
3.  **Expand Non-Functional Requirements:** Propose specific, measurable non-functional requirements for:
    *   Performance (e.g., "95% of operations complete under 200ms")
    *   Security (e.g., "Only document owners can invite collaborators")
    *   UX (e.g., "Should provide clear feedback when saving")
4.  **Update the PRD:** Integrate these insights and produce a revised, more robust version.
```

**3. Validation & Finalization Prompt:**
```markdown
Now, validate this PRD for completeness. Generate a final checklist for me to review:

- [ ] Are all `[NEEDS CLARIFICATION]` markers resolved?
- [ ] Is every requirement testable and unambiguous?
- [ ] Are success criteria measurable?
- [ ] Have all user personas been considered?
- [ ] Are key edge cases covered in the acceptance criteria?

Once validated, output the final, canonical `spec.md` document.
```

---

### **Phase 2: PLAN - The "How" Pipeline**
**Goal:** Translate the validated PRD into a technical implementation plan.
**LLM Persona:** Solution Architect / Tech Lead

#### **Workflow & Context Guidelines:**
1.  **Provide the Constitution:** The LLM *must* have architectural guidelines to follow. Paste your core principles (Simplicity, Library-First, etc.) into the chat first.
2.  **Provide the SPEC:** Always provide the finalized `spec.md` from Phase 1.
3.  **Demand Rationale:** Every technology choice must be justified against the spec and the constitution.

#### **Core Prompts:**

**1. Architecture & Planning Prompt:**
```markdown
**Role:** You are a pragmatic Solution Architect who values simplicity and maintainability.

**Constitutional Principles to Follow:** [Paste your principles here, e.g., "1. Library-First Principle 2. Prefer simplicity over cleverness 3. Maximum of 3 initial components..."]

**Input:** Here is the finalized PRD: `[Paste the final spec.md content here]`

**Instruction:** Create a high-level implementation plan (`plan.md`) based on the PRD. Your plan must include:

1.  **Technical Approach:** The overall architecture (e.g., "Client-side CRDTs with a WebSocket sync server").
2.  **Component Diagram:** Describe the main libraries/services and their responsibilities. (Keep it to ≤3 core components as per our principles).
3.  **Technology Choices:** Recommend specific libraries/frameworks (e.g., "Use Yjs for CRDTs"). For each choice, provide a **one-sentence rationale** linking it to a requirement (e.g., "Yjs is chosen because its CRDT model supports offline-first collaboration, fulfilling user story #2").
4.  **Data Model:** Outline the key entities and their relationships (e.g., `Document`, `User`, `EditSession`).
5.  **API/Interface Contracts:** Define the core methods/functions for the main components (e.g., `documentCollaborator.inviteUser(userId, permissionLevel)`).

**Remember:** This is a plan, not code. Keep it high-level and readable.
```

**2. Research & Validation Prompt:**
```markdown
Now, act as a research agent. For the key technology choices you proposed ([e.g., Yjs, Socket.IO, PostgreSQL JSONB]).

1.  **Compare:** Briefly compare the top 2 contenders for each choice.
2.  **Identify Risks:** What are the potential risks, limitations, or compatibility issues with our existing stack?
3.  **Propose Alternatives:** What is a simpler, more boring alternative if our primary choice fails?
4.  **Output a summary** in a `research.md` format.
```

**3. Compliance Gate Check Prompt:**
```markdown
Review your implementation plan against our architectural constitution.

**Simplicity Gate:**
- [ ] Are we using ≤3 main components? If not, justify the complexity.
- [ ] Are we avoiding future-proofing or over-engineering?

**Anti-Abstraction Gate:**
- [ ] Are we using frameworks directly instead of wrapping them?

**Test-First Gate:**
- [ ] What are the key integration contracts that must be tested first?

**Output a revised plan** that either passes these gates or has a justified "Complexity Tracking" section explaining why a gate must be violated.
```

---

### **Phase 3: TASKS - The "Do" Pipeline**
**Goal:** Decompose the technical plan into an executable task list for developers or AI coding agents.
**LLM Persona:** Tech Lead or Senior Developer

#### **Workflow & Context Guidelines:**
1.  **Provide All Artifacts:** The LLM needs the `spec.md`, `plan.md`, and `data-model.md` to create coherent tasks.
2.  **Focus on Actionability:** Tasks must be clear, discrete, and atomic.

#### **Core Prompt:**

**Task Derivation Prompt:**
```markdown
**Role:** You are a senior developer breaking down a project plan into executable tasks.

**Inputs:**
- **PRD (The What):** `[Paste final spec.md]`
- **Technical Plan (The How):** `[Paste final plan.md]`
- **Data Model:** `[Paste data-model.md]`

**Instruction:** Analyze these documents and generate a comprehensive task list (`tasks.md`). The rules are:
1.  **Derive from Contracts:** Create tasks from the API/Interface definitions in the plan (e.g., "Implement `DocumentService.save()` endpoint").
2.  **Mark Parallelization:** Identify tasks that can be worked on independently and mark them with `[P]`.
3.  **Order Logically:** Structure the list in a logical implementation order (e.g., data models first, then APIs, then UI components).
4.  **Include Testing:** Every feature task must have corresponding tasks for writing unit, integration, and/or contract tests. Assume Test-Driven Development (TDD).
5.  **Be Specific:** Tasks should be clear and action-oriented. Bad: "Make the backend." Good: "Create PostgreSQL `documents` table with columns: id (uuid), title (text), content (text), owner_id (uuid)."

**Output a task list** in a clear markdown checklist format.
```

---

### **Phase 4: IMPLEMENT - The "Build" Pipeline**
**Goal:** To generate code that strictly adheres to the tasks, plans, and specs.
**LLM Persona:** Senior Developer Pair Partner

#### **Workflow & Context Guidelines:**
1.  **ONE TASK AT A TIME:** This is the most critical rule. Never ask the LLM to implement the whole project. Feed it one task from the `tasks.md` list.
2.  **Provide Full Context:** For each task, provide the LLM with the relevant context from the spec, plan, and existing code.
3.  **TDD Mandatory:** For implementation tasks, always require tests first.

#### **Core Prompts:**

**1. Test-First Implementation Prompt (For a Single Task):**
```markdown
**Role:** You are a senior developer practicing strict Test-Driven Development (TDD).

**Current Task:** `[Paste a single, specific task from the tasks.md list, e.g., "Implement `Document.inviteUser(userId)` method"]`

**Relevant Context:**
- **Project Spec:** `[Link to/reference the relevant part of spec.md]`
- **Technical Contract:** `[Paste the exact API/interface definition from plan.md]`
- **Existing Code:** `[Paste the relevant existing code file(s) that this task will affect, if any]`

**Instruction:**
1.  **Red Phase:** Write the failing unit tests for this specific task. The tests must validate the contract and requirements from the spec.
2.  **Green Phase:** Write the minimal implementation code required to make the tests pass.
3.  **Output:** Show me the complete code for both the test file and the implementation file. Do not write any code for other tasks.
```

**2. Code Review & Refinement Prompt:**
*(After receiving the code)*
```markdown
Now, act as a code reviewer. Review the generated code for this task against our principles:
1.  **Simplicity:** Is it as simple as possible?
2.  **Clarity:** Is it easy to read and understand?
3.  **Adherence to Spec:** Does it correctly implement the acceptance criteria?
4.  **Consistency:** Does it follow the existing codebase's style and patterns?

Provide a bulleted list of feedback. Then, output a refined version of the code incorporating this feedback.
```

### **The Iterative Loop: Updating Artifacts**

Change is constant. The process isn't linear. When a requirement changes:

1.  **Go back to the SPEC (`spec.md`).** Update it using the **Specify-phase** prompts.
2.  **Regenerate the PLAN (`plan.md`).** Feed the updated spec to the **Plan-phase** prompts. The LLM will identify impacted areas.
3.  **Update the TASKS (`tasks.md`).** Feed the new spec and plan to the **Task-phase** prompt to see what tasks are added, changed, or removed.
4.  **IMPLEMENT** the new tasks one-by-one.

This ensures **`spec.md` remains the single source of truth**, and changes are propagated systematically through the entire stack, maintaining alignment and drastically reducing technical debt. This is the essence of "Intent is the source of truth."
