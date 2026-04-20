# Contributing to PyGuard

Thanks for considering a contribution to PyGuard. PyGuard is in a public beta and we actively want external feedback, bug reports, and pull requests.

This document is the practical guide for anyone working on the codebase. The higher-level narrative lives on the [Contribute page](https://pyguard-final-project-webpage.vercel.app/contribute) of our documentation site.

## Ways to contribute

- **File a bug report** using the [bug report template](./.github/ISSUE_TEMPLATE/bug_report.yml). The template collects the minimum context we need to reproduce.
- **Propose a feature** using the [feature request template](./.github/ISSUE_TEMPLATE/feature_request.yml). Describe the pain point first, the idea second.
- **Improve the docs**. The website content lives under [`website/app/`](./website/app). MDX edits and typo fixes are very welcome.
- **Send a pull request** following the guide below.

Please read the [Code of Conduct](./CODE_OF_CONDUCT.md) before you contribute. We enforce it on every issue, PR, and discussion.

## Our triage SLA

- **Acknowledgement:** every external issue gets a human reply within two business days.
- **Decision:** we return an accept / defer / decline call within five business days.
- **Severity 1** (crash in the primary flow, data loss, suspected security issue): same day.

We are a small team. Please be patient outside these windows.

## Setting up a development environment

1. Clone the repo:

   ```bash
   git clone https://github.com/dcsil/PyGuard.git
   cd PyGuard
   ```

2. Follow the [Setup guide](https://pyguard-final-project-webpage.vercel.app/setup) for the full install walkthrough. The short version of the backend setup is:

   ```bash
   cd backend
   python3 -m venv .venv
   source .venv/bin/activate
   pip install -r requirements.txt
   cp .env.example .env   # fill in your OpenAI, Composio, and Slack tokens
   uvicorn main:app --reload --port 8000
   ```

3. For the website, see [`website/README.md`](./website/README.md).

## Running the tests

All backend tests live in `backend/tests/` and are plain `unittest`.

```bash
cd backend
source .venv/bin/activate
python -m unittest discover -s tests -v
```

The suite must be green (26 passing tests at the time of this writing) before any PR is merged.

## Pull request flow

1. **Open an issue first for anything non-trivial.** This saves you from building something that does not fit the current roadmap.
2. **Fork and branch.** Use short, descriptive branch names: `fix/thread-lookup`, `feat/email-export`, `docs/troubleshooting-tectonic`.
3. **Small, focused PRs please.** One logical change per PR. It is far easier to review three small PRs than one large one.
4. **Run the test suite** locally. Add tests for new behaviour where it makes sense. The `tex_editor` module is a good example of what we think a well-tested module looks like.
5. **Update docs.** Any user-facing change should include a matching MDX update in `website/app/docs/`.
6. **Fill out the PR template** completely. The "how was this tested" section is not optional.
7. **Wait for review.** We aim to have a first response within two business days.

## Coding style

- **Python.** Follow the style of the surrounding code. No formatter is enforced in CI yet, but we lean PEP 8 with a 100-character soft limit.
- **TypeScript / React.** Standard Next.js conventions. Components live in `website/components/`; pages in `website/app/`. Prefer server components unless you need interactivity.
- **Comments.** Keep them short, explain intent when the code cannot. Avoid comments that narrate what the next line does.
- **Secrets.** Never commit real tokens or API keys. `backend/.env` is git-ignored; there is an example file next to it.

## Security

If you believe you have found a security vulnerability, please **do not open a public GitHub issue**. Instead, email the maintainer contact listed in [`CODE_OF_CONDUCT.md`](./CODE_OF_CONDUCT.md) so we can triage the report privately.

## Release cadence

This is a beta release. We cut a new tag roughly every two to three weeks or when a significant feature lands. Breaking changes are called out in the release notes.

## Questions?

- For product questions, start with the [docs](https://pyguard-final-project-webpage.vercel.app/docs/getting-started).
- For contribution questions that are not in this file, open a GitHub discussion or email the maintainer contact in [`CODE_OF_CONDUCT.md`](./CODE_OF_CONDUCT.md).

Thanks again.
