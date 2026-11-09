---
name: skilltotal-audit
description: Scan npm and PyPI packages, MCP servers, agent skills, repositories and local archives with SkillTotal before installation or adoption. Use when assessing dependency risk, investigating a SkillTotal rule ID, or reviewing third-party agent components.
---

# SkillTotal security audit

SkillTotal is a static security scanner. It reports findings from source files and does not execute the scanned package. A scan result is evidence for review, not proof that a component is safe.

## Workflow

1. Identify the **exact** component and version/ref to check. Supported examples: `npm:express`, `pypi:requests`, a Git URL, a repository path, or a local archive.
2. Check whether the `skilltotal` executable is available. If not, clearly report that no scan was run and explain the documented installation option (`pipx install skilltotal`); do not silently install packages.
3. Scan before installing or granting tools access:

   ```bash
   skilltotal scan npm:express --json
   skilltotal scan https://github.com/owner/repo --json
   ```

   Use the specific user-provided source instead of these examples. Never interpolate untrusted source text into an arbitrary shell expression.
4. Summarize the verdict, risk score, and all **malicious indicators**. For each substantive risk finding, include its rule ID and exact file/line evidence. Distinguish capability observations (shell, network, filesystem, credentials) from risk findings.
5. If the verdict is malicious, recommend not installing. For high/critical risk or incomplete scans, request human review before installation. For a clean result, state that static analysis is not a guarantee.
6. For a named rule, use `skilltotal rules list` (or the MCP `list_rules` tool, if available) and cite its documented description. Do not infer severity from the rule identifier alone.

## Enforcement and MCP

- For an explicitly requested pre-install gate, the CLI supports `skilltotal guard <source>` and returns a nonzero exit code when it blocks an installation.
- If the SkillTotal MCP server is registered, prefer its read-only `scan_component`, `diff_components` and `list_rules` tools to shell invocation.
- The Claude Code plugin already supplies a pre-install hook. This skill complements the hook but does **not** install or activate it; do not claim continuous install protection unless the hook is actually enabled.
- The CLI and MCP server must exist in the environment where the agent runs. A repository link does not install them.

## Source

See [SkillTotal CLI documentation](https://github.com/abyss68686/skilltotal/blob/main/docs/cli.md), [MCP documentation](https://github.com/abyss68686/skilltotal/blob/main/docs/mcp-server.md), and [plugin behavior](https://github.com/abyss68686/skilltotal/blob/main/docs/plugin.md).
