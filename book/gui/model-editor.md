# Constructing and editing a model

This guide describes the implemented editor in GUI **0.2.26**. The same parameter
conventions apply to graphical editing, numerical fitting and exported models.
For acquired data, first [select the intended detector/stage](scans-and-channels.md#select-each-scans-detector-independently)
and [prepare it if needed](preparing-data.md). To start from an existing template,
jump to [model reuse](#reuse-a-saved-model-on-current-data).

## Components and optional corrections

Components and Parameters can now dock, float or share tabs. Reopen them through
**View**; expand **Component controls** or **Constraints / corrections** for their
buttons. See [small-screen arrangements](layout-and-small-screens.md).

The component list identifies peaks, ionisation edges and background terms.
Select a component to inspect its parameter group. List checkboxes affect display
only: hiding a curve does not remove that component from the fit.

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

Peak names must be unique Python identifiers. With loaded data, double-left-click
and right-click placement use the clicked energy, while the initial height is estimated from
the spectrum above the current continuum; the vertical click position is not a
separate height request. For EMG, native μ placement can differ from its maximum.
Each main-plot double-left-click adds a new peak and selects it, without moving
existing peaks. Undo removes that addition. Double-clicks on the axes or residual
panel do not add peaks.

## Values, bounds, Free and Sharing

The table groups rows under coloured component headers.

New peaks start with the following bounds (energies in eV):

| Parameter | Default Min | Default Max |
| --- | --- | --- |
| μ | Initial μ − 1 | Initial μ + 1 |
| σ (Gaussian, Voigt and EMG) | 0.1 | 5 |
| τ (EMG) | 0 | 2 |

These are starting bounds, editable in the table or with the applicable drag
controls. Dragging an independent peak position translates its Min/Max interval
with it, retaining the original distances from μ. Numerical value edits do not
translate bounds. Existing models and duplicated peaks retain their configured
bounds until explicitly edited. In **Add multiple peaks…**, an
explicit starting σ or τ outside the default interval extends the necessary
endpoint to include that value; ordinary starting values keep the defaults.

| Field | Meaning |
| --- | --- |
| Value | Current model value and initial value for the next fit |
| Min / Max | Hard limits on an independent fitted parameter |
| Free checked | Allow the optimiser to vary that parameter |
| Free unchecked | Hold the parameter fixed |
| Sharing | `global` or `per_spectrum` for simultaneous series fits |
| Relations / notes | Generated links, area conventions or static specifications |

The [series scope guide](series-fitting.md#choose-targets-and-a-strategy) explains
when Sharing affects fitting. [Parameter-state definitions](../reference/constraints.md#four-different-parameter-states)
distinguish bounds, fixed values, linked targets and static specifications.

A fixed parameter still needs a valid ordered interval; do not make Min equal
Max to fix it. Use Free instead. A linked target is calculated from another
parameter and cannot be edited independently. Its previously stored independent
bounds/settings remain inactive until the link is removed.

Lorentzian/Voigt `gamma` is a static specification: edit its value, but it is not
optimised through a Free checkbox. See [lineshapes and parameters](../reference/lineshapes.md).

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

## Add several peaks in one operation

Open **Add multiple peaks…**. Set Count, First/Last μ and Shape, then **Generate
rows** to create evenly spaced candidates. Alternatively, **Clear rows**, choose
a shape and double-left-click the data preview above the table to place peaks.
Click placement preserves existing rows and edits; Generate rows replaces them.
Single clicks and double-right-clicks do not append peaks. Up to 200 can be staged.

Edit names, μ, shapes and the relevant sigma/tau/static-gamma cells. Leave Area A
blank or `auto` to match the data above the **current edges and linear background**.
Existing peaks are not subtracted when choosing a new peak's height. A nonpositive
height above that continuum gives zero nonnegative area. Outside the loaded data,
enter an explicit area instead of using auto.

**Preview initial areas** shows the calculated A values. Labels/markers follow
the pending rows. **Add peaks** validates the whole batch and adds it as one Undo
step; Cancel leaves the model unchanged. The preview retains the main plot's view
and edge-subtracted state. Its wheel follows the same energy limits/lower-edge
anchor; zooming does not edit candidate parameters. For EMG, placement specifies
native μ at the clicked displayed energy, which need not be its observed maximum.

:::{figure} ../media/images/batch-peaks-preview.png
:label: figure-batch-peaks
:alt: Add multiple peaks dialog with the current data above an editable candidate table and three labelled position markers.

Place candidates visually, then review their numerical starting parameters before adding them.
:::

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
Every edge has [edge-to-peak link controls](peak-constraints.md#link-the-first-ionisation-edge-to-a-peak).
The panel shows an Edge selector when more than one edge is present.

## Organise groups and line colours

Open **Peak groups / scaling…**. Group cells accept existing labels or custom names,
including spaces. Select rows with Ctrl/Shift, enter **Group for selected rows**,
then **Assign group** to name several together. Apply commits all assignments as
one Undo step. Labels appear in the component list/table and survive saving and
propagation; the series workflow can select parameters by named group.

Grouping collects group sums. It does **not** tie parameters, colour curves,
change activation, or scale areas. All GUI series peaks stay active, including
those labelled surface. **Scale parameter** separately selects an existing
global/edge parameter; `(none)` means no component multiplier. Additional named
scale parameters can come from model files/scripting; the dialog does not invent them.

Use **Actions… → Line colours…** or the component-list context menu to select one
or several peaks, edges, the envelope/background or comparison scans and assign
a colour. The envelope defaults to red. Model colours save with models/sessions
and participate in model Undo. Comparison colours are per-scan session view settings.
Matching colours and matching group names are independent choices.

## Inspect a busy spectrum

Use [Component display, edge subtraction and offsets](component-display.md) for
visibility checkboxes, Focus Mode, colours and saved display preferences. Its
[Subtract current edges](component-display.md#subtract-current-edges) and
[Offset components](component-display.md#offset-components) sections explain exactly
which curves change, the factor calculation, overlays and the unchanged residual/fit.

Horizontal wheel zoom-out stops at the displayed data extent plus **3 eV** on
each side. Vertical wheel-in/out uses a common lower-edge anchor to avoid drift;
scroll over an axis to change that axis only. Explicit framing/panning remains
available. Zooming alone does not set fitting limits or parameter bounds.
(reuse-a-saved-model-on-current-data)=
## Reuse a saved model on current data

Import the desired data first, then **Model → Open model** and choose a saved JSON.
This changes the active channel's model without replacing its data/preparation,
fit range, view or other scans. A supported session JSON can supply just its model.
The prior model is available through Undo; the prior fit statistics are cleared
because they do not describe the imported model. Fit again before exporting it.

**Session → Open session** instead restores the saved collection/workspace.
Compatibility notices explain retained inactive fields. An EMG's old gamma value,
for example, is retained but not treated as active broadening. See
[saving and exports](exporting-fits.md) for the artifact choices.

## Related topics

- [Arrange components and drag their position limits](drag-controls.md).
- [Separate display changes from numerical model edits](component-display.md).
- [Link parameters instead of aligning independent bounds by eye](peak-constraints.md).
- [Fit and review the constructed model](fitting-and-results.md).
- [Interpret native profiles and integrated areas](../reference/lineshapes.md).
