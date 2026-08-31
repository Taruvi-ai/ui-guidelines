# Rules that should be lint rules, not context

Every rule below is deterministic — a linter can decide it from the AST with no
model involvement and no token cost. Moving them out of the context files makes
those files cheaper to load *and* makes the rules actually enforced rather than
merely suggested.

A lint rule enforces for humans and agents alike, on every commit, at zero token
spend. A context rule only enforces when an agent happens to read it.

## High confidence — straightforward custom rules

| Rule | Check |
| --- | --- |
| No click handler on a non-interactive element | `onClick` on `Box`, `div`, `span` without a `role` |
| Icon-only controls need a name | `IconButton` with no text child and no `aria-label` |
| Rounded icon variants only | Import from `@mui/icons-material` not matching `*Rounded` |
| No `text.disabled` for content | `color="text.disabled"` outside a disabled control |
| Errors use `role="alert"` | Error components missing the role |
| No hardcoded brand hex | Hex literal in `sx` that matches a `taruviTokens` value |
| No hand-set chip color | `color`, `borderColor`, or a label color in `sx` on a themed `Chip` |
| Status fills pass on white | Any `status-*` token below 4.5:1 against `#ffffff` |
| Fill blue never on text | `fill-accent` / `#2b97ff` used as a `color` value |
| Controls carry a boundary | Input, select, or toggle with no `border-control` |
| Hit areas meet 24x24 | Interactive element whose computed box is under 24px either axis |
| Reduced motion honoured | A `transition` or `animation` with no `prefers-reduced-motion` rule |
| No theme-duplicating `sx` | `borderRadius`, `fontFamily`, `fontSize` on a themed component |
| Modals need a label | `Dialog` without `aria-labelledby` |
| No toasts ⚠️ **not yet enforceable — see below** | Any `Snackbar` / `toast` import or usage |
| No fixed DataGrid height | `height` set on a DataGrid instead of `autoHeight` |
| Sortable headers expose state | Sortable column header without `aria-sort` |
| Chip remove names the value | Chip `onDelete` whose `aria-label` is a bare "Remove"/"Delete" |
| Columns don't filter | Filter control rendered inside a column header or menu |
| No per-page overlay height | `--DataGrid-overlayHeight` set outside the theme |

**"No toasts" is not ready to ship, blocking or not.** `taruvi-refine-template`
currently wires `RefineSnackbarProvider`/`useNotificationProvider` and its
`AGENTS.md` mandates using it — that's the correct, current pattern, and this
rule would flag it as a violation. Don't enable this rule (even informationally)
until `UX.md`'s Action Feedback model is either actually built or the policy is
revised to match reality. See `UX.md`'s Action Feedback section and the
guidelines repo README's Open Items.

## Partial — lintable with a heuristic, still worth it

| Rule | Approach |
| --- | --- |
| One `<h1>` per page | Count `component="h1"` per route file |
| Inputs need a visible label | `TextField` with `placeholder` but no `label` |
| Explicit image dimensions | `<img>` without `width` and `height` |
| Filter state in Refine | `useState` holding something named like a filter in a list page |

## Not lintable — keep in context

Judgment calls that need a model: whether a chip is status or tag, whether a
foreground tone passes on a hovered row, whether an empty state's CTA matches
its cause, whether density suits the user, error message wording, glossary
adherence.

## Suggested order

1. Ship the high-confidence rules first — they cover the largest share of the
   accessibility list. Skip "no toasts" until its blocker (above) clears.
2. Strip those rules from `UX.md` once the lint rule lands, leaving a one-line
   pointer so a reader knows where enforcement moved.
3. Wire the rules into the existing `ui-ux-review.yml` workflow, and expand its
   trigger paths to include `themeOptions.ts` — today it only fires on
   `src/pages/**`/`src/components/**`, so a theme-only change that introduces
   token drift currently skips review entirely. Consider making the
   high-confidence set blocking, since a deterministic rule with no false
   positives is safe to block on — unlike the current informational agent.
