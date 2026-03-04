# Contributing to dojops-dops-tools

Thank you for contributing to the DojOps community module collection!

## Prerequisites

- [Node.js](https://nodejs.org/) >= 20
- [DojOps CLI](https://github.com/dojops/dojops) (for testing tools)
- Familiarity with the [.dops v2 format](https://github.com/dojops/dojops/blob/main/docs/tools.md)

## Categories

Place your module in the appropriate directory:

| Directory | Description |
|-----------|-------------|
| `ci-cd/` | CI/CD pipelines, cloud deployment, GitOps |
| `containers/` | Container runtimes, orchestration, reverse proxies |
| `monitoring/` | Metrics, logging, tracing, alerting |
| `security/` | Secrets, policies, scanning, compliance |

## Creating a New Module

### 1. Scaffold

Create `<category>/<module-name>.dops` following the v2 format:

```yaml
---
dops: v2
kind: tool

meta:
  name: my-tool          # lowercase, hyphens only
  version: 2.0.0
  description: "Generate <tool> configurations"
  author: your-github-username
  license: MIT
  tags: [relevant, tags]

context:
  technology: Tool Name
  fileFormat: yaml       # yaml, json, hcl, raw, toml
  outputGuidance: |
    Generate valid <tool> configuration.
    Output raw content directly — do NOT wrap in JSON or code fences.
  bestPractices:         # At least 5 entries
    - Practice one
    - Practice two
    - Practice three
    - Practice four
    - Practice five
  context7Libraries:
    - name: library-name
      query: "relevant documentation query"

files:
  - path: "output/path/file.ext"
    format: raw

detection:
  paths: ["pattern/*.ext"]
  updateMode: true

verification:
  structural:
    - path: "required.key"
      required: true
      message: "Missing required key"

permissions:
  filesystem: write
  child_process: none
  network: none

scope:
  write: ["output/path/*"]

risk:
  level: LOW             # LOW, MEDIUM, or HIGH
  rationale: "Why this risk level"

execution:
  mode: generate
  deterministic: false
  idempotent: true

update:
  strategy: replace
  inputSource: file
  injectAs: existingContent
---
```

### 2. Markdown Body

After the frontmatter, include these required sections:

- **`## Prompt`** — Expert role description with template variables: `{outputGuidance}`, `{bestPractices}`, `{context7Docs}`, `{projectContext}`
- **`## Keywords`** — Technology name and aliases for agent routing

> **Note:** Update mode is handled automatically by the v2 prompt compiler. Best practices and constraints should be defined in `context.bestPractices` in the frontmatter.

### 3. Validate

```bash
npm install js-yaml
node scripts/validate.mjs
```

### 4. Test with DojOps CLI

```bash
dojops modules validate <category>/<module-name>.dops
dojops --module <category>/<module-name>.dops "test prompt"
```

## Quality Checklist

- [ ] `meta.name` matches filename (without `.dops` extension)
- [ ] `meta.version` is `2.0.0`
- [ ] `context.bestPractices` has at least 7 entries (include both best practices and constraints)
- [ ] `verification` section uses structural or binary checks
- [ ] `risk.level` has an accurate `rationale`
- [ ] `## Keywords` includes the module name and common aliases
- [ ] `node scripts/validate.mjs` passes

## Pull Request Naming

Use the format: `modules(<category>): add <module-name>`

Examples:
- `modules(ci-cd): add jenkins`
- `modules(monitoring): add grafana`
- `modules(security): add vault`

## Code of Conduct

Be respectful and constructive. Focus on quality modules that help the community.
