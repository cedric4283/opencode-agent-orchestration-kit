# OpenCode Agent Orchestration Kit

<p align="center">
  <a href="https://github.com/jcarlosrodicio/opencode-agent-orchestration-kit/actions/workflows/check.yml"><img src="https://github.com/jcarlosrodicio/opencode-agent-orchestration-kit/actions/workflows/check.yml/badge.svg" alt="Check"></a>
  <a href="LICENSE"><img src="https://img.shields.io/github/license/jcarlosrodicio/opencode-agent-orchestration-kit" alt="License"></a>
  <a href="https://github.com/jcarlosrodicio/opencode-agent-orchestration-kit/releases"><img src="https://img.shields.io/github/v/release/jcarlosrodicio/opencode-agent-orchestration-kit" alt="Latest release"></a>
  <a href="https://github.com/sponsors/jcarlosrodicio"><img src="https://img.shields.io/badge/Sponsor-GitHub%20Sponsors-ea4aaa?logo=githubsponsors&logoColor=white" alt="Sponsor on GitHub"></a>
</p>

**An open-source multi-agent orchestration harness for OpenCode.**

OpenCode Agent Orchestration Kit (OAK) turns OpenCode into a structured
software-engineering team of specialized agents for research, design,
specification, implementation, review, and validation. It keeps OpenCode agent
orchestration local, inspectable, version-controlled, and reproducible.

Unlike systems designed primarily around large parallel worker pools, OAK
focuses on bounded engineering workflows with durable state, explicit
handoffs, independent review, validation evidence, and optional human approval
gates. It is model-agnostic within OpenCode: each role can use the default
model or an explicit provider/model available in the user's OpenCode setup.

**Apache-2.0 · Local-first · OpenCode-native · Model-agnostic**

> [!NOTE]
> This software-engineering agent harness is not a hosted platform, a fully
> autonomous software factory, or a replacement for engineering judgment.

## Why this exists

A basic coding-agent setup often starts simple:

- One general-purpose agent.
- A few custom prompts.
- Some copied skills.
- Ad hoc instructions.
- Manual review and validation.

Over time, that setup becomes harder to maintain:

- Agents overlap or contradict each other.
- Planning, research, implementation, and review blur together.
- Workflows become prompt-dependent and difficult to reproduce.
- Configuration changes become risky.
- Documentation and validation drift away from the actual setup.

This kit provides a documented OpenCode harness with:

- Specialized agents with explicit responsibilities.
- Predictable handoffs between research, design, specification, implementation, and review.
- Slash commands for common product-development workflows.
- Local process skills and reusable engineering checklists.
- Safe installation and uninstall scripts.
- Local validation for configuration and harness contracts.
- Optional integrations for Open Design, Impeccable, and observability.

The goal is not to force every request through a heavy process. Small, clear, low-risk changes can go directly to implementation. Larger or ambiguous work can follow a structured flow with research, planning, review, and evidence.

## What you get

| Capability | What it gives you |
|---|---|
| Role-based agents | Dedicated agents for routing, research, design, specification, implementation, review, and optional harness evolution |
| Product-development workflows | Structured flows for features, plans, scoped research, bounded engineering loops, MVP specs, testing, simplification, and review |
| Bounded routing | A default `lead` agent that routes simple requests directly and escalates only when needed |
| Local skills | Practical checklists for testing, debugging, security, performance, documentation, APIs, code review, and more |
| Process skills | OAK's own skills for each phase: task enrichment, adversarial review, runtime verification, documentation updates, and commits. No external plugin is required |
| Task runs and delivery | `oak run` links the sessions of one change; `oak deliver` is the only path that pushes a branch and opens a pull request |
| OpenCode 1 and 2 | One installation loads on OpenCode 1 (supported) and OpenCode 2 (experimental) |
| Safe installation | Backup-aware install and uninstall scripts, plus a project-local test mode |
| Validation | Mechanical checks for JSON configuration, agent contracts, command contracts, and harness consistency |
| Adversarial safety | Portable fixtures for injection, unsafe paths, wrapper/network permissions, approval integrity, supply-chain pins, and corrupt events |
| Optional design workflow | Open Design integration for editable UI workspaces and design-oriented flows |
| Optional token visibility | TUI plugin for lead and subagent token usage when OpenCode exposes session trees |
| Versioned documentation | Agent, command, evidence, and validation contracts stored alongside the configuration |

### Bounded autonomous execution

OAK supports explicitly requested autonomous local work without turning it
into unlimited or unattended execution:

- `/loop` requires an approved task contract before writes, allows at most
  three iterations per invocation within a task-specific one-to-six iteration
  budget, and can resume from durable state. Worktrees are explicit opt-in.
- `/autonomous` works on a feature branch, uses a task-specific one-to-six
  iteration budget, runs deterministic validation in every iteration, stops
  iterating as soon as an independent `reviewer` approves the result, and
  ends at an open pull request through `oak deliver`.
- Both workflows use a schema-versioned JSON snapshot, append-only JSONL
  history, an exclusive lock, contract hashing, idempotent action IDs, and
  explicit recovery for interrupted writes.

The `/autonomous` contract ends at an open pull request through `oak deliver`;
it never merges, deploys, releases or publishes, and it prohibits worktrees,
parallel execution, scheduling, write-enabled connectors, pushes to the default
branch, and force-pushes. Its seven stop reasons, including sensitive or
expanded scope, stop for human review. See the [`/loop`](opencode/commands/loop.md) and
[`/autonomous`](opencode/commands/autonomous.md) contracts for the exact
boundaries.

## Built around

This kit builds on and integrates with the following projects:

- [OpenCode](https://opencode.ai/) — the coding-agent runtime and configuration surface.
- [Open Design](https://github.com/nexu-io/open-design) — optional local-first design workspace integration.
- [Impeccable](https://github.com/pbakaus/impeccable) — optional design guidance for AI-generated frontend work.

Each integration is optional. The core workflow can be used with OpenCode alone.

## Who this is for

This kit is a good fit if you:

- Use OpenCode and want a repeatable development workflow instead of isolated prompts.
- Want specialized agent roles without building the whole system from zero.
- Want to separate research, planning, implementation, review, and validation.
- Prefer local configuration that can be inspected, customized, committed, and evolved.
- Work alone or in a small technical team.
- Want a practical starting point rather than a black-box orchestration platform.

## Who this is not for

This kit may not be the right fit if you:

- Only need one small custom prompt or agent.
- Do not use OpenCode.
- Want a hosted multi-agent platform with queues, dashboards, billing, and team administration.
- Need autonomous parallel execution across many repositories or worktrees.
- Expect every request to be fully automated without human oversight.

## How OAK compares

OpenCode orchestration projects optimize for different operating models. This
summary describes their documented emphasis; it is not a universal ranking.

| Approach | Documented emphasis | Consider it when |
|---|---|---|
| **OAK** | Versioned software-engineering workflows, specialized phase roles, durable bounded loops, independent review, and local safety contracts | You want research → specification → implementation → review with explicit scope and evidence |
| [`opencode-orchestrator`](https://github.com/agnusdei1207/opencode-orchestrator) | A persisted mission runtime with Commander, Planner, Worker, and Reviewer roles, local memory, and configurable agent concurrency | You want runtime-governed mission execution and a larger concurrent worker pool |
| [`oh-my-openagent`](https://github.com/code-yeongyu/oh-my-openagent) | A broad multi-model agent harness with specialized agents, integrated tools, background agents, and opt-in parallel Team Mode | You want an extensive agent/tool ecosystem and optional parallel teams |
| [`opencode-agent-tree`](https://github.com/beremaran/opencode-agent-tree) | A plugin that makes an orchestrator decompose and delegate every request, with hands-on tools blocked and configurable delegation depth | You want enforced delegation trees and parallel fan-out of independent subtasks |

### Choose OAK when

- You want OpenCode to behave like a structured engineering team.
- You want research, specification, implementation, validation, and independent review to remain distinct.
- You need durable, resumable workflows with explicit scope and acceptance criteria.
- You prefer bounded autonomy and inspectable local contracts over unconstrained agent loops.

### Consider another orchestration approach when

- Your primary goal is maximizing parallel worker throughput.
- You need autonomous execution across many worktrees or repositories.
- You need a hosted orchestration or control-plane product.
- You want an unattended software factory rather than bounded engineering workflows.

See [OpenCode Agent Orchestration Approaches](docs/comparison.md) for the
comparison method, architectural distinctions, limitations, and source links.

## Engineering quality

OAK's reproducibility and safety claims are backed by checked repository
artifacts rather than a maturity label:

- 1,000+ automated unit and script tests across the package and shipped harness.
- Mechanical validation of agent, command, workflow, evidence, and routing contracts.
- Type checking plus dependency vulnerability, integrity, and signature audits.
- Isolated installation and exact-package smoke tests for the `oak` and `oc-switch` CLIs.
- Compatibility checks across the declared OpenCode and Node.js boundaries.
- Durable-state recovery, corruption, idempotency, and adversarial fixtures.
- Public/private configuration boundary checks and release-provenance validation.

The [Validation](#validation) section explains the normal and release-grade
checks and their limits.

## How it works

Free-form requests start with the `lead` agent.

`lead` acts as a bounded router:

- Simple and low-risk implementation work goes directly to `developer`.
- Technical uncertainty goes to `researcher`.
- Visual or interaction work goes to `designer`.
- Planning gaps go to `specifier`.
- Larger feature work follows a structured orchestration flow.

```text
Simple change
lead
  └── developer
        └── validation

Feature work
lead
  ├── designer            when visual or interaction design is needed
  ├── researcher          when technical uncertainty must be resolved
  ├── specifier           creates tasks, acceptance criteria, and validation plan
  ├── developer           implements approved work
  └── reviewer            checks the final diff against the agreed scope

Plan-only work
lead
  └── researcher
        └── specifier
              └── reviewer

Bounded engineering loop
lead
  └── human approval gate
        └── developer
              └── reviewer
                    └── developer state sync

Scoped research and specification
scoper
  └── researcher
        └── scoper synthesis
              └── specifier

Optional harness evolution
evaluator
  └── debugger
        └── evolver
              └── lead approval
                    └── developer
                          └── evaluator
                                └── debugger
                                      └── reviewer
```

The normal workflow is intentionally not a fixed ceremony for every task.

Use the smallest useful flow:

- Direct implementation for a small change.
- `/plan` when you want an implementation-ready plan but no code changes.
- `/scope` when you need research and an MVP-oriented specification.
- `/feature` when the work deserves full orchestration.
- `/loop` when you want an approved, resumable implementation-review cycle with
  crash-recoverable structured state and a three-iteration cap.
- `/autonomous` for one explicitly requested task with a task-specific
  one-to-six-iteration budget, a hard ceiling of six, durable state, and
  reviewer-only closure. Final approval stops the cycle, and the run ends at an
  open pull request.

### Small-gate fast path

A declared `small` change of at most two files may skip final review when it
does not touch protected surfaces (auth, secrets, public API, migrations, CI,
or the harness itself), is not destructive, and does not require new tests.
`lead` then records `review_skipped_reason: small_gate_pass`. Medium and large
work, and any sensitive surface, still get a full `reviewer` verdict. The
unique gate definition lives in `opencode/docs/ai/harness/agents.md`.

## Included agents

| Agent | Responsibility |
|---|---|
| `lead` | Default router, feature orchestrator, and phase-barrier owner |
| `scoper` | Lightweight research-to-spec orchestration |
| `designer` | Product and interaction design using project context, optional Impeccable, and Open Design |
| `researcher` | Code, documentation, API, alternative, and risk investigation |
| `specifier` | Specs, tasks, acceptance criteria, scope boundaries, and validation plans |
| `developer` | Implementation, direct-mode changes, and focused validation |
| `reviewer` | Final, read-only review against scope, real diff, original evidence, and contextual architecture |
| `evaluator` | Optional benchmark and smoke-test evidence collection |
| `debugger` | Optional root-cause analysis from failures and traces |
| `evolver` | Optional evidence-driven harness improvement proposals |

The `evaluator`, `debugger`, and `evolver` agents are optional harness-evolution sidecars. They are not part of the normal feature-development path.

## Included commands

| Command | Use it when you want to... |
|---|---|
| `/feature` | Run the full feature workflow |
| `/plan` | Research and create an implementation-ready plan without implementing |
| `/scope` | Research a topic and produce a scoped MVP specification |
| `/loop` | Design and run a bounded, resumable engineering loop |
| `/mvp-spec` | Create a strict MVP spec with small tasks and explicit out-of-scope items |
| `/design` | Create or evolve a design through project context and Open Design |
| `/research` | Run a direct research task |
| `/spec` | Run a direct specification task |
| `/implement` | Implement approved work directly |
| `/test` | Reproduce a bug or run focused validation |
| `/code-simplify` | Simplify code without changing behavior |
| `/review` | Run the final read-only review of the current diff |
| `/evolve` | Run the optional harness-evolution workflow |
| `/init` | Initialize project-oriented context for the workflow |

Examples:

```text
/feature Create onboarding with plan selection and welcome screen

/plan Add a dry-run flag to the harness check without implementing it yet

/scope Research Stripe Checkout integration and generate an MVP spec

/loop Add request retries with focused tests and independent review

/loop resume request-retries

/mvp-spec Email notifications when an agent finishes a task

/design Read PRODUCT.md and DESIGN.md, create an editable Open Design project, and generate a first version

/test Reproduce the checkout regression with a focused test

/code-simplify Simplify the parser branch without changing behavior

/review
```

`/loop` stores its approved human-readable contract in Markdown and uses a
schema-versioned JSON snapshot, append-only JSONL history, and exclusive lock
for execution state. Contract hashing rejects stale approvals, action IDs make
retries idempotent, and explicit repair recovers interrupted writes without
turning the loop into unattended automation.

A small request can also be written directly:

```text
Change the Settings heading to Account settings and run the smallest relevant validation.
```

### One review policy across stacks

All review entry points use the same language- and framework-neutral policy.
The reviewer checks correctness, security, contracts, architecture,
maintainability, tests, and evidence—not only whether tests pass.

Generic backend and frontend profiles adapt those checks to the changed
surface. Strict Clean Architecture, DDD, hexagonal, CQRS, or layered profiles
activate only when the task or repository explicitly declares that
architecture; folder names alone do not activate them.

Only problems introduced or materially worsened by the current task can block
it. Relevant pre-existing debt is reported separately as a non-blocking
observation. `/review-preflight` remains deterministic, and focused
`/review-orchestrated` modes remain partial; only the general `/review` agent
emits a final verdict.

For this kind of request, `lead` should choose the direct path and delegate to `developer` without invoking the full feature workflow.

## Quick start: test it safely first

The safest way to try the kit is to load it from the repository directory without changing your global OpenCode configuration.

### Requirements

- Git
- Bash
- OpenCode installed and available on your `PATH`
- Node.js `^22.9.0 || ^24.0.0` and npm
- An OpenCode-compatible model provider configured through `opencode auth login`

### 1. Clone the repository

```bash
git clone https://github.com/jcarlosrodicio/opencode-agent-orchestration-kit.git
cd opencode-agent-orchestration-kit
```

### 2. Configure models

```bash
cp env.example .env
source .env
```

`env.example` provides a shared default model plus optional per-role overrides.

```bash
export OPENCODE_MODEL="openai/gpt-5.5"

export OPENCODE_SMALL_MODEL="$OPENCODE_MODEL"
export OPENCODE_LEAD_MODEL="$OPENCODE_MODEL"
export OPENCODE_SCOPER_MODEL="$OPENCODE_MODEL"
export OPENCODE_DESIGNER_MODEL="$OPENCODE_MODEL"
export OPENCODE_RESEARCHER_MODEL="$OPENCODE_MODEL"
export OPENCODE_SPECIFIER_MODEL="$OPENCODE_MODEL"
export OPENCODE_DEVELOPER_MODEL="$OPENCODE_MODEL"
export OPENCODE_REVIEWER_MODEL="$OPENCODE_MODEL"
export OPENCODE_EVALUATOR_MODEL="$OPENCODE_MODEL"
export OPENCODE_DEBUGGER_MODEL="$OPENCODE_MODEL"
export OPENCODE_EVOLVER_MODEL="$OPENCODE_MODEL"
```

You can use one model for every role or assign different models depending on cost, speed, and task complexity.

### Interactive OpenCode model switching

The repository also includes `oc-switch`, a standalone TUI for changing the
OpenCode default model, small/title model, and per-agent overrides. It builds
its provider list from OpenCode itself and reads the local model catalog from
`opencode models --pure`, so it only shows providers configured in your
OpenCode installation. Startup is cache-first; press `r` when you want an
explicit remote catalog refresh through `--refresh`. It does not share a
catalog with Pi.

From this checkout, run:

```bash
npm run oc-switch
```

To install the command globally from the published npm package:

```bash
npm install --global opencode-agent-orchestration-kit
oc-switch
```

To install the current cloned checkout instead:

```bash
npm install --global .
oc-switch
```

After a tagged release is published, you can install the same CLI directly
from GitHub without keeping a checkout:

```bash
npm install --global "git+https://github.com/jcarlosrodicio/opencode-agent-orchestration-kit.git#v1.1.1"
oc-switch
```

The package exposes `oc-switch` through its npm `bin` entry; no separate
wrapper or shell alias is required. Keep `~/.npm-global/bin` (or your npm
global bin directory) on `PATH` if npm is configured with a user prefix.

#### `oc-switch` quick guide

-