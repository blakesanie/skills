---
name: efficient-software-engineering
description: Enforces high-density, low-bloat software engineering principles—treating expressive code as the sole source of truth, managing boundary types via schemas, maintaining living docs, and optimizing PRs for human review.
---

## Core Philosophy

Engineering velocity is bounded by cognitive load and human review throughput, not typing speed. This skill governs all AI generation across code, documentation, specs, and version control artifacts to maximize knowledge density and eliminate redundancy.

---

## 1. Code as the Source of Truth

- **Expressive over Imperative:** Write self-documenting code with precise domain-level naming. Rely on compiler and build-time optimizations (e.g., inline expansions, dead-code elimination, profile-guided optimization) rather than manual micro-optimizations that degrade readability.
- **Zero-Comment Default:** Comments are a code smell indicating insufficient expressiveness. Omit them entirely unless an architectural invariant or non-obvious domain constraint strictly demands plain-english nuance, in which case restrict it to a single succinct line.

## 2. Strict Anti-Bloat Protocol

- **Maximal Information Density:** Every token must carry distinct semantic weight. Eliminate conversational filler, redundant restatements, and generic meta-phrasing across all outputs (code, docs, PR descriptions, and chat responses).
- **Single Source Definition:** Never duplicate specs, documentation, or code structures. Define a concept once at its canonical source and reference it everywhere else.

## 3. Spec-Driven Boundary Types

- **Contract-First Boundaries:** Never manually author type definitions for external or boundary-crossing interfaces (APIs, IPC, database schemas, serialized payloads) in application languages.
- **Codegen Pipelines:** Author standard contract specifications (JSON Schema, OpenAPI, Protobuf, GraphQL SDL) and use build-time plugins or generators to produce type-safe bindings for target languages automatically.

## 4. Continuous Design Documentation

- **Living Architecture Files:** Maintain topic-specific Markdown documents inside the project's `docs/` directory (e.g., `docs/authentication.md`, `docs/database-schema.md`, `docs/backend-arch.md`).
- **Atomic Sync:** When modifying system behavior, update the corresponding design file in the same commit. If a topic lacks a dedicated file, create it; if it exists, refine the snapshot to remain cohesive with the codebase. Let Git track the evolutionary history.

## 5. Review-Optimized Delivery (PR Stacks)

- **Human Review Bottleneck:** Code creation is cheap; human validation is expensive. Structure all changes into vertical, atomic GitHub PR stacks where each layer has a single, strictly scoped responsibility (see `/gh-stack`).
- **Stack Execution:** Build layers independently so reviewers can reason about isolated concerns sequentially. The final stack is squashed and merged as a unified unit upon approval.

## 6. Standardized Command Interface

- **Task Runner as Entrypoint:** Maintain a single canonical task runner (e.g., `justfile`, `Makefile`, `Taskfile.yml`) at the repository root to encapsulate build, test, codegen, and environment workflows.
- **Self-Documenting Recipes:** Name recipes clearly by intent so complex CLI invocations remain discoverable, deterministic, and executable by both humans and CI pipelines without memorizing flags.

## 7. Self-Synchronizing Pre-Commit Hooks

Shift-Left Guardrails: Strongly favor pre-commit hooks for rapid local validation, especially in early-stage repositories where CI/CD pipelines are intentionally minimal.

Zero-Maintenance Drift Detection: Configure hooks to self-verify against the repository's current hook definitions at execution time. If upstream hook definitions change (e.g., after a git pull), the hook must automatically re-register or update itself and execute the latest checks without manual user intervention.
