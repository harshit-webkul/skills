# Bagisto Agent Skills

This repository contains AI-agent skills for [Bagisto](https://github.com/bagisto/bagisto) development, compatible with [Agent Skills](https://github.com/bagisto/agent-skills) CLI. These skills can be installed in opencode, Claude Code, Cursor, and other supported AI coding agents.

## Available Skills

| Skill | Name | Description |
|-------|------|-------------|
| **APITestCraft** | `api-testing` | Professional REST and GraphQL API testing for Bagisto packages using Playwright and TypeScript. Generates test cases, Playwright automation, verifies database persistence, and reports package defects. |
| **Bagisto Blog Writer** | `bagisto-blog` | Writes deep-dive, step-by-step blog posts for Bagisto modules in WordPress Gutenberg HTML format. Analyzes module source, captures real screenshots via Playwright, seeds demo data, and mirrors a reference blog structure exactly. |

---

## Installation

Install skills directly from this GitHub repository using the `skills` CLI:

```bash
# Install a specific skill
npx skills add <YOUR_GITHUB_USERNAME>/<YOUR_REPOSITORY> --skill <skill-name>

# Install all skills from the repository
npx skills add <YOUR_GITHUB_USERNAME>/<YOUR_REPOSITORY>
```

### Examples

```bash
# Install APITestCraft (api-testing skill)
npx skills add harshit-webkul/skills --skill api-testing

# Install Bagisto Blog Writer (bagisto-blog skill)
npx skills add harshit-webkul/skills --skill bagisto-blog

# Install both skills
npx skills add harshit-webkul/skills
```

### Install for a Specific Agent

```bash
# For opencode
npx skills add harshit-webkul/skills -a opencode

# For Claude Code
npx skills add harshit-webkul/skills -a claude-code

# For Cursor
npx skills add harshit-webkul/skills -a cursor

# Install specific skill for specific agent
npx skills add harshit-webkul/skills --skill api-testing -a claude-code
```

### Verify Installation

List installed skills:

```bash
npx skills list
```

---

## Usage

After installation, open your Bagisto project in your AI coding agent and use the skills naturally:

### APITestCraft (api-testing)

```text
Test the REST and GraphQL APIs of this Bagisto package.
```

```text
Generate and execute Playwright API tests for this Bagisto module.
```

```text
Validate the package APIs and verify the database changes.
```

The agent will automatically discover and use the `api-testing` skill when your request matches API testing for Bagisto.

#### Explicit Invocation

```text
Use the api-testing skill to test this Bagisto package.
```

```text
Use APITestCraft for API testing.
```

---

### Bagisto Blog Writer (bagisto-blog)

```text
Write a blog post for the Marketplace module using this reference blog as structure.
```

```text
Create documentation article for the Stripe payment module according to this blog template.
```

The agent will use the `bagisto-blog` skill when you ask to write a blog/blog post/documentation article for a Bagisto module and reference a sample blog file.

#### Required Information

When using the blog skill, the agent will ask for:

1. **Module/package name** — e.g., `Marketplace`, `Stripe`, `RMA`, `CMS`
2. **Reference blog path** — absolute path to the `.html` (WordPress Gutenberg) file to mirror
3. **Module version / Bagisto version** — from `composer.json`
4. **Output path** — where to save the final `.html` file
5. **App running status** — whether the Bagisto app is accessible for screenshots (base URL + admin credentials)

---

## Repository Structure

```text
skills/
├── api-testing/
│   ├── SKILL.md       # APITestCraft skill definition
│   ├── README.md      # Skill-specific documentation
│   └── AGENTS.md      # Agent behavior rules
├── webkul-blog/
│   └── SKILL.md       # Bagisto Blog Writer skill definition
├── AGENTS.md          # Repository-level agent instructions
��── README.md          # This file
```

---

## Skill Details

### APITestCraft (`api-testing`)

**Capabilities:**
- Analyze Bagisto package workflow
- Discover REST and GraphQL APIs
- Generate API test cases (`TESTCASES.md`)
- Generate Playwright + TypeScript API automation
- Test authentication and authorization
- Verify database persistence
- Execute API tests
- Diagnose test and automation failures
- Identify genuine package/API defects (`BUGREPORT.md`)
- Produce execution and coverage summary

**Technology Stack:**
- Bagisto / Laravel
- Playwright + TypeScript
- REST API / GraphQL
- MySQL

**Full Skill Definition:** `skills/api-testing/SKILL.md`

---

### Bagisto Blog Writer (`bagisto-blog`)

**Capabilities:**
- Parse reference blog structure (headings, code blocks, images, lists)
- Deep module analysis (routes, controllers, models, config, views, migrations)
- Plan blog outline with screenshot mapping
- Capture real screenshots via Playwright (1120×880 WebP)
- Audit and seed demo data before screenshots
- Write WordPress Gutenberg HTML (`<!-- wp:... -->` block markup)
- Step-by-step instructions for every feature
- SEO and readability quality checks

**Output:**
- Final `.html` file (WordPress Gutenberg format)
- Screenshot folder with `.webp` images
- Module Analysis Notes document
- Seeded data summary

**Full Skill Definition:** `skills/webkul-blog/SKILL.md`

---

## Requirements

- Node.js (for `npx skills` CLI)
- Bagisto project (for skill execution)
- Playwright (installed by skills when needed)
- PHP / Composer (for Bagisto environment)

---

## License

Add your preferred license here.