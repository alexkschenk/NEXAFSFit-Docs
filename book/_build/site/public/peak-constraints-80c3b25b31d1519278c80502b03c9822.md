# Linking peaks and ionisation edges

This guide covers GUI **0.2.24**. Peak links relate parameters **within a model**.
[Series constraints](series-fitting.md#restrict-propagation-around-the-reference)
and [trends](series-fitting.md#trends-and-ordering) control different relationships.
Add components with the [model editor](model-editor.md) first. For independent
absolute μ limits, use [graphical bounds](drag-controls.md#set-position-bounds-by-dragging-lines).

## Keep a second peak within an energy separation

Suppose P2 should lie 1.8–2.2 eV above P1, while P1 can move within its own bounds.

1. Add both peaks and give them distinct names.
2. Open **Peak constraints…** above the parameter table.
3. Set **Target peak** to P2, **Reference peak** to P1 and **Parameter** to μ position.
4. Choose **Within a range**. Enter initial separation **2.0**, minimum **1.8**
   and maximum **2.2** eV.
5. Click **Add / update constraint**, check the list, then **Apply**.

The model now uses `P2_mu = P1_mu + delta_P2_mu`. The separation is an independent
fitted parameter inside [1.8, 2.2]; P2's position is derived. Put the absolute
allowed energy range on P1 and edit the generated separation to change the link.
This keeps the interval relative to P1 as P1 moves.

The target's old independent value/bounds/Free settings are preserved but inactive
while linked. They do not additionally clip the calculated target. Check the
reference and separation bounds together when deciding physically allowed positions.
EMG μ means the underlying Gaussian position, so the displayed peak maxima need
not have that same separation.

## Exact offsets and equal widths

Choose **Exact offset / equality** to keep a fixed separation. An exact position
offset of 2.0 gives P2 μ = P1 μ + 2.0. An exact sigma offset of zero gives equal
Gaussian sigma values. These are additive relations, not ratios.

The Parameter dropdown offers only parameters active on both selected shapes:
μ and A where supported, sigma for Gaussian/Voigt/EMG, and tau for EMG pairs.
Static gamma is not a fitted/linkable coordinate. Equal areas do not imply equal
heights when widths/shapes differ; equal Voigt sigma does not also tie gamma.

Select an existing constraint to edit it, or **Remove selected constraint** to
make its target independent again. **Apply** commits the dialog as one Undo step;
Cancel discards its staged edits. Cycles, self-links and incompatible references
are rejected. Rename updates supported relationships; removal of a peak needed
by a link requires resolving the dependency first.

:::{figure} ../media/images/peak-constraints-preview.png
:label: figure-peak-constraints
:alt: Peak parameter constraints dialog with a target/reference list and controls for an initial separation and allowed range.

A staged relative-position constraint on synthetic model components.
:::

(link-the-first-ionisation-edge-to-a-peak)=
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

The relation is **E_edge = selected peak μ + offset**. For peak μ = 285.00 eV,
an offset of +0.20 gives E_edge = 285.20 eV; a negative offset places it below μ.
It is an exact offset, not an independent fitted separation. For EMG the source
is its underlying μ, not its observed maximum. The model dE shifts both displayed
positions consistently, without changing their native separation.

Linking replaces the edge's independent position bounds/Free setting with the
derived position; put absolute position bounds on the controlling peak's μ.
In simultaneous fits, the edge's global/per-spectrum scope follows that peak μ.
To change the peak or offset, reopen the same panel. An existing link's peak and
offset appear initially, even if another component is selected. To release the
relationship, select the edge, clear **Link edge to peak μ** and Apply. This keeps the current derived edge
energy, makes E_edge free and resets its bounds to unbounded; review Min/Max/Free
before fitting it independently. The link/unlink model edit can be undone.

Each edge can follow a different peak, or several edges can follow the same peak
with different signed offsets. Width, step and post-slope stay independently
configured. All edges' linked positions use the same native exact-offset convention
and inherit the controlling peak's effective global/per-spectrum scope in series fits.
Renaming a controlling peak updates its edge links; unlink before removing a peak
that still controls an edge. Linked models retain these relationships when saved,
reopened or exported. Linking a peak does not insert a missing physical feature into the data.
If the button is unavailable, check that at least one peak and edge exist.
Select the controlling peak inside the panel; no plot selection is required.
Add an edge rather than expecting the link command to create one. For
links between peaks with a fitted separation interval, use the peak-constraint
procedure above instead.

## Links, sharing and saved models

In a simultaneous series fit, an independent parameter can be global or
per_spectrum. A linked target inherits the scope required by its source/separation;
its Sharing may therefore be inferred and unavailable for direct editing. Restrict
the controlling reference or `delta_<target>` when adding series bounds/trends.

Model/session JSON retains structured links and inactive target settings. Use
[model saving and reuse](sessions-and-models.md) to transfer the complete model,
and review [scientific constraint conventions](../reference/constraints.md)
when choosing its bounds and sharing.

## Related topics

- [Set an independent peak position interval visually](drag-controls.md#set-position-bounds-by-dragging-lines).
- [Apply reference-relative bounds or trends to a series](series-fitting.md).
- [Distinguish hard bounds, links, sharing and penalties](../reference/constraints.md).
- [Save or export a linked model](exporting-fits.md#choose-what-to-save).
