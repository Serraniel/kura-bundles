# Instructions for AI agents

This repository holds data that kura applies to users' systems. Read
[docs/format.md](docs/format.md) and kura's
[docs/decisions.md](https://github.com/Serraniel/kura/blob/main/docs/decisions.md)
first.

- Never add shell commands, scripts or anything executable. Only the actions
  listed in docs/format.md.
- Flows match typed error kinds, never pacman's text output.
- Every entry needs `en` text. Add `de`, `fr` and `ja` when you can; follow
  kura's docs/i18n.md glossary.
- State the source of every fix in the pull request (Arch news, forum, bug).
- Conventional Commit titles, `Signed-off-by` on every commit.
- Plain wording, written for people new to Linux. No marketing language, no
  emoji.
