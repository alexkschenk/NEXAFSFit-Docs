# Your first spectrum fit

:::{note} Current workflow
These instructions describe GUI **0.2.27**. The basic workflow is implemented;
button placement may receive minor refinements in later versions.
:::

This tutorial fits a supplied synthetic spectrum containing two Gaussian peaks
and a small linear background. It needs no experimental files, photodiode scan or
preprocessing.

## Launch from the source checkout

In Anaconda Prompt, change to the repository folder and run:

```bat
conda activate nexafs-fit
python -m nexafsfit_gui
```

Alternatively, run the repository's `run_gui.ps1` launcher from PowerShell.
For a new development installation, the repository README describes installing
the project into this environment. A standalone Windows installer/executable is
**planned**; these instructions currently assume the source checkout.

Reading the HTML manual requires only a web browser.

## Find the controls

The menu bar groups **Data import**, **Preparation**, **Model**, **Session**,
**History**, **Export** and **View** alongside **Help**. Hover over buttons for short
descriptions; detailed parameter conventions are in the [model editor](model-editor.md).
For the handles themselves, use [Dragging components and setting peak bounds](drag-controls.md).
Use [workspace layouts](layout-and-small-screens.md) to show hidden panels, choose
Compact/Standard/Plot only, or detach the component and parameter panels.

| Area | What it controls |
| --- | --- |
| Scans / Metadata browser | Active scan, each row's Detector channel, overlays and retained metadata; the browser can be detached or hidden |
| Component list and Component controls | Peak/edge selection, addition, removal, grouping and component context actions |
| Parameter table and Constraints / corrections | Values, bounds, Free/Sharing states, links and optional global corrections |
| Display options below the plot | Expand its header for Focus Mode, edge subtraction, fit-window/bound visibility and offsets |
| Fitting box | Expand the range header for limits/lock; Fit spectrum (F5), propagation, and More… in Compact for settings/results |
| Messages | Severity-labelled outcomes, expandable Details and History… for full recent messages |

The **History** menu supplies model Undo/Redo (Ctrl+Z/Ctrl+Y). The **History…**
button inside Messages instead opens recent feedback/errors. Messages can be
selected and copied; the history keeps up to 100 entries, omitting consecutive
duplicates, and is not saved in sessions. Ctrl+S saves a session.

:::{figure} ../media/images/workspace-cleanup-preview.png
:label: figure-fitted-csv-controls
:alt: NEXAFSFit workspace with command menus, scan browser, component list, parameter table, a blue Fit spectrum button beside series propagation and a coloured Messages panel.

The main controls illustrated with a synthetic fit containing four peaks and optional continuum terms. Display
settings affect the view; the selected scan/channel supplies the single-spectrum fit.
:::

## Import the example

1. Choose **Data import → Import**.
2. Open [two-peak-spectrum.csv](../examples/data/two-peak-spectrum.csv) under
   `docs/book/examples/data/` in the checkout.
3. Choose a comma delimiter, skip **1** header row, and select energy column **1**
   and intensity column **2**. Check the numerical preview before accepting.
4. The new scan becomes active in the scan browser.

With an empty model, the plot shows the data without a Total envelope or residual
panel. They appear when suitable components are added or loaded; see
[components and corrections](model-editor.md#components-and-optional-corrections).

The CSV is already suitable for fitting. Leave preparation off. For raw beamline
scans, use [importing and preparing data](preparing-data.md) instead.

## Load a starting model

Choose **Model → Open model** and open
[two-peak-model.json](../examples/data/two-peak-model.json) from the same folder.

This imports a model onto the loaded data. It does not replace the spectrum.
The model contains peaks **P1** and **P2**, an adjustable background level and a
fixed background slope. It deliberately starts away from the known solution.

Select P1 or P2 in the component list. The parameter table shows starting values,
bounds and **Free** checkboxes. The initial envelope should already resemble the
data, making the subsequent optimisation easy to inspect.

:::{tip} Build it yourself later
A new model starts without peaks, edges, background, gain or energy shift.
Use **Component controls** to add peaks and edges, and **Global corrections…**
to add a background or optional global terms. The [model editor guide](model-editor.md)
explains the controls.
:::

## Set the domain and fit

1. In **Fitting**, clear **Lock fit range** if necessary.
2. Set the limits to **281–292 eV**, covering this example's full data grid.
3. Lock the range again to avoid moving it accidentally.
4. Click the blue **Fit spectrum** button.
5. Read the outcome in **Messages**, then open **Last fit results**.

The main fit uses only the active spectrum's selected channel. Check the residual
plot as well as the envelope: the remaining deviations should resemble small
noise rather than a missed spectral feature.

The example's known parameters are approximately:

| Component | Area `A` | Position `mu` (eV) | Gaussian `sigma` (eV) |
| --- | ---: | ---: | ---: |
| P1 | 1.60 | 285.00 | 0.32 |
| P2 | 0.95 | 287.20 | 0.55 |

Noise means recovered values will not be exactly these numbers. These areas are
integrated areas, not peak heights.

## Save and export

- **Session → Save session** saves the loaded spectrum, model and retained fit.
- **Model → Save model** saves a reusable model without the acquired data.
- **Export → Fitted curves CSV…** saves the measured data, envelope, residual and
  component curves from the last successful fit.
- **Export → Fit parameters CSV…** saves values, bounds, uncertainties and states.

The [session/model guide](sessions-and-models.md) explains editable checkpoints.
The [export guide](exporting-fits.md) explains active/series selection and the
difference between saved fit results and later working-model edits.

Continue with the [three-scan GUI exercise](../examples/series-walkthrough.md)
to practise sharing, trends and a second fitting pass.

## If a fit fails

Use **Fit settings…** to inspect the evaluation budget and tolerances. The default
budget is 50,000 residual evaluations, not 50,000 optimiser iterations. First check
the model, selected channel, fitting range, bounds and Free states. Increasing a
budget cannot repair a missing component or an unsuitable constraint.

Errors appear in Messages and its **History…** button. A failed refit retains the previous
successful result; it does not turn the current trial into a successful fit.

For fit-domain controls, tolerance meanings and diagnostic checks, continue with
[running a fit and reviewing the result](fitting-and-results.md). To compare
channels without changing the active fit, use [scan management](scans-and-channels.md).

## Help and hover descriptions

Hover over buttons and menu commands for short tooltips. **Help → Documentation**
opens local help (F1); **Help → About…** reports the application version and
contributors. **Help → Set documentation file…** lets you select a local Markdown
or HTML help file. Keep the version with an error report, and use the Messages panel's
History… for its full text. Complete sessions preserve data/model/results for
later inspection; temporary message and model Undo histories are not persisted.

## Related topics

- [Choose channels and inspect your own scans](scans-and-channels.md).
- [Understand area, widths and energy conventions](../reference/lineshapes.md).
- [Try a shared fit on three known spectra](../examples/series-walkthrough.md).
- [Follow the full acquisition-folder workflow](../examples/beamline-workflow.md).
- [Resolve common problems or disabled controls](troubleshooting.md).
