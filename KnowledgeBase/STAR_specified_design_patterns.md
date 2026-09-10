### 1. Event-Driven Architecture (EDA)

#### Situation

In a distributed commerce system, tightly coupling services through synchronous calls creates cascading failures, limits scalability, and makes independent evolution difficult.

#### Task

Design a system where services can evolve independently, react to business events asynchronously, and remain resilient to partial failures.

#### Action
	•	Introduced Kafka as an event backbone
	•	Services publish immutable domain events:
	•	OrderCreated
	•	InventoryReservationSucceeded / Failed
	•	PaymentSucceeded / Failed
	•	Services react to events they care about without direct service-to-service calls

#### Result
	•	Loose coupling between bounded contexts
	•	Clear ownership of business decisions
	•	Asynchronous workflows that scale horizontally
	•	Failure in one service does not cascade synchronously to others

### 2. Saga Pattern (Choreography-Based)

#### Situation

Order fulfillment spans multiple services (Orders, Inventory, Billing), each with its own data store and failure modes. Traditional ACID transactions are not feasible across service boundaries.

#### Task

Guarantee business consistency across services without distributed transactions.

#### Action
	•	Implemented a choreography-based Saga
	•	Each service:
	•	Reacts to events
	•	Makes a local decision
	•	Emits a new event
	•	No central orchestrator
	•	Compensation is modeled via domain events (e.g., payment failure)

#### Result
	•	Clear, auditable business flow
	•	No global locks or two-phase commits
	•	Each service remains autonomous
	•	Saga progress is observable via Kafka topics

### 3. Database-per-Service

#### Situation

Sharing databases across services leads to hidden coupling, schema coordination issues, and blocked deployments.

####  Task

Ensure true service autonomy and independent scaling.

#### Action
	•	Each service owns its own database:
	•	Orders → MySQL
	•	Inventory → MSSQL
	•	Billing → MariaDB
	•	No cross-database queries
	•	All cross-service communication happens via events

#### Result
	•	Independent schema evolution
	•	Clear ownership boundaries
	•	Technology choices optimized per service
	•	Strong alignment with microservices best practices

### 4. Outbox Pattern (Transactional Messaging)

#### Situation

Publishing events directly after DB writes risks data inconsistency if the process crashes between operations.

#### Task

Guarantee exactly-once semantic intent between database state and published events.

####  Action
	•	Implemented the Outbox Pattern in each service
	•	Events are:
	•	Written to an outbox_events table in the same transaction
	•	Published asynchronously by a background publisher
	•	Deleted only after successful Kafka publish

#### Result
	•	No lost events
	•	No dual-write problems
	•	Reliable event publication under failure conditions
	•	Production-ready messaging guarantees

### 5. Idempotent Consumers

#### Situation

Kafka guarantees at-least-once delivery. Consumers may receive duplicate messages.

#### Task

Ensure safe reprocessing of events without corrupting state.

#### Action
	•	Each consumer records processed eventIds
	•	Duplicate events are detected and skipped
	•	Business logic is written to be idempotent

#### Result
	•	Safe retries
	•	No duplicated side effects
	•	Deterministic system behavior under failure and restart scenarios

### 6. Independent Microservices

#### Situation

Real-world systems rarely use a single language or runtime.

#### Task

Demonstrate language-agnostic architecture where services are unified by contracts, not implementations.

#### Action
	•	Orders: Java 25 + Spring Boot
	•	Inventory: .NET 9
	•	Billing: Node.js
	•	Shared contracts expressed via JSON events
  
#### Result
	•	Technology choices driven by domain needs
	•	Services communicate via stable, versionable events
	•	Architecture remains consistent despite language diversity

  
