# Constructing and editing a model

This guide describes the implemented editor in GUI **0.2.30**. The same parameter
conventions apply to graphical editing, numerical fitting and exported models.
For acquired data, first [select the intended detector/stage](scans-and-channels.md#select-each-scans-detector-independently)
and [prepare it if needed](preparing-data.md). To start from an existing template,
jump to [model reuse](model-files.md#reuse-a-saved-model-on-current-data).

## Components and optional corrections

Components and Parameters can now dock, float or share tabs. Reopen them through
**View**; expand **Component controls** or **Constraints / corrections** for their
buttons. See [small-screen arrangements](layout-and-small-screens.md).

The component list identifies peaks, ionisation edges and background terms.
Select a component to inspect its parameter group. List checkboxes affect display
only: hiding a curve does not remove that component from the fit.

:::{figure} ../media/images/model-components-controls.png
:label: figure-model-components-controls
:width: 260px
:alt: Components panel with Gaussian and EMG peaks, an added edge and background, and expanded controls for shape, addition, duplication, removal, grouping and edge links.

The Components panel of an existing synthetic model, with its controls expanded.
This example already includes optional edge/background terms; a new GUI model
does not add them automatically.
:::

| Control | Use |
| --- | --- |
| Shape dropdown, then **Add peak** | Add a Gaussian, Lorentzian, Voigt or EMG peak |
| Double-left-click inside the main data axes | Add a new peak at that energy using the Component controls shape dropdown |
| **Add multiple peaks…** | Stage several peaks in a table; double-left-click its data preview to place them |
| Right-click the main plot → **Add component here** | Place a chosen peak at a particular energy |
| **Duplicate** | Copy a selected peak for independent adjustment |
| **Add ionisation edge** | Add an optional edge; several edges are supported |
| **Remove** | Remove the selected component from the numerical model |
| **Global corrections…** | Add/remove optional gain, model energy offset or linear background |
| **Peak groups / scaling…** | Assign custom group labels and, explicitly, an existing scale parameter |

New GUI models contain no optional edge or background. Add only the terms the
spectrum needs. Peak starting heights account for the existing edge/background;
other existing peaks are not subtracted when seeding a new peak.

**Total** appears only when at least one peak or edge exists. The residual panel
appears when a peak, edge or explicitly added linear background is present;
gain/energy-shift corrections alone do not create a residual panel. Hiding a
component with its display checkbox keeps it in the model and residual.

Right-click the component list for rename, colours and other actions. Double-click
a peak or press F2 to rename it. **Undo/Redo** in History or Ctrl+Z/Ctrl+Y operate
on the active scan/channel's model.

### Place a peak with the mouse

Use [Adding peaks](adding-peaks.md) for click placement and batch creation.

## Graphical editing

Use the dedicated [component drag-control guide](drag-controls.md) for an actual
handle screenshot, position/area gestures, height-preserving width/tail controls,
fixed/linked behaviour and feedback. It also explains EMG μ versus the maximum,
one-sigma labels and how component offsets affect the visible handles.

## Set peak-position limits visually

The [bound-dragging walkthrough](drag-controls.md#set-position-bounds-by-dragging-lines)
covers labelled μ min/max lines, **μ bounds from zoom**, fixed peaks and energy
offsets. Viewing range, fitting domain and μ bounds are separate controls.
For bounds relative to another peak, use [peak constraints](peak-constraints.md).

## Add edges and global corrections explicitly

**Add ionisation edge** creates the first edge or further Edge2, Edge3… components.
Each has position E_edge, width w_edge, step height and post-edge slope; additional
edge parameters use their edge name prefix. Edit them in the table or use the
[edge drag controls](drag-controls.md#drag-ionisation-edge-parameters).
Select an edge/background and **Remove** to delete it from the numerical model.
List visibility checkboxes only hide curves. Old models keep their existing terms.

**Global corrections…** supplies optional gain, model dE and one linear background.
Added gain starts at 1 and dE at 0, both fixed until Free is checked. The background
uses b0/b1 outside gain and a full-grid mean-energy reference. A parameter used as
a component scale cannot be removed until that scale assignment is resolved.
See [the numerical model](../reference/lineshapes.md#background-and-ionisation-edges).
Every edge has [edge-to-peak link controls](edge-links.md#edge-links-position).
The panel shows an Edge selector when more than one edge is present.

## Organise groups and line colours

Open **Peak groups / scaling…**. Group cells accept existing labels or custom names,
including spaces. Select rows with Ctrl/Shift, enter **Group for selected rows**,
then **Assign group** to name several together. Apply commits all assignments as
one Undo step. Labels appear in the component list/table and survive saving and
propagation; the series workflow can select parameters by named group.

:::{important} Group labels do not impose constraints
Grouping collects group sums. It does **not** tie parameters, colour curves,
change activation, or scale areas. All GUI series peaks stay active, including
those labelled surface. **Scale parameter** separately selects an existing
global/edge parameter; `(none)` means no component multiplier. Additional named
scale parameters can come from model files/scripting; the dialog does not invent them.
:::

For colour editing and colour persistence, use
[Component visibility, focus and colours](component-display.md). Group labels
and matching colours remain separate choices.

## Inspect a busy spectrum

Use [Component display, edge subtraction and offsets](component-display.md) for
visibility checkboxes, Focus Mode, colours and saved display preferences. Its
[Subtract current edges](edge-subtraction.md#subtract-current-edges) and
[Offset components](component-offsets.md#offset-components) sections explain exactly
which curves change, the factor calculation, overlays and the unchanged residual/fit.

Horizontal wheel zoom-out stops at the displayed data extent plus **3 eV** on
each side. Vertical wheel-in/out uses a common lower-edge anchor to avoid drift;
scroll over an axis to change that axis only. Explicit framing/panning remains
available. Zooming alone does not set fitting limits or parameter bounds.

## Related procedures

### Values, bounds, Free and Sharing

See [Values, bounds, Free and Sharing](parameter-table.md#values-bounds-free-and-sharing) for this procedure.

### Add several peaks in one operation

See [Add several peaks in one operation](adding-peaks.md#add-several-peaks-in-one-operation) for this procedure.

### Reuse a saved model on current data

See [Reuse a saved model on current data](model-files.md#reuse-a-saved-model-on-current-data) for this procedure.

## Related topics

- [Arrange components and drag their position limits](drag-controls.md).
- [Separate display changes from numerical model edits](component-display.md).
- [Link parameters instead of aligning independent bounds by eye](peak-constraints.md).
- [Fit and review the constructed model](fitting-and-results.md).
- [Interpret native profiles and integrated areas](../reference/lineshapes.md).
