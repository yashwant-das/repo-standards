# Folder structure standard

Required conventions follow; layouts marked “should” are defaults.

## Shared conventions

Create directories only when they hold something useful. A standard layout is a
navigation aid, not a requirement to populate empty folders. Every repository
must include its profile's required Markdown files as defined in the
[documentation standard](documentation.md#documentation-profiles-and-minimum-markdown-baseline).

| Path | Purpose |
| --- | --- |
| `README.md` | Project entry point, badges, highlights, root Mermaid diagram, structure/stack overview and quickstart |
| `AGENTS.md` | Agent instructions, pinned `repo-standards` revision and documentation profile (`CLAUDE.md` may point here) |
| `LICENSE` | Repository license (where applicable) |
| `docs/architecture.md` | Component design, detailed Mermaid diagrams, lifecycle boundaries and trade-offs (`complete` profile) |
| `docs/development.md` | Setup, configuration matrix, quality gate commands, troubleshooting and adoption/exception records (`complete` profile) |
| `docs/assets/` | Screenshots, GIFs and exported documentation diagrams |
| `scripts/` or `tooling/` | Development, verification and maintenance helpers |
| `.github/workflows/` | SHA-pinned GitHub Actions workflows |
| `tests/` | Automated tests where the language or tool convention uses this path |

Consolidate extra uppercase root Markdown files (such as
`FRAMEWORK_ARCHITECTURE.md`, `TESTING_GUIDELINES.md`, `DESIGN.md` or
`PRODUCT.md`) into lowercase kebab-case guides under `docs/`. Retain standard
root community/project files (`CONTRIBUTING.md`, `SECURITY.md`, `CHANGELOG.md`,
`ROADMAP.md`) and intentional historical artifacts (such as
`ARCHITECTURE-GUIDE.md`) only when they serve a distinct purpose.

Keep manifests and tool configuration where the tools expect them. Keep secrets,
virtual environments, `node_modules`, browser state, reports, logs and build
output out of Git. A deliberately committed example fixture or recorded demo
replay is different from a run artifact. Document safe sample configuration and
provide `.env.example` if useful.

Preserve functional folders such as `tasks/`, `specs/`, `prompts/`, `screens/`,
agent definitions, `locustfiles/`, `target-app/`, `grafana/` provisioning or
Appium capabilities. Explain them in `README.md` and `docs/architecture.md`
rather than renaming them to make different projects look identical.

## Python

Packaged Python applications and libraries (such as `ai-py-playwright-workbench`,
`qa-py-selenium-requests`, `ai-py-llm-sandbox`, `ai-py-mlx-playground`) should
use:

```text
pyproject.toml
uv.lock
README.md
AGENTS.md
src/
  package_name/
tests/
docs/
  architecture.md
  development.md
```

Test-runner or multi-service Python projects (such as `qa-py-browserstack`,
`qa-py-selenium`, `qa-py-locust-grafana`) may keep top-level functional
directories (`pages/`, `tests/`, `locustfiles/`, `target-app/`, `grafana/`) at
the repository root when that matches the runner or Docker Compose layout.

Use `pyproject.toml` for project metadata and tooling, and uv for dependency
management. Existing pinned requirements workflows may remain as documented
exceptions under the dependency standard. Pytest fixtures and helpers belong
near the tests that use them; avoid a catch-all utilities directory when
refactoring.

## Java

Maven projects should follow Maven's source layout:

```text
pom.xml
mvnw
.mvn/
README.md
AGENTS.md
src/
  main/java/
  main/resources/
  test/java/
  test/resources/
docs/
  architecture.md
  development.md
```

Test-only frameworks (`qa-java-playwright`) may omit `src/main/`. Multi-module
repositories that bundle a sample system under test alongside the test suite
(`qa-java-restassured-payments`) keep each module in its own top-level directory
(such as `automation/` and `backend/`) with `docs/` at the repository root. Keep
source paths aligned with package declarations and separate bundled sample
application code from its test clients. Prefer a Maven wrapper for repeatable
builds; pin dependency and build-plugin versions in the POM.

Java QA frameworks use `com.qaframework` as the Maven `groupId` and
`com.qaframework.<project>` as the base package. Use short, descriptive lowercase
project names such as `playwright`, `selenium` or `payments`, with functional
subpackages such as `config`, `pages`, `listeners` and `tests`. Avoid personal
names, redundant language segments and unclear abbreviations. Keep artifact IDs
descriptive, such as `qa-java-playwright`.

Other owners’ and client repositories retain their approved namespaces unless
they opt into this convention.

## JavaScript and TypeScript

Application and library projects normally use `src/` and `tests/`. Test-only
Playwright, WebdriverIO/Appium or Promptfoo projects (`ai-ts-playwright-protocol`,
`ai-js-appium-mcp`, `ai-js-promptfoo-eval`, `qa-js-mysql`) may use runner-native
top-level directories (`tests/`, `pages/`, `screens/`, `prompts/`, `tasks/`,
`specs/`, `support/`) without `src/`. Follow the established runner convention
rather than forcing application folders:

```text
package.json
package-lock.json
playwright.config.ts
README.md
AGENTS.md
tests/
pages/
fixtures/
docs/
  architecture.md
  development.md
```

The example is for a Playwright test project; do not add Playwright files to
unrelated apps. Mixed Python/Node repositories (`ai-py-playwright-workbench` at
root, or `qa-js-mysql` with `python/`) keep each manifest at its real package
root and explain in `README.md` and `docs/architecture.md` which runtime serves
which component. Do not introduce a monorepo tool merely to align layouts.

## Other projects and migration

Swift apps (`meters`), portfolios (`yashwant-das.github.io`), resume pipelines
(`resume`) and reference repositories (`code-reference`) retain their platform
or content conventions while adopting the required documentation profile and
explaining unusual paths.

Existing projects migrate incrementally when changing related areas. Moving
files requires updating imports, tooling, CI and documentation links together.
For Java package changes, also update suite/listener names, package scans,
logging and Maven module references. Review documentation, diagrams and links as
part of migration; no shared validator is required.
