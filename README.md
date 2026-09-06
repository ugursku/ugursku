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
moved, re-runs your tests itself, and hands the diff to a second agent that has no
write access and never sees the first one's reasoning. Only then does it open a
pull request.

One run, end to end, with no human in the loop:

| | files | lines | tests | turns | cost at list rates |
| --- | --- | --- | --- | --- | --- |
| [patchery-dev/Patchery#2](https://github.com/patchery-dev/Patchery/pull/2) | 1 | +1 −1 | failed → passed | 9 | $0.2251 |

The second agent's verdict on that diff: not refuted, confidence 72, $0.3298. Proving
the fix cost more than making it, which is the part I find most interesting about
this problem.

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

**Where it came back empty**

Pointed at four repositories I don't own, it has opened nothing — and that record is
more useful than the one above. One spent 25 turns working out that the failing tests
were about my machine rather than the library, and refused to invent a change. One was
a stateful-to-stateless API redesign bigger than a single run; twice my own stall rule
cut it off just as it was about to start writing, which is how I found out the rule was
wrong. One was a real, reported break that had already healed on the newer Node I ran
on. One had no tests at all, so there was nothing to prove a fix against.

Not one of them is a wrong fix. The whole point is the refusing.

<br>

**Where this actually is:** Patchery is a GitHub Action you install yourself. There
is no hosted service, no revenue and no users. What exists is the code, the runs that
worked, the runs that did not, and the pull requests above. [patchery.dev](https://patchery.dev) checks its
own claims against the GitHub API in your browser while you read it — including the
sentence about nothing being merged.

Working in JavaScript and TypeScript, expanding from there.
