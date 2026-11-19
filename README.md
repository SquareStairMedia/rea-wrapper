# REA Wrapper

**A lightweight companion skill for [Reverse Engineer Anything (REA)](https://github.com/morluto/rea).**

REA can analyze many kinds of software, but some investigations benefit from additional tools such as native decompilers, managed-code decompilers, or platform-specific utilities. A working REA connection does not necessarily mean the most useful tools for your target are available.

**REA Wrapper reminds your AI coding agent to check that before the investigation starts.**

It helps the agent:

1. Confirm REA is usable in the current session.
2. Identify the software being investigated and the capabilities the task actually needs.
3. Consider whether supplementary tools could materially improve the result, *even if REA appears to work*.
4. Check whether those tools are already installed and whether REA, a CLI, or MCP is the appropriate connection method.
5. Recommend the smallest useful setup change, request approval, verify what works, and hand control back to the agent.

The wrapper may also conclude that **nothing else is needed**.

## What this is

One reusable agent skill: [`SKILL.md`](./SKILL.md). There is no Python, custom MCP server, install script, or separate application. REA performs the analysis; your coding agent makes the tool-selection decisions using the skill's instructions.

This project does **not** automatically install REA or other tools, guarantee that software can be reconstructed, or replace the original REA project.

## Install in a compatible AI coding agent

REA Wrapper uses the agent-skill format and is intended for AI coding agents that support skills and can access REA. It is not tied to a particular agent or vendor.

The open-source [`skills` CLI](https://github.com/vercel-labs/skills) can install skills from GitHub and lets you select a supported agent:

```sh
npx skills add SquareStairMedia/rea-wrapper
```

The CLI currently documents Node.js 22.20 or later as a requirement. By default, it installs to the current project; add `-g` to make the skill available across projects. Review the installer prompts and choose the agent you actually use. This repository's installation has **not yet been tested** in a live agent session.

Once installed, invoke `rea-wrapper` using your agent's supported skill selection or invocation method. Exact syntax varies by agent; a slash command is not guaranteed in every client. If the skill is not detected, consult your agent's skill documentation and installation location.

**REA itself is a separate dependency.** If its MCP tools are not already connected, the skill directs the agent to [REA's official setup instructions](https://github.com/morluto/rea/blob/main/docs/getting-started/installation.md). Installing a skill by itself does not create a live MCP connection.

## Example use

> Use rea-wrapper before we investigate this legacy desktop application. Check whether REA's current toolchain is sufficient, and flag any worthwhile extra tools before we start rebuilding it.

Possible outcomes include:

- **No additional tools recommended** for the current task.
- **ILSpy may help** when REA's .NET metadata/CIL results do not provide the level of readable code needed.
- **A supported native analysis provider is required** before deep native-binary decompilation can proceed.

A suggestion is not the same as a confirmed requirement. The agent is instructed to explain which is which.

## Scope

REA Wrapper is a **preflight assessment**, not a reverse-engineering pipeline, automation framework, or reconstruction agent. It deliberately avoids collecting every possible tool or installing tools just because they exist.

The skill relies on current REA and tool documentation rather than claiming a fixed list of universally required dependencies.

## Relationship to REA

This is an **unofficial, independent companion project** by SquareStair Media. It is not affiliated with or endorsed by the REA maintainers. REA and third-party tools remain under their respective licenses.

Use it only with software you own or are authorized to analyze. Do not run untrusted binaries or install untrusted integrations without appropriate precautions.

## Development workflow

Propose changes on a separate branch and open a pull request against `main`. The repository maintainer reviews and explicitly approves each PR before it is merged. Do not commit directly to `main` as part of the normal development workflow.

## License

MIT. See [`LICENSE`](./LICENSE).
