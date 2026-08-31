# MUI DataGrid v7

Written against `@mui/x-data-grid` v7. Re-verify every claim below against
MUI's release notes before bumping the major version in `package.json` — none
of this is guaranteed to still be true in v8. A caret-range dependency
(`^7.x.x`) never crosses a major version on its own, so this only needs
revisiting at a deliberate upgrade, not on every `npm install`.

Four behaviors that look like bugs and are not.

## Alignment

Left-align plus vertical-center is already the default. The theme's `cell` slot
flex-centers its children, including block-level `renderCell` output.

Do not set `align` or `headerAlign` to achieve the default. Set them only to
deviate from it.

## Cell padding

Cells take horizontal padding only (`0 16px`). Adding vertical padding pushes
text downward instead of centering it, because the cell is already a flex
container.

## Text overflow in flex cells

Flex cells do not inherit `text-overflow` from a bare string child. Any column
whose text can outgrow its width must ellipsize in its own element:

```tsx
<Box sx={{ overflow: "hidden", textOverflow: "ellipsis" }}>{value}</Box>
// or
<Typography noWrap>{value}</Typography>
```

## Grid height

Use `autoHeight`. Never a fixed pixel height.

The theme already sets `--DataGrid-overlayHeight`, which prevents the
autoHeight-collapses-overlays bug. Do not set it per page.

## Accessible names

Every row action needs an accessible name via `GridActionsCellItem`'s `label`.
A tooltip is not an accessible name.

## Column count

Show five to six columns. Beyond that, add a column picker or move the data to
the detail page.
