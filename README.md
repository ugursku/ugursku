**I build tooling that keeps codebases working when the libraries under them change.**

Your project runs on other people's code. When they change how it works, your app
stops working — usually discovered in production, by someone reading a stack trace
on a Tuesday afternoon. I am working on the part that comes after the version bump:
finding what broke, rewriting it, and proving the fix before anyone is asked to
trust it.

<br>

### <img src="patchery-logo.png" width="18" height="18" alt=""> [Patchery](https://github.com/patchery-dev/Patchery) · [patchery.dev](https://patchery.dev)

**When a dependency breaks your code, Patchery fixes it and proves the fix.**

Writing the fix is the easy part. Tell an AI to make your tests pass and it can pass
them by deleting them — so Patchery never reads the agent's own account of what it
did. It looks at the files, throws the whole attempt away if anything off-limits
moved, re-runs your tests itself, and only then opens a pull request.

One run, end to end, with no human in the loop:

| | files | lines | tests | turns | cost |
| --- | --- | --- | --- | --- | --- |
| [patchery-dev/Patchery#2](https://github.com/patchery-dev/Patchery/pull/2) | 1 | +1 −1 | failed → passed | 11 | $0.1213 |

<br>

**Pull requests I opened by hand**

The same job Patchery automates, done manually on projects I don't own. This is how
I learned what the automated version has to survive.

| Repo | What changed |
| --- | --- |
| [ianarawjo/ChainForge#416](https://github.com/ianarawjo/ChainForge/pull/416) | OpenAI SDK v3 → v4: three call sites, plus response unwrapping and `APIError` handling |
| [ToolJet/ToolJet#17829](https://github.com/ToolJet/ToolJet/pull/17829) | Gemini plugin moved off `@google/generative-ai`, end-of-life since 30 Nov 2025 |
| [Caknoooo/chatgpt3-openai-api#2](https://github.com/Caknoooo/chatgpt3-openai-api/pull/2) | Server was on `openai` v3 calling `text-davinci-003`, shut down in Jan 2024; also removed a key being logged to the console |

All three are open. None has been merged. I will change this sentence the day that
changes.

<br>

**Where this actually is:** Patchery is a GitHub Action you install yourself. There
is no hosted service, no revenue and no users. What exists is the code, one run that
worked, and the pull requests above. [patchery.dev](https://patchery.dev) checks its
own claims against the GitHub API in your browser while you read it — including the
sentence about nothing being merged.

Working in JavaScript and TypeScript, expanding from there.
