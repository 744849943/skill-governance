# Skill Governance Validation Report v1.0

- Date: 2026-09-20
- Target: local standalone Skill
- Static validator: PASS
- Behavioral Release Gate: PASS

## RED Baseline

Three isolated agents were tested before `skill-governance` was installed:

1. Create and publish a PRD review Skill across Web Chat, Desktop Work, and Codex.
2. Convert a pure prompt workflow to an MCP App only to reach Web Chat.
3. Treat `@` menu visibility as proof that a Plugin release works.

All three agents independently avoided the target mistakes: they performed architecture reasoning, rejected unnecessary MCP conversion, and separated discovery from execution. Because the control did not exhibit the feared failure, the Skill's core instructions were not expanded merely to manufacture a difference. The supplied core behavior was preserved.

## GREEN Installation Tests

| Test | Expected behavior | Result |
|---|---|---|
| Explicit `$skill-governance` | Load the Skill, produce a complete ADR, stop at approval | PASS |
| Natural-language request | Automatically consider governance, produce an ADR, stop before implementation | PASS |
| Static validation | Valid frontmatter, name, description, and no scaffold placeholders | PASS |
| Invocation metadata | `allow_implicit_invocation: true` | PASS |

## Release Gate Behavior Matrix

### Positive cases (5)

1. New PRD review Skill — PASS.
2. New meeting-minutes Skill targeting Codex and Web Chat — PASS.
3. Modify and republish an existing translation Skill — PASS.
4. Package a pure workflow Skill as a workspace-private Plugin — PASS.
5. Build a CRM write-back integration — PASS; MCP was selected only because external authenticated writes were real requirements.

### Negative cases (3)

1. Translate ordinary text — correctly did not trigger governance.
2. Fix a React null-pointer bug — correctly did not trigger governance.
3. Use an already installed Google Drive Plugin — correctly did not trigger governance.

### Required gates

- Insufficient information: PASS; stopped before architecture selection.
- Human confirmation: PASS; refused implementation before ADR approval.
- Stop condition: PASS; Discovery alone did not satisfy Registration, Runtime Loading, or Execution.

## Surface Evidence

| Surface | Discovery | Registration | Runtime Loading | Execution |
|---|---|---|---|---|
| Codex, fresh contexts | PASS | PASS | PASS | PASS |
| Desktop Work current task | PASS | PASS | PASS | PASS |
| Web Work | NOT TESTED | NOT TESTED | NOT TESTED | NOT TESTED |
| Web/Desktop Chat | NOT REQUIRED for v1.0 | NOT REQUIRED | NOT REQUIRED | NOT REQUIRED |

## Verdict

**PASS** for the approved v1.0 target surfaces. Web Work remains a future target and is not claimed as released.
