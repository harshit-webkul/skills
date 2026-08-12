# APITestCraft

**APITestCraft** is an AI-agent skill for professional REST and GraphQL API
testing of Bagisto packages using Playwright and TypeScript.

It is distributed as an [Agent Skills](https://github.com/bagisto/agent-skills)
compatible repository so AI coding agents can discover and install the skill
using the `skills` CLI.

## What APITestCraft Does

APITestCraft helps an AI agent:

- Analyze a Bagisto package workflow.
- Discover REST and GraphQL APIs.
- Generate API test cases.
- Generate Playwright + TypeScript API automation.
- Test authentication and authorization.
- Verify database persistence.
- Execute API tests.
- Diagnose test and automation failures.
- Identify genuine package/API defects.
- Generate `TESTCASES.md`.
- Generate `BUGREPORT.md`.
- Produce an execution and coverage summary.

## Installation

Install the APITestCraft skill directly from this GitHub repository:

```bash
npx skills add <YOUR_GITHUB_USERNAME>/<YOUR_REPOSITORY> --skill api-testing
```

For example:

```bash
npx skills add harshit-webkul/api-testing-agent --skill api-testing
```

You can also install all skills from the repository:

```bash
npx skills add <YOUR_GITHUB_USERNAME>/<YOUR_REPOSITORY>
```

Install for a specific supported agent:

```bash
npx skills add <YOUR_GITHUB_USERNAME>/<YOUR_REPOSITORY> -a claude-code
```

```bash
npx skills add <YOUR_GITHUB_USERNAME>/<YOUR_REPOSITORY> -a cursor
```

The `skills` CLI discovers skills under the repository's `skills/` directory.

### Verify Installation

List installed skills:

```bash
npx skills list
```

After installation, open your Bagisto project in your AI coding agent and ask:

```text
Test the REST and GraphQL APIs of this Bagisto package.
```

The agent should automatically discover and use the `api-testing` skill.

## Repository Structure

```text
api-testing-agent/
├── skills/
│   └── api-testing/
│       └── SKILL.md
├── AGENTS.md
└── README.md
```

`skills/api-testing/SKILL.md` is the actual APITestCraft skill.

## Automatic Activation

After installation, the AI agent should use the skill when the user's request
matches the skill description.

For example:

```text
Test the APIs of this Bagisto package.
```

```text
Generate and execute Playwright API tests for this Bagisto module.
```

```text
Run REST and GraphQL API testing for this package.
```

```text
Validate the package APIs and verify the database changes.
```

The user does not need to manually copy `SKILL.md` into the project.

## Explicit Skill Request

The user can also explicitly ask:

```text
Use the api-testing skill to test this Bagisto package.
```

or:

```text
Use APITestCraft for API testing.
```

## APITestCraft Workflow

```text
User Request
     ↓
AI Agent
     ↓
api-testing Skill
     ↓
Discover Bagisto Package
     ↓
Analyze Package Workflow
     ↓
Analyze REST / GraphQL APIs
     ↓
Generate TESTCASES.md
     ↓
Generate Playwright API Automation
     ↓
Configure Auth / DB / Fixtures
     ↓
Type-check
     ↓
Execute Tests
     ↓
Database Verification
     ↓
Classify Failures
     ↓
Fix Automation Defects
     ↓
Report Package Defects
     ↓
Final Execution
     ↓
PASS / FAIL / BLOCKED
```

## Generated Test Architecture

The skill uses an API-oriented Playwright architecture:

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
└── tests/
    ├── rest/
    └── graphql/
```

REST and GraphQL tests are separated into Playwright projects while maintaining
one `playwright.config.ts`.

## Test Coverage

APITestCraft aims to cover, where applicable:

- Positive API scenarios.
- Negative/validation scenarios.
- Authentication.
- Authorization.
- Security-related scenarios.
- Database persistence.
- State transitions.
- REST endpoints.
- GraphQL queries.
- GraphQL mutations.
- API workflow behavior.

The final report distinguishes tested, missing, and blocked coverage.

## Package Source Protection

APITestCraft does not modify Bagisto package business logic to make tests pass.

If a failure is caused by the package implementation, the agent reports it in:

```text
BUGREPORT.md
```

Test automation and test-support files may be created or updated as required.

## Result Status

The skill reports one of:

| Status | Meaning |
|---|---|
| `PASS` | Applicable tests were executed and passed. |
| `FAIL` | Confirmed test failures remain. |
| `BLOCKED` | Execution could not proceed because a required dependency, credential, environment, or decision was unavailable. |

## Technology

- Bagisto
- Laravel
- Playwright
- TypeScript
- REST API
- GraphQL
- MySQL

## Skill Definition

The complete agent workflow is defined in:

```text
skills/api-testing/SKILL.md
```

## License

Add your preferred license here.
