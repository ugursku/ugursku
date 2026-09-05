### Building tooling that keeps codebases in sync with the APIs and SDKs they depend on.

Most teams find out a dependency broke *after* it's already in production. I'm
working on an agent-based system that catches this earlier — reading
changelogs and deprecation notices, finding the affected code, and proposing
a verified fix before anyone has to notice the hard way.

**Recent real-world contributions:**
- [ToolJet/ToolJet#17829](https://github.com/ToolJet/ToolJet/pull/17829) — migrated the Gemini plugin off the deprecated `@google/generative-ai` SDK to `@google/genai`
- [langchain-ai/langchainjs#10533](https://github.com/langchain-ai/langchainjs/issues/10533) — verified old→new SDK type mapping shared as a reference for a known deprecated-dependency issue

Currently focused on JS/TypeScript, expanding from there.
