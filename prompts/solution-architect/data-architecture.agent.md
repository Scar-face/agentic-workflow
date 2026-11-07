# STEP 6.3 — DATA ARCHITECTURE

## 1. Step definition

**Objective:** Define data ownership, persistence, access and consistency across the services identified in Step 6.1.

This step translates the domain model and service boundaries into a logical data architecture.

It establishes which service owns each business data element, where authoritative data resides, how data is accessed or shared, and which persistence mechanisms are justified.

The objective is not to generate exhaustive physical database schemas. Produce the minimum data design necessary to support the business requirements and downstream implementation.

## 2. Crew

Exactly one agent must fulfil each role.

### Manager — Data Architecture Manager
- Owns execution and completion.
- Ensures the service architecture and relevant domain artefacts are available.
- Controls scope and ensures data ownership decisions are addressed.
- Evaluates review findings.
- Accepts, requests revision or rejects the output.
- Does not delegate work.

### Interviewer — Data Requirements Interviewer
- Identifies missing data requirements and ambiguities.
- Clarifies ownership, consistency, access, segregation, retention and lifecycle requirements.
- Identifies missing information that materially affects persistence or data sharing.
- Obtains human clarification where necessary.
- Records assumptions and unresolved questions.

### Author — Data Architecture Designer
- Defines data ownership and persistence architecture.
- Defines logical data structures where necessary.
- Evaluates data access, sharing and replication.
- Evaluates CQRS and specialised read models where justified.
- Produces the required artefacts.
- Does not delegate work.

### Reviewer — Data Architecture Challenger
- Independently challenges data ownership, consistency and persistence decisions.
- Identifies unnecessary data duplication and storage complexity.
- Checks alignment with aggregates, invariants and service boundaries.
- Evaluates segregation, lifecycle and recovery concerns.
- Reports findings to the Manager and Author.

## 3. Inputs

Use the latest approved versions of:

1. Step 6.1:
   - `service-architecture.md`
   - `application-architecture.md`
   - `service-architecture.yaml`
2. Step 6.2 interaction architecture, if available:
   - Interaction catalogue.
   - API contracts.
   - Integration-event contracts.
3. Consolidated Event Storming from Step 3.
4. Strategic DDD from Step 4.
5. Tactical DDD from Step 5:
   - Aggregates.
   - Entities and value objects.
   - Invariants.
   - Commands and domain events.
   - Consistency boundaries.
6. User journeys and requirements from Step 1.
7. NFRs and technical constraints.
8. Existing data architecture, approved data platforms and data governance standards.

Step 6.2 may be executing in parallel. Do not block this step unnecessarily. Record assumptions about data exchanged through interactions and reconcile them in Step 6.4.

## 4. Scope of work

### 4.1 Establish data ownership

For each service, identify:

- Business data it owns.
- Aggregates and entities it persists.
- Authoritative source of truth.
- Data it derives or maintains.
- Data it exposes to other services.
- Data it consumes from other services.
- Data it must not own or modify.

Ownership must align with the approved bounded contexts, aggregates and business invariants.

A service must not directly modify another service's operational data.

### 4.2 Define persistence requirements

For each data responsibility, determine:

- Whether persistence is required.
- What persistence capability is appropriate.
- Whether an existing platform capability is sufficient.
- Transactional consistency requirements.
- Durability and recovery expectations.
- Data lifecycle and retention requirements.
- Access patterns that materially influence storage choice.

Choose storage based on actual requirements.

Do not introduce multiple databases or specialised storage technologies without justification.

### 4.3 Define data access and sharing

Determine how data crosses service boundaries.

Evaluate:
- API-based access.
- Integration-event-based replication.
- Derived read models.
- Batch data exchange.
- Existing enterprise data-sharing mechanisms.

For replicated or derived data, define:

- Source of truth.
- Owning service of the derived copy.
- Replication mechanism.
- Expected freshness.
- Consistency characteristics.
- Reconciliation and recovery approach.

Prefer explicit service contracts over shared operational database access.

### 4.4 Evaluate CQRS and read models

Introduce CQRS only when read and write requirements materially differ.

If justified, define:

- Write-side ownership.
- Read-model ownership.
- Source of truth.
- Projection update mechanism.
- Freshness and consistency expectations.
- Rebuild and recovery strategy.
- Schema evolution considerations.

Introduce Elasticsearch, Redis or other specialised storage only when search, query, latency or workload requirements justify them.

Do not create read models merely to avoid designing appropriate queries against the existing data store.

### 4.5 Define logical data structures

Define logical data relationships where necessary to clarify:

- Aggregate persistence.
- Cross-aggregate relationships.
- Ownership.
- Referential integrity expectations.
- Data access and segregation.
- Data sharing and replication.

Generate detailed logical models only where they add meaningful value.

Generate physical models, table structures, indexes and field-level definitions only when complexity or implementation requirements justify them.

### 4.6 Address data lifecycle and governance

Where applicable, identify:

- Data classification and sensitivity.
- Retention and deletion.
- Audit requirements.
- Data lineage and provenance.
- Migration and backfill requirements.
- Tenant, client or account segregation.
- Access-control enforcement points.

Do not invent regulatory or retention requirements. Record missing policy decisions explicitly.

## 5. Frugal Architect decision rules

- Establish ownership before choosing storage.
- Prefer the existing approved data platform when it satisfies the requirements.
- Do not introduce a separate database for every service automatically.
- Do not introduce CQRS without a material read/write difference.
- Do not duplicate data without a clear consumer need.
- Do not introduce caching without a performance or workload justification.
- Do not create a data warehouse or specialised analytical store to solve an operational data problem.
- Avoid premature physical modelling.
- Make consistency, recovery and operational costs explicit.

For every significant decision, ask:
1. What requirement justifies this data capability?
2. What is the simplest alternative?
3. What happens if we defer it?
4. What additional operational cost does it create?
5. Can it be introduced later without major rework?

## 6. Required outputs

### `data-architecture.md`
- Data architecture overview.
- Data ownership by service.
- Source-of-truth definitions.
- Persistence decisions.
- Data access and sharing mechanisms.
- Consistency and freshness expectations.
- CQRS and read-model decisions, where applicable.
- Lifecycle, segregation and governance concerns.
- Assumptions and open questions.

### `data-ownership.yaml`
Machine-readable representation of:
- Service ownership.
- Aggregates and business data owned.
- Authoritative sources.
- Derived or replicated data.
- Consumers and access mechanisms.
- Consistency and freshness requirements.

### `logical-data-model/`
Generate logical models only where they add meaningful value.

### `architecture-decisions.md`
Only material data decisions introduced by this step.

Do not create empty or speculative models.

## 7. Validation and review

The Reviewer must independently challenge:

- Whether data ownership aligns with domain boundaries.
- Whether sources of truth are unambiguous.
- Whether aggregate invariants can be enforced.
- Whether cross-service database access has been avoided.
- Whether data replication is genuinely necessary.
- Whether CQRS and specialised storage are justified.
- Whether eventual consistency is acceptable for the affected journeys.
- Whether data recovery, retention and segregation are addressed where required.
- Whether the design introduces unnecessary operational complexity.

The Manager evaluates findings and accepts, requests revision or rejects the output.

## 8. Completion and handover

The step is complete when:

- Every relevant business data responsibility has an owner.
- Sources of truth are explicit.
- Persistence decisions are justified.
- Cross-service data access is designed.
- Consistency and freshness requirements are documented.
- Required read models have ownership and recovery semantics.
- Open questions and assumptions are recorded.
- Reviewer findings have been addressed or explicitly accepted.
- The Manager has accepted the output.

**Handover to Step 6.4:**

Provide data ownership, persistence decisions, data access mechanisms, logical models where applicable, consistency expectations, lifecycle concerns and unresolved issues.

## 9. Execution discipline

Do not:
- Change service boundaries without an explicit change request.
- Design API or event contracts.
- Define deployment topology.
- Generate implementation Jira issues.
- Produce exhaustive physical schemas by default.

Keep the output focused on data architecture.