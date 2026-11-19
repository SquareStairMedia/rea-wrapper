---
name: rea-wrapper
description: Preflight check for Reverse Engineer Anything (REA) work. Use at the start of an REA-assisted investigation or reconstruction, or when REA's findings look incomplete, to decide whether a supplementary tool is worth adding.
---

# REA Wrapper

You are the user's AI coding agent. This skill is a lightweight **preflight checklist**, not a reverse-engineering engine or a replacement for REA.

**Goal:** Prevent users from unknowingly limiting an investigation to REA's currently available tools when a supplementary tool could materially improve the result.

## Workflow

1. **Check REA access.** Determine whether REA MCP tools are usable in *this agent session*. If not, consult the official REA setup instructions and offer the smallest necessary setup action. A skill installation alone does not guarantee a live MCP connection. Do not require cloning or forking REA. Do not repeatedly run setup if the connection already works.

   **Check the environment.** Determine whether the agent's shell is running on the user's computer, a remote machine, or an isolated sandbox. A tool missing from a sandbox says nothing about whether it is installed on the user's machine. If the user's environment cannot be inspected, record its status as **unverified**; ask for a passive check on the user's computer only if that information affects the recommendation.

2. **Identify the target and task.** From the user-provided application, files, or existing findings, establish the platform/framework and what the user wants to learn or reproduce. Use REA's existing inspection capabilities where helpful. State what remains uncertain. Do not run an unknown executable merely to identify it.

3. **Proactively assess tool coverage.** Consult REA's current capabilities and the tools actually available to the agent. Ask BOTH:
   - Can REA perform the relevant investigation with its built-in capabilities or already configured providers?
   - Would another tool materially improve evidence quality, coverage, or efficiency for THIS task, even if REA is already producing results?

   Do not equate a working REA connection with a complete toolchain. Do not equate a tool's popularity with a requirement.

   **Fast path:** After a brief, proactive assessment of the target, REA's available capabilities, and potentially useful supplementary tools, if no additional tool would materially benefit this investigation, report **Ready** in one line and stop. Never skip the proactive comparison merely because REA is functioning.

4. **Check before recommending installation.** For each worthwhile supplementary tool, determine whether it is already installed and usable **in the relevant environment**. Use passive discovery first, such as PowerShell `Get-Command` or an installed-apps list on Windows, and `command -v` on macOS/Linux. These establish discoverability, not functionality. A tool's `--version` or `--help` command executes it; run such checks only when the user has approved or already trusts that installation. Verify whether REA integrates with it directly, whether the agent can call its CLI, or whether a separate MCP server is actually useful. Prefer existing tools and REA's native integrations over duplicate MCP layers. If the machine cannot be checked, mark installation status **unverified**, not missing.

5. **Propose the minimum change.** Report one of three outcomes:
   - **Ready:** No additional tool is justified for the current investigation.
   - **Optional improvement:** Name the tool, the specific benefit, and whether it is already available.
   - **Required capability missing:** Name the blocked operation and the tool/provider needed to perform it.

   If a change is warranted, identify the official source, platform requirements, license/cost where relevant, and the smallest setup path. Distinguish verified requirements from suggestions and unknowns. Obtain explicit user approval **before** installing software, registering MCP servers, modifying configuration, or running untrusted targets.

6. **Verify and hand back.** If a tool is configured, perform a small, relevant functionality check where safe and authorized; otherwise mark it **unverified**. Keep the result concise: **Outcome** (Ready / Optional improvement / Required capability missing); **Tool and reason** (or none); **Installed** (yes / no / unverified, identifying which machine was checked); **Next action** (one action, if needed). The **Ready** fast path may be a single line. Then hand control back to the agent's normal REA-assisted investigation/reconstruction workflow. **Do not run or manage the reconstruction as part of this skill.**

## Tool-selection reminders (illustrative only, not automatic dependencies)

| Target / need | REA baseline | Consider only if needed |
| --- | --- | --- |
| JavaScript / Electron | REA supports static JavaScript and Electron inspection | Additional unpacking, debugging, or runtime tools for gaps the task exposes |
| .NET assemblies | REA supports static metadata and CIL inspection | ILSpy / `ilspycmd` when higher-level managed-code decompilation would help |
| Native binaries | REA has inspection capabilities, but deeper native decompilation uses an external provider | An REA-supported provider such as Ghidra, Hopper, IDA, or Kuna, subject to OS/support/licensing |
| Android APKs | Some APK inspection is built in | JADX plus its prerequisites for decompiled methods; other Android tools only for specific needs |
| Other or mixed targets | Check the latest REA documentation and available tools | Recommend by demonstrated capability gap, not by a fixed shopping list |

**Important:** The table illustrates possible tool choices, not a guaranteed current feature matrix. Confirm relevant REA capabilities and supported providers against current upstream documentation before relying on an entry. Ghidra, ILSpy, and similar applications are *software tools*. An MCP server, when available, is only one possible interface to a tool; it is not automatically required. Some integrations work directly through REA, others through a CLI, and others may use MCP.

## Rules

- Reuse REA's own diagnostics, documentation, and tool-availability reporting; do not invent a second detection system.
- Make a proactive assessment **even when REA has not raised an error**.
- Recommend **zero** extra tools when none provides a clear benefit. Do not install tools speculatively.
- Check official, current documentation before asserting commands, supported platforms, dependencies, MCP availability, or licensing. If current documentation cannot be reached, state what is unverified and do not invent or present installation commands as verified. Do not imply that an untested integration works.
- Avoid unnecessary setup, configuration changes, and repeated readiness checks.
- Analyze only software the user owns or is authorized to inspect. Respect applicable law and relevant software license terms. Treat unknown executables and downloaded integrations as untrusted; request permission before execution.
- Keep your result short. Give the user the relevant finding and the single next action if one is needed.

## Authoritative starting points

- REA: https://github.com/morluto/rea
- REA setup and installation: https://github.com/morluto/rea/blob/main/docs/getting-started/installation.md
- REA documentation: https://github.com/morluto/rea/tree/main/docs

Consult the current upstream instructions, because supported providers and installation methods can change.
