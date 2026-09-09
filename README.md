I build tooling that keeps codebases working when the libraries under them change.

Your project runs on other people's code. When they change how it works, your app stops working — usually discovered in production, by someone reading a stack trace on a Tuesday afternoon. I work on the part that comes after the version bump: finding what broke, rewriting it, and proving the fix before anyone is asked to trust it.

## Patchery · [patchery.dev](https://patchery.dev)

**Dependabot tells you a dependency changed. Patchery works out what that means for your code — and will not claim a fix it cannot prove.**

This is not an alternative to Dependabot or Renovate. They do one job well: notice a new version and open the bump. By design they do not read your code, so when that bump turns your suite red they have nothing more to offer. Patchery starts there.

Writing the migration is the easy part. Telling a good migration from a plausible-looking one is not: an agent told to make tests pass can pass them by weakening them. So Patchery never reads the agent's own account of what it did. A mechanical guard checks which files moved and discards the whole attempt if anything off-limits did. The test suite is counted before and after, so a run that ends with fewer passing tests is rejected however green it looks — and so is one that ends with more tests *skipped*, because a suite can be made to agree by silencing it as easily as by deleting from it. Where the break stops the tests loading at all there is no count to take, and the pull request says so rather than leaving the row blank. The diff then goes to a second model in a separate call — on a different provider if you configure one — which never sees the first model's reasoning and is asked to refute it.

If the fix cannot be proved, nothing ships and you get the diagnosis instead. Refusing is a designed outcome rather than a shortfall — and how often it happens is one of the numbers the benchmark exists to report, so I would rather publish it than promise it.

It runs as a GitHub Action inside your own CI. The code never leaves your runner.

### Measuring it honestly

A tool like this is only worth what its failure rate says, so I built the benchmark before building more features. It scans well-known repositories for dependencies that have since shipped a breaking major, confirms in a container that the upgrade genuinely turns their own test suite red, then runs Patchery against the break and records what came back — fixed, refused, nothing, or wrong.

Real repositories, real breaks, losses reported alongside wins, and reproducible by anyone: the case list and the workflows are in the repository. Results are published when the full set has run under one set of rules.

The measurement has already been worth it. It found a bug where the action replaced the project's own Node version with its own, which silently healed the very break it had been asked to fix.

### Migrations I did by hand

The same job, done manually on projects I don't own. This is where I learned what the automated version has to survive.

| Repo | What changed |
|---|---|
| [ianarawjo/ChainForge#416](https://github.com/ianarawjo/ChainForge/pull/416) | OpenAI SDK v3 → v4: three call sites, response unwrapping, APIError handling |
| [ToolJet/ToolJet#17829](https://github.com/ToolJet/ToolJet/pull/17829) | Gemini plugin moved off `@google/generative-ai`, end-of-life since 30 Nov 2025 |
| [Caknoooo/chatgpt3-openai-api#2](https://github.com/Caknoooo/chatgpt3-openai-api/pull/2) | `openai` v3 calling `text-davinci-003`, shut down in January 2024; also removed an API key being logged to the console |

Patchery™ is a GitHub Action you install yourself — there is no hosted service and Patchery stores nothing. What leaves your runner goes only to the endpoint you configure: the failing test output, the files the agent opens, and the diff it writes. Source-available under BUSL-1.1, free for public repositories; licence covers code not name. Working in JavaScript, expanding from there.
