# From a scan folder to reviewed fit reports

This GUI **0.2.24** workflow connects importing, preparation, modelling and series
review. Use your own supported native SXR acquisitions; this page does not bundle
experimental data or prescribe an element-specific reference/model. For a first
exercise without beamline files, use the [synthetic first fit](../gui/getting-started.md).

## 1. Load and identify the acquisitions

Use **Data import → Load folder** for native `.asc` acquisitions. Include the
separate photodiode acquisition if the experiment requires one. Review any
[energy-order warning](../gui/import-diagnostics.md#review-before-importing-a-sorted-copy)
explicitly; sorting is a copied fitting view, not a repair for duplicate energies.
Keep successful imports if another file needs investigation.

In [Scans / metadata](../gui/scans-and-channels.md), check filenames, comments,
set-energy ranges, incidence and raw sample positions. Source/PV is the first
metadata column. R1 in the position tuple is the raw manipulator angle; the
identified incidence is R1 − 45° from the surface. Those two values need not match.
Record specimen/measurement observations in scan notes, then save an initial session.

## 2. Inspect channels before processing

Choose each row's Detector independently. You can inspect the photodiode signal
of its acquisition while another scan retains Channeltron or MCP. Use overlays
to check repeated measurements and likely artifacts; their default gradient follows
loaded scan order, not an automatic angular sort.

Overlay boxes select comparison curves. They do not select preparation or fitting
targets. Inspect compatible stages/intensity scales when comparing signals; the GUI
does not comparison-normalise a raw monitor to a prepared detector automatically.

## 3. Preview preparation for the intended sample scans

Open **Preparation → Prepare scans…** and explicitly check the sample acquisitions.
Select a separate photodiode reference when required; loading it into the browser
does not make it an automatic reference or a fitting target.

Choose the [preparation recipe](../gui/preparing-data.md):

- One reference channel and feature/window/target for energy alignment of all
  simultaneously acquired channels.
- I0 normalisation and any separate I0-normalised photodiode correction.
- Baseline window and mean or area scale window, if intensity preparation is needed.

Drag the baseline/scale boundaries and inspect comparison overlays to choose
windows appropriate to the set. Preview performs the selected processing; the
initial control values are not universal scientific recommendations. The optional
[reference inspection panel](../gui/preparing-data.md#inspect-the-reference-calibration)
shows the actual reference/I0 trace, returned feature energy and any local parabola.

Changing settings invalidates Apply eligibility. Run Preview again, inspect target
status and Channel issues, then Apply the reviewed results. Original acquisition
bytes and monitor arrays remain retained. Save a prepared-session checkpoint.

## 4. Select the prepared fitting channel and build a model

Select the intended detector/stage on a representative scan. New GUI models start
without optional edges/background/global corrections. Add the components the data
require, or **Model → Open model** to reuse a saved template without replacing data.

Use [component construction](../gui/model-editor.md), [drag controls and bounds](../gui/drag-controls.md)
and the parameter table to establish plausible seeds and allowed intervals. New
peak height estimates account for current edges/background; A remains integrated
area, not a height. EMG μ is not generally its observed maximum.

If physical assumptions call for them, add [peak constraints or edge-position links](../gui/peak-constraints.md).
Each edge can follow its own peak. Colours/groups aid inspection; sharing and
scale parameters are separate explicit choices. Edge subtraction and component
offsets are [display tools](../gui/component-display.md), not preparation steps.

## 5. Fit and review one spectrum

Set the fitting domain in Fitting, lock it, then **Fit spectrum**. Zoom and peak μ
bounds do not replace the fitting domain. Review residuals, parameter bounds,
fixed/linked states, uncertainties and whether results depend excessively on seeds.

[Fit settings](../gui/fitting-and-results.md#set-the-evaluation-budget-and-tolerances)
control the residual-evaluation budget and stopping tolerances. Increasing a budget
does not resolve a missing feature or an unidentifiable model. Add observations to
scan notes and save the reviewed model/session before propagation.

## 6. Optionally propagate the model or fit the series

Open **Propagate model / fit…** from the representative scan. Review targets on the
source channel/stage, which is a different selection from browser overlays. Choose
independent reference/previous-success fits, simultaneous shared fitting, or model
copying. Copying does not create a fitted result.

Add [reference-relative series bounds and overall trends](../gui/series-fitting.md)
only when justified. Use explicit scan order or increasing numeric coordinates;
review a named maximum/minimum and its membership. Global sharing, soft penalties
and strict constraints express different assumptions.

If using [two passes](../gui/two-pass-fitting.md), stage/review pass one first, then
choose parameter-specific seeds, averaging scans, narrower bounds and sharing for
pass two. A mean seed is not an averaged spectrum or an uncertainty estimate.
Review the intended pass and Apply successful results. Staging alone leaves the
working scans unchanged; failed targets retain their work.

## 7. Save editable work and export the reviewed fits

Save the complete session after applying preparation/series results and writing
notes. Save a model separately if it is a reusable template. Open session restores
the workspace; Open model imports only a model onto the active data.

Use [Fitted curves CSV and Fit parameters CSV](../gui/exporting-fits.md) for the active
fit or selected successful series scans. Check each row's Detector/stage before
export. Reports use retained successful fitted snapshots and full-grid numerical
curves, independently of later trial edits, offsets or edge-subtracted views.
Notes and editable preparation belong in sessions, not these fit CSV reports.

## Related topics

- [Resolve symptoms at any workflow stage](../gui/troubleshooting.md).
- [Inspect metadata, units and angle conventions](../gui/metadata.md).
- [Keep complete sessions and reusable models distinct](../gui/sessions-and-models.md).
- [Practise sharing/trends/two-pass fits on synthetic data](series-walkthrough.md).
- [Interpret scientific parameter conventions](../reference/lineshapes.md).
