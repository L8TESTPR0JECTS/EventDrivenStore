# EventDrivenStore

EventDrivenStore is a distributed system composed of independent bounded contexts, designed to demonstrate the design, implementation, and operation of a production type event driven architecture.

The system emphasizes service autonomy through deliberate domain partitioning, transactional integrity via relational databases, asynchronous communication, and reproducible infrastructure across multiple languages, frameworks, platforms, and runtimes, reflecting real-world distributed system design and engineering practices.


## Components (Repositories)

| Component | Repo | Responsibility | Tech |
|---|---|---|---|
| Orders Service | [`eds-orders`](https://github.com/programmeralek/eds-orders) | Owns order creation and persistence, and emits immutable domain events | Java 25 / Spring Boot / MySQL |
| Inventory Service | [`eds-inventory`](https://github.com/programmeralek/eds-inventory) | Owns inventory state and stock decisions, reacting to order-related domain events | .NET / C# / MSSQL |
| Billing Service | [`eds-billing`](https://github.com/programmeralek/eds-billing) | Owns billing and payment workflows, reacting asynchronously to domain events | Node.js / Docker / MariaDB |


## Overview of Distributed System Ownership


### 1.	Client → Gateway
####	•	Synchronous HTTP boundary
####	•	Routing resolved dynamically via Eureka
### 2.	Gateway → Domain Services
####	•	No hardcoded URLs
####	•	Service discovery enables elasticity
### 3.	Domain Services → Kafka
####	•	Domain events are emitted, not commands
####	•	Services never call each other directly
### 4.	Kafka → Domain Services
####	•	Services consume only what they own
####	•	No shared state, no implicit dependencies
### 5.	Each Service → Its Own Database
####	•	Local transactions only
####	•	State changes are private and protected

Each of these specifications can be validated through the following diagram:

![Overview](https://github.com/programmeralek/EventDrivenStore/blob/main/KnowledgeBase/EDS_Architecturial_Diagram.drawio.png)

## Design Patterns & Architectural Principles Demonstrated

This project intentionally demonstrates multiple distributed-systems design patterns, each implemented end-to-end in a realistic, production-style manner.

#### 1. Event-Droven Architecture
#### 2. Saga Pattern (Choreography Based)
#### 4. Database-per-Service
#### 4. Outbox Pattern (Transactional Messaging)
#### 5. Idempotent Consumers (ACID Principles)
#### 6. Independent Microservices


Each pattern is described using the STAR methodology (Situation, Task, Action, Result) to clearly articulate design intent and outcomes in a detailled manner in <a href="https://github.com/programmeralek/EventDrivenStore/blob/main/KnowledgeBase/STAR_specified_design_patterns.md" target="_blank">STAR_specified_design_patterns.md</a>
## Deployment Strategy

This project documents its deployment model as a first-class architectural concern.  
Rather than hiding deployment decisions inside scripts or tooling, they are explicitly described and versioned alongside the codebase.

The deployment strategy is broken down into the following sections:

### 1. Core Deployment Philosophy  
Describes the foundational principles behind how services are deployed, why service autonomy is enforced, and why shared deployment units were intentionally avoided.  
📄 [`core-deployment-philosophy.md`](KnowledgeBase/Deployment/1.coreDeploymentPhilosophy.md)

### 2. What Actually Gets Deployed  
Clarifies what is deployed versus what is not, distinguishing between services, infrastructure dependencies, runtime artifacts, and execution boundaries.  
📄 [`what-actually-gets-deployed.md`](KnowledgeBase/Deployment/2.whatActuallyGetsDeployed.md)

### 3. One Dockerfile per Service  
Explains why each service owns its own Dockerfile, how this enables independent lifecycle management, and why this is critical for elasticity and failure isolation.  
📄 [`one-dockerfile-per-service.md`](KnowledgeBase/Deployment/3.oneDockerfilePerService.md)

### 4. Environment Configuration Strategy  
Details how environment-specific configuration is handled across local development, CI, and production without leaking infrastructure concerns into business logic.  
📄 [`environment-configuration-strategy.md`](KnowledgeBase/Deployment/4.environmentConfigurationStrategy.md)

### 5. Service Startup and Dependency Order  
Documents how services are allowed to start independently, why strict startup ordering is avoided, and how eventual consistency is embraced at runtime.  
📄 [`service-startup-and-dependency-order.md`](KnowledgeBase/Deployment/5.serviceStartupAndDependencyOrder.md)

### 6. Scaling Strategy  
Covers how services are expected to scale independently, what components are stateless versus stateful, and how the architecture supports horizontal scaling.  
📄 [`scaling-strategy.md`](KnowledgeBase/Deployment/6.ScalingStrategy.md)

### 7. Failure and Recovery Behavior  
Explains how the system behaves under partial failure, including consumer retries, idempotency, outbox recovery, and service restarts.  
📄 [`failure-and-recovery-behavior.md`](KnowledgeBase/Deployment/7.failureAndRecoveryBehavior.md)

### 8. Deployment and CI/CD Readiness  
Outlines how the system is prepared for automated pipelines, container-based delivery, and future cloud-native deployment without architectural rewrites.  
📄 [`deployment-and-cicd-readiness.md`](KnowledgeBase/Deployment/8.deploymentAndCiCdReadiness.md)

## Data Structures & Persistence

This project documents data ownership explicitly to reinforce database-per-service boundaries and to make the Saga workflow traceable across stores.

### Data Documentation

1. [Data Ownership and Boundaries](KnowledgeBase/Data/1.data-ownership-and-boundaries.md) 

2. [Orders Service Data Model (MySQL)](KnowledgeBase/Data/2.orders-data-model.md)

3. [Inventory Service Data Model (MSSQL)](KnowledgeBase/Data/3.inventory-data-model.md)

4. [Billing Service Data Model (MariaDB)](KnowledgeBase/Data/4.billing-data-model.md)

5. [Cross-Cutting Persistence Tables (Outbox + Idempotency)](KnowledgeBase/Data/5.cross-cutting-tables-outbox-idempotency.md)

6. [Migrations and Schema History Strategy](KnowledgeBase/Data/6.migrations-and-schema-history.md)

## Local Setup Guide

This repo includes a step-by-step guide to run the system locally and verify the full Saga path through Kafka and the service-owned databases.

## Local Setup Guide

1. [Prerequisites](KnowledgeBase/LocalSetupGuide/1.prerequisites.md)

2. [Infrastructure Startup](KnowledgeBase/LocalSetupGuide/2.infrastructure-startup.md)

3. [Service Startup](KnowledgeBase/LocalSetupGuide/3.service-startup.md)

4. [Smoke Tests and Verification](KnowledgeBase/LocalSetupGuide/4.smoke-tests-and-verification.md)

5. [Troubleshooting](KnowledgeBase/LocalSetupGuide/5.troubleshooting.md)


