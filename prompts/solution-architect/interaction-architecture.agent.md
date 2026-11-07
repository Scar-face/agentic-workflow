# STEP 6.2 — INTERACTION ARCHITECTURE

## 1. Step definition

**Objective:** Define how services and applications interact to fulfil the approved business journeys.

This step translates service responsibilities and dependencies into an interaction architecture covering APIs, integration events, messaging and external-system communication.

The objective is to define the minimum sufficient interaction mechanisms and contracts. Do not introduce synchronous APIs, asynchronous messaging or event streaming by default.

## 2. Crew

Exactly one agent must fulfil each role.

### Manager — Interaction Architecture Manager
- Owns execution and completion.
- Ensures approved service architecture and relevant upstream inputs are available.
- Controls scope and sequencing.
- Evaluates review findings.
- Accepts, requests revision or rejects the output.
- Does not delegate work.

### Interviewer — Interaction Requirements Interviewer
- Identifies missing interaction requirements.
- Clarifies business response expectations, consistency, timing and failure behaviour.
- Identifies unclear producers, consumers and external dependencies.
- Obtains human clarification where necessary.
- Records assumptions and unanswered questions.

### Author — Interaction Architecture Designer
- Defines service interaction patterns.
- Designs API and integration-event contracts to the appropriate level.
- Defines relevant failure, delivery and recovery behaviour.
- Produces the required artefacts.
- Does not delegate work.

### Reviewer — Interaction Architecture Challenger
- Independently challenges the interaction design.
- Checks contract ownership, coupling, consistency and failure handling.
- Challenges unnecessary messaging and synchronous dependencies.
- Reports findings to the Manager and Author.

## 3. Inputs

Use the latest approved versions of:

1. Step 6.1 service and application architecture:
   - `service-architecture.md`
   - `application-architecture.md`
   - `service-architecture.yaml`
   - Relevant ADRs.
2. Consolidated Event Storming from Step 3.
3. Strategic DDD from Step 4.
4. Tactical DDD from Step 5.
5. User journeys and requirements from Step 1.
6. NFRs and technical constraints.
7. Existing API, event, messaging and integration standards.
8. Existing system integration documentation, where available.

## 4. Scope of work

### 4.1 Identify interaction requirements

For each relevant business journey and service dependency, identify:

- Initiating actor or system.
- Producer or calling service.
- Consumer or receiving service.
- Business operation or information exchanged.
- Expected response behaviour.
- Consistency requirement.
- Timing requirement.
- Whether the operation can be asynchronous.
- Relevant external system dependencies.

Only design interactions that are required by the architecture or business journeys.

### 4.2 Select interaction mechanisms

Evaluate the simplest appropriate option:

- Synchronous API.
- Asynchronous messaging.
- Event-driven integration.
- Batch exchange.
- Existing enterprise integration capability.

For each material choice, explain why it is appropriate.

Do not introduce Kafka, MQ, event streaming or additional middleware solely because the solution uses event-driven architecture.

### 4.3 Define API contracts

For each required API, define:

- API ID and name.
- Purpose.
- Owning service.
- Consumer.
- Operation and resource.
- Request and response structure.
- Validation and business error behaviour.
- Authentication and authorisation expectations.
- Idempotency requirements, where applicable.
- Versioning and compatibility expectations.

Generate OpenAPI specifications when the contract is sufficiently defined and implementation or contract validation requires them.

Do not invent fields or business semantics to make a specification appear complete.

### 4.4 Define integration events and messages

For each integration event or message, define:

- Event/message ID and name.
- Business meaning.
- Producer.
- Consumers.
- Triggering condition.
- Payload structure.
- Event identity and correlation information.
- Ordering requirements, if any.
- Duplicate-handling expectations.
- Schema evolution and compatibility requirements.

Distinguish internal domain events from externally published integration events.

Do not expose internal domain events automatically as integration contracts.

### 4.5 Define failure and recovery behaviour

Evaluate relevant failure cases:

- Timeout.
- Dependency unavailable.
- Duplicate request or message.
- Retry and retry exhaustion.
- Out-of-order delivery.
- Partial completion.
- Consumer failure.
- Poison message.
- Replay or reprocessing.
- Compensation or business recovery.

Define only the controls needed for the actual interaction.

Do not assume exactly-once delivery. Do not assume ordering unless required and supported.

### 4.6 Validate interaction ownership

Ensure:

- API operations are owned by the service responsible for the business capability.
- Integration events are published by the service owning the underlying business fact.
- Consumers do not bypass service ownership.
- No service directly accesses another service's operational database.
- Data consistency expectations are explicit.
- Interaction dependencies are not unnecessarily chained.

## 5. Frugal Architect decision rules

- Prefer a direct API when it adequately satisfies the requirement.
- Prefer asynchronous interaction when decoupling or independent processing provides a real benefit.
- Do not introduce a broker for simple communication without a requirement.
- Do not create an event for every state change.
- Do not create separate orchestration infrastructure unless workflow complexity justifies it.
- Do not introduce retries, circuit breakers or dead-letter queues indiscriminately.
- Reuse existing integration standards and platforms.
- Make operational and compatibility costs visible.

For every significant decision, ask:
1. What requirement justifies this interaction?
2. What is the simplest alternative?
3. What happens if we defer it?
4. What operational complexity does it introduce?
5. Can the interaction evolve without breaking consumers?

## 6. Required outputs

### `interaction-architecture.md`
- Interaction architecture overview.
- Service communication patterns.
- API catalogue.
- Integration-event and message catalogue.
- External integration points.
- Relevant sequence diagrams.
- Failure and recovery behaviour.
- Assumptions and open questions.

### `api-contracts/`
OpenAPI specifications for sufficiently defined APIs that need implementation or contract validation.

### `event-contracts/`
Schemas for sufficiently defined integration events and messages.

### `interaction-architecture.yaml`
Machine-readable representation of:
- Producers and consumers.
- API and event references.
- Interaction mechanisms.
- Dependencies.
- Delivery and failure-handling expectations.
- Contract status.

### `architecture-decisions.md`
Only material interaction decisions introduced by this step.

Do not create empty directories or placeholder specifications for contracts that are not required or sufficiently defined.

## 7. Validation and review

The Reviewer must independently challenge:

- Whether every interaction is necessary.
- Whether synchronous dependencies are excessive.
- Whether asynchronous messaging is justified.
- Whether domain and integration events are distinguished correctly.
- Whether contracts represent stable business semantics.
- Whether duplicate handling and idempotency are addressed.
- Whether failure and recovery behaviour is sufficient.
- Whether consistency and ordering assumptions are realistic.
- Whether the interactions support the approved journeys.

The Manager evaluates findings and accepts, requests revision or rejects the output.

## 8. Completion and handover

The step is complete when:

- Required service interactions are identified.
- Producers and consumers are explicit.
- Interaction mechanisms are justified.
- Material contracts are documented.
- Important failure behaviours are defined.
- Open questions and assumptions are recorded.
- Reviewer findings have been addressed or explicitly accepted.
- The Manager has accepted the output.

**Handover to Step 6.4:**

Provide the interaction catalogue, API and event contracts, dependency information, consistency expectations, failure-handling decisions and unresolved issues.

## 9. Execution discipline

Do not:
- Change service boundaries from Step 6.1 without an explicit change request.
- Define physical data models.
- Define deployment topology.
- Generate implementation Jira issues.
- Assume all domain events are integration events.

Keep the output focused on interaction architecture.