# Dragging components and setting peak bounds

:::{note} Implemented controls — GUI 0.2.26
This page describes the actual component handles and their parameter conversions.
Dragging edits the working model; it does not run an optimisation. Use **Fit spectrum**
after arranging the components and their allowed ranges.
:::

## Select a component and expose its controls

On a compact layout, select the **Components** tab or reopen it from **View**.
Expand **Display options** below the plot for Focus Mode, fit-window and μ-bound
visibility, edge subtraction and component offsets. See [layout controls](layout-and-small-screens.md).

1. Click a peak or edge curve, its component-list entry or its parameter-table header.
   Clicking a curve selects it; grab a handle to move it.
2. Keep **Lock fit range** checked while arranging peaks, so the shaded fitting
   region does not move accidentally. **Show fit window** can hide that region.
3. Leave **Focus Mode** on to fade unselected components, or turn it off for comparison.
4. Check **Show μ bounds** when you want to edit the selected peak's position limits.

The curve must be visible in the component list for its handles to appear.
The handle colours below identify controls; component line colours can be changed
independently. Edges have [their own drag controls](#drag-ionisation-edge-parameters);
the linear background is edited in the parameter table.

:::{figure} ../media/images/component-drag-controls-preview.png
:label: figure-component-drag-controls
:alt: Selected EMG component with an orange apex target, blue vertical mu line, green sigma arrows, purple tau arrow and labelled vertical mu-min/mu-max bounds; components are visually offset below the data.

Actual controls on a synthetic EMG component. **Offset components** is on, so its
curve and handles sit below the data. The green/purple **vertical** lines labelled
μ min/max set bounds; the **horizontal** arrows change width and tail.
:::

## Handle key

| Grab this | Drag direction | What changes |
| --- | --- | --- |
| Orange target at the component maximum | Left/right | Peak position μ; the shape's maximum-to-μ offset is retained during translation |
| Orange target | Up/down | Integrated area A, calculated from the requested component height |
| Blue vertical dashed line | Left/right | Native μ, displayed at μ − dE |
| Either green width-arrow endpoint | Away from/toward its centre | Increase/decrease Gaussian sigma, or static gamma for a Lorentzian |
| Right endpoint of the purple EMG tail arrow | Away from/toward its origin | Increase/decrease tau, preserving maximum height by adjusting A |
| Labelled μ min / μ max vertical line | Left/right | Lower/upper allowed μ bound, without moving the current peak |
| Shaded fit-region boundary or interior, when unlocked | Left/right | Fitting-domain endpoint(s), rather than a component parameter |

The orange target can move in both directions together. If μ is fixed/linked,
only its free area direction can move; if A is fixed/linked, only its free position
direction can move. Dragging the curve body is not a substitute for the target.

## Move position or change height

Drag the **orange target** horizontally to translate the peak, vertically to alter
its contribution, or diagonally to do both. It belongs to the individual component,
not the total envelope or data-plus-edge height. A is **integrated area**, so the
GUI converts the target height using the actual profile, gain and optional component
scale. It does not simply write the height into the A cell.

Use the **blue line** to change μ alone. For Gaussian, Lorentzian and Voigt peaks,
the maximum is at the displayed μ. For an EMG, the orange maximum is generally
to the right of its blue μ line. Moving the orange target preserves that shape
offset; it does not redefine EMG μ to be the maximum.

(position-drags-carry-mu-interval)=
### Position drags carry the μ interval

While dragging the blue μ line or the position direction of the orange target,
the original Min/Max limits do not clamp the movement. They translate with μ,
preserving their original, possibly asymmetric distances. Bound lines temporarily
hide during the gesture and return on release if **Show μ bounds** is enabled.
For example, μ = 285.00 with Min = 284.80 and Max = 285.50 becomes μ = 288.00,
Min = 287.80 and Max = 288.50 after dragging. Infinite endpoints stay infinite.

Fixed positions remain fixed. Linked positions still follow their source/separation;
dragging a source peak moves its linked peaks and linked edges without changing
those relationships. This shortcut does not disable links or change the fit domain.
The translated interval is the hard interval used by the **next fit**. Table value
edits retain existing bounds; dragging a μ-bound line edits the interval without
moving μ. A completed position drag, including its new limits, is one Undo step.

A double-left-click inside the main data axes adds a **new** peak at that energy,
using the shape selected in **Component controls**. Its height is estimated from
the data above the current continuum; existing peaks stay in place. Use the
orange target or blue line to move an existing peak. The **Add multiple peaks**
dialog uses its own double-clicks to stage candidates for later addition; see
[batch creation](model-editor.md#add-several-peaks-in-one-operation).

## Change width without changing maximum height

Grab either **green arrow endpoint**. Moving it farther from the centre broadens
the component; moving it inward narrows it. Both sides update symmetrically.

- Gaussian and EMG: the parameter is **sigma**.
- Voigt: the parameter is its Gaussian **sigma**; gamma stays a separate table setting.
- Lorentzian: the parameter is its static **gamma**, which can be edited graphically
  but is not an optimiser variable and has no Free checkbox.

The label reports **one sigma or gamma on each side**, matching its numerical
parameter. The full arrow span is twice that radius. These handles do not claim
to measure the total FWHM of every lineshape.

The drag adjusts A to keep the component's maximum height constant. A broader
peak therefore usually acquires more integrated area. Editing sigma/gamma directly
in the table instead changes only that value and can change height at constant A.
Width dragging requires a free, independent A and, for sigma, a free independent
width. If the required A falls outside its bounds, the height-preserving change
is rejected; inspect Messages rather than assuming the width control is broken.

## Change EMG asymmetry

The **purple horizontal tail arrow** appears only for EMG peaks. Its origin is
the displayed μ and its length is tau. Drag its **right endpoint** outward for
a longer high-energy tail, or inward for a shorter tail; tau = 0 is the Gaussian
limit used by the engine. The origin is not a second free drag endpoint.

μ and sigma stay unchanged while A adjusts to preserve maximum height. The maximum
can shift horizontally as the asymmetry changes, so the orange target can move
even though the blue μ line stays still. Tau must be free/independent, A must be
free/independent, and both sets of bounds apply. This arrow is a parameter control,
not a measured width or a peak-to-peak separation.

(drag-ionisation-edge-parameters)=
## Drag ionisation-edge parameters

Select a visible edge in the component list or click its curve. The same controls
work for the first edge and each additional edge; only the selected edge is edited.

| Edge control | Drag direction | Parameter change |
| --- | --- | --- |
| Blue vertical position line | Left/right | E_edge within its existing bounds |
| Orange **Move edge / step** target at the edge midpoint | Left/right and/or up/down | E_edge and/or edge_step; fixed or linked axes stay locked |
| Green width-arrow endpoint | Toward/away from the centre | w_edge, with the edge_step parameter unchanged |
| Purple post-edge diamond | Up/down | post_slope at the indicated post-edge energy; horizontal motion is ignored |

The orange target belongs to that edge alone, excluding peaks, the linear background
and other edges. It lies at the transition midpoint, not the final step level. Its
height is converted using the native edge response and gain; edge_step is a step
amplitude, **not integrated peak area A**. The width arrow shows w_edge on each
side; it is not an EMG tail or a peak-position bound.

Added post_slope starts fixed at zero. Enable its **Free** checkbox before dragging
the purple diamond. Other fixed/linked parameters likewise retain their state:
a peak-linked E_edge cannot move independently, but its free step, width or slope
can still be edited. Edge parameter bounds continue to clamp edge drags; the
travelling-interval behaviour above applies to peak μ only. With zero gain the
height/slope response cannot be inverted, so those controls are unavailable.

These controls follow **Offset components**, with the visual shift removed during
height conversion. They hide when **Subtract current edges** is checked or that
edge's visibility is off. Turn subtraction off to edit an edge visually. Plot
curves update live, the table refreshes on release, and each completed drag has
one Undo step.

:::{figure} ../media/images/edge-drag-controls-preview.png
:label: figure-edge-drag-controls
:alt: Selected ionisation edge with a blue position line, orange midpoint target, green width arrow and purple post-edge slope diamond, alongside its parameter table.

Edge controls on a synthetic spectrum, with a free post-edge slope and component
offset enabled. Marker heights refer to the individual edge contribution.
:::

## Set position bounds by dragging lines

1. Select the peak and enable **Show μ bounds**.
2. Find the vertical lines labelled with that peak's **μ min** and **μ max**.
   Their labels distinguish them from the blue current-position line.
3. Drag μ min to set the lowest permitted position and μ max to set the highest.
4. Release and check that peak's **Min**/**Max** cells in the parameter table.

For example, a Gaussian currently at μ = 285.00 eV can have Min = 284.80 and
Max = 285.20 eV. Moving either bound changes the allowed interval; it does not
move the current peak to that endpoint or start a fit.

A limit stops at the current μ if dragged across it; the two bounds cannot
collapse to the same value. A fixed peak can still have its bounds edited, but
remains fixed until Free is enabled. Its A, fixed/free state and Sharing are
unchanged by a bound drag. Use exact numerical Min/Max edits when you need a
precise final value.

### If a bound is infinite or outside the view

Zoom to the desired allowed interval, keeping the **blue μ line** inside it.
Right-click the main plot and select **μ bounds from zoom**. This sets both
position limits from the visible energy interval, switches their display on and
slightly widens the view so both lines are accessible. Then refine them by dragging
or table editing. The command is in the plot context menu, not a separate button.

Merely zooming or selecting a peak never changes bounds. If the selected interval
excludes the current μ, the command reports the problem and leaves bounds unchanged.

### Energy offsets, EMG positions and linked peaks

The table/labels report native μ bounds. Their lines are drawn at **bound − dE**
to match the model's energy convention; with dE = +0.20 eV, a lower μ bound of
284.80 eV appears at plot energy 284.60 eV. For EMG these constrain μ, not its
observed maximum. See [energy conventions](../reference/lineshapes.md#energy-and-integrated-area).

A linked μ has no independent bound lines. Change the reference peak's bounds
or the separation parameter in **Peak constraints…**, rather than trying to drag
the linked target. See [relative peak constraints](peak-constraints.md).

## Three different ranges

| Range | How to change it | Effect |
| --- | --- | --- |
| Visible plot range | Wheel, axis zoom or pan | Changes what you can see |
| Fitting energy range | Fitting box or unlocked shaded region | Chooses data points used by the optimiser |
| Selected peak's μ bounds | Bound lines, explicit μ bounds from zoom, or Min/Max cells | Restricts that parameter's allowed position |

The **μ bounds from zoom** command deliberately transfers a viewing interval to
parameter bounds; ordinary zoom does not. **Lock fit range** locks the shaded
domain, not peak positions or their bounds. For fit-domain details, see
[running a fit](fitting-and-results.md#separate-the-viewing-range-from-the-fitting-range).

## Offsets, feedback and controls that will not move

**Offset components** shifts peaks, edges/background and their handles together.
Continue grabbing the shifted handles: the GUI removes the visual offset before
converting height to A. Data, envelope and residual are not shifted. Edge-subtracted
views likewise do not alter the underlying fit or the parameter conventions.
For the full effect on each curve and the offset-factor calculation, see
[component display settings](component-display.md).

The plot updates during a gesture; the parameter table refreshes on release.
Dragging a bound line avoids recalculating unchanged spectra. Each completed drag is one
model **Undo** step (Ctrl+Z). Check Messages for clamping or rejected changes.

| Symptom | Check |
| --- | --- |
| No component handles | Select a visible peak or edge; turn edge subtraction off for edge handles; backgrounds use the table |
| Orange target moves in only one direction | Check that direction's Free state and whether its parameter is linked |
| Width/tail refuses to move | Check free/linked states for both shape and A, area bounds, and zero gain/scale |
| No purple tail arrow | It is an EMG-only control |
| Bound lines missing | Enable Show μ bounds; finite limits may be off-screen, or μ may be linked |
| μ moves past its old limits | Position dragging carries its interval; set the final Min/Max after arranging if absolute limits are needed |
| Purple edge diamond does not move | Enable Free for post_slope and check bounds/gain; do not confuse it with the EMG tail arrow |
| Dragging a line moves the entire fit interval | You grabbed the shaded fitting region; lock or hide it, then grab the labelled peak-bound line |

Graphical arrangement supplies starting values and allowed intervals. Review them
numerically, then fit and inspect the residual; a visually plausible arrangement
does not establish a unique or physically appropriate decomposition.

## Related topics

- [Edit values, Free states and Sharing numerically](model-editor.md#values-bounds-free-and-sharing).
- [Use relative constraints for linked positions](peak-constraints.md).
- [Change focus, colours, edge subtraction and offsets](component-display.md).
- [Choose the fit domain and optimise the arranged model](fitting-and-results.md).
