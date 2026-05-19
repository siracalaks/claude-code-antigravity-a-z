# Contributing

Thanks for considering a contribution. This repo is a single-file knowledge guide — small, focused contributions are the easiest to land.

**🌍 Language:** **English** · [Türkçe](CONTRIBUTING.tr.md)

---

## What kind of contributions are welcome?

- **Typo / grammar fixes** — open a PR directly, no issue needed.
- **Factual corrections** — if a CLI flag, command, version number, or behavior described in the guide is wrong, please correct it and cite the source (release notes, official docs, commit) in the PR description.
- **New content** — chapter expansions, additional workflow patterns, missing slash commands, new MCP examples.
- **Translations** — a new language? See the [Translations](#translations) section.
- **Examples** — runnable snippets in [`examples/`](examples/) (skills, hooks, GitHub Actions). PRs that move inline code blocks into real files are welcome.

## What is NOT welcome

- Adding ads, affiliate links, or self-promotion that does not serve the reader.
- Rewriting large sections without prior discussion — open an issue first.
- Splitting `GUIDE.md` into multiple files. The single-file design is intentional (see README "Quick Start").

---

## EN ↔ TR sync rule (the most important rule)

This repo maintains **two source-of-truth files** that must stay in sync:

- [`GUIDE.md`](GUIDE.md) — English
- [`REHBER.md`](REHBER.md) — Türkçe

**If you change one, you must change the other in the same PR.**

If you only speak one language:
1. Make the change in the language you know.
2. Add a checkbox-style note in the PR: `- [ ] Needs TR translation` or `- [ ] Needs EN translation`.
3. A maintainer or translator will handle the other side before merge.

PRs that update only one language without flagging the other will be asked to either translate or mark the gap explicitly.

---

## How to submit a PR

1. Fork the repo and create a branch: `git checkout -b fix/typo-chapter-4`
2. Make your change. Keep the diff focused — one fix per PR is easier to review.
3. Commit with a [Conventional Commit](https://www.conventionalcommits.org/) style message:
   - `fix: correct /compact threshold in cheat sheet`
   - `docs: add workflow pattern for monorepo`
   - `feat(examples): extract conventional-commit skill`
4. Push and open a PR against `main`. Use the PR template — it asks for source citations for factual changes.

## Style guide

- **Tone:** practical, terse, opinionated where useful. The guide is a working reference, not a marketing page.
- **No marketing language:** avoid "powerful", "amazing", "game-changing". State what something does.
- **Code blocks:** always tag the language (` ```bash `, ` ```yaml `, ` ```markdown `).
- **Tables:** prefer tables over bullet lists when comparing 3+ items across the same dimensions.
- **Anchors:** if you add a new section, add a matching anchor and link from the table of contents.
- **Date format:** ISO 8601 (`2026-05-19`) — relative dates rot.

## Translations

To add a new language (e.g., German):

1. Copy `GUIDE.md` to `GUIDE.de.md` and translate.
2. Copy `README.md` to `README.de.md` and translate.
3. Add the language to the language switcher at the top of each `README.*`.
4. Open a PR titled `feat(i18n): add German translation`.
5. Be prepared to keep your translation in sync with future EN/TR changes, or note in the PR that the translation may lag.

## Reporting issues

- **Bug / wrong information** → use the *Content correction* issue template.
- **Suggestion / new chapter** → use the *Content suggestion* issue template.
- **Translation help wanted** → use the *Translation* issue template.

## Code of conduct

Be kind. Disagreements about technical content are welcome; personal attacks are not. Maintainers reserve the right to close or lock unproductive threads.

---

**Questions?** Open a [discussion](https://github.com/siracalaks/claude-code-antigravity-a-z/discussions) or ping in the issue you're working on.
