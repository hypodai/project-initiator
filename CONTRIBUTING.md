# Contributing

Thanks for looking. Issues and pull requests are welcome — especially runs
of the skill that went wrong, with the prompt you used and what you expected.

## Before you change the skill

The product is the text in `skills/project-initiator/`. A few rules keep it
working for everyone:

- **Explain why, don't stack MUSTs.** The model follows reasoning better
  than rules. A new instruction should say what problem it prevents.
- **Keep `SKILL.md` lean** (under ~300 lines). Detail belongs in
  `references/`, which the skill loads only in the step that needs it.
- **Stay locale-neutral.** No assumed country, currency, regulation or
  language. Regional specifics (EU, GDPR, EUR…) appear only as one example
  among several. Everything in the repository is written in English; the
  skill itself answers in the user's language.
- **No vendor favouritism.** The skill exists to research and score the
  market, not to recommend a fixed list of tools. Don't add "always use X".

## With every change

1. Add a line to `CHANGELOG.md` under `[Unreleased]`.
2. If behaviour could change, run the relevant prompts from
   `evals/evals.json` with and without the skill and compare
   (`evals/RESULTS.md` shows how the first run was done and graded).
   Say in the pull request which evals you ran — a full run costs a lot of
   tokens, so one or two targeted evals are fine.
3. Keep templates and the self-check in `SKILL.md` consistent: if a template
   gains a placeholder or a file, the self-check must still pass.

## Security issues

Not in public issues — see [`SECURITY.md`](SECURITY.md).

## Licence

By contributing you agree that your contribution is licensed under the
[MIT licence](LICENSE).
