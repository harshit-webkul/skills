---
name: api-testing
description: >
  Professional REST and GraphQL API testing for Bagisto packages using
  Playwright and TypeScript. Use when testing Bagisto APIs, generating API
  automation, validating API contracts, verifying database changes, testing
  authentication or authorization, debugging API tests, or identifying API
  and package defects. Activates for requests to test, automate, validate,
  debug, or execute Bagisto REST or GraphQL APIs.
---

# APITestCraft — Bagisto API Testing

## Purpose

APITestCraft is an AI-agent skill for analyzing, generating, executing, and
debugging professional REST and GraphQL API automation for Bagisto packages
using Playwright and TypeScript.

The skill is execution-oriented. When the environment is available, do not
stop at documentation or test-case generation: generate the automation,
execute it, diagnose failures, and report the final result.

## When to Use

Use this skill when the user asks to:

- Test Bagisto REST APIs.
- Test Bagisto GraphQL APIs.
- Test both REST and GraphQL APIs.
- Generate Playwright API automation.
- Generate API test cases.
- Validate API request/response contracts.
- Verify API database persistence.
- Test authentication and authorization.
- Debug API automation.
- Investigate API/package defects.
- Run API regression tests for a Bagisto package.

Examples:

```text
Test the APIs of this Bagisto package.
```

```text
Generate and execute Playwright API tests for this module.
```

```text
Run REST and GraphQL API testing for this package.
```

## Invocation

This skill is normally activated automatically when the user's request matches
the description above.

If the user explicitly requests the skill, they may say:

```text
Use APITestCraft to test this package.
```

or:

```text
Use the api-testing skill.
```

The installation command is provided by the `skills` CLI and is not a custom
runtime command created by this skill.

## Execution Contract

When invoked, follow this lifecycle:

```text
INVOCATION
    ↓
DISCOVER WORKSPACE
    ↓
COLLECT REQUIRED INPUTS
    ↓
VALIDATE ENVIRONMENT
    ↓
ANALYZE PACKAGE WORKFLOW
    ↓
ANALYZE REST / GRAPHQL CONTRACTS
    ↓
GENERATE TEST CASES
    ↓
GENERATE PLAYWRIGHT API AUTOMATION
    ↓
CONFIGURE AUTH / DB / FIXTURES
    ↓
TYPE-CHECK
    ↓
EXECUTE TESTS
    ↓
CLASSIFY FAILURES
    ↓
FIX TEST/AUTOMATION DEFECTS
    ↓
REPORT PACKAGE DEFECTS
    ↓
FINAL EXECUTION
    ↓
REPORT PASS / FAIL / BLOCKED
```

Do not claim successful testing unless the tests were actually executed.

---

# 1. Discover the Workspace

Start by identifying the Bagisto host application and target package.

Inspect:

```bash
pwd
ls -la
```

Look for:

```text
artisan
composer.json
bootstrap/
config/
packages/
tests/
package.json
playwright.config.ts
```

For package discovery:

```bash
find packages -maxdepth 3 -name composer.json -type f
```

Confirm the target package exists before generating tests.

---

# 2. Required Inputs

Collect the information necessary to run the requested scope.

### Required where applicable

- Bagisto version.
- Package path/name.
- Host application.
- API scope: REST, GraphQL, or both.
- API endpoints/operations if known.
- Application URL.
- Database connection details when not already available from the project.
- Required admin/customer test accounts.

If the user requests "all APIs" or "complete package testing", discover the
candidate API surface from source code and runtime routes.

Do not invent:

- passwords
- API tokens
- database credentials
- test accounts
- application URLs

If a required value is unavailable, ask the user rather than guessing.

---

# 3. Environment Validation

Before execution, validate the project tooling:

```bash
php -v
composer --version
node -v
npm -v
```

When database connectivity is required, validate the configured database.

Inspect the project's existing:

```text
composer.json
package.json
.env
```

Do not expose secrets in output.

Do not change persistent environment configuration without appropriate user
authorization.

---

# 4. Package Workflow Analysis

Analyze the package before generating API tests.

Do not generate tests from route names alone.

Inspect, where present:

```text
packages/Webkul/<Package>/composer.json
packages/Webkul/<Package>/README.md
packages/Webkul/<Package>/src/
packages/Webkul/<Package>/routes/
packages/Webkul/<Package>/config/
packages/Webkul/<Package>/database/
packages/Webkul/<Package>/graphql/
```

Trace the implementation:

```text
API Entry Point
    ↓
Route
    ↓
Middleware
    ↓
Controller / Resolver
    ↓
Validation
    ↓
Service
    ↓
Repository / Model
    ↓
Events / Listeners / Jobs
    ↓
Database
    ↓
Response
```

Identify:

- happy paths
- alternate branches
- validation rules
- authorization rules
- state transitions
- database side effects
- asynchronous behavior
- response structure
- error behavior

---

# 5. REST API Analysis

For each REST endpoint, determine:

- HTTP method
- URI
- middleware
- authentication requirement
- authorization requirement
- headers
- request body
- required fields
- optional fields
- validation rules
- expected status codes
- response structure
- database side effects

Inspect the actual source implementation before deciding expected behavior.

Relevant status codes may include:

```text
200
201
204
400
401
403
404
409
422
429
500
```

Do not assume a status code when the source or API contract can establish it.

---

# 6. GraphQL API Analysis

For each GraphQL query or mutation, inspect:

```text
schema definitions
queries
mutations
input types
resolvers
validation
authorization
```

Determine:

- operation name
- query/mutation type
- arguments
- variables
- required/optional fields
- resolver
- authorization
- response shape
- error behavior
- database side effects

Verify runtime GraphQL failures through:

```text
errors
errors[].message
errors[].extensions
data
```

Prefer source/schema analysis over relying only on live introspection.

---

# 7. Security Coverage

For every applicable API, cover relevant security scenarios.

### Authentication

Test applicable cases:

- guest
- authenticated customer
- authenticated admin
- missing token
- invalid token
- expired token

### Authorization

Test applicable cases:

- unauthorized role
- ownership violation
- tampered resource ID
- cross-user access
- privilege escalation

### Input Security

Where applicable, test:

- unexpected fields
- malformed identifiers
- invalid input types
- excessive values
- injection-oriented inputs
- sensitive field exposure
- mass-assignment risks

Only report a vulnerability when supported by observed behavior and evidence.

---

# 8. Generate `TESTCASES.md`

Create or update:

```text
TESTCASES.md
```

Use traceable IDs:

```markdown
# API Test Cases — <Package>

## REST

| ID | Method | Endpoint | Category | Preconditions | Expected Result | DB Verification | Priority |
|----|--------|----------|----------|---------------|-----------------|-----------------|----------|
| TC-001 | POST | /api/... | Positive | ... | ... | Yes | High |

## GraphQL

| ID | Operation | Category | Preconditions | Expected Result | DB Verification | Priority |
|----|-----------|----------|---------------|-----------------|-----------------|----------|
| TC-002 | createX | Positive | ... | ... | Yes | High |
```

Each applicable API should include:

- positive coverage
- negative coverage
- security coverage

Each applicable state-changing test must define its expected database effect.

---

# 9. Playwright Architecture

Use:

```text
Playwright + TypeScript
```

unless the existing repository has an established compatible API-testing
architecture that should be preserved.

Recommended structure:

```text
api-test/
├── playwright.config.ts
├── tsconfig.json
├── api/
│   └── <Resource>Api.ts
├── graphql/
│   └── <Resource>Gql.ts
├── fixtures/
│   └── api.fixtures.ts
├── utils/
│   ├── auth.ts
│   ├── db.ts
│   └── seed.ts
├── tests/
│   ├── rest/
│   │   └── <resource>.rest.spec.ts
│   └── graphql/
│       └── <resource>.graphql.spec.ts
├── TESTCASES.md
├── BUGREPORT.md
└── test-results/
```

## Single Playwright Configuration

Maintain exactly one:

```text
playwright.config.ts
```

Use Playwright projects to separate API surfaces:

```typescript
projects: [
  {
    name: 'rest',
    testMatch: '**/tests/rest/**/*.spec.ts',
  },
  {
    name: 'graphql',
    testMatch: '**/tests/graphql/**/*.spec.ts',
  },
]
```

Run:

```bash
npx playwright test --project=rest
npx playwright test --project=graphql
```

or:

```bash
npx playwright test
```

---

# 10. API Page Object Model

Use API-oriented POMs.

REST:

```text
api/<Resource>Api.ts
```

GraphQL:

```text
graphql/<Resource>Gql.ts
```

POM classes must:

- receive `APIRequestContext`
- expose API actions
- build requests
- return responses/results
- remain reusable

POM classes must not:

- contain `expect()`
- contain test-specific assertions
- depend on `page`
- contain unrelated test state
- mix REST and GraphQL responsibilities

---

# 11. Test Specifications

REST:

```text
tests/rest/*.spec.ts
```

GraphQL:

```text
tests/graphql/*.spec.ts
```

Specs must:

- use POM classes
- contain assertions
- use descriptive test names
- reference test case IDs
- use authentication fixtures
- avoid hard-coded API URLs where configuration is available
- avoid duplicating API implementation logic

Example:

```typescript
test('TC-001: creates a reward and persists it', async ({ adminApi }) => {
  const rewardApi = new RewardApi(adminApi);

  const response = await rewardApi.create({
    points: 50,
  });

  expect(response.status()).toBe(201);

  const body = await response.json();

  expect(body.data).toBeDefined();

  const row = await checkRow('rewards', {
    id: body.data.id,
  });

  expect(row).not.toBeNull();
});
```

---

# 12. Authentication

Centralize authentication through:

```text
utils/auth.ts
fixtures/api.fixtures.ts
```

Provide reusable fixtures where required:

```text
adminApi
customerApi
guestApi
```

Do not repeatedly implement token acquisition inside individual specs.

Never hard-code credentials into source files.

---

# 13. Database Verification

For applicable state-changing API tests, verify the resulting database state.

Use:

```text
mysql2/promise
```

or the project's existing approved database utility.

Verify, where applicable:

- inserted records
- updated values
- deleted records
- relationships
- counters
- status changes
- side effects

Use parameterized queries.

For asynchronous operations, prefer condition-based polling:

```typescript
await expect
  .poll(async () => checkRow('table_name', { id }))
  .not
  .toBeNull();
```

Avoid arbitrary sleeps when polling can express the required condition.

---

# 14. Test Data

Test data must be deterministic and as idempotent as practical.

Prefer:

- controlled seed scripts
- existing project fixtures
- `php artisan tinker` when appropriate
- database setup utilities

Do not create arbitrary PHP files inside the Playwright test directory solely
for seeding.

Do not continuously duplicate data on every test run.

---

# 15. Validate and Execute

Before running the suite:

```bash
npx tsc --noEmit
```

Then execute the applicable projects:

```bash
npx playwright test --project=rest
npx playwright test --project=graphql
```

or:

```bash
npx playwright test
```

The agent must execute the generated or modified tests before reporting success.

---

# 16. Failure Classification

Classify failures before modifying anything.

## A. Test Expectation Defect

The assertion does not match the actual source-level API contract.

Action:

```text
Fix the test.
```

## B. Environment/Setup Defect

Examples:

- incorrect URL
- missing environment variable
- invalid test account
- missing seed data
- unavailable database
- missing package registration

Action:

```text
Fix setup and rerun.
```

## C. Automation Defect

Examples:

- incorrect fixture
- incorrect POM
- invalid request construction
- TypeScript error
- incorrect matcher

Action:

```text
Fix automation and rerun.
```

## D. Package/API Defect

Examples:

- incorrect response
- incorrect status code
- missing validation
- authorization failure
- incorrect database persistence
- unexpected sensitive data
- GraphQL resolver/schema defect

Action:

```text
Do not modify package source.
Create or update BUGREPORT.md.
```

---

# 17. Bug Reporting

For confirmed package defects, create/update:

```text
BUGREPORT.md
```

Use:

```markdown
# API Bug Report

## BUG-001 — <Short Description>

- Severity: High
- Test Case: TC-XXX
- API: <METHOD> <URI>
- Surface: REST / GraphQL

### Reproduction

<Exact request/variables/body and required auth context>

### Expected

<Expected behavior based on the implementation/contract>

### Actual

<Observed behavior>

### Database State

<Relevant database evidence, if applicable>

### Evidence

- Source: `path/to/file.php:<line>`
- Test: `tests/rest/example.spec.ts:<line>`

### Root Cause

<Source-level explanation>

### Status

Open
```

Never change package business logic to make the test pass.

---

# 18. Source Modification Policy

## Allowed

The agent may create or modify test-related files such as:

```text
Playwright specs
API POMs
GraphQL POMs
fixtures
test utilities
seed/test-data utilities
playwright.config.ts
tsconfig.json
TESTCASES.md
BUGREPORT.md
CI configuration when requested
```

## Protected

Do not modify Bagisto package business logic merely to satisfy tests:

```text
controllers
services
repositories
models
migrations
GraphQL resolvers
GraphQL schema
API routes
package business logic
```

If these contain the defect, report it.

---

# 19. Safety Rules

The agent must:

- Never invent credentials.
- Never expose secrets.
- Never commit credentials or tokens.
- Never delete production data.
- Never run destructive database commands without appropriate confirmation.
- Never disable failing tests merely to get a green build.
- Never use skip to hide a confirmed defect without a documented reason.
- Never reduce coverage to make the suite pass.
- Never modify package source to satisfy an assertion.
- Never claim tests passed without execution evidence.

---

# 20. CI/CD

If CI/CD is requested, or the repository already has CI configuration that
should be extended, support:

```text
Install dependencies
    ↓
Prepare Bagisto
    ↓
Register package
    ↓
Prepare test data
    ↓
Start application
    ↓
Install Playwright
    ↓
Run REST tests
    ↓
Run GraphQL tests
    ↓
Publish results
    ↓
Publish Playwright report
```

Do not create a second Playwright configuration.

---

# 21. Coverage

The final report must distinguish actual coverage from missing coverage.

Report:

### Workflow Coverage

- workflows analyzed
- workflows tested
- workflows not tested

### REST Coverage

- endpoints tested
- methods tested
- positive cases
- negative cases
- security cases
- DB-verified mutations

### GraphQL Coverage

- queries tested
- mutations tested
- positive cases
- negative cases
- security cases
- DB-verified mutations

### Missing/Blocked Coverage

Explicitly list:

- APIs not tested
- unavailable credentials
- unavailable environment
- unsupported operations
- blocked dependencies

Never claim 100% coverage unless it was actually established.

---

# 22. Final Result

The final response must be concise and professional.

Use:

```text
## APITestCraft Execution Summary

### Environment
- Bagisto: <version>
- Package: <package>
- Base URL: <url>
- API Surface: REST / GraphQL / Both

### Test Results

REST:
- Passed: X
- Failed: Y
- Skipped: Z

GraphQL:
- Passed: X
- Failed: Y
- Skipped: Z

### Coverage
- REST endpoints tested: X
- GraphQL operations tested: X
- DB-verified mutations: X
- Workflows covered: X

### Generated/Updated
- TESTCASES.md
- REST POMs/specs
- GraphQL POMs/specs
- playwright.config.ts
- Fixtures/utilities
- BUGREPORT.md

### Defects
- BUG-001: <summary>

### Status
PASS / FAIL / BLOCKED

Reason:
<concise explanation>
```

`PASS` means the applicable executed tests passed.

`FAIL` means confirmed test failures remain.

`BLOCKED` means execution could not proceed because a required dependency,
credential, environment, or user decision was unavailable.

---

# 23. Agent Decision Rules

Use this decision tree:

```text
User asks to test Bagisto APIs
        ↓
Is api-testing applicable?
        │
       Yes
        ↓
Discover package
        ↓
Are required inputs available?
        │
   ┌────┴────┐
   No        Yes
   │          │
Ask user    Continue
              ↓
       Analyze implementation
              ↓
       REST / GraphQL scope
              ↓
       Generate test cases
              ↓
       Generate automation
              ↓
       Type-check
              ↓
       Execute
              ↓
       Failure?
       ┌──────┴──────┐
      No             Yes
       │              │
    Report          Classify
    success            │
                 ┌─────┼─────┐
                 │     │     │
              Setup  Test   Package
                 │     │     │
               Fix   Fix  Report
                 │     │     │
                 └─────┴─────┘
                       ↓
                    Rerun
                       ↓
                 Final report
```

The skill should prefer evidence from the Bagisto package source, runtime
responses, test execution, and database state over assumptions.
