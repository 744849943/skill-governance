---
name: skill-governance
description: Use before creating, modifying, packaging, or releasing a Skill, Plugin, or MCP integration. Determine target ChatGPT/Codex surfaces, architecture, distribution, portability, and testing gates before implementation. Do not begin implementation until the architecture decision is approved.
---

# Skill Governance

Use this skill as a mandatory architecture gate before `skill-creator` or `plugin-creator`.

## Workflow

1. Read `references/skill-development-standard.md`.
2. Identify the user's actual goal and target surfaces.
3. Classify the capability:
   - pure workflow/instructions/templates
   - external tools/data/actions
   - workflow + tools
4. Determine packaging:
   - standalone Skill
   - skill-only Plugin
   - MCP App
   - Skill + MCP Plugin
5. Determine distribution:
   - local
   - cloud standalone Skill
   - workspace-private Plugin
   - public Plugin
   - developer-mode MCP App
6. Check portability:
   - local paths
   - localhost
   - shell
   - persistent service
   - authentication
7. Produce an Architecture Decision Record using
   `references/architecture-decision-template.md`.
8. STOP and ask for approval before implementation.

## Release Gate

When invoked after implementation:

1. Compare the result against the approved Architecture Decision Record.
2. Verify single canonical source and version consistency.
3. Test each target surface separately at four levels:
   - discovery
   - registration
   - runtime loading
   - execution
4. Do not treat `@` visibility as successful execution.
5. Verify new-chat behavior after installation or update.
6. Run positive, negative, insufficient-evidence, confirmation-gate, and stop-condition tests.
7. Return PASS / FAIL / BLOCKED with evidence.

## Non-negotiable rules

- Do not introduce MCP merely to work around a surface limitation unless the capability genuinely requires tools and the user approves the operational cost.
- Do not equate local Marketplace installation with cloud distribution.
- Do not keep multiple same-name production identities without explicit version governance.
- Do not implement before the Entry Gate is approved.
