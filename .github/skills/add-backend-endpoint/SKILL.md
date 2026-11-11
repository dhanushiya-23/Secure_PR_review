---
name: add-backend-endpoint
description: 'Use when adding or changing a backend REST endpoint in this Java 17 and Spring Boot banking project. Guides GitHub Copilot to implement the endpoint through the existing controller, service, repository, validation, and test layers without making frontend changes.'
---

# Add a backend REST endpoint

Use this skill when asked to create or extend a backend API endpoint in this repository. Implement backend code only; do not create or modify frontend/UI code unless the user explicitly requests it.

## Project-specific context

- Java 17, Maven, Spring Boot 2.7.x (`pom.xml`).
- Base package: `com.example.paul`.
- REST controllers: `src/main/java/com/example/paul/controllers/`.
- Business services: `src/main/java/com/example/paul/services/`.
- Spring Data repositories: `src/main/java/com/example/paul/repositories/`.
- JPA models: `src/main/java/com/example/paul/models/`.
- Request/input helpers and validation: `src/main/java/com/example/paul/utils/`.
- Existing endpoints use `/api/v1` and JSON. Follow nearby controller patterns and preserve existing API behavior unless the request explicitly asks to change it.
- Tests are under `src/test/java/com/example/paul/unit/` and `src/test/java/com/example/paul/integration/`.

## Workflow

1. **Understand the requested contract.** Identify the endpoint's purpose, HTTP method and path, request fields, response shape, expected status codes, and relevant business rules. Check existing controllers, services, inputs, models, repositories, and tests before editing. If critical requirements are ambiguous, ask a focused question rather than inventing banking behavior.
2. **Design a minimal layered change.** Put HTTP mapping and HTTP-level response handling in a `*RestController`; put business rules in a `*Service`; use repositories for persistence. Add a request/input class when needed, using the existing `*Input` naming pattern. Add a response DTO when returning an entity would expose persistence fields or create an unstable API contract.
3. **Implement request validation.** Use Jakarta/Javax validation APIs already available in this Spring Boot 2.7 project (`javax.validation`, as in nearby code), `@Valid`, and clear constraints where suitable. Follow existing `InputValidator` conventions when custom cross-field or domain validation is needed. Handle absent, malformed, out-of-range, and invalid identifiers/amounts explicitly. Never rely on client-side validation.
4. **Implement business logic safely.** Keep controllers thin and avoid duplicating domain rules. For banking operations, validate accounts and amounts, preserve invariants, prevent overdrafts or invalid state transitions unless the request explicitly defines otherwise, and avoid accidental double processing where relevant. Use a service-level transaction (`@Transactional`) when an operation requires multiple related database changes to succeed or fail together. Do not silently change existing transaction semantics.
5. **Use safe data and error handling.** Do not log account numbers, credentials, or other sensitive request data. Do not expose stack traces, internal exception details, or unnecessary entity fields in API responses. Return clear, consistent errors and appropriate HTTP status codes for new behavior, while maintaining compatibility with existing routes. Do not invent authentication/authorization infrastructure: inspect what exists and call out missing access control when relevant to the requested endpoint.
6. **Add tests.** Add controller tests using the repository's existing `@WebMvcTest` and `MockMvc` patterns for success, invalid/missing input, and relevant not-found or conflict cases. Add service tests for business rules and failure paths. Add an integration test when persistence or multiple layers need to be verified. Keep tests deterministic and do not depend on external services.
7. **Validate the change.** Run `./mvnw clean test`. Review the diff for unintended changes, API compatibility, sensitive logging, validation gaps, and tests that do not assert the endpoint contract. Report any commands that could not be run and why.

## Implementation constraints

- Keep the change limited to the requested backend endpoint and its necessary tests.
- Use constructor injection for new Spring components and dependencies; prefer it over field injection.
- Match nearby code style, package layout, API versioning, and JSON conventions.
- Do not add dependencies, alter the database schema, or refactor unrelated code unless required and explicitly justified.
- Do not return `200 OK` for every outcome by default. Choose status codes that match the specified contract, while preserving legacy behavior on existing endpoints.
- For monetary values, avoid introducing new floating-point calculations. Prefer `BigDecimal` for new monetary request/model logic where compatible with the existing design; do not make a broad migration of existing fields as part of an endpoint-only task.
- Keep endpoint implementation and test code readable, focused, and maintainable.

## Completion summary

When finished, summarize the endpoint method/path, the main backend layers/files changed, test coverage added, and whether `./mvnw clean test` passed. Mention assumptions or security limitations that could not be resolved from the repository.
