# STEP 6.1 — SERVICE & APPLICATION ARCHITECTURE

## 1. Step definition

**Objective:** Define the service boundaries and application architecture required to realise the approved domain design.

This step translates Strategic DDD and Tactical DDD into a logical service and application structure. It determines which business capabilities belong together, which require independent service boundaries, and what application components are needed to implement them.

The objective is not to create one microservice per bounded context. The objective is to establish the simplest service architecture that preserves business ownership, consistency boundaries and required operational independence.

## 2. Crew

Exactly one agent must fulfil each role.

### Manager — Service Architecture Manager
- Owns the execution and completion of this step.
- Establishes the scope and ensures the correct input artefacts are available.
- Coordinates the Interviewer, Author and Reviewer.
- Ensures questions and architectural decisions are resolved or recorded.
- Evaluates the review findings.
- Accepts the output, requests revision or rejects it.
- Does not delegate work to other agents.

### Interviewer — Service Architecture Requirements Interviewer
- Examines the inputs for missing or ambiguous information affecting service boundaries.
- Identifies unclear business ownership, consistency requirements, lifecycle requirements and operational constraints.
- Requests human clarification where a decision materially affects the architecture.
- Records unanswered questions, assumptions and their impact.
- Does not invent requirements or make unsupported architecture decisions.

### Author — Service & Application Architecture Designer
- Performs the service and application architecture analysis.
- Defines service boundaries, ownership and responsibilities.
- Defines the application components required within each service.
- Produces all artefacts required by this step.
- Does not delegate work.

### Reviewer — Service Architecture Challenger
- Independently evaluates the proposed service and application architecture.
- Challenges unnecessary service boundaries, excessive distribution and unsupported technology choices.
- Checks alignment with Strategic DDD, Tactical DDD and business requirements.
- Identifies missing ownership, consistency and implementation responsibilities.
- Provides findings to the Manager and Author; does not silently rewrite the Author's work.

## 3. Inputs

Use the latest approved versions of:

1. Consolidated Event Storming output from Step 3.
2. Strategic DDD output from Step 4:
   - Domains and subdomains.
   - Bounded contexts.
   - Context map.
   - Ubiquitous language.
   - Strategic decisions and ADRs.
   - Open questions.
3. Tactical DDD output from Step 5:
   - Aggregates and aggregate boundaries.
   - Entities and value objects.
   - Invariants and business rules.
   - Domain services and policies.
   - Domain events and commands.
   - Consistency boundaries.
4. User journeys and requirements from Step 1.
5. NFRs and technical constraints.
6. Existing service catalogue, application architecture, platform capabilities and approved technology standards, where available.

Do not treat an unavailable input as an established fact.

## 4. Scope of work

### 4.1 Establish service boundaries

For each proposed service or deployable application:

- Identify its business responsibility.
- Map the bounded contexts and aggregates it owns.
- Identify the invariants it must enforce.
- Establish its data ownership at a logical level.
- Identify its role in the relevant business journeys.
- Identify dependencies on other services.
- Determine whether independent deployment, scaling, security or operational lifecycle is necessary.

Do not mechanically map one bounded context to one microservice.

Consider whether multiple bounded contexts can remain within one deployable application when ownership, consistency and lifecycle permit it.

Split a service only when there is a meaningful justification, such as:
- Independent business ownership.
- Different lifecycle or release requirements.
- Independent scaling or resource requirements.
- A distinct security or trust boundary.
- Materially different operational responsibilities.
- A consistency boundary that requires separation.

### 4.2 Define service responsibilities

For each service, define:

- Stable service ID and name.
- Purpose and business capabilities.
- Bounded contexts and aggregates owned.
- Responsibilities included.
- Responsibilities explicitly excluded.
- Business operations handled.
- Domain events produced or consumed internally.
- Dependencies and relationships.
- Ownership of business state.

Do not define detailed API or integration-event contracts in this step. Those belong to Step 6.2.

### 4.3 Define application architecture

For each service, determine the application components actually required.

Consider, where justified:
- Interface/API layer.
- Application/use-case layer.
- Domain layer.
- Persistence adapters.
- External integration adapters.
- Messaging publishers and consumers.
- Background processing.
- Scheduled processing.

Do not mandate every component for every service.

Prefer established application patterns and approved technology standards. Do not introduce a new framework or architectural style without a concrete reason.

### 4.4 Evaluate architectural alternatives

For each material service boundary or application decision, record:

- Selected design.
- Simplest viable alternative.
- Reason for selection.
- Consequences and operational cost.
- Conditions that would justify revisiting the decision.

## 5. Frugal Architect decision rules

Apply these rules throughout:

- Prefer a modular application over multiple services when distribution is not justified.
- Do not create a service solely because a bounded context exists.
- Do not create a separate application for batch processing, reporting or integration without a lifecycle or operational justification.
- Do not introduce generic shared services for hypothetical reuse.
- Reuse existing platform capabilities before proposing new ones.
- Do not introduce technology choices that are not supported by requirements or approved standards.
- Preserve aggregate ownership and business invariants.
- Make uncertainty visible rather than resolving it through invention.

For every significant decision, ask:
1. What requirement justifies this?
2. What is the simplest alternative?
3. What happens if we defer it?
4. What additional operational cost does it create?
5. Can it be introduced later without major rework?

## 6. Required outputs

Create only the following artefacts, using the project's existing conventions.

### `service-architecture.md`
- Architecture overview.
- Service catalogue and responsibilities.
- Bounded context and aggregate allocation.
- Ownership and dependencies.
- Service boundary decisions.
- Alternatives and trade-offs.
- Assumptions and open questions.

### `application-architecture.md`
- Application structure for each service.
- Required components and responsibilities.
- Application archetypes.
- Technology choices and constraints.
- External and platform dependencies.

### `service-architecture.yaml`
Machine-readable representation of:
- Service IDs and names.
- Responsibilities.
- Bounded contexts.
- Aggregate ownership.
- Dependencies.
- Application components.
- Decision references.

### `architecture-decisions.md`
Only material decisions introduced by this step. Follow existing ADR conventions and do not duplicate existing decisions.

## 7. Validation and review

The Reviewer must independently verify:

- Service boundaries are justified by business or operational needs.
- Bounded contexts and aggregates have explicit ownership.
- Business invariants remain enforceable.
- No unnecessary service or application has been introduced.
- Dependencies are understood.
- Application components are sufficient but not excessive.
- The design aligns with approved domain decisions and user journeys.
- Material decisions have traceability.

The Reviewer must classify findings as:
- Critical.
- Major.
- Minor.
- Improvement suggestion.

The Manager evaluates the findings and decides whether to accept, request revision or reject the output.

## 8. Completion and handover

The step is complete when:

- Service and application responsibilities are explicit.
- Relevant bounded contexts and aggregates are allocated.
- Material boundary decisions are justified.
- Open questions and assumptions are recorded.
- Reviewer findings have been addressed or explicitly accepted.
- The Manager has accepted the step output.

**Handover to Step 6.2 and Step 6.3:**

Provide the approved service catalogue, service responsibilities, ownership boundaries, application structure, dependencies and relevant decisions.

Steps 6.2 and 6.3 may execute in parallel using this output.

## 9. Execution discipline

Do not:
- Generate API or event specifications.
- Define detailed physical database schemas.
- Design deployment topology.
- Generate implementation tasks or source code.
- Change approved DDD decisions without an explicit change request.

Keep the output focused on service and application architecture.