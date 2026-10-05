---
version: alpha
name: Taruvi
description: Visual identity for the Taruvi platform. Portable design context for environments without access to the Taruvi component library.
colors:
  primary: "#1AB3E6"          # primary-700, the brand swatch — illustrations/swatches only, never an affordance
  primary-50: "#F2FBFF"       # hovered/selected row background — verify foreground tones against this, not just against card
  primary-300: "#9DE5FD"      # dark-mode foreground substitute for the light-mode hover/foreground role
  success-500: "#10B981"
  # Status fills — all carry a white label. Every value below is >=4.5:1 on
  # white and sits in a 0.137-0.182 luminance band so no status outshouts another.
  # Do not lighten any of these without re-checking both.
  status-in-progress: "#1976d2"
  status-review: "#bf360c"        # matches the existing warning-800 step; checked against error, see Colors
  status-complete: "#2e7d32"
  status-todo: "#00838f"
  on-status: "#ffffff"
  # Interaction blue — three non-interchangeable jobs, see Colors below.
  button-primary-default: "#1E88E5"   # non-text only: rings, borders, fills — 3:1 bar
  button-primary-fill: "#1976d2"      # fill behind white text (contained buttons)
  button-primary-hover: "#1565C0"     # foreground/text tone (light mode) — links, labels, focused fields
  # Chart-only tones — never an interface affordance.
  status-resolved: "#008751"
  status-chart-primary: "#1e88f5"
  status-under-review: "#FF8C00"
  # Error — verified 5.87:1 white-on-fill, clears the same 4.5:1 bar the status fills use.
  error: "#c2185b"
  on-error: "#ffffff"
  # Neutrals — PROPOSED, pending themeOptions.ts. Steps are deliberately wide:
  # this system takes depth from containment, not shadow, so surface separation
  # carries the whole edge. page/card sits at 1.20, border/card at 1.42.
  surface-page: "#e8ebf0"
  surface-card: "#ffffff"
  surface-subtle: "#f5f7fa"
  # Two border tokens with different jobs. A hairline that merely separates is
  # decorative and exempt from 1.4.11. A border that IDENTIFIES a control must
  # clear 3:1 against the surface behind it. Do not use border-hairline on a
  # control, and do not use border-control as a divider — it reads as heavy.
  border-hairline: "#e6eaf0"      # dividers, card edges, table rules
  border-control: "#767f8e"       # input, select, toggle track, segmented control
  fg-accent: "#1668c4"            # links, foreground blue, 5.51 on card
  fill-accent: "#2b97ff"          # non-text only: fills, rings, tracks
  text-primary: "#171a1f"
  text-secondary: "#3f4854"
  text-muted: "#5a6472"
  input-fill: "#F3F3F5"
  theme-blue: "#2b97ff"
  theme-dark: "#004369"
  on-blue-border: "rgba(255,255,255,0.7)"
  # Tag / category rotation — eight bg+text pairs, existing measured pairs from
  # themeOptions.ts. Distinguish, never rank; see Colors below.
  tag-blue-bg: "#E0F6FE"       tag-blue-text: "#004369"     # 9.36:1
  tag-purple-bg: "#EDE7F6"     tag-purple-text: "#4527A0"   # 8.47:1
  tag-green-bg: "#E8F5E9"      tag-green-text: "#1B5E20"    # 7.00:1
  tag-orange-bg: "#FFF3E0"     tag-orange-text: "#BF360C"   # 5.11:1
  tag-teal-bg: "#C8F7F3"       tag-teal-text: "#00514D"     # 7.90:1
  tag-pink-bg: "#FFE5FB"       tag-pink-text: "#68315B"     # 8.19:1
  tag-lime-bg: "#EFF0D1"       tag-lime-text: "#41480E"     # 8.37:1
  tag-rose-bg: "#FFE4DF"       tag-rose-text: "#732F2C"     # 8.02:1

typography:
  h1: { fontFamily: Quicksand, fontWeight: 800, fontSize: 2.25rem }    # 36px
  h2: { fontFamily: Quicksand, fontWeight: 700, fontSize: 1.75rem }    # 28px
  h3: { fontFamily: Quicksand, fontWeight: 700, fontSize: 1.375rem }   # 22px
  h4: { fontFamily: Quicksand, fontWeight: 700, fontSize: 1.125rem }   # 18px
  h5: { fontFamily: Quicksand, fontWeight: 700, fontSize: 0.9375rem }  # 15px
  h6: { fontFamily: Quicksand, fontWeight: 700, fontSize: 0.8125rem }  # 13px
  body-md:
    fontFamily: Open Sans
    fontSize: 13px
    fontWeight: 400
    lineHeight: 1.6
  label-button: { fontFamily: Quicksand, fontWeight: 700 }
  label-chip: { fontFamily: Quicksand, fontWeight: 700 }
  label-table-head: { fontFamily: Quicksand, fontSize: 11px, fontWeight: 700 }
  label-chart-title: { fontFamily: Quicksand, fontWeight: 600 }
  input: { fontFamily: Open Sans, fontSize: 16px, fontWeight: 400 }

rounded:
  none: 0px
  button: 4px
  input: 6px
  table: 8px
  card: 10px
  full: 9999px

spacing:
  unit: 8px                   # base scale
  sm: 16px
  card-padding: 28px          # dialogs, forms, prose containers
  card-padding-dense: 20px    # containers wrapping a table or grid
  cell-padding-x: 16px
row-height:
  table: 36px                 # 2.8x body size; 44px reads adrift at 13px type
  table-head: 34px

components:
  button-primary:
    typography: "{typography.label-button}"
    rounded: "{rounded.button}"
    height: 36px
  button-primary-small: { height: 28px }
  button-primary-large: { height: 44px }
  chip-status:
    typography: "{typography.label-chip}"
    rounded: "{rounded.full}"
  chip-tag:
    typography: "{typography.label-chip}"
    rounded: "{rounded.full}"
  card:
    rounded: "{rounded.card}"
    padding: "{spacing.card-padding}"
  card-dense:
    rounded: "{rounded.card}"
    padding: "{spacing.card-padding-dense}"
  dialog:
    rounded: "{rounded.card}"
    padding: "{spacing.card-padding}"
  input:
    backgroundColor: "{colors.input-fill}"
    typography: "{typography.input}"
    rounded: "{rounded.input}"
  table-wrapper: { rounded: "{rounded.table}" }
  table-head: { typography: "{typography.label-table-head}" }
  table-cell: { padding: "{spacing.cell-padding-x}" }
  avatar: { size: 34px }
  avatar-table-row: { size: 30px }
  icon-inline: { size: 20px }
  icon-standard: { size: 24px }
  icon-feature: { size: 32px }
---

# Taruvi

## Scope

**Use this file when you cannot import the Taruvi component library** — external
design tools, prototyping environments, client demos, greenfield work, and
customer theming. It describes how to build Taruvi-looking UI from scratch.

**Inside a Taruvi application, do not use this file to build components.** Use
the `taruvi-ui` reference docs (`components.md`, `datagrid.md`) instead. The
theme already implements everything specified below; rebuilding a Button from
these values produces a duplicate that will drift from the real one and will
not inherit fixes. Import the component, pass the prop. This file is the spec
for a copy of the system, not an instruction manual for the system itself.

This file covers page content only. The application shell — navigation, header,
search, notifications, account menu — comes from NavKit inside a Taruvi app and
is not specified here.

Behavioral and research context lives in `UX.md`.

## Token Contract

The token *names* below are the shared contract, not the values. Every valid
Taruvi `DESIGN.md` — including a client's reskinned copy forked for their own
brand — must define these names, because `UX.md`, `components.md`, and
`datagrid.md` reference them directly. A fork is free to change every value; it
is not free to rename or drop a token, or every cross-reference into those
other files silently breaks.

Required: the four `status-*` fills plus `on-status`, the `fg-accent`/
`fill-accent` split (or a documented reason a given brand doesn't need the
split — see Elevation & Depth), `border-hairline`/`border-control`, the four
surface tones, `error`/`on-error`, and the eight `tag-*-bg`/`tag-*-text` pairs.
Everything else (exact radii, spacing scale, type ramp) can vary per brand
without breaking a cross-reference.

If you are forking this file for a client brand, see Do's and Don'ts — a
palette swap alone does not carry the accessibility guarantees below with it;
each one needs re-verifying against the new values, not just copied forward.

## Overview

Taruvi is enterprise application software: dense, data-forward, and read more
than it is browsed. The interface should feel engineered rather than expressive.
Restraint is the default — decoration is absent, color is rationed, and every
visual element earns its place by carrying information. Where a rule is silent,
choose the quieter option.

## Colors

Color is functional, never ornamental. The palette divides into three families
that do not cross over.

- **Brand tones** (`success-500`, `primary`) — illustrations and swatches
  only. Never an interface affordance.
- **Status tones** (`status-in-progress` #1976d2, `status-review` #bf360c,
  `status-complete` #2e7d32, `status-todo` #00838f) — chips, alerts, buttons,
  sidebar. These carry severity.
- **Chart tones** (`status-resolved`, `status-chart-primary`,
  `status-under-review`) — charts only, never affordances.

The interaction blue has three non-interchangeable jobs.
`button-primary-default` is for non-text only — rings, borders, fills, held to a
3:1 bar. `button-primary-fill` sits behind white text in contained buttons.
`button-primary-hover` is the foreground tone for links, labels, and focused
fields. Dark mode substitutes a `primary-300`-class tone for the foreground role.

Every status fill takes a white label. This holds because the four fills are
luminance-matched, not because white is a safe default — it is not. Any new
status fill must clear 4.5:1 against white and land inside the existing
luminance band before it joins the set. A fill that needs a dark label does not
belong in this family; darken it until it does not.

`error`/`on-error` follow the same rule: white on `error` (#c2185b) measures
5.87:1, comfortably inside the 4.5:1 floor.

Separately, a foreground tone must pass on `background.default` and on a hovered
`primary-50` row, not merely on `paper`. A tone that passes only on paper fails
on hover.

**Review vs. error, checked.** `#bf360c` is the lightest orange that clears
4.5:1 on white; every lighter value fails (`#e65100` reaches only 3.79:1). It
is also already a used step in the theme's warning ramp (`warning[800]`), not
a new color. Measured against `error` (#c2185b) with CIEDE2000 — the same
metric the tag palette below is held to — the two are 24.0 apart, over 3x the
7.5 floor that palette treats as "distinguishable." Rendered side by side they
read as orange and magenta, not as two shades of red. No collision; no
exception needed.

The eight tag variants exist to distinguish, not to rank. Hash the tag name to an
index for deterministic rotation. Append to the palette; never reorder it, and
never borrow it for status — rotation means "different," status means "different
in severity."

## Typography

Two families with fixed roles. **Quicksand** carries structure — headings,
buttons, chips, table heads, chart titles — at 700 weight, 800 for h1, 600 for
chart titles. **Open Sans** carries content at 13px on a 1.6 line height.

Button and chip labels are uppercase. Table heads are uppercase at 11px. Inputs
are set at 16px specifically to defeat iOS zoom-on-focus, not for hierarchy.

## Layout

Padding splits by what the container holds. Dialogs, forms, and prose take 28px.
A container wrapping a table or grid takes 20px — at 28px the data reads as
adrift rather than contained, because the table draws its own edge and does not
need a second one.

Table rows are 36px against 13px type, a ratio of 2.8. Taller rows look airy
rather than dense, and dense is the point. Table cells take horizontal padding
only — vertical padding pushes text off center rather than centering it.

## Elevation & Depth

Depth comes from containment and radius, not shadow. Content sits in cards on a
plainer background; hierarchy is conveyed by grouping and by the radius ladder
rather than by stacking shadows.

**Two blues, two jobs — this is the easiest rule in the system to break.**
`fill-accent` (#2b97ff) is non-text only: button fills, focus rings, toggle
tracks, selected borders. It measures 3.01 on card, which clears the 3:1
non-text bar and fails the 4.5:1 text bar. `fg-accent` (#1668c4) is the only
blue permitted on text — links, "Clear all", active tab labels, foreground
icons paired with text. Using the fill blue as a text colour is a WCAG failure,
not a taste call.

A brand whose single accent color already clears 4.5:1 as text doesn't need
this split at all — the split exists because Taruvi's accent doesn't. Don't
manufacture two tokens where a client's own color only needs one; note the
simplification in Do's and Don'ts if you drop it.

**Because there is no shadow, surface separation carries the entire edge.** It
therefore needs wider steps than a shadowed system would use, not narrower ones.
Hold page-to-card at roughly 1.20 and border-to-card at roughly 1.42. Anything
under about 1.10 stops registering as an edge and the whole screen flattens into
one plane — the most common way this system goes wrong.

Four surfaces is the ceiling: page, card, subtle, and input fill. A fifth grey
will land inside the gaps and blur the four that matter.

## Shapes

A tight radius ladder, ascending with container size: buttons 4px, inputs 6px,
table wrappers 8px, cards and dialogs 10px. Chips are fully rounded pills. Do not
mix radii within one composition.

## Components

Buttons come in three heights — 28px, 36px, 44px — resolving to 44px under
`(pointer: coarse)`.

Chips always carry a text label; a bare colored dot or fill is never sufficient.
Status chips use the fixed label set COMPLETE / IN PROGRESS / REVIEW / DELAYED.
Priority chips are outlined with HIGH / MEDIUM / LOW. Tag chips use the
eight-variant rotation.

Inputs take a `#F3F3F5` fill and a 2px focus ring. **The fill alone does not
identify the control** — it measures 1.11 against card, far under the 3:1 that
1.4.11 requires. Every input, select, toggle track, and segmented control
therefore carries a `border-control` boundary as well. A borderless filled input
is a conformance failure however clean it looks.

**Hit areas are 24x24 minimum even where the visual is smaller** (2.5.8). The
checkbox is a 16px visual in a 24px target; the toggle is 34x20 in a 34x24
target; a chip's remove control is a ~14px glyph in a 24px target. Pad the
target, do not enlarge the glyph.

**A filter control and an active filter chip must not look alike.** One is a
control you open, the other is state you dismiss, and they sit adjacent on every
list view. Controls read as controls — surface fill with a visible border and a
disclosure affordance. Active chips read as state — tinted, pill, with a remove
affordance. Separate the two groups with a divider when they share a row.

Avatars are 34px by default, 30px inside table rows. Icons run 20px inline, 24px
standard, 32px and up for feature and empty-state use.

For import-and-prop level detail (which component, which variant, which prop) —
inside a real Taruvi app only — see `taruvi-ui/components.md` and
`taruvi-ui/datagrid.md`.

## Iconography

Icons are affordances, not decoration — add one only where it is tied to an
action or control. Decorative glyphs on stat tiles, section headers, and card
corners read as machine-generated. Empty-state illustrations are the exception.

Use `*Rounded` variants exclusively. Never let one icon mean two things.

## Charts

Legend top-right or bottom-center. Y axis starts at zero. Title set in Quicksand
600, top-left.

Each series must clear 3:1 against the background. Do not attempt 3:1 *between*
series fills once you have five or more categorical series — it is unsatisfiable
and forces a sequential luminance ramp that destroys hue coding. Distinguish by a
second channel instead.

## Do's and Don'ts

- **Do import the existing component when one exists — never rebuild from this
  spec inside a Taruvi app.**
- Do check a new status fill against white at 4.5:1 *and* against the band before
  adding it. Never add a fill that needs its own label rule.
- Do verify foreground tones on `background.default` and on a hovered row.
- Don't use the tag rotation palette to express status.
- Don't use brand tones for affordances or status tones for charts.
- Don't mix radii within a single composition.
- Do maintain 4.5:1 for text and 3:1 for non-text.
- **Forking this file for a client brand?** Do keep every token *name* in the
  Token Contract above. Don't carry any contrast fact forward unchanged — every
  status fill's white-label pass, the fill/text accent split (or its absence),
  and the surface-separation ratios all need re-verifying against the new
  values. A palette swap without re-verifying is how a rebrand ships
  inaccessible.
