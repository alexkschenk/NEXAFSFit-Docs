# Link or unlink edge positions

GUI **0.2.30**. Add at least one peak and one ionisation edge with the [model editor](model-editor.md). This panel relates edge positions to peak μ; [peak constraints](peak-constraints.md) handle peak-to-peak relationships.

(edge-links-position)=

## Link ionisation edges to peaks

To keep an edge a specified energy distance from a component:

1. Add the required edges using **Add ionisation edge**, if absent, and add their controlling peaks.
2. Optionally select an edge or controlling peak in the component list or plot.
3. Click **Link/Unlink edge to μ…** in Component controls.
4. When multiple edges are present, choose **Edge**. With one edge, this selector
   is hidden. Check **Link edge to peak μ**, choose the controlling **Peak**, enter
   the exact **Offset (eV)**. Cancel discards panel changes.
5. Switch edges to configure other links if needed, then Apply once. Check each
   edge's position relationship in the parameter table. Moving or fitting a
   controlling peak moves its linked edges with it.

:::{figure} ../media/images/edge-link-preview.png
:label: figure-edge-link-panel
:alt: Compact edge-link panel with a link checkbox, peak dropdown, signed offset and Apply/Cancel buttons.

One edge keeps the panel compact, without an Edge selector.
:::

:::{figure} ../media/images/edge-links-multiple-preview.png
:label: figure-multiple-edge-links
:alt: Edge-link panel with Edge2 selected, linked to peak Q with a signed offset and Apply/Cancel controls.

Multiple edges add one selector. Changes across edges are staged together until Apply;
switching the selector alone does not add links. Cancel discards all staged edits.
:::

### Read the edge-position relationship

The relation is **E_edge = selected peak μ + offset**. For peak μ = 285.00 eV,
an offset of +0.20 gives E_edge = 285.20 eV; a negative offset places it below μ.
It is an exact offset, not an independent fitted separation. For EMG the source
is its underlying μ, not its observed maximum. The model dE shifts both displayed
positions consistently, without changing their native separation.

Linking replaces the edge's independent position bounds/Free setting with the
derived position; put absolute position bounds on the controlling peak's μ.
In simultaneous fits, the edge's global/per-spectrum scope follows that peak μ.
To change the peak or offset, reopen the same panel. An existing link's peak and
offset appear initially, even if another component is selected.

### Unlink an edge

Select the edge, clear **Link edge to peak μ** and Apply. The link/unlink model
edit can be undone.

:::{warning} Review position bounds after unlinking
Unlinking keeps the current derived edge energy, makes E_edge free and resets
its bounds to unbounded. Review Min/Max/Free before fitting it independently.
:::

### Manage several linked edges

Each edge can follow a different peak, or several edges can follow the same peak
with different signed offsets. Width, step and post-slope stay independently
configured. All edges' linked positions use the same native exact-offset convention
and inherit the controlling peak's effective global/per-spectrum scope in series fits.
Renaming a controlling peak updates its edge links; unlink before removing a peak
that still controls an edge. Linked models retain these relationships when saved,
reopened or exported. Linking a peak does not insert a missing physical feature into the data.

### If the link command is unavailable

If the button is unavailable, check that at least one peak and edge exist.
Select the controlling peak inside the panel; no plot selection is required.
Add an edge rather than expecting the link command to create one. For
links between peaks with a fitted separation interval, use the peak-constraint
procedure above instead.

## Related topics

- [Add edges and corrections](model-editor.md#add-edges-and-global-corrections-explicitly).
- [Edit edge parameters graphically](drag-controls.md#drag-ionisation-edge-parameters).
- [Choose peak-to-peak constraints](peak-constraints.md).
