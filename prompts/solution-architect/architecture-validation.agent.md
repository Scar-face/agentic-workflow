# STEP 6.4 — ARCHITECTURE INTEGRATION & VALIDATION

## 1. Step definition

**Objective:** Integrate the service, application, interaction and data architectures into one coherent solution architecture, then independently validate and challenge the result.

This step ensures that the architecture is internally consistent, supports the approved business journeys, preserves domain invariants and addresses the established NFRs.

It also defines deployment and cross-cutting architecture concerns at the level justified by the requirements.

This step has two distinct activities:

1. **Architecture integration:** reconcile and assemble the outputs from Steps 6.1, 6.2 and 6.3.
2. **Independent architecture challenge:** assess whether the integrated architecture is correct, sufficient and appropriately simple.

The Author performs integration. The Reviewer independently challenges the result.

## 2. Crew

Exactly one agent must fulfil each role.

### Manager — Solution Architecture Manager
- Owns execution and completion.
- Ensures all required upstream architecture outputs are available.
- Controls integration and validation scope.
- Coordinates clarification of conflicts and unresolved decisions.
- Evaluates independent review findings.
- Accepts, requests revision or rejects the integrated architecture.
- Does not delegate work.

### Interviewer — Architecture Integration Interviewer
- Identifies conflicts, gaps and ambiguous decisions across the architecture workstreams.
- Identifies missing deployment, operational, security and NFR information.
- Obtains human clarification when unresolved decisions materially affect the architecture.
- Records assumptions and the impact of unanswered questions.
- Does not silently resolve conflicts by inventing requirements.

### Author — Solution Architecture Integrator
- Integrates the approved architecture outputs.
- Reconciles compatible decisions and identifies conflicts.
- Defines deployment and cross-cutting concerns where justified.
- Validates journeys and NFR coverage.
- Produces the canonical solution architecture and supporting validation artefacts.
- Does not delegate work.

### Reviewer — Solution Architecture Challenger
- Independently evaluates the integrated architecture.
- Challenges business correctness, technical consistency, operational suitability and unnecessary complexity.
- Validates important journeys and failure paths independently of the Author's conclusions.
- Identifies material omissions and risks.
- Reports findings to the Manager and Author.
- Does not silently rewrite the architecture.

## 3. Inputs

Use the latest approved versions of:

### Stage 6 outputs
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
   - Logical data models, where generated.
   - Architecture decisions.

### Upstream outputs
4. Requirements and fact register from Step 1.
5. User journeys from Step 1.
6. Consolidated Event Storming from Step 3.
7. Strategic DDD from Step 4.
8. Tactical DDD from Step 5.
9. NFRs and technical constraints.
10. Existing deployment, infrastructure, security and operational standards.

If a required input is missing, identify the dependency. Do not claim the integrated architecture is complete.

## 4. Scope of work

### 4.1 Integrate the architecture

Create one coherent view of:

- Services and deployable applications.
- Application responsibilities.
- Bounded contexts and aggregate ownership.
- APIs and integration events.
- Data ownership and persistence.
- External dependencies.
- Important runtime interactions.

Ensure the integrated architecture references the canonical outputs of Steps 6.1, 6.2 and 6.3 rather than duplicating their detailed content.

### 4.2 Reconcile cross-workstream decisions

Identify contradictions, omissions and incompatible assumptions.

Examples:

- A service consumes data without an approved access mechanism.
- An API operation conflicts with aggregate ownership.
- An integration event is published by a service that does not own the underlying business fact.
- A read model has no clear source of truth.
- An interaction violates a consistency boundary.
- A synchronous dependency chain introduces unnecessary coupling.
- Deployment assumptions conflict with NFRs.

For each issue, record:

- Issue ID.
- Conflicting artefacts or decisions.
- Evidence and affected requirements or journeys.
- Business and technical impact.
- Recommended resolution.
- Artefacts requiring change.
- Approval requirement.
- Downstream impact.

Do not silently overwrite approved decisions from earlier steps.

The Author may reconcile compatible decisions but must raise an explicit change request when an approved decision must change.

### 4.3 Define deployment architecture

Determine deployable units and runtime topology.

Where justified, define:

- Deployable units.
- Runtime dependencies.
- Communication paths.
- Scaling and isolation requirements.
- Environment configuration.
- Secrets and configuration management.
- Network and trust boundaries.
- Health and readiness requirements.
- Deployment and release considerations.

Use existing organisational deployment standards.

Do not introduce additional infrastructure, clusters, service meshes or platform components without an explicit requirement or platform justification.

Do not invent capacity, instance counts, throughput or sizing figures.

### 4.4 Address cross-cutting concerns

Address the following only to the level required by the solution.

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

Do not create separate comprehensive documents for each concern unless complexity or governance requirements justify them.

### 4.5 Validate business journeys

Walk through each important approved journey using the integrated architecture.

For each journey:

- Trace the initiating actor and business trigger.
- Identify the responsible service.
- Trace the application operation.
- Identify synchronous and asynchronous interactions.
- Trace data ownership and updates.
- Validate aggregate invariants.
- Identify consistency expectations.
- Identify failure and recovery paths.
- Determine whether the journey can complete as required.

Pay particular attention to:

- Cross-service transactions.
- Partial failures.
- Duplicate commands or events.
- Out-of-order events.
- Long-running workflows.
- Compensation and reversal.
- Operations requiring immediate consistency.

Do not claim a journey is supported simply because the required services exist.

### 4.6 Validate NFR coverage

For every available NFR, identify:

- NFR ID and description.
- Architecture mechanism addressing it.
- Relevant service or interaction.
- Evidence supporting the design.
- Remaining assumptions or risks.
- Validation required during implementation or testing.

Do not claim measurable performance or capacity targets are met without evidence.

### 4.7 Perform the Frugal Architecture challenge

Independently examine the integrated design:

1. Can any service be removed or merged without violating a real requirement?
2. Can any infrastructure component be avoided?
3. Are patterns present without sufficient justification?
4. Is data duplicated unnecessarily?
5. Are synchronous dependencies avoidable?
6. Are abstractions being introduced for hypothetical future needs?
7. Are operational responsibilities unnecessarily fragmented?
8. Is the design more complicated than the business problem requires?
9. Have material risks been hidden by high-level diagrams?
10. Can the architecture evolve without unnecessary upfront investment?

For each proposed simplification, explain the consequences. Do not simplify by removing necessary controls or business invariants.

## 5. Frugal Architect decision rules

- Integrate existing approved decisions before introducing new ones.
- Prefer the existing deployment and platform capabilities.
- Do not add infrastructure simply to make the architecture appear complete.
- Do not invent NFRs or operational targets.
- Do not create generic shared components for hypothetical future reuse.
- Do not introduce additional databases, brokers or runtime platforms without justification.
- Keep deployment topology proportional to actual requirements.
- Separate confirmed decisions from assumptions and recommendations.
- Preserve traceability and identify the cost of material choices.

For every significant new decision, ask:
1. What requirement justifies this?
2. What is the simplest viable alternative?
3. What happens if we defer it?
4. What operational cost does it introduce?
5. Can it be introduced later without major rework?

## 6. Required outputs

### `solution-architecture.md`
The canonical integrated architecture document containing:

- Architecture overview.
- Principles and constraints.
- Service and application architecture summary.
- Interaction architecture summary.
- Data architecture summary.
- Deployment topology.
- Cross-cutting concerns.
- Important runtime flows.
- Material architectural decisions.
- Known risks and unresolved questions.

Reference detailed contracts and models rather than duplicating them.

### `solution-architecture.yaml`
Machine-readable integrated representation of:

- Deployable units and services.
- Responsibilities and ownership.
- Bounded contexts and aggregates.
- APIs and integration events.
- Data ownership and persistence.
- Dependencies.
- Deployment relationships.
- Architecture decision references.

### `architecture-validation.md`
Include:

- Journey-to-architecture validation.
- NFR-to-architecture validation.
- Domain invariant validation.
- Cross-workstream consistency checks.
- Failure-path coverage.
- Identified gaps and risks.
- Required follow-up actions.

### `architecture-change-requests.md`
Include only changes required to approved upstream decisions.

For each change request, record:
- Change request ID.
- Affected artefact and decision.
- Evidence and rationale.
- Business and technical impact.
- Recommended resolution.
- Approval requirement.
- Downstream steps affected.

If no change requests exist, explicitly record that none were identified.

### `architecture-review.md`
Record the independent Reviewer's findings:

- Critical issues.
- Major issues.
- Minor issues.
- Frugality and complexity findings.
- Required corrections.
- Accepted risks.
- Final review recommendation.

### `stage-6-manifest.yaml`
Record:
- Step execution status.
- Input artefact references and versions.
- Output artefact references and versions.
- Decisions introduced.
- Open questions.
- Change requests.
- Review status.
- Dependencies and recommended rerun scope.

Do not duplicate canonical artefact content in the manifest.

## 7. Validation and review

The Reviewer must independently assess four dimensions.

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
- Has unnecessary operational burden been introduced?

Classify findings as:
- Critical.
- Major.
- Minor.
- Improvement suggestion.

Identify the specific artefact and decision affected.

The Reviewer must not rewrite the architecture. Findings requiring changes must be returned to the Author or raised as explicit change requests.

The Manager evaluates the findings and accepts, requests revision or rejects the integrated architecture.

Critical issues must be resolved or explicitly accepted by an authorised human before the step can be marked approved.

## 8. Completion and handover

The step is complete when:

- Steps 6.1, 6.2 and 6.3 have been integrated.
- Cross-workstream inconsistencies are addressed or explicitly recorded.
- Deployment and operational architecture are sufficiently defined.
- Important business journeys have been validated.
- NFR coverage and limitations are documented.
- Material risks and open questions are visible.
- The independent Reviewer has completed the challenge.
- Critical findings are resolved or explicitly accepted.
- The Manager has accepted the output.

**Handover to Stage 7 — Journey-to-Architecture Mapping:**

Provide the canonical integrated architecture, service catalogue, contracts, data ownership, deployment view, validation results, open questions and accepted risks.

Stage 7 must independently map the journeys to the architecture and identify any gaps. Do not treat the Stage 6 journey validation as a substitute for Stage 7.

## 9. Execution discipline

Do not:
- Generate implementation Jira tickets.
- Produce source code.
- Perform repository-level implementation impact analysis.
- Make unapproved changes to business requirements or DDD.
- Claim architecture approval while critical issues remain unresolved.

Keep the output focused on integration, deployment, cross-cutting architecture and independent validation.