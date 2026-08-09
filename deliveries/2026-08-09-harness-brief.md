# Daily Harness Brief — August 9, 2026

🤯 **HERDR CAN NOW RESTART WITHOUT AMNESIA — THE AGENT CONVERSATION COMES BACK WITH THE PANE**

Herdr v0.6.3 closes one of the ugliest gaps in persistent terminal orchestration: restoring a workspace used to bring back the pane or shell while the underlying AI conversation was gone. With native session restore, supported Pi, Claude Code, Codex, OpenCode, and Hermes panes can reconnect to the actual agent session after the Herdr server restarts. The workflow is concrete: install and verify the native integration, enable agent resume, start a conversation, restart Herdr, and confirm the restored pane continues with its previous conversational state. This is not arbitrary process checkpointing—unsupported tools still fall back to ordinary shells—but it turns a restart from “rebuild the agent’s context manually” into a recoverable infrastructure event. For long-running workspaces, that is the difference between visual persistence and genuine cognitive persistence.

⬇️ **Dethrones:** *Pane-only restoration that preserved the terminal layout but silently discarded the agent conversation behind it.*

💡 **Why you care:** Your persistent workspaces can survive a Herdr restart without forcing Codex or Claude sessions to reconstruct their state from scratch.

🔗 [Read more](https://github.com/herdrdev/herdr/releases/tag/v0.6.3)  ·  🧠 PERSISTENT AGENTS  ·  *evidence: exact public release + reproducible restart workflow*

🛡️ **ZED MOVES AGENT SAFETY BELOW THE PROMPT — THE OPERATING SYSTEM NOW BLOCKS THE DAMAGE**

Zed v1.14.2 replaces fragile command-pattern restrictions with operating-system enforcement for supported Agent terminal and fetch operations. On macOS it uses Seatbelt; Linux uses Bubblewrap; Windows uses WSL. The practical test is refreshingly direct: ask the agent to write outside the project, mutate `.git`, or contact an unapproved host. The resource should be blocked until you explicitly grant it, after which the same action can be retried. That matters because shell allowlists can be bypassed through indirection, while a filesystem or network boundary constrains the underlying resource regardless of how the command was assembled. The protection is not universal—Zed’s direct edit tool, ACP agents, MCP/LSP servers, normal terminals, and external applications sit outside this boundary—but it is a serious shift from “the model was told not to” toward fail-closed execution.

⬇️ **Dethrones:** *Advisory shell allowlists as the primary safety boundary for agent-generated terminal commands.*

💡 **Why you care:** You can give an autonomous coding loop more freedom inside its project without granting the same freedom to your home directory, Git metadata, or arbitrary hosts.

🔗 [Read more](https://zed.dev/blog/sandboxing)  ·  🛡️ AGENT SAFETY  ·  *evidence: exact release + documented OS-enforced denial workflow*

🤖 **CLAUDE CODE TURNS A FLEET OF AGENTS INTO AN OBSERVABLE COMPLETION SYSTEM**

Claude Code’s recent releases join three pieces that previously required manual babysitting. `claude agents` shows running, blocked, and completed sessions in one fleet view; `/goal` keeps working across turns until a concrete completion condition passes; and background-by-default subagents now emit `agent_needs_input` and `agent_completed` events. Worktree agents can also finish by committing, pushing, and opening a draft pull request. The real breakthrough is the workflow: launch several isolated sessions, define an outcome such as “the auth tests and lint both pass,” watch blockers and progress centrally, then inspect the resulting worktree or PR instead of repeatedly polling every agent. The limits are real—each session consumes quota, goal evaluation spends extra turns, permissions still apply, and pushing code creates an external side effect—but the control surface is moving from parallel chat windows toward supervised autonomous execution.

⬇️ **Dethrones:** *Manually checking multiple agent terminals and deciding by eye whether each one has actually finished.*

💡 **Why you care:** This is the operating model for a low-bureaucracy engineering team where humans specify completion and intervene only when an agent is blocked.

🔗 [Read more](https://github.com/anthropics/claude-code/releases/tag/v2.1.198)  ·  🛠️ HARNESS & AGENTS  ·  *evidence: two exact releases + callable fleet and goal workflow*

📱 **CMUX PUTS THE IPHONE SIMULATOR BESIDE THE AGENT — AND REMOTE SESSIONS SURVIVE NETWORK CHANGES**

cmux v0.64.21 adds native iPhone and iPad Simulator panes that live beside the build terminal, giving an agent-addressable workspace a visual mobile target instead of forcing a jump into a separate app. The same release adds first-class Mosh transport for remote workspaces, so the terminal connection can survive Wi-Fi changes, sleep, and unstable networks; tmux can preserve the named remote session behind it. That combination is more than UI polish: an agent can build, launch, inspect, and iterate on an iOS app in one controllable workspace, while a remote development session remains recoverable during normal laptop movement. It requires macOS 14 or later, compatible Mosh components on both ends, and does not magically persist processes without the remote session layer. The release also reports fixing a leaked loop that consumed roughly 90% idle CPU, but that number lacks an independent test protocol.

⬇️ **Breaks through:** *The split workflow where mobile UI inspection lives outside the agent workspace and remote terminals die whenever the network changes.*

💡 **Why you care:** This is a credible path to agents that can own an iOS build-and-inspect loop without losing their remote terminal every time your Mac sleeps.

🔗 [Read more](https://github.com/manaflow-ai/cmux/releases/tag/v0.64.21)  ·  📱 COMPUTER USE  ·  *evidence: exact release + reproducible Simulator and Mosh workflows*

🔌 **CODEX PLUGINS BECOME PORTABLE PACKAGES INSTEAD OF A PILE OF MANUAL CONFIGURATION**

Codex CLI 0.147.0 introduces portable Agent Plugins that can be discovered across local, personal, workspace, and remote catalog scopes. A plugin can travel as one installable unit instead of asking every developer to separately reconstruct skills, tools, connectors, and project configuration. The practical path is simple: open `/plugins`, choose a configured marketplace, install the package, start a new session, and invoke it. Authentication, connector access, approvals, and the workspace sandbox still apply, and installation does not retroactively modify the current session. The shared ChatGPT/Codex directory exists today, but the exact release claim is narrower: the new delta is portable packages plus multi-scope discovery, not proof that this version alone created the universal directory. Even with that boundary, it is a meaningful distribution primitive for repeatable agent workflows.

⬇️ **Dethrones:** *Copy-pasted skill folders and one-off MCP configuration as the default way to share a complete Codex workflow across projects and machines.*

💡 **Why you care:** You can package the useful parts of your harness once and install the same governed workflow in personal, workspace, or remote contexts.

🔗 [Read more](https://learn.chatgpt.com/docs/plugins)  ·  🔌 PLUGINS & MARKETPLACES  ·  *evidence: exact Codex release + documented install and catalog workflow*
