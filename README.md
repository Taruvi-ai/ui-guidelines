# Taruvi design context

Four documents. Each is read in a different situation; none duplicates another.
If you find the same value in two files, that is a bug — delete one.

## What to read when

| You are… | Read |
| --- | --- |
| Building UI inside a Taruvi app: provider wiring, hooks, list/filter/sort/pagination | `taruvi-refine-providers` skill (in `Taruvi-ai/taruvi-skills`) |
| Building UI inside a Taruvi app: which component, which prop, DataGrid v7 specifics | `taruvi-ui/components.md`, `taruvi-ui/datagrid.md` (this repo) |
| Deciding what to build, or how it should behave | `UX.md` |
| Building Taruvi-looking UI with no component library | `DESIGN.md` |
| Orienting in a specific app repo | that repo's own `AGENTS.md` |

## The files

### `DESIGN.md` — tokens and visual rationale

Follows the open DESIGN.md format: machine-readable tokens in YAML front matter,
rationale in prose. Lint with `npx @google/design.md lint DESIGN.md`; export to
Tailwind or DTCG with `npx @google/design.md export`.

**Scope matters.** This file describes how to build Taruvi-looking UI from
scratch. That is correct for external design tools, prototyping, and client
demos. Inside a Taruvi app it is the wrong file — using it there produces
duplicate components that drift from the real ones.

Its Token Contract section lists which token *names* every valid Taruvi
`DESIGN.md` must define, including a client's reskinned copy — the names are
the shared contract, not the values behind them.

### `UX.md` — what to build and why

User model, world model, research synthesis, glossary, interaction standards,
page patterns. Rules here are behavioral, not visual.

Three sections are deliberately unfilled — Research Synthesis, and parts of the
user and world models. They are the highest-leverage content in the whole set,
and inventing them would have been worse than leaving them empty. A finding
belongs here only if it changes what gets generated.

### `taruvi-ui/` — component and DataGrid reference

Two files, read only when the task needs them: `components.md` (which chip
variant, which button size, which prop) and `datagrid.md` (MUI DataGrid v7
behaviors that look like bugs and are not, pinned to v7 — see the version note
at the top of that file). Not a skill with its own install/routing machinery —
plain reference docs, kept separate from `taruvi-refine-providers` because that
skill already owns Refine/provider wiring and these don't overlap it.

### `AGENTS.md` — per-app orientation

Template. Copy into each app repo and fill in the paths. Deliberately separate
from the shared docs because paths differ per app and go stale fastest.

A client's own `DESIGN.md` (a rebrand/reskin) also lives per-app, next to that
app's `themeOptions.ts` — never in this shared repo. See `DESIGN.md`'s Do's and
Don'ts for what must carry over from Taruvi's default and what must be
re-verified.

### `LINT_CANDIDATES.md` — working list, not a permanent doc

Rules a linter can enforce deterministically at zero token cost. Delete each
entry as the rule ships, and delete the rule from `UX.md` at the same time.

## Open items

1. **Review chip hue.** `status-review` is `#bf360c` — it's an existing step in
   the theme's warning ramp, not a new color, but it may still collide with the
   error chip. Compare them side by side before treating this as settled.
2. **"No toasts" vs. the current app.** `UX.md`'s Action Feedback model and
   `LINT_CANDIDATES.md`'s corresponding rule describe a toast-free feedback
   system that `taruvi-refine-template` does not implement today — it wires
   `RefineSnackbarProvider`/`useNotificationProvider`, and its own `AGENTS.md`
   mandates that. Resolve one way or the other — build the row/undo/banner
   model for real, or revise the policy to match what's actually shipped —
   before enabling the lint rule even informationally.
3. **`ui-ux-reviewer.md` needs updating**, in `taruvi-refine-template`, to read
   `DESIGN.md` + `UX.md` + `taruvi-ui/*` instead of the retired single
   `UI_Guidelines.md` URL, or its WCAG audit quietly gets narrower than before.
4. **`ui-ux-review.yml`'s trigger paths** only fire on `src/pages/**`/
   `src/components/**` — a `themeOptions.ts`-only change, exactly where token
   drift gets introduced, currently skips review entirely.
5. **`AGENTS.md` paths** need filling per app.
6. **Error/on-error.** Filled from `themeOptions.ts` (`error[600]`, verified at
   5.87:1 white-on-fill) — the claim that these previously "borrowed
   `status-review`" didn't check out against the actual app code, so it was
   dropped rather than carried forward.

## Maintaining this

The set is never finished. It changes when the product changes, when research
lands, and when you catch an agent getting something wrong — that last one is an
input, not a failure. Log the correction rather than re-prompting around it.

Two hazards worth naming, both found while building this:

**Named exception lists certify everything absent from them.** A rule that said
two specific chips needed a measured label left two other failing chips
shipping unnoticed for as long as the rule existed. Prefer a rule that applies
to a whole family with no exceptions, and enforce it in lint.

**Rules without reasons don't generalize.** A prohibition tells an agent what
not to do on a screen you anticipated. The user model tells it what to do on one
you didn't.
