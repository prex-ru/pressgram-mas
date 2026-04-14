# Pressgram MAS

A downstream fork of [matrix-authentication-service](https://github.com/element-hq/matrix-authentication-service)
for [Pressgram](https://pressgram.ru).

Upstream base: **v1.14.0**.

The fork adds the minimum Pressgram-specific layer on top of unmodified
MAS — everything outside `templates/` and `translations/` tracks upstream
and builds with the standard MAS toolchain (`cargo build --release`).

## What this fork changes

| Area | Change |
|------|--------|
| Brand palette | Inline `<style>` in `templates/base.html` and `templates/app.html` remapping `--cpd-color-green-*` to the pressgram-blue Leonardo palette — login, reauth, and the /account/ app all pick up Pressgram Blue without rebuilding compound-web |
| Email button | `templates/emails/recovery.html` — Element charcoal `#1B1D22` / hover / active → Pressgram blue `#1071e0` / 700 / 800 |
| App name | `translations/ru.json` + `en.json` — `app.name` → `"pressgram-id"`, `app.human_name` → `"Pressgram ID"` (drives page titles and `pages/index.html` header) |
| Email copy | `mas.emails.recovery.subject` / `headline` and `mas.emails.verify.subject` — Pressgram wording in both ru and en (MAS renders emails in the user's account locale with en fallback) |
| Deep-link invite flow | `templates/pages/register/steps/registration_token.html` reads the `pressgram_reg_token` cookie (written by prex-ru/pressgram-web's app.tsx on `/join/<server>/<token>`) and pre-fills the invite token input so users don't need to copy-paste it; cookie is cleared on read |

## Building

Same as upstream:

```sh
cargo build --release -p mas-cli
```

The frontend and templates are embedded at compile time — rebuild
after editing either.

## Keeping in sync with upstream

```sh
git remote add upstream https://github.com/element-hq/matrix-authentication-service.git
git fetch upstream --tags
git rebase v1.14.1  # or the next tag
```

All changes touch a narrow surface: `templates/base.html`,
`templates/app.html`, `templates/emails/recovery.html`,
`templates/pages/register/steps/registration_token.html`, and
`translations/{ru,en}.json`. Upstream bumps should rebase cleanly
unless one of those files changes upstream.

## Related Pressgram forks

- [`prex-ru/pressgram-web`](https://github.com/prex-ru/pressgram-web) — Element Web fork (writes the deep-link cookie)
- [`prex-ru/pressgram-compound-design-tokens`](https://github.com/prex-ru/pressgram-compound-design-tokens) — source of the pressgram-blue palette hex values used inline here

## License

Inherits the upstream dual AGPL-3.0 / commercial licence — see
`LICENSE` and `LICENSE-COMMERCIAL` in the project root.
