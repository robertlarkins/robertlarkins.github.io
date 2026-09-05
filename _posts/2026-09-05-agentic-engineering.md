 # AI-Assisted & Agentic Software Development

 A practical reference for using AI to improve software development productivity, while developing the skills and practices needed for AI-native / agentic software engineering.

---

 ## 1\. Core Idea

 The goal is not simply:

 > "Use AI to write code faster."

 A more useful goal is:

 > **Design a development environment in which AI can reliably turn human intent into quality software, with appropriate constraints, feedback and human oversight.**

 The developer's role shifts from producing every line of code toward:

 - defining intent
- specifying behaviour
- designing architecture
- providing context
- decomposing problems
- supervising agents
- designing verification mechanisms
- reviewing results
- making decisions where judgement is required

 The valuable skill is therefore not primarily knowing how to prompt an AI model.

 It is knowing **how to engineer the environment around an AI agent**.

---

 # 2\. Evolution of AI-Assisted Development

 AI-assisted development can be viewed as a progression.

 ## Level 1 — AI assistance

```
Human → AI suggestion → Human writes/reviews code
```

 Examples:

 - autocomplete
- code generation
- explanations
- debugging suggestions
- documentation generation

 The human remains firmly in control.

---

 ## Level 2 — AI pair programming

```
Human → AI → code → Human review → next task
```

 The AI can produce larger pieces of code, but the human drives the workflow.

---

 ## Level 3 — Agentic development

```
Human
  ↓
Goal / Specification
  ↓
Agent
  ↓
Inspect repository
  ↓
Plan
  ↓
Implement
  ↓
Run tests
  ↓
Diagnose failures
  ↓
Fix
  ↓
Repeat
  ↓
Human review
```

 The agent is given tools and permission to perform multiple development actions autonomously.

---

 ## Level 4 — Autonomous engineering workflows

```
Issue
  ↓
Specification
  ↓
Planning
  ↓
Implementation
  ↓
Automated verification
  ↓
AI review
  ↓
Fixes
  ↓
PR
  ↓
Human approval
```

 The human increasingly becomes responsible for **intent, constraints and acceptance**, while the agent performs much of the implementation work.

---

 # 3\. The Agent Harness

 The **harness** is the environment in which an AI coding agent operates.

 It includes:

 - repository structure
- instructions
- specifications
- architecture documentation
- tools
- permissions
- tests
- linters
- compilers
- CI
- databases
- development environments
- feedback mechanisms
- Git history
- review processes

 The objective is:

 > **Make it easy for an agent to do the right thing and difficult for it to silently do the wrong thing.**

 A good harness can be more important than a clever prompt.

---

 # 4\. Context Engineering

 Rather than relying entirely on prompts, important project knowledge should live in durable, machine-readable artefacts.

 For example:

```
project/
├── AGENTS.md
├── README.md
│
├── docs/
│   ├── architecture.md
│   ├── domain-model.md
│   └── decisions/
│       ├── 001-database.md
│       └── 002-authentication.md
│
├── specs/
│   ├── authentication.md
│   ├── subscriptions.md
│   └── reporting.md
│
├── src/
└── tests/
```

 This creates **portable context**.

 If the AI tool or model changes, the project's accumulated knowledge remains in the repository.

---

 # 5\. Specifications

 A specification describes **what correct behaviour means**.

 It is different from a conventional work item or Jira ticket.

 ### Work item / ticket

 Answers:

 > What do we want to accomplish?

 Example:

 > As a user, I want to import my transactions.

 ### Specification

 Answers:

 > What exactly does correct behaviour look like?

 A specification might contain:

```
# Transaction Import

## Goal

Allow users to import transactions from CSV files.

## Requirements

- Accept CSV files up to 10 MB.
- Validate required columns.
- Reject malformed transactions.
- Do not create duplicate transactions.

## Behaviour

...

## Error handling

...

## Acceptance criteria

- ...
- ...
- ...

## Non-goals

- XLSX support
- OFX support
```

 The specification becomes a **contract between intent and implementation**.

---

 # 6\. Are Specifications Permanent?

 Not every task needs a permanent specification.

 Distinguish between:

 ### Ephemeral tasks

 Examples:

 - Rename a method.
- Add a loading spinner.
- Refactor a private class.

 These can simply be tasks that disappear into Git history once completed.

 ### Durable behaviour

 Examples:

 - Imports must be idempotent.
- Users cannot access another user's data.
- Monetary values must preserve currency and precision.
- An API operation must be safe to retry.

 These are valuable as permanent specifications.

 ### Architectural decisions

 Examples:

 - Why PostgreSQL was chosen.
- Why a particular authentication architecture is used.
- Why a particular messaging pattern was selected.

 These should generally be retained as architectural decision records.

---

 # 7\. Specification vs Source Code vs Tests

 These artefacts answer different questions.

```
Specification
     │
     │ defines intended behaviour
     ▼
Source code
     │
     │ implements behaviour
     ▼
Tests
     │
     │ verify behaviour
     ▼
CI / verification
```

 ### Specification

 > What should the system do?

 ### Source code

 > What does the system currently do?

 ### Tests

 > What behaviour are we automatically checking?

 The source code should not automatically replace the specification.

 The specification represents **intent**.

 The code represents **implementation**.

 Tests provide **executable verification**.

---

 # 8\. Specifications Should Evolve

 A specification should normally describe the **current desired behaviour**.

 If a requirement changes:

```
10 MB → 50 MB
```

 update the specification.

 Git provides the historical record.

 The repository therefore contains:

 - current intent in the specification
- current implementation in the code
- executable expectations in tests
- historical evolution in Git

 This is generally preferable to maintaining many obsolete specification documents.

---

 # 9\. The Basic Agentic Development Loop

 A useful default workflow is:

```
1. Understand the task
2. Read the relevant specification
3. Inspect the existing codebase
4. Identify ambiguities
5. Create a plan
6. Review/approve the plan
7. Implement
8. Run tests
9. Diagnose failures
10. Fix
11. Repeat
12. Review the diff
13. Verify acceptance criteria
```

 The critical idea is the **loop**.

 Do not optimise only for getting a good first response.

 Optimise for giving the agent a reliable mechanism for discovering and correcting mistakes.

---

 # 10\. Feedback Loops

 The quality of an agentic system is strongly influenced by the quality of its feedback.

 A useful loop is:

```
             ┌──────────────┐
             │ Specification│
             └──────┬───────┘
                    ↓
                 Agent
                    ↓
                  Code
                    ↓
        ┌───────────┼───────────┐
        ↓           ↓           ↓
     Compiler     Tests       Linter
        │           │           │
        └───────────┼───────────┘
                    ↓
                 Result
                    ↓
              Agent diagnoses
                    ↓
                Agent fixes
                    │
                    └─────── loop
```

 A key principle:

 > **Don't just ask the agent to be correct. Give it mechanisms that allow it to discover when it is incorrect.**

---

 # 11\. Verification

 Agent-generated code should generally have multiple layers of verification.

 Depending on the project:

 - compiler
- type checker
- unit tests
- integration tests
- end-to-end tests
- static analysis
- linters
- security analysis
- database constraints
- API contract tests
- browser tests
- CI
- human review
- independent AI review

 The more autonomous the agent becomes, the more important automated verification becomes.

---

 # 12\. AI Code Review

 A useful advanced pattern is using a second agent as an independent reviewer.

```
Specification
     ↓
Implementation Agent
     ↓
Code
     ↓
Tests
     ↓
Review Agent
     ↓
Findings
     ↓
Implementation Agent
     ↓
Fix
     ↓
Tests
```

 The reviewer should ideally evaluate against explicit criteria rather than personal stylistic preferences.

 For example:

 > Review this implementation against the specification. Identify unmet requirements, incorrect assumptions, missing edge cases, security issues, data integrity issues, concurrency problems and insufficient tests.

 The specification acts as an **oracle for the review**.

---

 # 13\. Multi-Agent Development

 Multiple agents can eventually be given different responsibilities.

 For example:

```
                    Specification
                         │
          ┌──────────────┼──────────────┐
          ↓              ↓              ↓
    Design Agent    Implementation   Test Agent
                         Agent
          │              │              │
          └──────────────┼──────────────┘
                         ↓
                    Review Agent
```

 Possible roles include:

 - planner
- implementer
- tester
- reviewer
- security reviewer
- documentation agent
- research agent

 However:

 > **Do not introduce multiple agents simply because you can.**

 First establish that a single agent can reliably complete tasks.

 Multi-agent orchestration adds complexity and coordination overhead.

---

 # 14\. Tool Use and MCP

 An agent becomes considerably more capable when it can interact with development tools rather than merely edit files.

 Potential tools include:

```
GitHub
 ├── Issues
 ├── Pull requests
 └── CI

Database
 └── Development database

Browser
 └── Application verification

Documentation
 └── Project knowledge

Cloud
 └── Development environment
```

 MCP and similar tool protocols can provide structured access to external systems.

 Introduce tools incrementally.

 Start with local development tools and read-only access where practical.

---

 # 15\. Browser / Full-Stack Agentic Loops

 For full-stack SaaS development, automated browser verification can extend the loop beyond backend tests.

 Example:

```
Specification
     ↓
Implement
     ↓
Start application
     ↓
Open browser
     ↓
Authenticate
     ↓
Perform workflow
     ↓
Verify result
     ↓
Failure?
   ↙       ↘
 Yes       No
  ↓         ↓
Fix       Complete
  │
  └──→ repeat
```

 This allows an agent to verify behaviour from the user's perspective rather than only through unit or integration tests.

---

 # 16\. Security and Permissions

 Agentic development introduces a fundamentally different security model.

 There is a major difference between:

```
AI suggests code
```

 and:

```
AI can execute shell commands
```

 and:

```
AI can access credentials and external services
```

 and:

```
AI can deploy to production
```

 Treat an autonomous coding agent as an entity with meaningful system privileges.

 Prefer:

```
Agent
  ↓
Sandbox / development environment
  ↓
Local repository
  ↓
Tests
```

 before progressing toward:

```
Agent
  ↓
Production credentials
  ↓
Production systems
```

 Use least privilege wherever possible.

---

 # 17\. Local vs Cloud Models

 There are three broad approaches.

 ## Cloud-first

 Use a powerful hosted model for most tasks.

 Advantages:

 - highest capability
- large context
- minimal infrastructure
- easy experimentation

 Disadvantages:

 - ongoing cost
- source code leaves the machine
- vendor dependence

 ## Local-first

 Run open models locally.

 Advantages:

 - low marginal cost
- privacy
- experimentation
- no API dependency

 Disadvantages:

 - hardware requirements
- potentially weaker models
- more configuration

 ## Hybrid

 Use:

```
Simple task → local / inexpensive model

Complex task → frontier cloud model
```

 This is potentially an attractive long-term architecture.

---

 # 18\. Model Routing

 Not every task needs the most capable model.

 A possible strategy:

```
                   Task
                     │
                Classifier
                /         \
               /           \
        Simple task      Complex task
             ↓                ↓
        Cheap model      Strong model
```

 Use inexpensive models for:

 - documentation
- simple tests
- straightforward transformations
- explanations
- boilerplate

 Use stronger models for:

 - architecture
- difficult debugging
- unfamiliar codebases
- large refactoring
- complex agentic tasks

 Measure actual results rather than assuming the most expensive model is always best.

---

 # 19\. Measure Cost per Successful Outcome

 Subscription price is not the only useful metric.

 Track things such as:

```
Task:
Model:
Agent:
Cost:
Human time:
Agent iterations:
Tests passed:
Manual fixes:
Quality:
```

 A more meaningful metric is:

 > **Cost per successfully completed feature**

 rather than:

 > Cost per month.

 A more expensive model can be cheaper overall if it substantially reduces human intervention.

---

 # 20\. Context Is More Valuable Than Prompt Cleverness

 Prefer durable project context over increasingly complicated prompts.

 Useful context includes:

 - specifications
- architecture
- coding standards
- domain rules
- ADRs
- examples
- tests
- repository structure
- CI rules
- acceptance criteria

 The prompt can then be relatively simple:

 > Implement the specified behaviour according to the repository's engineering guidelines. Run the relevant tests and iterate until the acceptance criteria are satisfied.

 The repository supplies the context.

---

 # 21\. AGENTS.md / Repository Instructions

 A repository-level instruction file can encode the project's development conventions.

 Example:

```
# Project Instructions

## Before coding

1. Read the relevant specification.
2. Inspect the existing implementation.
3. Search for existing patterns before creating abstractions.
4. Identify ambiguities.

## Architecture

- Follow the existing architecture.
- Do not introduce new frameworks without justification.
- Respect dependency boundaries.

## C#

- Nullable reference types are enabled.
- Treat warnings as errors.
- Prefer asynchronous APIs for I/O.

## Testing

- New behaviour requires automated tests.
- Prefer integration tests for API behaviour.
- Run the complete test suite before completion.

## Completion

A task is not complete until:

- the project builds
- tests pass
- static analysis passes
- acceptance criteria are satisfied
- the Git diff has been reviewed
```

 The exact format is less important than the principle:

 > **Encode repeatable engineering knowledge in the environment rather than repeatedly explaining it to the agent.**

---

 # 22\. AI-Native Software Engineering

 A useful way to think about the emerging discipline is:

```
Traditional engineering
        ↓
Requirements
        ↓
Design
        ↓
Implementation
        ↓
Testing
        ↓
Review

AI-native engineering
        ↓
Intent
        ↓
Specification
        ↓
Context / Harness
        ↓
Agent planning
        ↓
Agent implementation
        ↓
Automated verification
        ↓
Agent iteration
        ↓
Independent review
        ↓
Human judgement
```

 The human is not removed.

 The human's role moves upward toward:

 - intent
- architecture
- constraints
- prioritisation
- judgement
- validation

 while more implementation work becomes delegatable.

---

 # 23\. What to Learn

 For someone wanting to become effective at AI-native development, useful areas include:

 1. **Agentic coding**
2. **Specification-driven development**
3. **Context engineering**
4. **Agent harness design**
5. **Tool use**
6. **MCP**
7. **Automated verification**
8. **AI code review**
9. **Subagents**
10. **Agent loops**
11. **Browser-based verification**
12. **Model routing**
13. **Local models**
14. **AI security and permissions**
15. **Evaluation / measuring agent quality**
16. **Cost management**

 The goal isn't to master every tool.

 The goal is to understand the **underlying patterns**, so that the tools can change without invalidating your knowledge.

---

 # 24\. Recommended Progression

 A sensible progression is:

 ## Stage 1 — Single agent

```
Spec
 ↓
Agent
 ↓
Code
 ↓
Tests
 ↓
Fix
```

 Learn how to delegate effectively.

 ## Stage 2 — Better harness

 Add:

 - repository instructions
- architecture documentation
- specifications
- tests
- scripts
- CI

 ## Stage 3 — Autonomous tasks

 Move toward:

 > "Implement this specification and produce a PR."

 ## Stage 4 — Independent review

 Add a separate review agent.

 ## Stage 5 — Tool integration

 Add:

 - GitHub
- databases
- browsers
- documentation
- development environments

 ## Stage 6 — Multi-agent workflows

 Experiment with:

 - planners
- implementers
- reviewers
- test agents

 ## Stage 7 — Optimisation

 Experiment with:

 - model routing
- local models
- cost optimisation
- parallel agents
- automated evaluation

---

 # 25\. Practical Principles

 ### Principle 1

 **Specify outcomes, not implementation details, where possible.**

 Give the agent room to solve the problem.

 ### Principle 2

 **Make acceptance criteria explicit.**

 The agent needs to know what "done" means.

 ### Principle 3

 **Give the agent feedback mechanisms.**

 Tests, compilers and linters are tools for the agent, not merely tools for the developer.

 ### Principle 4

 **Keep durable knowledge in the repository.**

 Don't rely on conversational history.

 ### Principle 5

 **Separate intent from implementation.**

 Specifications describe desired behaviour; code implements it.

 ### Principle 6

 **Prefer small autonomous loops before large autonomous systems.**

 Reliability comes before complexity.

 ### Principle 7

 **Use human review for judgement, not for tasks that can be deterministically verified.**

 ### Principle 8

 **Treat agents as privileged software.**

 Give them only the access they need.

 ### Principle 9

 **Measure outcomes.**

 Track human time, quality, iterations and cost.

 ### Principle 10

 **Stay vendor-neutral.**

 Learn principles that survive changes in models and AI coding tools.

---

 # 26\. A Useful Mental Model

 The most useful overall model is:

```
                    HUMAN
                      │
                 Intent / Goal
                      │
                      ▼
                SPECIFICATION
                      │
                      ▼
              AGENT / HARNESS
                      │
          ┌───────────┼───────────┐
          │           │           │
       Context      Tools      Constraints
          │           │           │
          └───────────┼───────────┘
                      │
                      ▼
                 IMPLEMENTATION
                      │
                      ▼
                 VERIFICATION
                      │
              ┌───────┴───────┐
              │               │
            PASS            FAIL
              │               │
              │               ▼
              │             AGENT
              │               │
              │             FIX
              │               │
              │               └──────┐
              │                      │
              └──────────────────────┘
                      │
                      ▼
                 AI / HUMAN
                   REVIEW
                      │
                      ▼
                    PR
                      │
                      ▼
              HUMAN APPROVAL
```

 The fundamental loop is:

 > **Intent → Specification → Agent → Verification → Feedback → Iteration → Human judgement**

 That is the core pattern to keep in mind as the terminology, models and tools evolve.

 This should work well as a durable `AI-Agentic-Development.md` reference. The key thing I'd preserve is the distinction between **tickets (what/why), specs (what correct means), code (implementation), and tests (executable verification)**—that distinction becomes increasingly important as you delegate more work to agents.
