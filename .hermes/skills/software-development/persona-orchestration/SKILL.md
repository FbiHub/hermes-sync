---
name: persona-orchestration
description: Manage and audit logic-based agent teams (personas/prompts) within event-driven servers.
---

# Persona Orchestration

Use this skill when auditing or configuring logic-based agent teams (personas/prompts) that run inside a larger application (like FastAPI or event-driven signal systems), rather than as persistent OS daemons.

## Core Principles
- **No Resource Bloat**: Prefer on-demand function calls to persona-based prompts.
- **Declarative Persona Specs**: Persona behavior and rules should live in documentation (e.g., implementation plans, dedicated persona YAMLs).
- **Audit Steps**: 
  1. Locate the master specification (e.g., `implementation_plan.md` Section 9).
  2. Map personas to code entry points (e.g., `app/risk.py`, `app/main.py`).
  3. Validate the invocation logic against the specification.
- **Maintainability**: Ensure new personas are added as templates/prompts, not new persistent processes.

## Pitfalls
- **Over-Engineering**: Do not turn every persona into a separate background process.
- **Drift**: If the code implementation diverges from the `implementation_plan.md`, patch the code, not the docs.

## References
- `references/persona-mapping.md`: Keep a summary of the 11 personas and their entry points here.
