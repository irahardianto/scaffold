---
name: technical-constitution
description: Generates technical implementation plans and architectural strategies that enforce the Project Constitution. Use when designing new features, starting implementation tasks, refactoring code, or ensuring compliance with critical standards like Hexagonal Architecture, security mandates, testing strategies, and error handling.
---

# The Constitution: Technical Preference Universal Principles

**⚖️ CRITICAL GOVERNANCE NOTICE**

**CRITICAL:** BEFORE doing ANYTHING else, Read and Comprehend this document COMPLETELY.

MANDATORY COMPLIANCE CHECK:

Before executing your task, you must perform a "Constitution Lookup":

1. **IDENTIFY** the domain of your task (e.g., API, DB, Auth, logging, etc.).  
2. **SEARCH** the relevant sections (e.g., grep "API Design").  
3. **REVIEW** Critical Constraints table for domain
4. **VERIFY** your plan against the discipline and constraints before proceeding.

## 1. The Supremacy Clause & Conflict Resolution

### Meta-Instructions

**CRITICAL:** This document defines the *Architectural Laws* of the project.

**Law vs. Idiom:**
- You must implement these laws using the target language's native idioms. For example:
    - Go: Use implicit interfaces defined in consumer (Domain layer). Do not use Java-style `IInterface` naming.
    - Python: Use `typing.Protocol` for Ports, to enforce contracts without inheritance coupling.
    - TypeScript: Use `interface` for Ports with clear Dependency Injection (functional or class-based) to satisfy the Hexagonal boundaries.

**Knowledge Retrieval:**
- You are an expert in the target language. Do not ask for syntax guides.
- Use your internal knowledge base for syntax, libraries, and tooling.

**Execution Protocol:**
- **Before Coding:** Mentally "load" relevant principles for the task (e.g., if writing an API handler, load 'Hexagonal Architecture' and 'Error Handling').  
- **After Coding:** Verify output against specific constraints in this document (e.g., "Did I return the standard JSON error format?").

### Rule Precedence Hierarchy (Highest to Lowest)

When directives conflict or seem impossible to satisfy simultaneously, follow this precedence:

**1. Security Principles (Non-Negotiable)**
- OWASP Top 10 compliance
- Input validation and sanitization
- Authentication and authorization
- Secrets management
- No information leakage
- **Action:** Security ALWAYS wins. Refactor other requirements to satisfy security.

**2. Hexagonal Architecture (Structural Law)**
- Dependency inversion (dependencies point inward)
- Port/adapter separation
- Domain purity (no infrastructure in core)
- **Action:** If language pattern conflicts (e.g., Active Record), refactor to satisfy architecture.
- *This supersedes the general "Law vs. Idiom" rule above when conflicts arise.*

**3. Error Handling Standards (Reliability Law)**
- No silent failures
- Correlation IDs required
- JSON envelope format
- Resource cleanup in all paths
- **Action:** All error paths must be explicit. No exceptions.

**4. Language Idioms (Implementation Preference)**
- Use native patterns (defer, RAII, context managers)
- Follow community conventions
- Leverage standard library
- **Action:** Implement above laws idiomatically, don't blindly copy other languages.

**5. Performance Optimizations (Measure First)**
- Appropriate data structures
- Profile before optimizing
- Caching strategies
- **Action:** Only optimize with measured bottlenecks. Correctness > speed.

### Escalation Protocol

**If following a directive is impossible:**

1. **STOP coding immediately**
2. **Document the conflict:**
   - Which directives conflict?
   - Why is compliance impossible?
   - What are the tradeoffs?
3. **Propose alternatives:**
   - Option A: [approach + which rules satisfied/violated]
   - Option B: [approach + which rules satisfied/violated]
   - ...
4. **ASK human for decision**
   - Present the precedence hierarchy
   - Recommend least-harmful deviation option with justification based on hierarchy
   - Wait for explicit approval

**CRITICAL: NEVER silently violate**, If following directive is impossible, **STOP** and ask human

#### Example:

**Scenario 1: Framework Requires Inheritance (Architecture vs Language Idiom)**
- Conflict: Django ORM requires model inheritance, but domain must be pure
- Hierarchy: Architecture (Level 2) > Framework idiom (Level 4)
- Resolution: Create Database Model (inherits ORM) + Pure Domain Entity + Mapper pattern
- PROCEED: Architecture wins

**Scenario 2: Performance vs Security**
- Conflict: Caching would speed up response but contains PII
- Hierarchy: Security (Level 1) > Performance (Level 5)
- Resolution: Sanitize PII before caching OR skip caching for PII endpoints
- PROCEED: Security wins

**Scenario 3: Testing Pure Functions with Time Dependencies**
- Conflict: Domain needs current timestamp but should be pure
- Hierarchy: Architecture purity (Level 2) maintained via Time Port (Level 4 idiom)
- Resolution: Inject Clock/Time interface as driven port, mock in tests
- PROCEED: Both satisfied through proper port design

## 2. Critical Constraints Manifest (Context Triggers)

*Agent Note: If the user query touches on these specific topics, prioritize the following internal rules over general knowledge. These are the High-Priority Constraints. Violating these is an automatic failure.*

| Topic | Critical Constraint (Summary) |
| :---- | :---- |
| **Architecture** | Hexagonal (Ports & Adapters). Domain is pure. Feature-based packaging. |
| **Essential Software Design** | SOLID Principles, Essential Design Practices(DRY, YAGNI, KISS), Code Organization Principles. |
| **Error Handling** | JSON format only (`code`, `message`, `correlationId`). No silent failures. No `try/catch` swallowing. |
| **Testing** | 70/20/10 Pyramid. Mock Ports, not Internals. Integration tests use real infrastructure (Testcontainers). |
| **Concurrency** | Avoid shared memory. Use message passing/channels. Timeout ALL I/O operations. |
| **Config** | Hybrid approach: YAML for structure, `.env` for secrets. Fail fast on missing config. |
| **API Design** | Resource-based URLs. Standard HTTP status codes. Envelope response format (`data`, `meta`). |
| **Security (Auth)** | Deny by default. Server-side checks EVERY request. RBAC/ABAC. MFA for sensitive ops. Rate limit (5/15min). Bcrypt/Argon2 only. |
| **Security (Data)** | TLS 1.2+. Encrypt at rest. No secrets in code. PII redacted in logs. |
| **Security (Input)** | **Validation:** Validated Zod/Pydantic Schemas at ALL boundaries. **Sanitization:** Parameterized queries ONLY. Sanitize output. |

## 3. Table of Contents

*Agent Note: Refer to the sections below for detailed implementation rules on all 16 topics.*

- [The Constitution: Technical Preference Universal Principles](#the-constitution-technical-preference-universal-principles)
  - [1. The Supremacy Clause \& Conflict Resolution](#1-the-supremacy-clause--conflict-resolution)
    - [Meta-Instructions](#meta-instructions)
    - [Rule Precedence Hierarchy (Highest to Lowest)](#rule-precedence-hierarchy-highest-to-lowest)
    - [Escalation Protocol](#escalation-protocol)
      - [Example:](#example)
  - [2. Critical Constraints Manifest (Context Triggers)](#2-critical-constraints-manifest-context-triggers)
  - [3. Table of Contents](#3-table-of-contents)
  - [Architectural Patterns](#architectural-patterns)
    - [Hexagonal Architecture (Ports \& Adapters)](#hexagonal-architecture-ports--adapters)
      - [Layout Examples](#layout-examples)
    - [Related Principles](#related-principles)
  - [Core Design Principles](#core-design-principles)
    - [SOLID Principles](#solid-principles)
    - [Essential Design Practices](#essential-design-practices)
    - [Code Organization Principles](#code-organization-principles)
  - [Error Handling Principles](#error-handling-principles)
    - [Error Categories](#error-categories)
    - [Recoverable vs Non-Recoverable Errors](#recoverable-vs-non-recoverable-errors)
    - [Universal Error Handling Principles](#universal-error-handling-principles)
    - [Application Error Object (Internal/Log Format)](#application-error-object-internallog-format)
    - [Error Handling Checklist](#error-handling-checklist)
    - [Related Principles](#related-principles-1)
  - [Concurrency and Threading Principles](#concurrency-and-threading-principles)
    - [When to Use Concurrency](#when-to-use-concurrency)
    - [Universal Concurrency Principles](#universal-concurrency-principles)
    - [Concurrency Models by Use Case](#concurrency-models-by-use-case)
    - [Testing Concurrent Code](#testing-concurrent-code)
    - [Related Principles](#related-principles-2)
  - [Resource and Memory Management Principles](#resource-and-memory-management-principles)
    - [Universal Resource Management Rules](#universal-resource-management-rules)
    - [Memory Management by Language Type](#memory-management-by-language-type)
    - [Related Principles](#related-principles-3)
  - [API Design Principles](#api-design-principles)
    - [RESTful API Standards](#restful-api-standards)
    - [Related Principles](#related-principles-4)
  - [Testing Strategy](#testing-strategy)
    - [Test Pyramid](#test-pyramid)
    - [Test-Driven Development (TDD)](#test-driven-development-tdd)
    - [Test Doubles Strategy](#test-doubles-strategy)
    - [Test Organization](#test-organization)
    - [Test Quality Standards](#test-quality-standards)
    - [Related Principles](#related-principles-5)
  - [Configuration Management Principles](#configuration-management-principles)
    - [Separation of Configuration and Code](#separation-of-configuration-and-code)
    - [Configuration Validation](#configuration-validation)
    - [Configuration Hierarchy](#configuration-hierarchy)
    - [Configuration Organization](#configuration-organization)
    - [Related Principles](#related-principles-6)
  - [Performance Optimization Principles](#performance-optimization-principles)
    - [Measure Before Optimizing](#measure-before-optimizing)
    - [Choose Appropriate Data Structures](#choose-appropriate-data-structures)
    - [Avoid Premature Abstraction](#avoid-premature-abstraction)
    - [Optimization Techniques](#optimization-techniques)
  - [Data Serialization and Interchange Principles](#data-serialization-and-interchange-principles)
    - [Validate at System Boundaries](#validate-at-system-boundaries)
    - [Handle Encoding Explicitly](#handle-encoding-explicitly)
    - [Serialization Format Selection](#serialization-format-selection)
    - [Security Considerations](#security-considerations)
    - [Related Principles](#related-principles-7)
  - [Logging and Observability Principles](#logging-and-observability-principles)
    - [Universal Logging Standards](#universal-logging-standards)
    - [Observability Strategy](#observability-strategy)
    - [Related Principles](#related-principles-8)
  - [Code Idioms and Conventions](#code-idioms-and-conventions)
    - [Universal Principle](#universal-principle)
    - [Idiomatic Code Characteristics](#idiomatic-code-characteristics)
    - [Avoid Cross-Language Anti-Patterns](#avoid-cross-language-anti-patterns)
  - [Dependency Management Principles](#dependency-management-principles)
    - [Version Pinning](#version-pinning)
    - [Minimize Dependencies](#minimize-dependencies)
    - [Organize Imports](#organize-imports)
    - [Avoid Circular Dependencies](#avoid-circular-dependencies)
  - [Command Execution Principles](#command-execution-principles)
    - [Security](#security)
    - [Portability](#portability)
    - [Error Handling](#error-handling)
    - [Related Principles](#related-principles-9)
  - [Documentation Principles](#documentation-principles)
    - [Self-Documenting Code](#self-documenting-code)
    - [Documentation Levels](#documentation-levels)
  - [Security Principles](#security-principles)
    - [OWASP Top 10 Enforcement](#owasp-top-10-enforcement)
    - [Authentication \& Authorization](#authentication--authorization)
    - [Input Validation \& Sanitization](#input-validation--sanitization)
    - [Logging \& Monitoring (Security Focus)](#logging--monitoring-security-focus)
    - [Secrets Management](#secrets-management)
    - [Related Principles](#related-principles-10)

## Architectural Patterns

### Hexagonal Architecture (Ports & Adapters)

**Philosophy:** Isolate business logic from infrastructure concerns through clear boundaries and dependency inversion.

**Core Concepts:**

**1. Domain Layer (Center/Hexagon Core)**

- **What it is:** Pure business logic, entities, domain services, business rules, validation  
- **Dependencies:** NONE - no frameworks, no infrastructure libraries, no external dependencies  
- **Responsibility:** Implement business rules and domain-specific validations  
- **Test with:** Unit tests with mocked ports (fast, no real infrastructure)

**2. Ports (Interfaces/Contracts)**

- **What they are:** Abstract contracts between domain and infrastructure  
    
- **Two types:**  
    
  - **Driving Ports (Primary/Inbound):** How external actors USE the domain  
      
    - Examples: Use cases, command handlers, application services, queries  
    - Called BY: Driving adapters (HTTP controllers, CLI commands, message consumers)  
    - Direction: External world → Domain

    

  - **Driven Ports (Secondary/Outbound):** How domain USES external services  
      
    - Examples: Repository interfaces, gateway interfaces, event publishers, notification services  
    - Implemented BY: Driven adapters (database repos, API clients, message producers)  
    - Direction: Domain → External world


- **Location:** Defined in domain layer, close to where they're used

**3. Adapters (Implementations)**

- **What they are:** Concrete implementations that connect domain to real world  
    
- **Two types:**  
    
  - **Driving Adapters (Primary):** Entry points to the system  
      
    - Examples: HTTP REST controllers, GraphQL resolvers, CLI commands, message queue consumers, gRPC servers  
    - Responsibility: Translate external requests into domain operations

    

  - **Driven Adapters (Secondary):** External dependencies  
      
    - Examples: PostgreSQL repositories, Redis caches, S3 file storage, external API clients, email services  
    - Responsibility: Implement driven ports using specific technologies


- **Location:** Infrastructure layer, completely separate from domain

**The Dependency Rule:**
```
Infrastructure → Ports (Interfaces) ← Domain

     ↑                                   ↑
     └───────────────────────────────────┘

         Dependencies point INWARD
```
1
**Key Principles:**

- Adapters depend on ports (interfaces), never directly on domain implementations  
- Domain defines ports based on ITS needs, knows nothing about adapters  
- All dependencies point toward the domain (Dependency Inversion Principle)  
- Domain can be compiled and tested without any infrastructure code

**Benefits:**

- ✅ **Testability:** Test domain logic without real databases/APIs (use in-memory implementations)  
- ✅ **Flexibility:** Swap PostgreSQL for MongoDB without changing domain code  
- ✅ **Maintainability:** Changes to infrastructure don't affect business logic  
- ✅ **Team autonomy:** Teams can work on adapters independently  
- ✅ **Technology evolution:** Migrate frameworks without rewriting domain  
- ✅ **Clear boundaries:** Obvious separation between "what" (domain) and "how" (infrastructure)

**Testing Strategy:**

- **Unit Tests:** Domain services with mocked ports (fast, no infrastructure, 70% of tests)  
- **Integration Tests:** Adapters with real infrastructure using Testcontainers (medium speed, 20% of tests)  
- **E2E Tests:** Full system through driving adapters (slow, critical paths only, 10% of tests)

**Package Structure Philosophy:**


- **Organize by FEATURE, not by technical layer**  
- Each feature is a vertical slice containing domain + ports + adapters  
- Enables modular growth, clear boundaries, and independent deployability  

**Universal Rule: Context → Feature → Layer**

**1. Level 1: Repository Scope (Conditional)**
   - **Scenario A (Monorepo/Full-Stack):** Root contains `src/` grouping distinct applications (e.g., `src/backend`, `src/web`).
   - **Scenario B (Single Service):** Root **IS** the application. Do not create `src/backend` wrapper. Start directly at Level 2.

**2. Level 2: Feature Organization**
   - **Rule:** Divide application into vertical business slices (e.g., `user/`, `order/`, `payment/`).
   - **Anti-Pattern:** Do NOT organize by technical layer (e.g., `controllers/`, `models/`, `services/`) at the top level.

**3. Level 3: Hexagonal Layers (Internal Feature Structure)**
   - **Domain:** Pure business logic and entities.
   - **Ports:** Interfaces defining interactions.
   - **Adapters:** Infrastructure implementations.

#### Layout Examples

**A. Standard Single Service (Backend, Microservice or MVC)**
```
  src/  
    user/                       # Feature: User management      
      domain/                   # Business logic (center)     
        user.{ext}              # Entity      
        user-service.{ext}      # Domain service  
        user-repository.{ext}   # Driven port (interface)  
      adapters/      
        http/                   # Driving adapter  
          user-controller.{ext}  
        postgres/               # Driven adapter  
          postgres-user-repository.{ext}  
        redis/                  # Another driven adapter  
          redis-user-cache.{ext}  
    order/                      # Feature: Order management  
      domain/  
      adapters/
```

**B. Monorepo Layout (Multi-Stack):**
**Use this structure when managing monolithic full-stack applications with backend, frontend, mobile in a single repository.*
*Clear Boundaries: Backend business logic (Hexagonal) is isolated from Frontend UI logic, even if they share the same repo*
```    
  src/
    backend/                        # Backend application source code  
        user/                       # Feature: User management      
        domain/                     # Business logic (center)     
            user-entity.{ext}       # Entity      
            user-service.{ext}      # Domain service  
            user-repository.{ext}   # Driven port (interface)  
        adapters/      
            http/                   # Driving adapter  
            user-controller.{ext}  
            postgres/               # Driven adapter  
            postgres-user-repository.{ext}  
            redis/                  # Another driven adapter  
            redis-user-cache.{ext}  
        order/                      # Feature: Order management  
        domain/  
        adapters/
    frontend/                       # Frontend application source code
      assets/                       # Fonts, Images
      components/                   # Shared Technical UI (Buttons, Inputs) - Dumb UI, No Domain Logic
      layouts/                      # App shells (Sidebar, Navbar wrappers)
      utils/                        # Date formatting, validation helpers
      features/                     # Business Features (Hexagonal Slices)
        auth/                       # Feature: Auth (Frontend)
          domain/                   # Pure TS, Logic: Session State, Validation, Interfaces (Ports)
            auth.entity.ts
            auth.service.ts         # State (Pinia store often lives here or is called by this)
            auth.api.interface.ts   # Driven Port 
            auth.schema.ts          # Zod validation
          ui/                       # Driving Adapter: Components / Pages
            LoginForm.{ext}         # Component
            LoginPage.{ext}         # Page
          api/                      # Driven Adapter: Axios/Fetch Client
            auth.api.ts             # Axios/Fetch calls
        profile/
          domain/
          ui/
          api/
```
> This Feature/Domain/UI/API structure is framework-agnostic. It applies equally to React, Vue, Svelte, and Mobile (React Native/Flutter). 'UI' always refers to the framework's native component format (.tsx, .vue, .svelte, .dart).

### Related Principles
- [SOLID: Dependency Inversion](#dependency-inversion-principle-dip) - Ports as abstractions
- [Testing: Mock Ports Strategy](#test-doubles-strategy) - Unit test isolation
- [Code Organization: Feature Packaging](#code-organization-principles) - Vertical slices
- [Dependency Management: Avoid Circular](#avoid-circular-dependencies) - Layer separation


## Core Design Principles

### SOLID Principles

**Single Responsibility Principle (SRP):**

- Each class, module, or function should have ONE and ONLY ONE reason to change  
- Generate focused, cohesive units of functionality  
- If explaining what something does requires "and", it likely violates SRP

**Open/Closed Principle (OCP):**

- Software entities should be open for extension but closed for modification  
- Design abstractions (interfaces, ports) that allow behavior changes without modifying existing code  
- Use composition and dependency injection to enable extensibility

**Liskov Substitution Principle (LSP):**

- Subtypes must be substitutable for their base types without altering program correctness  
- Inheritance hierarchies must maintain behavioral consistency  
- If substituting a subclass breaks functionality, LSP is violated

**Interface Segregation Principle (ISP):**

- Clients should not be forced to depend on interfaces they don't use  
- Create focused, role-specific interfaces rather than monolithic ones  
- Many small, cohesive interfaces > one large, general-purpose interface

**Dependency Inversion Principle (DIP):**

- Depend on abstractions (interfaces/ports), not concretions (implementations/adapters)  
- High-level modules should not depend on low-level modules; both should depend on abstractions  
- Core principle enabling hexagonal architecture and testability

### Essential Design Practices

**DRY (Don't Repeat Yourself):**

- Eliminate code duplication through proper abstraction, shared utilities, composable functions  
- Each piece of knowledge should have single, authoritative representation  
- Don't duplicate logic, algorithms, or business rules

**YAGNI (You Aren't Gonna Need It):**

**CRITICAL:** Code maintainability always prevail

- Avoid implementing functionality before it's actually required  
- Don't add features based on speculation about future needs  
- Build for today's requirements, refactor when needs change

**KISS (Keep It Simple, Stupid):**

**CRITICAL:** Code maintainability always prevail

- Prefer simple(simple to maintain), straightforward solutions over complex, clever ones  
- Complexity should be justified by actual requirements, not theoretical flexibility  
- Simple code is easier to test, maintain, and debug

**Separation of Concerns:**

- Divide program functionality into distinct sections with minimal overlap  
- Each concern should be isolated in its own module or layer  
- Hexagonal architecture enforces this (domain vs infrastructure)

**Composition Over Inheritance:**

- Favor object composition and delegation over class inheritance for code reuse  
- Composition is more flexible and easier to test  
- Use interfaces/traits for polymorphism instead of deep inheritance hierarchies

**Principle of Least Astonishment:**

- Code should behave in ways that users and maintainers naturally expect  
- Avoid surprising or counterintuitive behavior  
- Follow established conventions and patterns

### Code Organization Principles

- Generate small, focused functions with clear single purposes (typically 10-50 lines)  
- Keep cognitive complexity low (cyclomatic complexity < 10 for most functions)  
- Maintain clear boundaries between different layers (presentation, business logic, data access)  
- Design for testability from the start, avoiding tight coupling that prevents testing  
- Apply consistent naming conventions that reveal intent without requiring comments

## Error Handling Principles

### Error Categories

**1. Validation Errors (4xx):**

- User input doesn't meet requirements (wrong format, missing fields, out of range)  
- Examples: Invalid email, password too short, required field missing  
- Response: 400 Bad Request with detailed field-level errors  
- User can fix: Yes, by correcting input

**2. Business Errors (4xx):**

- Domain rule violations (insufficient balance, duplicate email, order already shipped)  
- Examples: Can't delete user with active orders, can't process refund after 30 days  
- Response: 400/409/422 with business rule explanation  
- User can fix: Maybe, depends on business context

**3. Authentication Errors (401):**

- Identity verification failed (invalid credentials, expired token, missing token)  
- Response: 401 Unauthorized  
- User can fix: Yes, by providing valid credentials

**4. Authorization Errors (403):**

- Permission denied (user identified but lacks permission)  
- Response: 403 Forbidden  
- User can fix: No, requires admin intervention

**5. Not Found Errors (404):**

- Resource doesn't exist or user lacks permission to know it exists  
- Response: 404 Not Found  
- User can fix: No

**6. Infrastructure Errors (5xx):**

- Database down, network timeout, external service failure, out of memory  
- Response: 500/502/503 with generic message  
- User can fix: No, system issue

### Recoverable vs Non-Recoverable Errors

**Recoverable (4xx - User can fix):**

- Invalid input, missing fields, wrong format  
- Action: Allow retry with corrected input  
- Response: Detailed error message with guidance

**Non-Recoverable (5xx - System issue):**

- Database down, disk full, out of memory  
- Action: Log details, alert ops team, return safe generic error  
- Response: Generic message, correlation ID for support

### Universal Error Handling Principles

**1. Never Fail Silently:**

- All errors must be handled explicitly (no empty catch blocks)  
- If you catch an error, do something with it (log, return, transform, retry)

**2. Fail Fast:**

- Detect and report errors as early as possible  
- Validate at system boundaries before processing  
- Don't process invalid data hoping it'll work out

**3. Provide Context:**

- Include error codes, correlation IDs, actionable messages  
- Enough information for debugging without exposing sensitive details  
- Example: "Database query failed (correlation-id: abc-123)" not "SELECT * FROM users WHERE..."

**4. Separate Concerns:**

- Different handlers for different error types  
- Business errors ≠ technical errors ≠ security errors

**5. Resource Cleanup:**

- Always clean up in error scenarios (close files, release connections, unlock resources)  
- Use language-appropriate patterns (defer, finally, RAII, context managers)

**6. No Information Leakage:**

- Sanitize error messages for external consumption  
- Don't expose stack traces, SQL queries, file paths, internal structure to users  
- Log full details internally, show generic message externally

### Application Error Object (Internal/Log Format)
```
{
  "status": "error",
  "code": "VALIDATION_ERROR",
  "message": "User-friendly error message",
  "correlationId": "uuid-for-tracking",
  "details": {
    "field": "email",
    "issue": "Invalid email format",
    "provided": "invalid-email",
    "expected": "valid email format (user@example.com)"
  }
}
```

### Error Handling Checklist

- [ ] Are all error paths explicitly handled (no empty catch blocks)?  
- [ ] Do errors include correlation IDs for debugging?  
- [ ] Are sensitive details sanitized before returning to client?  
- [ ] Are resources cleaned up in all error scenarios?  
- [ ] Are errors logged at appropriate levels (warn for 4xx, error for 5xx)?  
- [ ] Are error tests written (negative test cases)?  
- [ ] Is error handling consistent across application?

### Related Principles
- [API Design: Error Response Format](#api-design-principles) - JSON envelope structure
- [Logging: Correlation IDs](#logging-and-observability-principles) - Traceability
- [Security: No Information Leakage](#security-principles) - Sanitization
- [Testing: Negative Test Cases](#testing-strategy) - Error path coverage
- [Concurrency: Error in Thread Context](#concurrency-and-threading-principles) - Thread failures

## Concurrency and Threading Principles

### When to Use Concurrency

**I/O-Bound Operations (async/await, event loops):**

- Network requests, file I/O, database queries  
- Waiting for external responses dominates execution time  
- Use: Asynchronous I/O, event-driven concurrency, coroutines

**CPU-Bound Operations (threads, parallel processing):**

- Heavy computation, data processing, video encoding  
- CPU cycles dominate execution time  
- Use: OS threads, thread pools, parallel workers

**Don't Over-Use Concurrency:**

- Adds significant complexity (race conditions, deadlocks, debugging difficulty)  
- Use only when there's measurable performance benefit  
- Profile first, optimize second

### Universal Concurrency Principles

**1. Avoid Race Conditions**

**What is a race condition:**

- Multiple threads access shared data concurrently  
- At least one thread writes/modifies the data  
- No synchronization mechanism in place  
- Result depends on unpredictable thread execution timing

**Prevention strategies:**

- Synchronization: Locks, mutexes, semaphores  
- Immutability: Immutable data is thread-safe by default  
- Message passing: Send data between threads instead of sharing  
- Thread-local storage: Each thread has its own copy

**Detection:**

- Go: Run with `-race` flag (race detector)  
- Rust: Miri tool for undefined behavior detection  
- C/C++: ThreadSanitizer (TSan)  
- Java: JCStress, FindBugs

**2. Prevent Deadlocks**

**What is a deadlock:**

- Two or more threads waiting for each other indefinitely  
- Example: Thread A holds Lock 1, waits for Lock 2; Thread B holds Lock 2, waits for Lock 1

**Four conditions (ALL must be true for deadlock):**

1. Mutual exclusion: Resources held exclusively (locks)  
2. Hold and wait: Holding one resource while waiting for another  
3. No preemption: Can't force unlock  
4. Circular wait: A waits for B, B waits for A

**Prevention (break any one condition):**

- Lock ordering: Always acquire locks in same order  
- Timeout: Use try_lock with timeout, back off and retry  
- Avoid nested locks: Don't hold multiple locks simultaneously  
- Use lock-free data structures when possible

**3. Prefer Immutability**

- Immutable data = thread-safe by default (no synchronization needed)  
- Share immutable data freely between threads  
- Use immutable data structures where possible (Rust default, functional languages)  
- If data must change, use message passing instead of shared mutable state

**4. Message Passing Over Shared Memory**

- "Don't communicate by sharing memory; share memory by communicating" (Go proverb)  
- Send data through channels/queues instead of accessing shared memory  
- Reduces need for locks and synchronization  
- Easier to reason about and test

**5. Graceful Degradation**

- Handle concurrency errors gracefully (timeouts, retries, circuit breakers)  
- Don't crash entire application on one thread failure  
- Use supervisors/monitors for fault tolerance (Erlang/Elixir actor model)  
- Implement backpressure for producer-consumer scenarios

### Concurrency Models by Use Case

- **I/O-bound:** async/await, event loops, coroutines, green threads  
- **CPU-bound:** OS threads, thread pools, parallel processing  
- **Actor model:** Erlang/Elixir actors, Akka (message passing, isolated state)  
- **CSP (Communicating Sequential Processes):** Go channels, Rust channels

### Testing Concurrent Code

- Write unit tests with controlled concurrency (deterministic execution)  
- Test timeout scenarios and resource exhaustion  
- Test thread pool full, queue full scenarios

### Related Principles
- [Resource and Memory Management Principles](#resource-and-memory-management-principles)
- [Error Handling Principles](#error-handling-principles)  
- [Testing Strategy](#testing-strategy) 

## Resource and Memory Management Principles

### Universal Resource Management Rules

**1. Always Clean Up Resources**

**Resources requiring cleanup:**

- Files, network connections, database connections  
- Locks, semaphores, mutexes  
- Memory allocations (in manual-memory languages)  
- OS handles, GPU resources

**Clean up in ALL paths:**

- Success path: Normal completion  
- Error path: Exception thrown, error returned  
- Early return path: Guard clauses, validation failures

**Use language-appropriate patterns:**

- Go: defer statements  
- Rust: Drop trait (RAII)  
- Python: context managers (with statement)  
- TypeScript: try/finally  
- Java: try-with-resources

**2. Timeout All I/O Operations**

**Why timeout:**

- Network requests can hang indefinitely  
- Prevents resource exhaustion (connections, threads)  
- Provides predictable failure behavior

**Timeout recommendations:**

- Network requests: 30s default, shorter (5-10s) for interactive  
- Database queries: 10s default, configure per query complexity  
- File operations: Usually fast, but timeout on network filesystems  
- Message queue operations: Configurable, avoid indefinite blocking

**3. Pool Expensive Resources**

**Resources to pool:**

- Database connections: Pool size 5-20 per app instance  
- HTTP connections: Reuse with keep-alive  
- Thread pools: Size based on CPU count (CPU-bound) or I/O wait (I/O-bound)

**Benefits:**

- Reduces latency (no connection setup overhead)  
- Limits resource consumption (cap on max connections)  
- Improves throughput (reuse vs create new)

**Connection Pool Best Practices:**

- Minimum connections: 5 (ensures pool is warm)  
- Maximum connections: 20-50 (prevents overwhelming database)  
- Idle timeout: Close connections idle >5-10 minutes  
- Validation: Test connections before use (avoid broken connections)  
- Monitoring: Track utilization, wait times, timeout rates

**4. Avoid Resource Leaks**

**What is a leak:**

- Acquire resource (open file, allocate memory, get connection)  
- Never release it (forget to close, exception prevents cleanup)  
- Eventually exhaust system resources (OOM, max connections, file descriptors)

**Detection:**

- Monitor open file descriptors, connection counts, memory usage over time  
- Run long-duration tests, verify resource counts stay stable  
- Use leak detection tools (valgrind, ASan, heap profilers)

**Prevention:**

- Use language patterns that guarantee cleanup (RAII, defer, context managers)  
- Never rely on manual cleanup alone (use language features)

**5. Handle Backpressure**

**Problem:** Producer faster than consumer

- Queue grows unbounded → memory exhaustion  
- System becomes unresponsive under load

**Solutions:**

- Bounded queues: Fixed size, block or reject when full  
- Rate limiting: Limit incoming request rate  
- Flow control: Consumer signals producer to slow down  
- Circuit breakers: Stop accepting requests when overwhelmed  
- Drop/reject: Fail fast when overloaded (better than crashing)

### Memory Management by Language Type

**Garbage Collected (Go, Java, Python, JavaScript, C#):**

- Memory automatically freed by GC  
- Still must release non-memory resources (files, connections, locks)  
- Be aware of GC pauses in latency-sensitive applications  
- Profile memory usage to find leaks (retained references preventing GC)

**Manual Memory Management (C, C++):**

- Explicit malloc/free or new/delete  
- Use RAII pattern in C++ (Resource Acquisition Is Initialization)  
- Avoid manual management in modern C++ (use smart pointers: unique_ptr, shared_ptr)

**Ownership-Based (Rust):**

- Compiler enforces memory safety at compile time  
- No GC pauses, no manual management  
- Ownership rules prevent leaks and use-after-free automatically  
- Use reference counting (Arc, Rc) for shared ownership

### Related Principles
- [Concurrency and Threading Principles](#concurrency-and-threading-principles) - Thread safety, timeouts
- [Error Handling Principles](#error-handling-principles) - Resource cleanup in error paths

## API Design Principles

### RESTful API Standards

**Resource-Based URLs:**

- Use plural nouns for resources: `/api/{version}/users`, `/api/{version}/orders`  
- Hierarchical relationships: `/api/{version}/users/:userId/orders`  
- Avoid verbs in URLs: `/api/{version}/getUser` ❌ → `/api/{version}/users/:id` ✅

**HTTP Methods:**

- GET: Read/retrieve resource (safe, idempotent, cacheable)  
- POST: Create new resource (not idempotent)  
- PUT: Replace entire resource (idempotent)  
- PATCH: Partial update (idempotent)  
- DELETE: Remove resource (idempotent)

**HTTP Status Codes:**

- 200 OK: Success (GET, PUT, PATCH)  
- 201 Created: Resource created successfully (POST)  
- 204 No Content: Success with no response body (DELETE)  
- 400 Bad Request: Invalid input, validation failure  
- 401 Unauthorized: Authentication required or failed  
- 403 Forbidden: Authenticated but insufficient permissions  
- 404 Not Found: Resource doesn't exist  
- 409 Conflict: Conflict with current state (duplicate)  
- 422 Unprocessable Entity: Validation errors  
- 429 Too Many Requests: Rate limit exceeded  
- 500 Internal Server Error: Unexpected server error  
- 502 Bad Gateway: Upstream service failure  
- 503 Service Unavailable: Temporary unavailability

**Versioning:**

- URL path versioning: `/api/{version}/users` e.g.`/api/v1/users` (explicit, clear)

**Pagination:**

- Limit results per page (default 20, max 100)  
- Cursor-based: `?cursor=abc123` (better for real-time data)  
- Offset-based: `?page=2&limit=20` (simpler, less accurate for changing data)

**Filtering and Sorting:**

- Filtering: `?status=active&role=admin`  
- Sorting: `?sort=created_at:desc,name:asc`  
- Searching: `?q=search+term`

**Response Format:**
```
{
  "data": { /* resource or array of resources */ },
  "meta": {
    "total": 100,
    "page": 1,
    "perPage": 20
  },
  "links": {
    "self": "/api/v1/users?page=1",
    "next": "/api/v1/users?page=2",
    "prev": null
  }
}
```
**API Error Response Format:**

All API errors must follow a consistent envelope structure, matching the success format where possible or using a standard error envelope.
```
{
  "status": "error",                      // Transport: Always "error" or "fail"
  "code": 400,                            // Transport: Redundant HTTP Status
  "error": {                              // Domain: The actual business problem
    "code": "VALIDATION_ERROR",           // Machine-readable business code (UPPER_SNAKE)
    "message": "Invalid email format",    // Human-readable message
    "details": {                          // Optional: Structured context
      "field": "email",
      "reason": "Must be a valid address"
    },
  "correlationId": "req-1234567890",      // Ops: Traceability
  "doc_url": "https://..."                // Optional: Help link
  }
}
```

### Related Principles
- [Error Handling Principles](#error-handling-principles)  
- [Security Principles](#security-principles)
- [Logging and Observability Principles](#logging-and-observability-principles) 

## Testing Strategy

### Test Pyramid

**Unit Tests (70% of tests):**

- **What:** Test domain logic in isolation with mocked dependencies  
- **Speed:** Fast (<100ms per test)  
- **Scope:** Single function, class, or module  
- **Dependencies:** All external dependencies mocked (repositories, APIs, time, random)  
- **Coverage Goal:** >85% of domain logic

**Integration Tests (20% of tests):**

- **What:** Test adapters against real infrastructure  
- **Speed:** Medium (100ms-5s per test)  
- **Scope:** Component interaction with infrastructure (database, cache, message queue)  
- **Dependencies:** Real infrastructure via Testcontainers  
- **Coverage Goal:** All adapter implementations, critical integration points

**End-to-End Tests (10% of tests):**

- **What:** Test complete user journeys through all layers  
- **Speed:** Slow (5s-30s per test)  
- **Scope:** Full system from HTTP request to database and back  
- **Dependencies:** Entire system running (or close approximation)  
- **Coverage Goal:** Happy paths, critical business flows

### Test-Driven Development (TDD)

**Red-Green-Refactor Cycle:**

1. **Red:** Write a failing test for next bit of functionality  
2. **Green:** Write minimal code to make test pass  
3. **Refactor:** Clean up code while keeping tests green  
4. **Repeat:** Next test

**Benefits:**

- Tests written first ensure testable design  
- Comprehensive test coverage (code without test doesn't exist)  
- Faster development (catch bugs immediately, not in QA)  
- Better design (forces thinking about interfaces before implementation)

### Test Doubles Strategy

**Unit Tests:** Use mocks/stubs for all driven ports

- Mock repositories return pre-defined data  
- Mock external APIs return successful responses  
- Mock time/random for deterministic tests  
- Control test environment completely

**Integration Tests:** Use real infrastructure

- Testcontainers spins up PostgreSQL, Redis, message queues  
- Firebase emulator spins up Firebase Authentication, Cloud Firestore, Realtime Database, Cloud Storage for Firebase, Firebase Hosting, Cloud Functions, Pub/Sub, and Firebase Extensions  
- Test actual database queries, connection handling, transactions  
- Verify adapter implementations work with real services

**Best Practice:**

- Generate at least 2 implementations per driven port:  
  1. Production adapter (PostgreSQL, GCP GCS, etc.)  
  2. Test adapter (in-memory, fake implementation)

### Test Organization

**Universal Rule: Co-locate implementation tests; Separate system tests.**

**1. Unit & Integration Tests (Co-located)**
- **Rule:** Place tests **next to the file** they test.
- **Why:** Keeps tests visible, encourages maintenance, and supports refactoring (moving a file moves its tests).
- **Naming Convention Example:**
  - **TS/JS:** `*.spec.ts` (Unit), `*.integration.spec.ts` (Integration)
  - **Go:** `*_test.go` (Unit), `*_integration_test.go` (Integration)
  - **Python:** `test_*.py` (Unit), `test_*_integration.py` (Integration)
  - **Java:** `*Test.java` (Unit), `*IT.java` (Integration)
  > You must strictly follow the convention for the target language. Do not mix `test` and `spec` suffixes in the same application context.

**2. End-to-End Tests (Separate)**
- **Rule:** Place in a dedicated `e2e/` folder
  - **Single Service:** `e2e/` at project root
  - **Monorepo:** `src/e2e/` subdivided by test scope:
    - `src/e2e/api/` for full API flow E2E tests (HTTP → Database)
    - `src/e2e/ui/` for full-stack E2E tests (Browser → Backend → Database)
- **Why:** E2E tests cross boundaries and don't belong to a single feature.
- **Naming:** Follow `{feature}-{ui/api}.e2e.test.{ext}` (Universal - configure test runner to match this pattern `**/*.e2e.test.*`)
  - Example: 
    - `user-registration-api.e2e.test.ts`       # Full API flow: HTTP → DB
    - `user-registration-ui.e2e.test.ts`        # Full-stack: Browser → Backend → DB


**Directory Layout Example:**

**A. Backend (Hexagonal)**
```
src/
  user/
    domain/
      user-service.ts
      user-service.spec.ts # Unit Test
    adapters/
      postgres/
        user-repo.ts
        user-repo.integration.spec.ts           # Integration Test
e2e/
  user-registration-api.e2e.test.ts             # E2E: Full API flow
  user-login-api.e2e.test.ts
```

**B. Frontend (Feature-Sliced)**
```
src/
  features/
    auth/
      domain/
        auth.service.ts
        auth.service.spec.ts                    # Unit Test
      ui/
        LoginForm.tsx
        LoginForm.spec.tsx                      # Component Test
e2e/
  user-signup-flow-ui.e2e.test.ts               # E2E: Full UI journey
```

**C. Monorepo (Multi-Stack)**
```
src/
  backend/
    user/...
  frontend/
    features/...
  e2e/                                            # Shared E2E suite
    api/
      user-crud-api.e2e.test.ts                   # API-level E2E
    ui/
      checkout-flow.e2e.test.ts                   # UI-level E2E
```

### Test Quality Standards

**AAA Pattern (Arrange-Act-Assert):**
```
// Arrange: Set up test data and mocks
const user = { id: '123', email: 'test@example.com' };
const mockRepo = createMockRepository();

// Act: Execute the code under test
const result = await userService.createUser(user);

// Assert: Verify expected outcome
expect(result.id).toBe('123');
expect(mockRepo.save).toHaveBeenCalledWith(user);
```
**Test Naming:**

- Descriptive: `should [expected behavior] when [condition]`  
- Examples:  
  - `should return 404 when user not found`  
  - `should hash password before saving to database`  
  - `should reject email with invalid format`

**Coverage Requirements:**

- Unit tests: >85% code coverage  
- Integration tests: All adapter implementations  
- E2E tests: Critical user journeys

### Related Principles
- [Hexagonal Architecture (Ports & Adapters)](#hexagonal-architecture-ports--adapters)
- [Error Handling Principles](#error-handling-principles)  

## Configuration Management Principles

### Separation of Configuration and Code

**Configuration:**

- Environment-specific values (URLs, credentials, feature flags, timeouts)  
- Changes between dev/staging/prod  
- Can change without code deployment

**Code:**

- Business logic and application behavior  
- Same across all environments  
- Requires deployment to change

**Never hardcode configuration in code:**

- ❌ `const DB_URL = "postgresql://prod-db:5432/myapp"`  
- ✅ `const DB_URL = process.env.DATABASE_URL`

### Configuration Validation

**Validate at startup:**

- Check all required configuration is present  
- Fail fast if required config is missing or invalid  
- Provide clear error messages for misconfiguration  
- Example: "DATABASE_URL environment variable is required"

**Validation checks:**

- Type (string, number, boolean, enum)  
- Format (URL, email, file path)  
- Range (port numbers 1-65535)  
- Dependencies (if feature X enabled, config Y required)

### Configuration Hierarchy

**Precedence (highest to lowest):**

1. **Command-line arguments:** Override everything (for testing, debugging)  
2. **Environment variables:** Override config files  
3. **Config files:** Environment-specific (config.prod.yaml, config.dev.yaml)  
4. **Defaults:** Reasonable defaults in code (fallback)

**Example:**

Database port resolution:

1. Check CLI arg: --db-port=5433

2. Check env var: DB_PORT=5432

3. Check config file: database.port=5432

4. Use default: 5432

### Configuration Organization

Hybrid Approach (config files + .env files): define the structure of configuration in config files (e.g. config/database.yaml) and use .env files to inject the secret values.

**.env files:** Description: A file dedicated to a specific environment (development) for production these values comes from secrets/environment platfrom or manager not a physical `.env` file on disk. When to Use: Use this only for secrets (API keys, passwords) and a few environment-specific values (like a server IP). These files except `.env.template` should never be committed to version control (git).

- `.env.template` - Consist of credentials and secrets with blank value (SHOULD commit to git)  
- `.env.development` - Local development credentials and secrets (SHOULD NOT commit to git)  

**Example `.env.development`:**
```
DEV_DB_HOST=123.45.67.89
DEV_DB_USERNAME=prod_user
DEV_DB_PASSWORD=a_very_secure_production_password
```

**Feature files:** Description: Settings are grouped into files based on what they do (database, auth, etc.). This keeps your configuration organized. When to Use: Use this as your primary method for organizing non-secret settings. It’s the best way to keep your configuration clean and scalable as your application grows.

- `config/database.yaml` - Database settings  
- `config/redis.yaml` - Cache settings  
- `config/auth.yaml` - Authentication settings

**Example `config/database.yaml`:**
```
default: &default
  adapter: postgresql
  pool: 5
development:
  <<: *default
  host: localhost
  database: myapp_dev
  username: <%= ENV['DEV_DB_USERNAME'] %> # Placeholder for a secret
  password: <%= ENV['DEV_DB_PASSWORD'] %>
production:
  <<: *default
  host: <%= ENV['PROD_DB_HOST'] %>
  database: myapp_prod
  username: <%= ENV['PROD_DB_USERNAME'] %>
  password: <%= ENV['PROD_DB_PASSWORD'] %>
```

### Related Principles
- [Security Principles](#security-principles) - Secrets management, validation

## Performance Optimization Principles

### Measure Before Optimizing

**"Premature optimization is the root of all evil" - Donald Knuth**

**Process:**

1. **Measure:** Profile to find actual bottlenecks (don't guess)  
2. **Identify:** Find the 20% of code consuming 80% of resources  
3. **Optimize:** Improve that specific bottleneck  
4. **Measure again:** Verify improvement with benchmarks  
5. **Repeat:** Only if still not meeting performance goals

**Don't optimize:**

- Code that's "fast enough" for requirements  
- Code that's rarely executed  
- Without measurable performance problem

### Choose Appropriate Data Structures

**Selection matters:**

- Hash map: O(1) lookup, unordered  
- Array/list: O(1) index access, O(n) search, ordered  
- Binary tree: O(log n) operations, sorted order  
- Set: O(1) membership testing, unique elements

**Wrong choice causes performance degradation:**

- Using array for lookups: O(n) when O(1) possible with hash map  
- Using list for sorted data: O(n log n) sort vs O(log n) tree operations

### Avoid Premature Abstraction

**Abstraction has costs:**

- Runtime overhead (indirection, virtual dispatch, dynamic resolution)  
- Cognitive overhead (understanding layers of abstraction)  
- Maintenance overhead (changes ripple through abstractions)

**Start concrete, abstract when pattern emerges:**

- Write straightforward code first  
- Identify duplication and common patterns  
- Abstract only when there's clear benefit  
- Don't add "for future flexibility" without evidence

### Optimization Techniques

**Caching:**

- Store expensive computation results  
- Cache database queries, API responses, rendered templates  
- Use appropriate cache invalidation strategy  
- Set TTL (time-to-live) for cache entries

**Lazy Loading:**

- Compute only when needed  
- Load data on-demand, not upfront  
- Defer expensive operations until required

**Batching:**

- Process multiple items together  
- Batch database queries (N queries → 1 query)  
- Batch API requests where possible

**Async I/O:**

- Don't block on I/O operations  
- Use async/await for concurrent I/O  
- Process multiple I/O operations in parallel

**Connection Pooling:**

- Reuse expensive resources (database connections, HTTP connections)  
- See "Resource and Memory Management Principles"

## Data Serialization and Interchange Principles

### Validate at System Boundaries

**All data entering system must be validated:**

- API requests, file uploads, message queue messages  
- Validate type, format, range, required fields  
- Fail fast on invalid data (don't process partially valid data)  
- Return clear validation errors to client

### Handle Encoding Explicitly

**Default to UTF-8:**

- UTF-8 for all text data (API responses, file contents, database strings)  
- Specify encoding explicitly when reading/writing files  
- Handle encoding errors gracefully (replacement characters or error)

**Encoding errors:**

- Invalid UTF-8 sequences (malformed bytes)  
- Mixing encodings (reading UTF-8 as ISO-8859-1)  
- Always validate and normalize encoding

### Serialization Format Selection

**JSON:**

- Human-readable, widely supported, language-agnostic  
- Good for: APIs, configuration files, web applications  
- Limitations: No binary support, larger size than binary formats

**Protocol Buffers:**

- Compact binary format, fast serialization/deserialization  
- Schema evolution (backward/forward compatibility)  
- Good for: Internal microservices, high-throughput systems  
- Limitations: Not human-readable, requires schema definition

**MessagePack:**

- Binary JSON-like format, faster and more compact than JSON  
- Good for: Internal APIs, when JSON too slow but readability still desired  
- Limitations: Less widely supported than JSON

**XML:**

- Verbose, legacy systems, document-oriented  
- Good for: Enterprise systems, SOAP APIs, RSS/Atom feeds  
- Limitations: Verbosity, complexity, security issues (XXE attacks)

**YAML:**

- Human-friendly, good for configuration files  
- Good for: Config files, Infrastructure as Code (Kubernetes, CI/CD)  
- Limitations: Complex parsing, performance, security issues (arbitrary code execution)

### Security Considerations

**Validate before deserialization:**

- Prevent deserialization attacks (arbitrary code execution)  
- Set size limits on payloads (prevent memory exhaustion)  
- Whitelist allowed types/classes for deserialization

**Disable dangerous features:**

- XML: Disable external entity processing (XXE prevention)  
- YAML: Disable unsafe constructors  
- Python pickle: Never deserialize untrusted data

### Related Principles
- [Error Handling Principles](#error-handling-principles)  
- [Security Principles](#security-principles)
- [API Design Principles](#api-design-principles)

## Logging and Observability Principles

### Universal Logging Standards

**Log Levels:**

1. **DEBUG:** Detailed diagnostic information for development (high volume, not in prod)  
2. **INFO:** General informational messages about application flow (prod-safe)  
3. **WARN:** Warning messages for potentially harmful situations (always enabled)  
4. **ERROR:** Error events that might still allow application to continue (always enabled)  
5. **FATAL/CRITICAL:** Severe errors causing application termination (rare)

**Structured Logging:**

- Use JSON format for machine parsing in production  
- Include standard fields in every entry:  
  - timestamp (ISO 8601 with timezone)  
  - level (debug/info/warn/error)  
  - message (human-readable)  
  - correlationId (request tracking)  
  - service (application name)  
  - version (app version)  
  - environment (dev/staging/prod)

**Correlation and Tracing:**

- Correlation ID: Unique ID per request, propagated across all services  
- Generate at entry point, pass via header (X-Correlation-ID)  
- Include in all log entries for that request  
- Enables end-to-end request tracing

**What to Log:**

- ✅ Application lifecycle (startup, shutdown, config loaded)  
- ✅ Business events (order created, payment processed)  
- ✅ Performance metrics (slow queries, high latency)  
- ✅ External service calls (API requests, response codes, duration)  
- ✅ Background job execution (started, completed, failed)

**What NOT to Log:**

- ❌ Passwords, API keys, tokens, secrets  
- ❌ Credit card numbers, SSNs, PII (unless required and encrypted)  
- ❌ Session tokens, JWTs  
- ❌ Full request/response payloads (unless sanitized)

**Best Practice:** "Use logger middleware redaction (e.g., pino-redact, zap masking) rather than manual string manipulation."

### Observability Strategy

**Three Pillars:**

1. **Logs:** What happened (events, errors, state changes)  
2. **Metrics:** How much/how many (quantitative measurements)  
3. **Traces:** How did it happen (request flow through system)

**Key Metrics:**

- **RED (for services):**  
    
  - Rate: Requests per second  
  - Errors: Error rate/count  
  - Duration: Latency (p50, p95, p99)


- **USE (for resources):**  
    
  - Utilization: % resource in use (CPU, memory, disk)  
  - Saturation: How full (queue depth, wait time)  
  - Errors: Error count

**Health Checks:**

- `/health`: Simple "am I alive?" (process health only)  
- `/ready`: "Am I ready to serve?" (includes dependencies)

### Related Principles
- [Error Handling Principles](#error-handling-principles)  
- [Security Principles](#security-principles)
- [API Design Principles](#api-design-principles)

## Code Idioms and Conventions

### Universal Principle

**Write idiomatic code for the target language:**

- Code should look natural to developers familiar with that language  
- Follow established community conventions, not personal preferences  
- Use language built-ins and standard library effectively  
- Apply language-appropriate patterns (don't force patterns from other languages)

### Idiomatic Code Characteristics

- Leverages language features (don't avoid features unnecessarily)  
- Follows language naming conventions  
- Uses appropriate error handling for language (exceptions vs Result types)  
- Applies established community patterns

### Avoid Cross-Language Anti-Patterns

- ❌ Don't write "Java in Python" or "C in Go"  
- ❌ Don't force OOP patterns in functional languages  
- ❌ Don't avoid language features because they're "unfamiliar"  
- ✅ Learn and apply language-specific idioms

## Dependency Management Principles

### Version Pinning

**Production:** Pin exact versions (1.2.3, not ^1.2.0)

- Prevents supply chain attacks  
- Prevents unexpected breakage from patch updates  
- Ensures reproducible builds

**Use lock files:**

- package-lock.json (Node.js)  
- Cargo.lock (Rust)  
- go.sum (Go)  
- requirements.txt or poetry.lock (Python)

### Minimize Dependencies

**Every dependency is a liability:**

- Potential security vulnerability  
- Increased build time and artifact size  
- Maintenance burden (updates, compatibility)

**Ask before adding dependency:**

- "Can I implement this in 50 lines?"  
- "Is this functionality critical?"  
- "Is this dependency actively maintained?"  
- "Is this the latest stable version?"

### Organize Imports

**Grouping:**

1. Standard library  
2. External dependencies  
3. Internal modules

**Sorting:** Alphabetical within groups

**Cleanup:** Remove unused imports (use linter/formatter)

### Avoid Circular Dependencies

**Problem:** Module A imports B, B imports A

- Causes build failures, initialization issues  
- Indicates poor module boundaries

**Solution:**

- Extract shared code to third module  
- Restructure dependencies (A→C, B→C)  
- Use dependency injection

## Command Execution Principles

### Security

**Never execute user input directly:**

- ❌ `exec(userInput)`  
- ❌ `shell("rm " + userFile)`  
- ✅ Use argument lists, not shell string concatenation  
- ✅ Validate and sanitize all arguments

**Run with minimum permissions:**

- Never run commands as root/admin without explicit human approval. If elevated permissions are absolutely required, STOP and request authorization.
- Use least-privilege service accounts

### Portability

**Use language standard library:**

- Avoid shell commands when standard library provides functionality  
- Example: Use file I/O APIs instead of `cat`, `cp`, `mv`

**Test on all target OS:**

- Windows, Linux, macOS have different commands and behaviors  
- Use path joining functions (don't concatenate with /)

### Error Handling

**Check exit codes:**

- Non-zero exit code = failure  
- Capture and log stderr  
- Set timeouts for long-running commands  
- Handle "command not found" gracefully

### Related Principles
- [Security Principles](#security-principles)

## Documentation Principles

### Self-Documenting Code

**Clear naming reduces need for comments:**

- Code shows WHAT is happening  
- Comments explain WHY it's done this way

**When to comment:**

- Complex business logic deserves explanation  
- Non-obvious algorithms (explain approach)  
- Workarounds for bugs (link to issue tracker)  
- Performance optimizations (explain trade-offs)

### Documentation Levels

1. **Inline comments:** Explain WHY for complex code  
2. **Function/method docs:** API contract (parameters, returns, errors)  
3. **Module/package docs:** High-level purpose and usage  
4. **README:** Setup, usage, examples  
5. **Architecture docs:** System design, component interactions

## Security Principles

### OWASP Top 10 Enforcement

* **Broken Access Control:** Deny by default. Validate permissions *server-side* for every request. Do not rely on UI state.  
* **Cryptographic Failures:** Use TLS 1.2+ everywhere. Encrypt PII/Secrets at rest. Use standard algorithms (AES-256, RSA-2048, Ed25519). *Never* roll your own crypto.  
* **Injection:** ZERO TOLERANCE for string concatenation in queries. Use Parameterized Queries (SQL) or ORM bindings. Sanitize all HTML/JS output.  
* **SSRF Prevention:** Validate all user-provided URLs against an allowlist. Disable HTTP redirects in fetch clients. Block requests to internal IPs (metadata services, localhost).  
* **Insecure Design:** Threat model every new feature. Fail securely (closed), not openly.  
* **Vulnerable Components:** Pin dependency versions. Scan for CVEs in CI/CD.

### Authentication & Authorization

* **Passwords:** Hash with Argon2id or Bcrypt (min cost 12). Never plain text.  
* **Tokens:**  
  * *Access Tokens:* Short-lived (15-30 mins). HS256 or RS256.  
  * *Refresh Tokens:* Long-lived (7-30 days). Rotate on use. Store in `HttpOnly; Secure; SameSite=Strict` cookies.  
* **Rate Limiting:** Enforce strictly on public endpoints (Login, Register, Password Reset). Standard: 5 attempts / 15 mins.  
* **MFA:** Required for Admin and Sensitive Data access.  
* **RBAC:** Map permissions to Roles, not Users. Check permissions at the Route AND Resource level.

### Input Validation & Sanitization

* **Principle:** "All Input is Evil until Proven Good."  
* **Validation:** Validate against a strict Schema (Zod/Pydantic) at the *Controller/Port* boundary.  
* **Allowlist:** Check for "Good characters" (e.g., `^[a-zA-Z0-9]+$`), do not try to filter "Bad characters."  
* **Sanitization:** Strip dangerous tags from rich text input using a proven library (e.g., DOMPurify equivalent).

### Logging & Monitoring (Security Focus)

* **Redaction:** SCRUB all PII, Secrets, Tokens, and Passwords from logs *before* writing.  
* **Events:** Log all *failed* auth attempts, access denied events, and input validation failures.  
* **Format:** JSON structured logs with `correlationId`, `user_id`, and `event_type`.  
* **Anti-Tamper:** Logs should be write-only for the application.

### Secrets Management

* **Storage:** Never commit secrets to git. Use `.env` (local) or Secret Managers (Prod - e.g., Vault/GSM).

### Related Principles
- [Error Handling Principles](#error-handling-principles)  
- [API Design Principles](#api-design-principles)
- [Logging and Observability Principles](#logging-and-observability-principles) 
- [Configuration Management Principles](#configuration-management-principles)