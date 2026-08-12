# AGENTS.md

## Repository

This repository contains one Agent Skill:

```text
skills/api-testing/SKILL.md
```

The skill name is:

```text
api-testing
```

The skill's branded name is:

```text
APITestCraft
```

## Source of Truth

Treat:

```text
skills/api-testing/SKILL.md
```

as the authoritative definition of the API testing workflow.

Do not duplicate or contradict the detailed workflow in other repository files.

## Repository Structure

Maintain:

```text
/
├── skills/
│   └── api-testing/
│       └── SKILL.md
├── AGENTS.md
└── README.md
```

Do not add unrelated skills unless explicitly requested.

## Skill Scope

The `api-testing` skill provides AI-agent instructions for:

- Bagisto REST API testing.
- Bagisto GraphQL API testing.
- Playwright API automation.
- API test-case generation.
- API POM architecture.
- Authentication and authorization testing.
- Database verification.
- API automation debugging.
- API/package defect reporting.

## Skill Frontmatter

The `SKILL.md` frontmatter must retain:

```yaml
---
name: api-testing
description: ...
---
```

The description must clearly explain when the skill should activate.

Do not remove or weaken the activation description without an explicit
request.

## Agent Behavior

When working on the skill:

1. Preserve the single-skill repository structure.
2. Keep `SKILL.md` executable and agent-oriented.
3. Prefer explicit instructions over vague documentation.
4. Keep the workflow focused on Bagisto API testing.
5. Do not introduce unrelated tooling or skills.
6. Keep REST and GraphQL testing responsibilities clear.
7. Preserve the package-source protection rules.
8. Preserve the requirement to execute tests before claiming success.

## Testing Rules

The skill should use:

- Playwright
- TypeScript
- `APIRequestContext`
- REST API POMs
- GraphQL POMs
- Authentication fixtures
- Database verification

Maintain one Playwright configuration for the generated API test suite.

Use separate Playwright projects for:

```text
rest
graphql
```

## Package Source Protection

Never modify Bagisto package business logic merely to make an API test pass.

Protected package areas include:

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

If the package implementation is responsible for a failure, the agent must
report the defect instead.

## Allowed Skill Changes

When explicitly requested, the agent may modify:

```text
skills/api-testing/SKILL.md
README.md
AGENTS.md
```

and may add skill-supporting documentation/templates when needed.

Do not create additional skills unless explicitly requested.

## Security

Never:

- Commit passwords.
- Commit API tokens.
- Expose secrets in documentation.
- Invent credentials.
- Use production credentials as test data.
- Run destructive operations against production data.
- Hide test failures to obtain a green result.

## Validation

When changing `SKILL.md`:

1. Validate YAML frontmatter.
2. Verify the skill name is `api-testing`.
3. Verify the description is meaningful and activation-oriented.
4. Verify all referenced paths exist.
5. Keep examples consistent with the actual skill.
6. Check that the repository remains discoverable by the `skills` CLI.

## Final Review Checklist

Before considering repository changes complete:

```text
[ ] skills/api-testing/SKILL.md exists
[ ] SKILL.md has valid YAML frontmatter
[ ] name is api-testing
[ ] description explains activation conditions
[ ] README.md documents installation
[ ] README.md documents usage
[ ] AGENTS.md contains repository-level instructions
[ ] No unrelated skills were added
[ ] No credentials or secrets were added
[ ] Package-source protection is preserved
```
