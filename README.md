**Building tooling that keeps codebases in sync with the APIs and SDKs they depend on.**

Most teams find out a dependency broke *after* it's already in production. I built an agent-based system that catches this earlier — it reads changelogs and deprecation notices, finds the affected code, verifies the fix against your own tests, and only then opens a PR.

<br>

### <img src="patchery-logo.png" width="18" height="18" alt=""> [Patchery](https://github.com/patchery-dev/Patchery)

When a dependency breaks your code, Patchery fixes it and proves the fix. It never trusts the agent's own report — it re-runs your tests and reverts everything if the agent touched a test file, `node_modules`, or a lockfile.

[See it catch and fix a real breaking change, end-to-end, with no human in the loop →](https://github.com/patchery-dev/Patchery/pull/2)

<br>

**Recent real-world contributions**

| Repo | What changed |
| --- | --- |
| [ianarawjo/ChainForge#416](https://github.com/ianarawjo/ChainForge/pull/416) | Migrated OpenAI SDK usage (chat, image generation, Together.ai) from v3 to v4 |
| [ToolJet/ToolJet#17829](https://github.com/ToolJet/ToolJet/pull/17829) | Migrated the Gemini plugin off the deprecated `@google/generative-ai` SDK |
| [langchain-ai/langchainjs#10533](https://github.com/langchain-ai/langchainjs/issues/10533) | Verified old→new SDK type mapping, shared as a reference on a known issue |

<br>

Currently focused on JS/TypeScript, expanding from there.
