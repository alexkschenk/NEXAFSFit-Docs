# Offset component curves for inspection

GUI **0.2.30**. Use the controls in **Display options** below the main plot. This page explains the visual shift and its factor.

(offset-components)=

## Offset components

Check **Offset components** on the same display row, then adjust its numeric factor.
This shifts the individual peak, edge and background curves downward, together
with their peak handles. A dotted **Component zero (offset)** line marks their
shifted zero; a curve below ordinary zero does not imply that its fitted area
became negative.

:::::{tab-set}
::::{tab-item} No offset

:::{figure} ../media/images/model-display-ordinary.png
:label: figure-offset-view-ordinary
:alt: Synthetic data, envelope and component curves sharing the ordinary intensity zero with Offset components unchecked.

The unshifted component curves and handles on a synthetic spectrum.
:::
::::
::::{tab-item} Offset enabled

:::{figure} ../media/images/model-display-offset.png
:label: figure-offset-view-enabled
:alt: The same synthetic spectrum with Offset components enabled at 0.100, lowered component curves and handles, and a labelled component-zero line below the unchanged data.

Offset enabled at **0.100**. Only components and their handles move down; data,
Total and residual stay in place. The labelled shifted-zero line is the reference
for reading component heights in this view.
:::
::::
:::::

### Read the offset factor

The shift is

```{math}
:label: equation-component-display-offset
\Delta I = f\,\max\!\left(\max(y)-\min(y),\;\max|y|\right),
```

where $f$ is the factor (initially **0.1**) and $y$ is the active working signal.
Previously saved explicit factors are restored. For a signal ranging from −0.2
to 1.8, the scale is 2.0; factor 0.1 shifts components down by 0.2 intensity unit.
Increasing the factor moves them farther
down. Factor zero gives no shift, and unchecking the toggle restores normal positions.
When no data are loaded, the model preview supplies the scale.
For an all-zero signal, the renderer uses a scale of 1.

### Drag and compare shifted components

Data, comparison scans, Total and residual do not move. Parameters and numerical
fits/exports are unchanged. The vertical view expands to include the shifted
curves while horizontal framing is retained. Drag the shifted handles normally:
the GUI removes the visual shift before converting their height back into A.
The [drag-control screenshot](drag-controls.md#select-a-component-and-expose-its-controls)
shows this arrangement.

You can combine offset components with edge subtraction. The total/data then
show the edge-subtracted view while individual component curves are displaced
below them. Focus Mode can still fade unselected components in either view.

## Related topics

- [Change focus and colours](component-display.md).
- [Inspect current edge subtraction](edge-subtraction.md).
- [Use handles on shifted components](drag-controls.md).
