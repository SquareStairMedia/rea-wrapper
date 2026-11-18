---
name: rea-wrapper
description: Check whether Reverse Engineer Anything (REA) would benefit from additional tools before an AI coding agent investigates or recreates existing software. Use at the start of a REA-assisted investigation or when REA's findings appear incomplete.
---

# REA Wrapper

You are the user's AI coding agent. This skill is a lightweight **preflight checklist**, not a reverse-engineering engine or a replacement for REA.

**Goal:** Prevent users from unknowingly limiting an investigation to REA's currently available tools when a supplementary tool could materially improve the result.

## Workflow

1. **Check REA access.** Determine whether REA MCP tools are usable in *this agent session*. If not, consult the official REA setup instructions and offer the smallest necessary setup action. A skill installation alone does not guarantee a live MCP connection. Do not require cloning or forking REA. Do not repeatedly run setup if the connection already works.

2. **Identify the target and task.** From the user-provided application, files, or existing findings, establish the platform/framework and what the user wants to learn or reproduce. Use REA's existing inspection capabilities where helpful. State what remains uncertain. Do not run an unknown executable merely to identify it.

3. **Proactively assess tool coverage.** Consult REA's current capabilities and the tools actually available to the agent. Ask BOTH:
   - Can REA perform the relevant investigation with its built-in capabilities or already configured providers?
   - Would another tool materially improve evidence quality, coverage, or efficiency for THIS task, even if REA is already producing results?

   Do not equate a working REA connection with a complete toolchain. Do not equate a tool's popularity with a requirement.

4. **Check before recommending installation.** For each worthwhile supplementary tool, determine whether it is already installed and usable, using non-destructive checks appropriate to the user's OS. Verify whether REA integrates with it directly, whether Codex can use its CLI, or whether a separate MCP server is actually useful. Prefer existing tools and REA's native integrations over duplicate MCP layers.

5. **Propose the minimum change.** Report one of three outcomes:
   - **Ready:** No additional tool is justified for the current investigation.
   - **Optional improvement:** Name the tool, the specific benefit, and whether it is already available.
   - **Required capability missing:** Name the blocked operation and the tool/provider needed to perform it.

   If a change is warranted, identify the official source, platform requirements, license/cost where relevant, and the smallest setup path. Distinguish verified requirements from suggestions and unknowns. Obtain explicit user approval **before** installing software, registering MCP servers, modifying configuration, or running untrusted targets.

6. **Verify and hand back.** If a tool is configured, perform a small, relevant functionality check where safe and authorized; otherwise mark it *unverified*. Summarize the available toolchain and any remaining limitations. Then hand control back to the agent's normal REA-assisted investigation/reconstruction workflow. **Do not run or manage the reconstruction as part of this skill.**

## Tool-selection reminders (examples, not automatic dependencies)

| Target / need | REA baseline | Consider only if needed |
| --- | --- | --- |
| JavaScript / Electron | REA supports static JavaScript and Electron inspection | Additional unpacking, debugging, or runtime tools for gaps the task exposes |
| .NET assemblies | REA supports static metadata and CIL inspection | ILSpy / `ilspycmd` when higher-level managed-code decompilation would help |
| Native binaries | REA has inspection capabilities, but deeper native decompilation uses an external provider | An REA-supported provider such as Ghidra, Hopper, IDA, or Kuna, subject to OS/support/licensing |
| Android APKs | Some APK inspection is built in | JADX plus its prerequisites for decompiled methods; other Android tools only for specific needs |
| Other or mixed targets | Check the latest REA documentation and available tools | Recommend by demonstrated capability gap, not by a fixed shopping list |

**Important:** Ghidra, ILSpy, and similar applications are *software tools*. An MCP server, when available, is only one possible interface to a tool; it is not automatically required. Some integrations work directly through REA, others through a CLI, and others may use MCP.

## Rules

- Reuse REA's own diagnostics, documentation, and tool-availability reporting; do not invent a second detection system.
- Make a proactive assessment **even when REA has not raised an error**.
- Recommend **zero** extra tools when none provides a clear benefit. Do not install tools speculatively.
- Check official, current documentation before asserting commands, supported platforms, dependencies, MCP availability, or licensing. Do not invent commands or imply that an untested integration works.
- Avoid unnecessary setup, configuration changes, and repeated readiness checks.
- Analyze only software the user owns or is authorized to inspect. Treat unknown executables and downloaded integrations as untrusted; request permission before execution.
- Keep your result short. Give the user the relevant finding and the single next action if one is needed.

## Authoritative starting points

- REA: https://github.com/morluto/rea
- REA setup and installation: https://github.com/morluto/rea/blob/main/docs/getting-started/installation.md
- REA documentation: https://github.com/morluto/rea/tree/main/docs

Consult the current upstream instructions, because supported providers and installation methods can change.
