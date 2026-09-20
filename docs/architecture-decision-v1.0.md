# Skill Architecture Decision

## 1. User Goal

Provide a reusable governance gate that decides how a Skill, Plugin, MCP integration, or App should be packaged, distributed, and tested before implementation, then verifies it again before release.

## 2. Target Surfaces

| Surface | MUST / SHOULD / NOT REQUIRED | Notes |
|---|---|---|
| Web Chat | NOT REQUIRED | No direct local Skill loading is assumed. |
| Desktop Chat | NOT REQUIRED | Not a release target for v1.0. |
| Web Work | SHOULD | Can use an appropriately registered standalone Skill in a future distribution. |
| Desktop Work | MUST | Primary interactive governance surface. |
| Codex | MUST | Primary local development surface. |
| Mobile | NOT REQUIRED | No mobile-specific packaging in v1.0. |

## 3. Required Capabilities

- Instructions/templates only: Yes.
- External data: No runtime dependency.
- External actions: No runtime dependency.
- Authentication: None.
- Local filesystem: Only for installation and reading project evidence during governed work.
- Shell: Not required by the Skill itself.
- Persistent service: None.

## 4. Packaging Choice

- [x] Standalone Skill
- [ ] Skill-only Plugin
- [ ] MCP App
- [ ] Skill + MCP Plugin

Reason: the capability is a decision and verification workflow. It does not expose tools, data, or external actions.

## 5. Distribution Choice

- [x] Local
- [ ] Cloud standalone Skill
- [ ] Workspace-private Plugin
- [ ] Public Plugin
- [ ] Developer-mode MCP App

Reason: v1.0 is installed as one local standalone Skill. GitHub is the public canonical source, not a Plugin installation identity.

## 6. Invocation Strategy

- Explicit: `$skill-governance` in Codex; `@Skill Governance` where a registered standalone Skill is supported.
- Implicit: enabled through `agents/openai.yaml` and a discriminating description.
- Trigger description: creation, modification, packaging, migration, or release of a Skill, Plugin, MCP integration, or App.

## 7. Portability Constraints

- Absolute paths: none in Skill instructions or references.
- localhost: none.
- shell: none required at runtime.
- local cache: none required.
- external service availability: none required.

## 8. Risks

- Surface compatibility: local installation does not imply Web Chat availability.
- Privacy: governance records may cite local project evidence; they must not be published automatically.
- Maintenance: global `AGENTS.md` must remain minimal and should not override unrelated work.
- Version drift: GitHub is canonical; local installs must be compared with its release commit.

## 9. Test Matrix

| Surface | Discovery | Registration | Loading | Execution |
|---|---|---|---|---|
| Web Chat | N/A | N/A | N/A | N/A |
| Desktop Chat | N/A | N/A | N/A | N/A |
| Web Work | FUTURE | FUTURE | FUTURE | FUTURE |
| Desktop Work | REQUIRED | REQUIRED | REQUIRED | REQUIRED |
| Codex | REQUIRED | REQUIRED | REQUIRED | REQUIRED |

Behavior tests include explicit invocation, natural-language routing, non-triggering ordinary work, incomplete architecture evidence, the approval stop gate, and the post-implementation Release Gate.

## 10. Final Decision

- Architecture: instruction-only standalone Skill with supporting references and UI metadata.
- Distribution: GitHub canonical source plus one local install.
- Canonical source: `https://github.com/744849943/skill-governance`.
- Entry Gate: **APPROVED** by the user's direct execution request dated 2026-09-20, which explicitly fixed the architecture, distribution, invocation, and validation requirements.
