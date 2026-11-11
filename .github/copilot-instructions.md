# GitHub Copilot Instructions for Secure_PR_review

## Project overview
This repository is a Java 17 and Spring Boot 2.7.0 banking backend example built with Maven. It provides account and transaction APIs, including account creation, balance checks, deposits, withdrawals, and transfers.

The code follows a layered structure under `src/main/java/com/example/paul`:
- `controllers/` for REST endpoints
- `services/` for business logic
- `repositories/` for Spring Data persistence
- `models/` for domain entities
- `utils/` for input types, validation, and helpers
- `src/main/resources/` for application configuration and SQL initialization

## Technology and compatibility
- Use Java 17.
- Keep changes compatible with Spring Boot 2.7.0 and dependencies declared in `pom.xml`.
- Use Maven Wrapper (`./mvnw`) for local builds and tests. Do not assume Maven is globally installed.
- Keep code in the existing `com.example.paul` package hierarchy.
- Avoid new frameworks and dependencies unless required by the requested change.

## Architecture and coding conventions
- Keep controllers focused on HTTP request/response concerns; implement business rules in services.
- Use repository interfaces for database access. Avoid embedding persistence logic in controllers.
- Follow existing class naming: `*RestController`, `*Service`, `*Repository`, and `*Input`.
- Use constructor injection for new Spring components and dependencies; prefer it over field injection.
- Match nearby code style and favor straightforward, readable Java over unnecessary abstractions.
- Keep changes scoped to the requested feature; do not perform unrelated refactors.

## API and banking behavior
- Existing endpoints use the `/api/v1` prefix and JSON request/response bodies. Preserve existing routes and behavior unless the task specifically requests an API change.
- For a new or modified operation, validate request data, apply business rules in the service layer, access persistence through repositories, and return a clear HTTP response.
- Validate account identifiers and financial inputs on the server. Reject missing or malformed identifiers and non-positive or otherwise invalid amounts as appropriate to the operation.
- Treat account balances, account identifiers, owner details, and transaction metadata as sensitive. Do not log sensitive request or account data.
- Do not silently change existing transaction or balance semantics. For operations that update multiple related records, consider transaction boundaries and consistency.
- Avoid exposing JPA entities or internal error details in new public API responses when a suitable response DTO is needed.

## Validation and error handling
- Reuse the existing validation patterns in `utils/` where appropriate, and use Bean Validation annotations with `@Valid` for request types when suitable. This project uses `javax.validation` with Spring Boot 2.7.
- Fail clearly on invalid input and preserve established error behavior for existing endpoints.
- Use appropriate HTTP status codes for new endpoints. Do not assume every outcome should return `200 OK`.
- Avoid broad catch blocks and do not return stack traces or sensitive implementation details.

## Tests
- Add or update unit/controller tests under `src/test/java/com/example/paul/unit/`.
- Add or update integration tests under `src/test/java/com/example/paul/integration/` when verifying full API/persistence behavior.
- Follow the existing JUnit and Spring test patterns, including `MockMvc` for controller behavior.
- Cover success, validation failure, and relevant not-found/business-rule cases.

## Build and verification
The GitHub Actions workflow at `.github/workflows/maven.yml` sets up Java 17 and runs:

```bash
mvn -B package --file pom.xml
```

For local work, use the Maven Wrapper equivalents:

```bash
./mvnw -B package --file pom.xml
```

For a focused test run or a clean test/build, use:

```bash
./mvnw test
./mvnw clean test
```

Do not claim that a build or tests passed unless the command was actually run and its result is known. If execution is unavailable, state that validation could not be performed.

## Docker and local runtime
The README documents building and running the container with:

```bash
docker build -t "spring-boot:banking" .
docker run -p 8080:8080 spring-boot:banking
```

The current local application configuration uses an in-memory H2 database and port 8080. Treat the H2 console and demo configuration as development-oriented; do not expose them as production-safe settings without explicit security and deployment configuration.

## GitHub Copilot guidance
- Generate code specifically for GitHub Copilot assistance in this Java/Spring Boot repository.
- Follow this instruction file and the nearest existing implementation; inspect related controller, service, repository, input, and test classes before making changes.
- Prefer minimal, maintainable changes that fit this project over broad rewrites or unrelated framework migrations.
- Include or update tests for behavior changes and report the exact validation command and outcome.
