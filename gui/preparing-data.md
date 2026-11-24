# Importing and preparing data

:::{note} Current workflow — GUI 0.2.27
One dialog prepares a single acquisition or selected scans. Preview stages results
from retained originals; Apply commits them. The reference panel is optional.
:::

## Import a spectrum or a folder

Choose **Data import → Import** for an individual file or **Load folder** for native
SXR `.asc` acquisitions. Include the separate PD acquisition if it will be used.
This preparation workflow requires retained native channels. Follow
[import and channel selection](scans-and-channels.md#import-files-or-a-folder)
for header/column handling, ordinary CSVs and folder-import behaviour.

### Measured-energy jitter

Resolve the explicit [energy-order review](import-diagnostics.md#review-before-importing-a-sorted-copy)
before preparation. Sorting is a reviewed copy operation; it does not smooth,
average or discard measurements or repair duplicate/nonfinite energies.

## The scan browser

Each row has an independent Detector selection. Switching it reads retained
arrays without processing other scans. [Channel caches and overlays](scans-and-channels.md#select-each-scans-detector-independently)
explain working-state preservation; [metadata](metadata.md) and [docking](scans-and-channels.md#detach-or-hide-the-browser)
cover angle sources and the detachable browser. Overlay checkboxes do not select
preparation or fitting targets.

## Preparation is explicit

Open **Preparation → Prepare scans…** on a native scan. The active scan is checked
initially. On smaller screens expand **Scans to prepare** to see the target table;
its header reports the selected count. Settings scroll independently of the plots.
See [small-screen arrangements](layout-and-small-screens.md#preparation-and-series-dialogs).

Leave just the active row checked for one scan, or check additional scans
for a series. **Only active scan**, **Select all data scans except the chosen
photodiode reference** and **Clear selection** make the scope explicit.

Preview the proposed changes, inspect them, then **Apply to selected scans**.
Cancellation leaves all working scans intact. Preparation starts from original copies, rather than
normalising an earlier result again.

| Step | What to choose or inspect |
| --- | --- |
| I0 normalisation | Divide mapped detector signals by the incident monitor |
| Energy alignment | Select one reference channel, feature window, maximum/minimum and target energy |
| Separate photodiode correction | Choose an imported PD acquisition or a separate file and its PD channel |
| Intensity preparation | Choose baseline/scale windows and mean-to-one or area-to-one scaling |

One reference-derived energy offset aligns all channels within an acquisition;
they are acquired in parallel. Intensity preparation applies to the mapped
detector channels, not just the preview detector. Raw I0/beam-current monitor
arrays remain available for inspection. A failing optional channel is reported.

Photodiode choices expand only when **Divide by separate I0-normalised photodiode
scan** is checked. Baseline, scale and scale-method controls likewise appear under
the intensity-preparation checkbox. Unchecking a section disables that operation;
the stored choices remain available when it is checked again.

The default reference and window values come from an earlier workflow. They are
editable starting suggestions, not universal calibration energies or suitable
windows for every absorption edge.

### What Preview runs

Clicking **Preview** runs the selected recipe on copies in this order:

1. I0-normalise detector signals, when enabled.
2. Locate the selected reference feature in reference/I0 and calculate its energy
   offset, when alignment is enabled. This reference/I0 calculation also requires
   the I0 monitor when detector I0 normalisation is unchecked.
3. If selected, independently prepare/align the separate photodiode acquisition
   and divide by its response over the common range.
4. Subtract the baseline-window mean and apply the selected scale method, when
   intensity preparation is enabled.

Reference calibration therefore precedes baseline subtraction and scaling; it
does not depend on those windows. **Preview** reruns the whole selected recipe
from the originals each time, rather than further processing the previous output.

| Action | Processing or state change |
| --- | --- |
| Open the dialog or edit settings/windows | No processing; edits invalidate any previous apply-ready result |
| Preview | Recalculate all selected targets and stage the results for inspection |
| Show reference calibration, select its scan or zoom | Read retained diagnostic results; no processing or target changes |
| Apply to selected scans | Commit a successful current preview; no extra calibration pass |
| Cancel | Discard staged changes; keep working scans intact |

The acquisition's **reference search window** uses original measured energy.
The **baseline and scale windows** use the prepared energy axis, including the
calibration offset when alignment is enabled. The raw/prepared axes line up
visually, but their signals can have different energy offsets. Reference inspection
uses measured energy so its marker identifies the feature before alignment.

## Inspect the reference calibration

With energy alignment checked, click **Preview**, then **Show reference calibration**
to expand an optional panel on the right. If that would exceed the available
screen, it opens as a **Reference calibration** preview tab instead; switch back
to **Raw / prepared preview** for the signals. **Hide reference calibration** collapses
it again. The panel is hidden initially and does not change preparation targets.

Choose an acquisition in its dropdown to see the actual reference-channel/I0
signal used by that acquisition's preview. A separate **PD reference** entry
shows the photodiode acquisition's own calibration when PD correction is enabled.
The chosen energy-reference channel is respected, including alternatives to the foil.

For **parab3**, orange points identify the three native input samples and the red
curve shows their **local interpolating parabola**. The vertical marker and readout
give the measured feature energy; the summary also reports the aligned target
and applied offset. This is feature localisation, not a full-spectrum Gaussian
or component fit. **argextreme** and boundary fallbacks show the measured point
and explain why no parabola is drawn. See [the calibration convention](../reference/lineshapes.md#reference-energy-calibration).

The horizontal axis is **measured energy before alignment**. It initially spans
the recorded reference search window; wheel/drag zoom can inspect the wider
retained trace. **Show search window** restores the recorded limits. The fitted
curve is drawn only across its three input samples, rather than extrapolated
over the entire reference spectrum.

Opening the panel, changing its scan or zooming never runs calibration. It reads
results retained by Preview. After settings change, the panel labels those results
as **previous preparation / preview**; Preview again to update. Unpreviewed scans,
older sessions without an inspection record and alignment-disabled preparations
show an explicit unavailable message. A failed preview does not replace an older
display with an apparently successful result. Apply still requires a successful
complete preparation preview.

:::{figure} ../media/images/reference-calibration-preview.png
:label: figure-reference-calibration
:alt: Expanded preparation dialog with a reference acquisition selector, measured reference over I0 signal, local red parabola, orange input samples and purple feature-energy marker.

Optional reference inspection in GUI 0.2.24. These scans are synthetic; measured
feature energy and the target/offset readout describe the native calibration.
:::

## Choose normalisation windows by inspection

After Preview, drag the **blue Baseline** and **orange Scale** boundaries on the
lower prepared-energy plot. Drag either boundary to change a limit, or drag the
shaded interval to move a whole window. The numeric fields update when the drag
is released; numeric edits also move the markers. The markers are hidden when
baseline/scale intensity preparation is unchecked.

If a default window lies outside your scan, enter suitable limits numerically
before Preview. The defaults are suggestions, not windows inferred from the data.

The lower plot uses the prepared energy axis, including the chosen alignment.
The raw-count plot above remains a comparison with the original acquisition.
Use the preview-stage dropdown to inspect corrected or I0-normalised signals as
well as the fit-ready signal, and enable overlays to compare selected scans.

Changing a window or recipe invalidates Apply. The previous curves remain visible
as an explicitly labelled **previous preview**, so features do not disappear
while placing the windows. No processing runs during a drag. Click **Preview**
again and review the result before **Apply to selected scans** becomes available.

:::{figure} ../media/images/preparation-windows-preview.png
:label: figure-preparation-windows
:alt: Unified preparation dialog with scan-selection checkboxes, a shared photodiode reference, and blue baseline and orange scale window boundaries on the lower prepared-energy plot.

The same dialog prepares one scan or an explicitly selected series. These
illustrative scans are synthetic; matching colours identify corresponding traces.
:::

After optional I0/PD correction, intensity preparation subtracts the mean in the
baseline window. **Scale method** then selects the denominator:

- **Mean to one** (default): mean of the baseline-subtracted signal in the scale window.
- **Area to one (trapezoidal)**: signed integral of that signal against measured
  energy, using consecutive points inside the scale window. It requires at least
  two points and a nonzero finite area. Endpoints are not interpolated or extrapolated.

Area scaling accounts for unequal energy spacing; it makes the measured window's
integral one, rather than its average height. It also changes the prepared signal's
units relative to mean scaling. Negative signed areas are allowed, as negative
means are in mean mode. Choose suitable windows and inspect the preview. These
preparation areas are distinct from individual fitted peak areas $A$.

The native engine supplies baseline/mean preparation; optional area scaling is
implemented in the GUI's Qt-independent preparation wrapper. Existing sessions
without a scale-method setting keep mean scaling. The choice and denominator
are retained with the processed channels and session. See also
[scientific conventions](../reference/lineshapes.md#preparation-scaling).

The PD acquisition is I0-normalised and, when selected, independently energy
aligned. Its response is interpolated for division over the common energy
range. The overlap can shorten the prepared spectrum; inspect the preview and
retained point counts. Missing references, zero denominators and unsuitable
windows are errors to resolve, not reasons to silently invent a correction.

## Apply one recipe to a series

Use that same **Prepare scans…** dialog, select the sample scans and a common
photodiode reference, then Preview. Use the multi-scan overlays to inspect how
the chosen windows affect the set before applying. Select targets explicitly;
the PD acquisition should normally be excluded from the sample targets.

After Apply, the Detector dropdown offers retained stages such as aligned counts,
I0-normalised, I0/PD-corrected and prepared signals when available. Preparation
changes invalidate the affected fits. Older cached channels that could not be
replaced are labelled **previous preparation**.

Save a complete session before major processing changes if you need a persistent
checkpoint. Preparation is not a single collection-wide model Undo operation.
Original source files are not rewritten by Preview or Apply.

## If preparation needs attention

| What you see | What to check |
| --- | --- |
| No reference trace or marker | Enable alignment, include that acquisition among the targets and complete Preview. For the PD entry, enable PD correction and select a separate reference. |
| Reference points and marker but no red curve | Check the localisation method. Argextreme selects a sample; parab3 also falls back to a sample at the search-window boundary or with degenerate inputs. |
| Previous preparation / preview | The record is saved or stale. Correct the settings/selection and Preview again; inspecting it does not refresh it. |
| Apply disabled after changing windows | The old curves remain for positioning, but new settings require a successful Preview. |
| No points in a window, or invalid scaling | Check window energy coordinates, overlap after PD correction and detector values. Area scaling needs at least two measured points and a finite nonzero signed area. |
| A target fails, or only an optional detector fails | Read each target's status and Channel issues. A failed target blocks the batch; unavailable optional channels are reported separately. |

A new reference diagnostic currently requires a successful **complete** Preview.
If a later preparation step fails, the panel can show an earlier record as stale,
but it does not expose the incomplete attempt as a new successful calibration.
For the numerical definitions, use the [shared scientific reference](../reference/lineshapes.md).

## Related topics

- [Select the prepared detector/stage](scans-and-channels.md#select-each-scans-detector-independently) after Apply without repeating processing.
- [Construct and reuse a model](model-editor.md) on the prepared spectrum, then [run a single fit](fitting-and-results.md).
- [Set up series fitting](series-fitting.md) when acquisition preparation is complete; its bounds/trends are model constraints.
- [Save a session](exporting-fits.md#choose-what-to-save) to retain original/processed channels and a preparation checkpoint.
- [Follow the surrounding acquisition workflow](../examples/beamline-workflow.md).
- [Diagnose stale previews, calibration and scaling errors](troubleshooting.md#preparation-and-calibration).
