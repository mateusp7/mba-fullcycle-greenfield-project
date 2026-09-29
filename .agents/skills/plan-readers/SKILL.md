---
name: plan-readers
description: Apply the project's bounded, read-only planning artifact extraction procedures when plan-context or another pipeline stage needs a plan, decision, phase, or inventory reader.
---

# Planning readers for Codex

The six reader procedures are registered as read-only Codex custom agents under `.codex/agents/`. Prefer delegating to the matching role (`plan-reader`, `phases-reader`, `inventory-digest-reader`, `decisions-reader`, `decisions-detail-reader`, or `decisions-correlator`) and pass its bounded input contract. The copies in `references/` are a manual fallback: when delegation is unavailable, open the matching reference and follow it in the current thread. Return the requested compact result to the caller. Do not edit source artifacts while acting as a reader, do not read beyond the reference's stated scope, and do not claim that a separate agent ran.

Available procedures: `plan-reader`, `phases-reader`, `decisions-reader`, `decisions-detail-reader`, `decisions-correlator`, and `inventory-digest-reader`.
