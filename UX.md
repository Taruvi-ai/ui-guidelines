---
version: alpha
name: Taruvi UX
description: UX context for AI-generated work on the Taruvi platform. Research, user and world models, glossary, and interaction standards.
references:
  visual-standards: ./DESIGN.md
  implementation: taruvi-ui/components.md, taruvi-ui/datagrid.md
targets:
  hit-area-min: 24px
  touch-min: 24px
  primary-action-min: 44px
  icon-button-table-row: 30px
  focus-ring-width: 2px
contrast:
  text-min: 4.5
  non-text-min: 3
  control-boundary-min: 3
timing:
  search-debounce-min: 300ms
  search-debounce-max: 500ms
defaults:
  page-size: 10
  list-columns-visible: 6
---

# Taruvi UX

This file is context for AI tools generating Taruvi interfaces. It is not written
for humans to be persuaded by — its success is measured by whether generated
output improves.

Inside a Taruvi application, `taruvi-refine-providers` is the primary source for
*how to build* — provider wiring, Refine v5 hooks, list/filter/sort/pagination
wiring. `taruvi-ui/components.md` and `taruvi-ui/datagrid.md` cover component
import/prop guidance and MUI DataGrid v7 specifics. This file covers *what to
build and why*. `DESIGN.md` holds token values and is only for environments
without the component library.

Rules marked **[lint]** are candidates to move into ESLint; see
`LINT_CANDIDATES.md`. Once a lint rule ships, delete the rule from here.

Sections are ordered by how much they change generation. The models and findings
at the top tell an agent what to build; the standards below tell it what not to
get wrong.

---

## User Model

> **Status: needs research input.** The entries below are inferred from the
> existing guidelines and should be confirmed, corrected, or replaced. An
> inferred user model is still better than none, but it is not evidence.

- **Expertise:** Inferred expert. Users operate the product regularly and know
  the domain vocabulary; they are not being onboarded on each visit.
- **Session shape:** Inferred long-session. Dense tables, row virtualization,
  server-side pagination, and 13px body type all imply sustained work rather
  than glances.
- **Primary activity:** Inferred as triage and record management — scanning many
  records, filtering to a subset, opening one, acting on it.
- **What doesn't work for them:** *(unfilled — what has research established?)*
- **What they are trying to accomplish:** *(unfilled)*

**Generation consequence:** favor density and information retention over
progressive disclosure. Keep more in view. Do not add tutorial affordances,
welcome tours, or explanatory helper text that an expert would find noisy.

## World Model

> **Status: needs research input.** Conditions users are under while working.

- **Interruption:** *(unfilled — are users interrupted mid-task?)*
- **Stress and stakes:** *(unfilled — what is the cost of an error?)*
- **Compliance:** *(unfilled — does any action require an audit trail?)*
- **Device and environment:** Mixed. `(pointer: coarse)` handling and 44px
  primary actions imply real tablet or touch use, not desktop-only.
- **Connectivity and session length:** Session expiry is a live concern — the
  rule that expiry must preserve drafts implies users hold unsaved work for long
  stretches.

**Generation consequence:** assume work in progress can be interrupted. Never
discard entered data on navigation, expiry, or error.

## Research Synthesis

> **Status: empty.** This is the highest-leverage section in the file.

Format each entry as a finding followed by the constraint it places on
generation. A finding that does not change what gets built does not belong here.

```
- **Finding:** Users abandon setup when asked for information they don't have on hand.
  **Constraint:** Never block progress on a field the user may not know. Defer it,
  or make it editable after creation.
```

Seed this from whatever research already exists, even informally — support
tickets, UAT findings, the things people complain about in review.

## Glossary

> **Status: seeded from existing status vocabulary. Extend with domain terms.**

The words the product uses. If users say one word and the codebase says another,
the AI should generate the user's word.

| Term | Meaning | Notes |
| --- | --- | --- |
| COMPLETE | Terminal success state | Status chip label. Not "Done", not "Closed". |
| IN PROGRESS | Active work state | Not "Active", not "Started". |
| REVIEW | Awaiting review | Not "Pending", not "In Review". |
| DELAYED | Behind schedule | Not "Overdue", not "Late". |
| TO DO | Not yet started | |
| ON HOLD | Paused deliberately | Distinct from DELAYED — intent, not slippage. |
| HIGH / MEDIUM / LOW | Priority levels | Priority chips only. |
| *(unfilled)* | | Domain nouns — what do users call a record, a project, a client? |

## Interaction Standards

How the product behaves.

### Confirmation and undo

- Destructive actions route through a confirmation dialog, including bulk
  destructive actions.
- *(unfilled — is undo available anywhere? If so, prefer undo over confirmation
  for reversible actions.)*

### Error wording

- *(unfilled — house voice for error messages. Terse and technical, or plain and
  apologetic?)*
- Errors use `role="alert"`.
- Error and 404 pages offer a path forward — home link, search, or a relevant
  suggestion.

### Accessibility

- `<button>` for actions, `<a>`/`<Link>` for navigation. Never a click handler on
  a `Box` or `div`.
- One `<h1>` per page. `variant` is visual, `component` is semantic. Never skip
  heading levels.
- Icon-only controls need `aria-label`. A tooltip is not an accessible name.
- Color never carries meaning alone — chips, charts, and errors carry text, or
  icon plus text.
- Never use `text.disabled` for content text (2.8:1 at its current value, fails
  AA — re-measure if reskinned). Use `text.secondary`.
- Never use `fill-accent` on text. Links and foreground blue use `fg-accent`
  **[lint]**.
- Every control carries a boundary at 3:1 against its surface — a fill alone
  does not identify a control (1.4.11) **[lint]**.
- Hit areas are 24x24 minimum even where the visual is smaller (2.5.8) — pad the
  target rather than enlarging the glyph **[lint]**.
- Never remove a focus outline without a `:focus-visible` replacement.
- Touch targets ≥24px; primary actions ≥44px. A `size="small"` `IconButton`
  (30px) is table-row only.
- Landmarks present (`<main>`, `<nav>`, `<header>`). `document.title` updates on
  route change.
- Modals set `aria-labelledby` to the `DialogTitle` id.
- Group focus with `:focus-within` for compound controls.
- `aria-live="polite"` on the header status line, result counts, and selection
  counts. Skip link first in tab order.

### Forms

- Single column by default. Two columns only for genuinely paired inputs — Start
  and End, City and Country.
- Section titles use `component="h2"`. A form's only other heading is its `<h1>`,
  so `h3` here skips a level; use `h3` only beneath a genuine intervening `h2`.
- Every input has a visible label above it. A placeholder is never the only
  label, and it is an example rather than an instruction (`e.g. jane@acme.com`).
- Correct `type` and `inputmode` (`email`, `tel`, `url`, `inputmode="numeric"`)
  plus `autocomplete` tokens.
- Related radios and checkboxes go in `<FormControl component="fieldset">` with
  `<FormLabel component="legend">`.
- Tab order matches visual order. `Enter` submits.
- Disable spellcheck on emails, codes, and usernames.
- Warn before navigating away with unsaved changes.
- A checkbox or radio shares one hit target with its label — no dead zones.
- Constrain before validating: date pickers over free text, selects for finite
  sets, masks for known formats. Smart defaults wherever the app can reasonably
  guess.

**Validation timing.** Format errors validate on blur, everything else on
submit. Never on keystroke — validating a half-typed email tells the user they
are wrong while they are still being right. Clear an error the moment the field
is edited.

**Error placement.** Directly beneath its field, never in a summary at the top,
never only in a toast. `role="alert"`, with the error tone on both the message
and the field ring so the association is visible as well as programmatic.

**Footer.** Inside the form card, below a hairline, on `surface-subtle`. Primary
action first, then cancel, then the dirty-state marker right-aligned. **The
footer does not stick.** It sits at the end of the form and scrolls with it.

Because it does not stick, a form long enough for the footer to feel out of
reach is too long — split it into steps rather than pinning the actions. Length
is the problem a sticky footer hides.

**Dirty state.** Visible whenever the form has unsaved changes, since the rule
above requires warning on navigation away. Text only — no colored dot.

### Content and formatting

- Ellipsis character `…`, not `...`. Loading states end with it too (`Saving…`).
- Dates render `MMM DD, YYYY`. Never raw ISO, never two formats in one table.
- Numeric columns meant for comparison use `font-variant-numeric: tabular-nums`.
- Headings use `text-wrap: balance` where supported.
- Long text truncates via `noWrap` or `line-clamp`. Flex children holding
  truncatable text need `min-width: 0`.
- Empty cells render `—`, never `null` or blank.
- Anticipate short, average, and very long user-generated content. Test with
  real-length data, not Lorem ipsum.

### Images

- `<img>` needs explicit `width` and `height` to prevent layout shift.
- Below-fold images use `loading="lazy"`; above-fold critical images use
  `fetchpriority="high"`.

### Touch and pointer

- `touch-action: manipulation` on interactive elements.
- `overscroll-behavior: contain` in modals, drawers, and sheets.
- During drag: disable text selection, mark the dragged element `inert`, and
  provide a keyboard or tap alternative.
- `autoFocus` sparingly — desktop only, single primary input, avoid on mobile.
- Interactive elements need a hover state. Hover, active, and focus all increase
  contrast over rest.

### Navigation and flow

- Every flow has a visible exit — cancel, close, or working back navigation.
- Browser back behaves: modals and drawers close, wizard steps step back, no
  broken intermediate state.
- Multi-step flows go backward without losing entered data.
- Session expiry preserves work — draft retained, re-auth in place, no silent
  data loss.
- Current location is always clear — active nav state, breadcrumbs on deep
  hierarchies, accurate page title.
- Never open a modal from a modal.
- Nothing auto-advances, auto-plays, or auto-refreshes without user control.

## Page Patterns

**A card is one logical unit, and its actions live inside it.** A toolbar, a form
footer, or a section save belongs in the same card as the thing it acts on,
separated by a hairline and set on `surface-subtle` — never split into its own
`<Paper>`. A detached action bar reads as an unrelated fragment.

### List page

Search and filtering are centralized above the list. **Columns sort; columns do
not filter.** A user should never need to know which column holds a value in
order to find a record, or open several column menus to build one view. This is
a platform pattern — an app may choose *which* fields go where, never *how* the
controls behave.

Header regions, top to bottom, in this order:

| Region | Contains | Present |
| --- | --- | --- |
| Global search | One input across identifying and free-text fields | Always |
| Quick filters | 3–5 highest-frequency filters, inline | Always |
| More filters | Overflow panel for the rest | Only if overflow exists |
| Active filter chips | One chip per applied value, plus Clear all | Only when filters active |
| Result count | `24 of 312 tickets`, beside the chips | When filtered |
| List | Sortable column headers | Always |

Nothing filter-related goes below the list or inside the table body.

**Global search.** Bound to server-side search, debounced 300–500ms, with a real
label (visually hidden is fine). Full width on `xs`, 280–320px on `sm` and up.
Placeholder names what is searched — `Search by ID, subject, or customer`, never
a bare `Search…`. Partial and case-insensitive. Combines with filters using AND;
it narrows within the filtered set and never resets it. Clearing search does not
clear filters.

Do not build a filter control for a field global search already covers. Free-text
and identifier fields belong in search.

**Quick filters.** The 3–5 filters used most in that app's daily work. Choose by:
routine usage frequency, then a bounded enumerable value set (status, category,
assignee, type, priority), then whether the field defines how a team divides work.
Each renders as a labelled control showing its current state — never a generic
"Filter" button. Multi-select where the value set is small enough. A sixth
candidate moves to More filters; it does not get squeezed inline.

**More filters.** Everything else — lower-frequency fields, secondary dates, and
anything added later. Opens as a panel or drawer without leaving the list.
Applies on explicit `Apply`, not per change, so a multi-field view composes in
one pass. Carries a count badge when filters inside it are active, so hidden
state stays visible from the list.

**New fields default to More filters.** Promote to a quick filter only on
observed usage.

**Combination logic, fixed.** AND across different fields; OR within one
multi-select field. Date fields take a from/to range. Never expose query
builders, nested groups, or user-selectable operators here — that belongs in a
reporting or saved-view surface.

**Chips.** Every active filter renders as a chip regardless of where it was set.
The label states field and value — `Status: Open`, not `Open`. Each chip is
individually removable without reopening a menu, so a multi-select field renders
**one chip per value**, not one combined chip. Chips are the single source of
truth for what the user is looking at; an active filter with no chip is a defect.

**Clear all.** Present whenever any filter is active. Clears filters and **does
not clear global search** — they are separate controls, and clearing one must not
silently discard the other. This is platform-wide, not per app.

**Sorting.** Column headers cycle ascending then descending, one column at a
time unless the app has a specific multi-sort need. Sort state shows on the
active column and is exposed via `aria-sort` **[lint]**. Sorting reorders what is
shown; filtering changes what is included. Applying or removing a filter
preserves sort.

**State.** Search, filters, sort, and pagination live in the URL so a view can be
bookmarked and shared. Restore last-used filter state within a session. Do not
persist filters across sessions without visible indication — a user landing on a
list that silently hides most of its records reads it as missing data.

**Pagination.** Server-side, 10 rows by default.

**Responsive.** Global search stays full width. Quick filters collapse into More
filters rather than wrapping or scrolling horizontally. The chip row stays
visible — wrap or scroll it, never hide it.

**Accessibility.** Every filter control keyboard operable in logical tab order.
The More filters panel traps focus while open and returns focus to its trigger on
close. Programmatic label on every control; a placeholder is not a label. Chip
remove buttons name the value — `Remove filter Status: Open`, not "Remove"
**[lint]**. Result count updates announce via a live region. Filter state never
conveyed by color alone. Chips and their remove affordances are the usual target-
size failure — check them.

One card, not three: heading and action in the header, then search, then quick
filters, then chip row, then rows.

Field classification per app is a required artifact — see `AGENTS.md`.

### Detail / show page

Breadcrumb (`<nav aria-label="Breadcrumb">`, current item `aria-current="page"`)
· `<h1>` with status chip beside it · Edit / Delete / More right-aligned, Delete
routed through a confirmation dialog · meta line (`body2 secondary`) below the
title · tabs labelled `Label (count)`, each tab body carrying its own empty and
loading states.

### Empty states

Floor: never blank. Every list has at least an *empty* state and an *error*
state, each with a heading and a next action.

| Condition | Icon | Action |
| --- | --- | --- |
| `total===0 && !search && !filters` | `FolderOpenRounded` | contained "+ Create" |
| search active, 0 rows | `SearchOffRounded` | outlined "Clear search" |
| filters active, 0 rows | `FilterListRounded` | outlined "Clear all" |
| `isError` | `ErrorRounded` | contained "Try again" |

`role="status"`, or `role="alert"` for the error variant. Icons `aria-hidden`.

Filtered-empty is never the same state as truly-empty. A search miss followed by
a "+ Create" call to action is the canonical failure — the user was looking for
something that may well exist, and the screen tells them nothing does.

### Tables

Every action needs an accessible name — `GridActionsCellItem`'s `label`, not a
tooltip. Show five to six columns; beyond that, add a column picker or move the
data to the detail page. MUI DataGrid v7 rendering specifics (alignment
defaults, cell padding, flex-cell text overflow) live in `taruvi-ui/datagrid.md`.

### Bulk actions toolbar

A list with selection checkboxes needs one — checkboxes without it are dead
controls. It appears only when at least one row is selected. The count sits in an
`aria-live="polite"` region. The clear-selection control carries an `aria-label`.
Destructive bulk actions route through the confirmation rules. Outlined controls
on a blue ground use `rgba(255,255,255,0.7)` — `0.5` fails 3:1 against the
current blue; re-measure if that token's reskinned.

### Entity card

Use when records are mobile-first, drag-ordered, or richer than scalars — not as
a default alternative to rows.

`<Card><CardContent>` → title row (`h5` plus overflow menu) → description → chip
stack → footer (date, edit, delete). Every `IconButton` names its record
(`aria-label="Delete Website Redesign"`, not "Delete"). Whole-card navigation
uses `<CardActionArea>` with no nested buttons. Drag-to-reorder needs a keyboard
or tap alternative.

### Stat / KPI card

A plain themed `<Card>`. The number is the hero — `h3` with `component="p"`, not
a heading — and the label sits above it. No colored accent border, no decorative
icon puck. If color carries meaning, say so in text as well. A navigable tile
uses `<CardActionArea>` and a trailing chevron only.

### Confirmation dialogs

Destructive and irreversible actions route through one. The title names the
record — `Delete "Belted wool coat"?` — so a mis-click is caught by reading the
title alone. The body states scope and reversibility: what else goes with it,
what is unaffected, and whether it can be undone.

Actions sit in a footer inside the dialog, on `surface-subtle` below a hairline,
right-aligned: Cancel first, then the destructive action. The destructive button
is filled in the error tone, and its label restates the verb and object
(`Delete product`), never a bare `OK` or `Confirm`. Focus lands on Cancel, and
Escape cancels.

Type-to-confirm is reserved for bulk destructive actions and anything that
cannot be recovered from a backup. Do not use it routinely — a friction step
applied everywhere stops being read.

### Action feedback

> **Status: not yet true of any shipped Taruvi app — see README's Open Items.**
> The model below is the intended target. `taruvi-refine-template` currently
> wires `RefineSnackbarProvider`/`useNotificationProvider`, which this section
> replaces. Treat `LINT_CANDIDATES.md`'s "no toasts" rule as non-blocking until
> that gap is resolved one way or the other.

**There are no toasts.** Nothing floats over the data.

A successful action is confirmed by the interface changing — the row leaves, the
count drops, the chip changes state, the field saves. Announcing what the user
can already see is noise, and a channel that mostly carries noise stops being
read when it finally carries something.

So feedback splits three ways:

| Outcome | Where it surfaces |
| --- | --- |
| Success, visible in the UI | Nothing. The change is the confirmation. |
| Success, reversible | Undo in the affected row, or in the header status line when no row applies |
| Success, invisible (background job, export queued) | Header status line |
| Failure | Banner |

**Undo in the row.** A reversible bulk action leaves its rows in place, struck
through and muted, with `Archived · Undo` inline. The rows clear on the next
fetch or on navigation. This beats a confirmation dialog for anything
recoverable — offer undo rather than asking twice.

**Header status line.** A single line beneath the page heading, above a hairline,
for outcomes with no row to attach to. It is a live region (`aria-live="polite"`)
and it replaces rather than stacks. It clears on the next user action.

**Failures use the banner** below. A failure is never silent, never transient,
and never only in a row — a partial bulk failure states how many succeeded and
names what did not.

### Motion

Three durations, one curve. Nothing else moves.

| Duration | Applies to |
| --- | --- |
| 80ms | Hover, focus ring, pressed |
| 160ms | State change — row, chip, toggle, checkbox |
| 220ms | Enter and exit — banner, drawer, menu, dialog |

Easing is `cubic-bezier(.2, 0, 0, 1)` everywhere.

No page transitions, no skeleton shimmer, no stagger. Rows in a set change
together — a stagger looks considered at four rows and becomes a wave at two
hundred.

`prefers-reduced-motion: reduce` drops every duration to 1ms rather than to
zero, so `transitionend` handlers still fire and nothing hangs waiting for an
event that never arrives.

Motion is confirmation, never decoration. Because there are no toasts, a row
changing state *is* the feedback — which makes these durations load-bearing
rather than cosmetic.

### Banners

Page-level messaging is a flush strip above the page heading, running the full
width of the content area. Full-width scoping is the point — a banner inside a
card reads as belonging to that card, and this is telling the user something
about the whole page.

Square, not rounded, since it meets the page edges. Icon, then a lead sentence
in semibold, then supporting detail in the same line, then a single action
right-aligned. Text and icon take the darker stop of the role's own ramp, never
`text-primary` on a tint.

One banner at a time. If two conditions apply, the more severe wins and the
other waits — a stack of banners pushes the actual page below the fold.

Dismissible only when the condition is informational. A banner describing a
problem stays until the problem is gone; letting someone dismiss it means the
page silently lies afterwards.

### Menus

Overflow menus group by consequence. Ordinary actions first, then a divider,
then destructive actions last in the error tone. The trigger is an icon button
named after its record. Never a menu of one item — promote it to a button.

### Loading states

Skeletons, not spinners. A skeleton holds the layout so nothing jumps when data
lands, which is the same reason images carry explicit dimensions.

The table header persists while rows load — it is known before the data is. Show
as many placeholder rows as the page size, so the container does not resize on
arrival. Placeholder bars use `border-subtle`; they do not pulse or shimmer.

A spinner is correct only where the layout is genuinely unknown ahead of time,
and inside a button during submit.

### Pagination

Numbered pages with first and last always visible, an ellipsis for the gap, and
previous/next arrows. The count sits to the left as `Showing 1–10 of 312` with
tabular numerals. Disabled arrows stay visible rather than disappearing, so the
control does not change width at the boundaries.

### Controls

**Toggle or checkbox** is decided by when the change takes effect. A toggle
applies immediately and needs no save; a checkbox is a value in a form that is
saved later. Never a toggle inside a form with a save button, and never a
checkbox for a setting that applies on click.

**Segmented control** switches between views of the same data — table and board.
Each segment carries an icon and a label. The choice persists per user per list.
Do not use it for filtering; that belongs in the filter row.

**Badges** carry counts only, never status. Neutral by default, accent when the
count is something to act on, error tone when it represents a problem. Cap at
`99+`. A badge with a zero count is not rendered.

### Charts

- `role="img"` with an `aria-label` stating the takeaway, not the chart type —
  "Ticket status, Q2: 42% resolved".
- Expose the underlying data as a `<table>` or `<figcaption>`.
- Distinguish series by a second channel besides color — labels, dash patterns,
  marker shapes — repeated in the legend swatch.
- Never put data only in a hover tooltip. It is unreachable on touch.

## Do's and Don'ts

- Do give every list a heading, a global search, 3–5 quick filters, and four
  empty states.
- Do keep filtering above the list and sorting in the column headers.
- Do preserve filter, sort, and pagination state across back-navigation.
- Do name every icon-only control after the record it acts on.
- Do keep a card's actions inside that card.
- Do name the record in a confirmation title and in the destructive button label.
- Don't put a toggle in a form that has a save button.
- Don't use a badge to carry status — that is a chip.
- Don't add a toast unless the app has adopted the toast-free feedback model
  above — see Action Feedback's status note.
- Do offer undo instead of a confirmation dialog wherever the action is
  recoverable.
- Do use the glossary's word, not the codebase's.
- Don't stack modals.
- Don't use a tooltip as an accessible name.
- Don't let color be the only channel carrying meaning.
- Don't add onboarding affordances for expert users.

## Maintenance

This file is never finished. It changes when the product changes, when new
research lands, and when you catch an AI tool getting something wrong — the last
of those is a genuine input, not a failure. Log the correction here rather than
re-prompting around it.

If this file outgrows a single read, convert it to an index over a `ux/` folder —
one file per section, with this file holding the pointers and the constraints
that apply to every task.
