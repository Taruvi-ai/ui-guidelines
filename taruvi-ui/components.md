# Components: import and prop guidance

Prefer the themed component with the right prop over `sx`. Reach for `sx` only
where noted below.

For color rules (which family, which token, the three-role blue split) see
`DESIGN.md`'s Colors section — not repeated here.

## Status chips

```tsx
<Chip color="success|info|warning|error" label="COMPLETE|IN PROGRESS|REVIEW|DELAYED" />
```

The theme supplies the tone and a white label. Do not set `color`, `borderColor`,
or a label color by hand — there are no per-chip exceptions.

Status fills are luminance-matched so one rule covers all of them. If you are
adding a new status, it must clear 4.5:1 on white before it ships; a fill that
needs a dark label is the wrong fill.

## Priority chips

```tsx
<Chip variant="outlined" color="error|warning|success" label="HIGH|MEDIUM|LOW" />
```

## Tag / category chips

```tsx
<Chip variant="tagBlue|tagPurple|tagGreen|tagOrange|tagTeal|tagPink|tagLime|tagRose" />
```

Eight variants. Hash the tag name to an index for deterministic rotation. Append
to the palette; never reorder it — reordering re-colors every existing tag.

Never use the tag rotation to express status. Rotation means "different"; status
means "different in severity."

## Filter controls vs active chips

These sit next to each other on every list view and must stay visually distinct.
A filter control is a control you open; an active chip is state you dismiss.
The theme handles both — do not restyle either to match the other.

## All chips

Every chip carries a text label. A bare colored dot or fill is never sufficient.

## Buttons

Three sizes map to the 28/36/44px heights. The theme handles the coarse-pointer
bump to 44px — do not add a media query for it.

## Icons

`*Rounded` variants only — `EditRoundedIcon`, not `EditIcon`.

Sizes: 20px inline, 24px standard, 32px and up for feature and empty-state use.
Decorative icons get `aria-hidden`; meaningful ones get `titleAccess` or a
labelled parent.

Icons are affordances. Do not add decorative glyphs to stat tiles, section
headers, or card corners.

## Avatars

34px default, 30px inside table rows.

## Cards

`<Card><CardContent>`. Whole-card navigation uses `<CardActionArea>` with no
nested buttons inside it.
