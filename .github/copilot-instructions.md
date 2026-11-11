# GitHub Copilot Instructions for Secure_PR_review

## Project overview
This repository is a Spring Boot banking microservice example implemented in Java 17 with Maven. The application models basic banking flows such as creating accounts, depositing funds, withdrawing funds, checking balances, and transfer-related operations.

The codebase is organized around a simple layered architecture:
- `controllers/` for REST endpoints
- `services/` for business logic
- `repositories/` for persistence access
- `models/` for domain entities
- `utils/` for request/input validation and helper logic
- `resources/` for configuration and SQL initialization

This project is intended for Java + Spring Boot development and should follow idiomatic, production-quality patterns expected by GitHub Copilot assistance in this repository.

## Core technology constraints
- Use Java 17 syntax and conventions.
- Build and test with Maven, specifically `./mvnw`.
- Keep compatibility with Spring Boot 2.7.x and the project’s existing dependencies.
- Prefer the existing package structure: `com.example.paul`.
- Do not introduce new frameworks or major dependency changes unless clearly required.

## Architecture and coding expectations
- Keep REST controller classes thin and focused on HTTP concerns.
- Move business logic into `services` classes.
- Use repository interfaces for persistence access through Spring Data JPA.
- Keep domain entities in `models` and make them consistent with JPA usage.
- Reuse the existing patterns for validation, input parsing, and numeric checks rather than introducing ad hoc logic.
- Prefer explicit, readable code over clever abstractions.

## Repository conventions
- Follow the current naming patterns:
  - `*RestController` for controllers
  - `*Service` for service classes
  - `*Repository` for repositories
  - `*Input` for request or validation helper classes
- Match existing class and method naming conventions.
- Keep file names aligned with domain responsibilities.
- Use package-private or public classes appropriately for Spring Boot scanning and testing.

## API and business logic guidance
- When adding or modifying bank operations, maintain a consistent flow:
  1. validate request data
  2. fetch/update account and transaction data via repositories
  3. apply business rules in service layer
  4. return a clear response model or HTTP response
- Validate inputs before processing financial operations.
- Treat account balance, deposit amount, and withdrawal amount as sensitive business values.
- Preserve existing semantics for transactions and account actions, especially around error scenarios and invalid values.
- Keep response payloads understandable and consistent with current controller outputs.

## Validation and error handling
- Prefer clear validation logic over silent failures.
- Reject invalid input early, especially negative amounts, missing account identifiers, and invalid account data.
- Preserve meaningful exceptions and HTTP error behavior already used in the project.
- Avoid broad catch blocks unless the project pattern already requires them.

## Testing expectations
- Add or update unit tests in `src/test/java/com/example/paul/unit` for service/controller logic.
- Add or update integration tests in `src/test/java/com/example/paul/integration` for end-to-end banking flows.
- Keep tests focused, deterministic, and aligned with the project’s existing style.
- Prefer the existing Spring Boot test setup and JUnit patterns already used in this repo.

## Commands
Use these commands for local validation:

```bash
./mvnw clean test
```

For a full build without running tests:

```bash
./mvnw clean install -DskipTests=true
```

For Docker-based local run:

```bash
docker build -t "spring-boot:banking" .
docker run -p 8080:8080 spring-boot:banking
```

## Preferred code generation style for Copilot
- Generate Java code that is idiomatic, explicit, and maintainable.
- Favor Spring Boot conventions over custom framework patterns.
- Keep code readable for someone maintaining a banking microservice in a learning/demo project.
- Make changes minimal and consistent with existing project structure.
- Do not add unnecessary abstractions, generated boilerplate, or unrelated refactors.
- If in doubt, match the style and structure of nearby classes in the same package.

## Scope of assistance
Copilot should assist with:
- adding banking features
- improving validation and model logic
- updating controller/service/repository implementations
- adding test coverage
- fixing issues in Java/Spring Boot code

Copilot should avoid:
- broad rewrites with no clear benefit
- framework migrations unrelated to the repo
- introducing non-Spring patterns for a Spring Boot project
- over-engineering for a simple demo banking application

## Final guidance
When generating code for this repository, prioritize correctness, maintainability, and consistency with the existing Spring Boot banking application design. Keep the implementation aligned with the repo’s structure, Java 17 conventions, and the current testing/build workflow.
