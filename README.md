# Taruvi UI/UX Guidelines

Design-system rules for apps built on the Taruvi MUI theme — the parts a
theme file can enforce with code, and the parts (page anatomy, copy,
behavior, accessibility) it can't.

## What's here

- [`UI_Guidelines.md`](UI_Guidelines.md) — the guidelines themselves.

## How this repo is consumed

This repo is versioned separately from any one app so every fork/app
reads the same rules instead of a vendored copy that drifts. Consumers
`WebFetch` the raw file fresh on every use rather than reading a local
copy:

```
https://raw.githubusercontent.com/Taruvi-ai/ui-guidelines/main/UI_Guidelines.md
```

In the `taruvi-refine-template` repo, this fetch is the mandatory UI
preflight documented in `AGENTS.md`, run by the `taruvi-frontend` and
`ui-ux-reviewer` agents before touching anything that renders UI.

There's no version pinning yet — `main` is the only branch, and every
consumer always gets the latest commit.

## Token values live elsewhere, on purpose

`UI_Guidelines.md` states *which* token to reach for and *why* — it never
restates a token's literal value (hex, ratio, px). Values live in exactly
one place: the consuming app's own theme file (`taruviTokens`, typically
`themeOptions.ts`). This isn't only about preventing drift — different
apps/forks legitimately have different values in there (a client reskin, a
rebrand), so there is no single correct value this repo could state even if
it wanted to. This repo owns the *shape* of `taruviTokens` (the token names
and what each is for) — every fork's design is expected to expose those same
names — never the values behind them.

If you're editing this file and about to type a hex code, stop and use the
token's dotted path (`taruviTokens.status.review`) instead. The one
exception is a computed accessibility fact (a contrast ratio, an overlay
alpha) that only makes sense stated as a number — those are allowed, but
must name the token they're computed against and say the fact only holds
"at its current value" / "re-measure if reskinned." An unqualified ratio in
this file is a bug: it reads as a universal fact when it's actually true for
Taruvi's own default palette only.
