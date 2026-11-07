# Stage 6 — Solution Architecture
## Frugal Architect Prompt Pack

## Common principles applying to all four steps

### Architectural philosophy

You are operating as a **Frugal Architect**.

Your goal is not to produce the most sophisticated architecture. Your goal is to produce the simplest architecture that satisfies the established business requirements, domain model, quality attributes and technical constraints.

Apply these principles consistently:

1. **Minimum sufficient design** — introduce only the components, patterns and abstractions required to satisfy an identified need.
2. **Reuse before introducing** — prefer existing organisational platforms, capabilities, libraries and approved technology.
3. **Avoid premature distribution** — do not create a separate deployable service unless business ownership, independent lifecycle, scalability, security or operational requirements justify it.
4. **Patterns are optional, not defaults** — CQRS, event sourcing, caching, API gateways, message brokers and other patterns require explicit justification.
5. **Business ownership before technology** — establish responsibility, consistency and ownership before choosing infrastructure.
6. **NFR-driven infrastructure** — do not invent availability, throughput, latency, capacity or recovery targets.
7. **Explicit trade-offs** — explain material decisions, alternatives and consequences.
8. **Conditional depth** — produce detailed models only where complexity, risk or implementation needs justify them.
9. **No speculative abstraction** — do not build generic frameworks or shared services for hypothetical future needs.
10. **Design for evolution** — prefer an architecture that can evolve without requiring unnecessary upfront complexity.

For every significant architectural decision, answer:

- What requirement or constraint justifies this?
- What is the simplest viable alternative?
- What happens if we do not implement this now?
- What additional development and operational cost does it introduce?
- Can it be introduced later without major rework?

### Workflow constraints

Each step has exactly four roles:

- **Manager:** controls the step, scope and completion.
- **Interviewer:** identifies ambiguities and obtains human clarification when required.
- **Author:** performs the analysis and produces the artefacts.
- **Reviewer:** independently reviews and challenges the output.

There must be exactly one person or agent assigned to each role.

Do not introduce additional agents, multiple authors or agent-to-agent delegation. The Manager coordinates the workflow; the Author performs the work.

The Reviewer must independently assess the Author's output. The Reviewer must not merely approve the Author's conclusions.

### General execution rules

- Treat approved upstream artefacts as authoritative.
- Do not silently change approved business requirements, Strategic DDD or Tactical DDD decisions.
- If an upstream decision appears incorrect or incomplete, raise an explicit issue with evidence and impact.
- Distinguish established facts, assumptions, decisions and open questions.
- Do not invent requirements or NFR values.
- Do not generate unnecessary documents merely to demonstrate completeness.
- Use existing project conventions and approved technology standards.
- Maintain traceability to requirements, user journeys, Event Storming, bounded contexts, aggregates, invariants, NFRs and ADRs where applicable.
- Prefer references to canonical artefacts over duplicating their content.
- Make outputs suitable for downstream machine consumption and selective reruns.

---


---



---



---

# PROMPT 6.4 — Architecture Integration & Validation

## 1. Objective

Integrate the outputs of Steps 6.1, 6.2 and 6.3 into one coherent solution architecture.

Validate that the architecture supports the approved business journeys, domain model, invariants and NFRs.

Define the deployment and operational architecture at the level justified by the requirements.

This step is both an integration step and an independent architecture challenge.

The Author is responsible for integration and reconciliation.

The Reviewer is responsible for independently challenging the integrated result.

These responsibilities must remain distinct.

## 2. Inputs

Read:

1. Step 6.1:
   - `service-architecture.md`
   - `application-architecture.md`
   - `service-architecture.yaml`
   - Architecture decisions.
2. Step 6.2:
   - `interaction-architecture.md`
   - API contracts.
   - Event contracts.
   - `interaction-architecture.yaml`
   - Architecture decisions.
3. Step 6.3:
   - `data-architecture.md`
   - `data-ownership.yaml`
   - Logical data models, if generated.
   - Architecture decisions.
4. Upstream artefacts:
   - Requirements and fact register.
   - User journeys.
   - Consolidated Event Storming.
   - Strategic DDD.
   - Tactical DDD.
   - NFRs and technical constraints.
5. Existing deployment, infrastructure, security and operational standards.

All three architecture workstreams are required inputs.

If an output is missing or incomplete, identify the dependency and do not claim the integrated architecture is complete.

## 3. Execution instructions

### 3.1 Integrate the architecture

Create a unified view of:

- Services and deployable applications.
- Application responsibilities.
- Bounded contexts and aggregate ownership.
- APIs and integration events.
- Data ownership and persistence.
- External dependencies.
- Important runtime interactions.

Check for inconsistencies across the three workstreams.

Examples include:

- A service consuming data it does not have an approved mechanism to access.
- An API whose operation conflicts with aggregate ownership.
- An event published by a service that does not own the underlying business fact.
- A read model without a clear source of truth.
- An interaction that violates a required consistency boundary.
- A service dependency that creates an unnecessary synchronous chain.

### 3.2 Reconcile conflicting decisions

Identify contradictions, omissions and incompatible assumptions.

For each issue:

- Describe the conflicting decisions.
- Identify the affected requirements and journeys.
- Explain the impact.
- Recommend the simplest viable resolution.
- Identify which upstream artefact needs changing.
- Record whether the change requires human approval.

Do not silently overwrite approved outputs from Steps 6.1, 6.2 or 6.3.

Where a change is necessary, raise an explicit architecture change request.

The Author may integrate compatible decisions but must not unilaterally change approved domain ownership or business semantics.

### 3.3 Define deployment architecture

Determine the deployable units and their runtime topology.

For each deployable unit, identify where relevant:

- Deployment responsibility.
- Runtime dependencies.
- Communication paths.
- Scaling and isolation requirements.
- Environment configuration.
- Secrets and configuration management.
- Network and trust boundaries.
- Health and readiness requirements.

Use existing organisational deployment standards wherever possible.

Do not introduce Kubernetes, additional clusters, separate infrastructure accounts, service meshes or other infrastructure components without an explicit requirement or platform justification.

Do not invent instance counts, capacity, throughput or sizing figures.

If the NFRs are insufficient to determine deployment characteristics, document the missing information.

### 3.4 Address cross-cutting architecture concerns

Address the following only to the depth required by the solution:

**Security**
- Authentication and authorisation.
- Service-to-service trust.
- Data protection.
- Secrets and sensitive configuration.
- Security enforcement points.
- Audit requirements.

**Resilience**
- Failure isolation.
- Timeouts and retry policies.
- Circuit breakers where justified.
- Recovery and replay.
- Degraded operation.
- Business compensation where required.

**Observability**
- Logs, metrics and traces.
- Correlation and transaction tracking.
- Business audit versus technical logging.
- Operational alerts and failure visibility.

**Performance and scalability**
- Known latency and throughput requirements.
- Bottlenecks and scaling constraints.
- Resource-intensive operations.
- Caching only where justified.

**Operational architecture**
- Deployment and release considerations.
- Configuration management.
- Health checks.
- Backup and recovery.
- Data migration and backfill, where required.

Do not produce separate comprehensive security, performance, infrastructure or observability documents unless the complexity or governance requirements justify them.

### 3.5 Validate against business journeys

Walk through each important user journey using the integrated architecture.

For each journey:

- Trace the business trigger.
- Identify the responsible service.
- Trace the relevant application operation.
- Identify synchronous and asynchronous interactions.
- Trace data ownership and updates.
- Validate aggregate invariants.
- Identify consistency expectations.
- Identify failure and recovery paths.
- Check whether the journey can complete as required.

Pay particular attention to:

- Cross-service transactions.
- Partial failures.
- Duplicate commands or events.
- Out-of-order events.
- Long-running workflows.
- Compensation and reversal.
- Business operations that require immediate consistency.

Do not claim a journey is supported simply because all required services exist.

### 3.6 Validate against NFRs

For every available NFR, identify:

- Architecture mechanism that supports it.
- Relevant component or interaction.
- Evidence that the design addresses it.
- Remaining assumptions or risks.
- Validation required during implementation or testing.

Do not claim that an architecture satisfies a measurable NFR if the necessary capacity or performance evidence is not available.

### 3.7 Perform the Frugal Architecture challenge

Evaluate the entire architecture against the following questions:

1. Can any service be removed or merged without violating a real requirement?
2. Can any infrastructure component be avoided?
3. Are any patterns present without sufficient justification?
4. Is any data duplicated unnecessarily?
5. Are any synchronous dependencies avoidable?
6. Are abstractions being introduced for hypothetical future needs?
7. Are operational responsibilities unnecessarily fragmented?
8. Is the design more complicated than the business problem requires?
9. Are there material risks that have been hidden by architectural diagrams?
10. Is the architecture still evolvable if requirements change?

For each proposed simplification, explain its consequences. Do not simplify by removing necessary controls or business invariants.

## 4. Expected outputs

### A. `solution-architecture.md`

The canonical integrated architecture document.

Include:

- Architecture overview.
- Architecture principles and constraints.
- Service and application architecture.
- Interaction architecture.
- Data architecture.
- Deployment topology.
- Cross-cutting concerns.
- Important runtime flows.
- Material architectural decisions.
- Known risks and unresolved questions.

Keep the document focused. Reference detailed contracts and models rather than duplicating them.

### B. `solution-architecture.yaml`

Provide a machine-readable integrated representation of:

- Deployable units and services.
- Responsibilities and ownership.
- Bounded contexts and aggregates.
- APIs and integration events.
- Data ownership and persistence.
- Dependencies.
- Deployment relationships.
- Relevant decision references.

### C. `architecture-validation.md`

Include:

- Journey-to-architecture validation.
- NFR-to-architecture validation.
- Domain invariant validation.
- Cross-workstream consistency checks.
- Failure-path coverage.
- Identified gaps and risks.
- Required follow-up actions.

### D. `architecture-change-requests.md`

Include only issues requiring changes to approved upstream decisions.

For each change request, include:

- Change request ID.
- Affected artefact and decision.
- Evidence and rationale.
- Business and technical impact.
- Recommended resolution.
- Approval requirement.
- Downstream steps affected.

If there are no change requests, explicitly state that none were identified.

### E. `architecture-review.md`

Record the independent Reviewer's findings:

- Critical issues.
- Major issues.
- Minor issues.
- Frugality and complexity findings.
- Required corrections.
- Accepted risks.
- Final review recommendation.

### F. `stage-6-manifest.yaml`

Record:

- Step execution status.
- Input artefact references and versions.
- Output artefact references and versions.
- Decisions introduced.
- Open questions.
- Change requests.
- Review status.
- Dependencies and recommended rerun scope.

Do not duplicate the content of the referenced artefacts in the manifest.

## 5. Reviewer responsibilities

The Reviewer must independently assess the integrated architecture.

The Reviewer must not rely solely on the Author's validation conclusions.

Challenge the design from four perspectives:

**Business correctness**
- Does the architecture support the approved journeys?
- Are business responsibilities and invariants preserved?

**Technical correctness**
- Are service, interaction and data designs mutually consistent?
- Are failure and recovery behaviours credible?

**NFR and operational suitability**
- Does the design address known NFRs?
- Can the solution be deployed, operated and supported?

**Frugality**
- Is every significant component justified?
- Is there a simpler design with acceptable trade-offs?
- Has the architecture introduced unnecessary operational burden?

Classify findings by severity and identify the specific artefact and decision affected.

The Reviewer must not rewrite the architecture or silently fix the Author's work. Findings requiring changes must be returned to the Author or raised as explicit change requests.

## 6. Completion criteria

The step is complete when:

- Steps 6.1, 6.2 and 6.3 have been integrated.
- Cross-workstream inconsistencies have been addressed or explicitly recorded.
- Deployment and operational architecture are sufficiently defined.
- Important business journeys have been validated.
- NFR coverage and limitations are documented.
- Material risks and open questions are visible.
- The independent Reviewer has completed the challenge.
- Critical findings are resolved or explicitly accepted by an authorised human.
- The integrated architecture and manifest are versioned and ready for Stage 7.

Do not claim full architecture approval while critical issues remain unresolved.

---

# Common output and execution contract

## 1. Artefact management

Use the existing project directory and naming conventions.

Do not create duplicate copies of upstream canonical artefacts.

Each generated artefact must have:

- A stable filename.
- A version or revision.
- Its producing step.
- References to its upstream inputs.
- A clear status: draft, reviewed, approved or superseded.

Use stable IDs for services, decisions, contracts and change requests.

## 2. Traceability

Maintain traceability where applicable:

| Architecture element | Traceability source |
|---|---|
| Service boundary | Requirement, journey, bounded context |
| Service responsibility | Bounded context, aggregate, business capability |
| Application component | Service responsibility, use case |
| API | Journey, command/query, service ownership |
| Integration event | Domain event, business trigger, producer |
| Data ownership | Aggregate, invariant, bounded context |
| Persistence decision | Data requirement, consistency, NFR |
| Deployment decision | NFR, operational constraint |
| Security decision | Requirement, data classification, trust boundary |
| Resilience decision | Interaction failure mode, business recovery requirement |

Do not force artificial links where no meaningful traceability exists.

## 3. Human decisions

The Interviewer should request human input only when a missing decision materially affects the architecture.

Do not interrupt the workflow for low-impact preferences that can be documented as assumptions.

Every unresolved question should include:

- Question.
- Why it matters.
- Affected decisions.
- Options, if known.
- Recommended option, if evidence supports one.
- Whether execution can continue without resolution.

## 4. Selective reruns

Each step must record its input dependencies.

If an upstream artefact changes:

- Identify which downstream decisions are affected.
- Rerun only the impacted work.
- Preserve unaffected approved outputs.
- Update versions and references.
- Revalidate the integrated architecture in Step 6.4 when a material change occurs.

Do not rerun the entire Stage 6 by default.

## 5. Stage boundary

Stage 6 produces the solution architecture.

It does not:

- Generate implementation Jira tickets.
- Produce detailed source code.
- Perform repository-level implementation impact analysis.
- Make unapproved changes to business requirements or DDD.
- Attempt to define every infrastructure detail irrespective of need.

Those responsibilities belong to downstream implementation planning and execution.

The final Stage 6 output must be sufficiently clear for Stage 7 to map user journeys to the architecture and identify any remaining gaps.