# Daily Harness and Plugins SOTA — 2026-08-09

Status: **AUDIT: PASS**  
Validated findings: **6**  
Model setting shown in the Work chat: **GPT-5.6 Luna, Extra High**  
Research and same-chat audit: [Work chat](https://chatgpt.com/c/6a780289-2c90-83e9-8435-7f9732043316)

The final audit retained four core-harness findings and two plugin findings.
It narrowed or removed claims whose exact release evidence did not support the
researcher's first wording. All six findings are NEW against the canonical
ledger and independent receipt history.

## Core harnesses

### 1. Herdr v0.6.3 native agent-session restore

Verdict: **NEW — Evidence grade B**

- Release: `v0.6.3`, 2026-05-27
- Commit: `8d8eac3f85f52dd1900f3f266ff05ba4e6732518`
- Sources: [release](https://github.com/herdrdev/herdr/releases/tag/v0.6.3), [integrations](https://herdr.dev/docs/integrations/)

Herdr can restore the underlying supported Pi, Claude Code, Codex, OpenCode,
or Hermes conversation after a Herdr server restart, rather than restoring
only the pane or shell layout.

Callable test: install and verify a native integration, enable agent resume,
start a conversation, restart Herdr, and confirm that the same pane resumes
with the previous conversational state. Unsupported integrations fall back to
ordinary shells; this is not arbitrary process checkpointing.

### 2. Zed v1.14.2 OS-enforced Agent sandbox

Verdict: **NEW — Evidence grade B**

- Release: `v1.14.2`, 2026-08-05
- Commit: `02abf5b08fa12c1c20a155ae3f796ef4c6c1a01e`
- Sources: [release](https://github.com/zed-industries/zed/releases/tag/v1.14.2), [sandbox design](https://zed.dev/blog/sandboxing), [sandbox documentation](https://zed.dev/docs/ai/sandboxing)

For supported Agent terminal and fetch operations, Zed moves enforcement from
advisory command matching to operating-system filesystem and network controls.
A reproducible acceptance test is an attempted write outside the project, a
`.git` mutation, or access to an unapproved host, followed by an explicit
grant and successful retry.

Audit boundary: this is not a universal Zed sandbox. It does not cover the
direct edit tool, ACP agents, MCP/LSP servers, normal terminals, or external
applications. Platform requirements include Seatbelt on macOS, non-setuid
Bubblewrap on Linux, and WSL on Windows.

### 3. Claude Code agent fleet view and completion goals

Verdict: **NEW — Evidence grade B**

- Initial release: `v2.1.139`, commit `fdfbc06c7a6d9ace49c55b3761b1be05d276da6d`, 2026-05-11
- Maturation release: `v2.1.198`, commit `75709eacf1334051ea293fb87a0e88a1e6812f94`, 2026-07-01
- Sources: [v2.1.139](https://github.com/anthropics/claude-code/releases/tag/v2.1.139), [v2.1.198](https://github.com/anthropics/claude-code/releases/tag/v2.1.198)

`claude agents` provides a fleet view of running, blocked, and completed
sessions. `/goal` persists a completion condition across turns. The later
release adds background-by-default subagents, `agent_needs_input` and
`agent_completed` hooks, plus worktree commit/push/draft-PR workflows.

Callable test: launch several sessions, inspect them through `claude agents`,
set a concrete `/goal` such as passing an auth test and lint suite, and verify
that completion or input-needed events lead to an inspectable worktree or
draft PR. Limits include additional quota, model turns, permissions, and
external GitHub side effects.

### 4. cmux v0.64.21 Simulator and Mosh workflows

Verdict: **NEW — Evidence grade B**

- Release: `v0.64.21`, 2026-08-02
- Commit: `33ac210ab4cc36642749701cbc3d3fec30af0934`
- Sources: [release](https://github.com/manaflow-ai/cmux/releases/tag/v0.64.21), [changelog](https://cmux.com/docs/changelog), [SSH/Mosh documentation](https://cmux.com/docs/ssh)

cmux adds native iPhone/iPad Simulator panes and first-class Mosh transport for
remote workspaces. An agent can build beside an addressable Simulator pane,
or use a Mosh-backed remote terminal that survives Wi-Fi, sleep, and network
changes. macOS 14+ is required; Mosh needs compatible local and remote
components, while tmux remains responsible for named-session persistence.

Audit correction: the release's approximately 90% idle-CPU claim is retained
only as vendor-reported context. No independent hardware or test protocol was
published.

## Plugins and marketplaces

### 5. Codex CLI 0.147.0 portable Agent Plugins

Verdict: **NEW — Evidence grade B**

- Release: `rust-v0.147.0`, 2026-08-07
- Commit: `be6e8eac029b183056b7e4402879f15d2c85f61b`
- Sources: [release](https://github.com/openai/codex/releases/tag/rust-v0.147.0), [plugin documentation](https://learn.chatgpt.com/docs/plugins), [changelog](https://learn.chatgpt.com/docs/changelog)

Portable Agent Plugins can be discovered and installed through local,
personal, workspace, and remote catalog scopes. The callable workflow is
`/plugins`, select a configured marketplace, install the plugin, start a new
session, and invoke it. Authentication, connectors, approvals, and workspace
sandbox rules still apply.

Audit correction: the shared ChatGPT/Codex public directory is verified as
current availability, but is not claimed as newly introduced by this exact
release. The exact delta is portable, multi-catalog Agent Plugins.

### 6. Herdr v0.7.0 executable plugin host

Verdict: **NEW — Evidence grade B**

- Release: `v0.7.0`, 2026-06-15
- Commit: `0bf9bb58eb9c9495f660aed1bd9336121b953fdc`
- Source: [release](https://github.com/herdrdev/herdr/releases/tag/v0.7.0)

Herdr v0.7.0 adds plugin manifests, executable actions, event hooks, managed
panes, install/link/list/uninstall flows, keybindings, and command logs.

Callable test: create a repository containing `herdr-plugin.toml`, install it
with `herdr plugin install owner/repo[/subdir]`, invoke an action, and verify
its output and command log. Plugins run as ordinary user code with access to
the Herdr CLI and environment; manifest validation is not a security sandbox.

Audit correction: the automatic marketplace was **not live at v0.7.0**. The
marketplace portion of the draft claim is excluded and is not recorded as a
finding.

## Screened out

- JCode: useful lifecycle and persistent-plan work, but no disclosed hardware
  or reproducible comparative result.
- Conductor: the relevant exact change was documentation or selected-team
  alpha without comparative reliability evidence.
- Pi: recent RPC and harness improvements lacked a pinned, comparable measured
  breakthrough.
- Vercel skills, AGENTS.md/rules directories, desktop-app changes, and trending
  harness candidates did not produce another exact-revision result that met
  every gate.

Delivery status: **ELIGIBLE_FOR_FUTURE_RELAY**
