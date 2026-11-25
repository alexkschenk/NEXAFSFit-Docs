# Constrain relationships between peaks

This guide covers GUI **0.2.30**. Peak links relate parameters **within a model**.
[Series constraints](series-constraints.md#restrict-propagation-around-the-reference)
and [trends](series-trends.md#trends-and-ordering) control different relationships.
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

:::{important} Bound the source and separation
The target's old independent value/bounds/Free settings are preserved but inactive
while linked. They do not additionally clip the calculated target. Check the
reference and separation bounds together when deciding physically allowed positions.
:::

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

## Links, sharing and saved models

In a simultaneous series fit, an independent parameter can be global or
per_spectrum. A linked target inherits the scope required by its source/separation;
its Sharing may therefore be inferred and unavailable for direct editing. Restrict
the controlling reference or `delta_<target>` when adding series bounds/trends.

Model/session JSON retains structured links and inactive target settings. Use
[model saving and reuse](sessions-and-models.md) to transfer the complete model,
and review [scientific constraint conventions](../reference/constraints.md)
when choosing its bounds and sharing.

## Related procedures

(link-the-first-ionisation-edge-to-a-peak)=

### Link ionisation edges to peaks

See [Link ionisation edges to peaks](edge-links.md#edge-links-position) for this procedure.

## Related topics

- [Set an independent peak position interval visually](drag-controls.md#set-position-bounds-by-dragging-lines).
- [Apply reference-relative bounds or trends to a series](series-fitting.md).
- [Distinguish hard bounds, links, sharing and penalties](../reference/constraints.md).
- [Save or export a linked model](exporting-fits.md#choose-what-to-save).
