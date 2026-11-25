# Preview and apply scan preparation

:::{note} Current workflow — GUI 0.2.30
One dialog prepares a single acquisition or selected scans. Preview stages results
from retained originals; Apply commits them. The reference panel is optional.
:::

Import the acquisitions first using [Import data](importing-data.md), resolving
any [energy-order review](energy-order-review.md). Preparation requires retained
native channels; ordinary text/CSV imports do not supply native monitor/reference
records. Select the intended acquisition in the [scan browser](scans-and-channels.md).

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

:::{figure} ../media/images/prepare-scans-overview.png
:label: figure-prepare-scans-overview
:alt: Prepare scans dialog with two sample acquisitions selected, a photodiode reference excluded from the targets, processing settings, raw and prepared overlays, and Preview and Apply buttons.

Prepare scans after a successful synthetic preview. The target table is at the
top, recipe settings on the left, and matching raw/prepared traces on the right.
The separate PD scan is a reference, not a sample target. The shown settings are
illustrative rather than recommended calibration or normalisation values.
:::

| Step | What to choose or inspect |
| --- | --- |
| I0 normalisation | Divide mapped detector signals by the incident monitor |
| Energy alignment | [Configure and inspect the reference feature](reference-calibration.md) |
| Separate photodiode correction | [Select a shared PD acquisition and channel](photodiode-correction.md) |
| Intensity preparation | [Place baseline/scale windows and choose mean or area](normalisation-windows.md) |

:::{important} One energy alignment per acquisition
One reference-derived energy offset aligns all channels within an acquisition;
they are acquired in parallel. Intensity preparation applies to the mapped
detector channels, not just the preview detector. Raw I0/beam-current monitor
arrays remain available for inspection. A failing optional channel is reported.
:::

### Enable only the operations you need

Photodiode choices expand only when **Divide by separate I0-normalised photodiode
scan** is checked. Baseline, scale and scale-method controls likewise appear under
the intensity-preparation checkbox. Unchecking a section disables that operation;
the stored choices remain available when it is checked again.

:::{warning} Check the reference and window defaults
The default reference and window values come from an earlier workflow. They are
editable starting suggestions, not universal calibration energies or suitable
windows for every absorption edge.
:::

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

### Use the correct energy axis for each window

Reference searches use original measured energy. Baseline/scale windows use the
prepared axis; see [reference coordinates](reference-calibration.md#use-the-correct-energy-axis-for-each-window)
and [normalisation windows](normalisation-windows.md).

## Apply one recipe to a series

Use that same **Prepare scans…** dialog, select the sample scans and a common
photodiode reference, then Preview. Use the multi-scan overlays to inspect how
the chosen windows affect the set before applying. Select targets explicitly;
the PD acquisition should normally be excluded from the sample targets.

After Apply, the Detector dropdown offers retained stages such as aligned counts,
I0-normalised, I0/PD-corrected and prepared signals when available. Preparation
changes invalidate the affected fits. Older cached channels that could not be
replaced are labelled **previous preparation**.

:::{tip} Keep a preparation checkpoint
Save a complete session before major processing changes if you need a persistent
checkpoint. Preparation is not a single collection-wide model Undo operation.
Original source files are not rewritten by Preview or Apply.
:::

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
For the numerical definitions, use [acquisition processing conventions](../reference/preparation.md).

## Related procedures

### Import a spectrum or a folder

The import process is described in [Import data](importing-data.md).

### The scan browser

See [scan and channel selection](scans-and-channels.md). Browser overlays do not
select preparation targets.

### Inspect the reference calibration

See [Inspect the reference calibration](reference-calibration.md#inspect-the-reference-calibration) for this procedure.

### Choose normalisation windows by inspection

See [Choose normalisation windows by inspection](normalisation-windows.md#choose-normalisation-windows-by-inspection) for this procedure.

## Related topics

- [Select the prepared detector/stage](scans-and-channels.md#select-each-scans-detector-independently) after Apply without repeating processing.
- [Construct and reuse a model](model-editor.md) on the prepared spectrum, then [run a single fit](fitting-and-results.md).
- [Set up series fitting](series-fitting.md) when acquisition preparation is complete; its bounds/trends are model constraints.
- [Save a session](exporting-fits.md#choose-what-to-save) to retain original/processed channels and a preparation checkpoint.
- [Follow the surrounding acquisition workflow](../examples/beamline-workflow.md).
- [Diagnose stale previews, calibration and scaling errors](troubleshooting.md#preparation-and-calibration).
