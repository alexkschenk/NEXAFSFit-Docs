# Worked examples and introductory data

Choose a known synthetic dataset to practise the GUI. The examples use
distributable files and recorded generating truth; they are not experimental
component assignments.
For your own native acquisitions, the [folder-to-results workflow](beamline-workflow.md)
connects the same controls without prescribing an experimental model.

## Choose an example

| Goal | Instructions |
| --- | --- |
| Fit one spectrum and learn the model vocabulary | [First-fit tutorial](../gui/getting-started.md); files described below |
| Fit shared shapes across scans, then explore trends/reseeding | [Series walkthrough and its files](series-walkthrough.md) |
| Connect import, preparation, a representative fit and selected-series reports | [Native acquisition workflow](beamline-workflow.md), using your own supported files |

## Introductory two-peak dataset

A small, reproducible example is supplied for learning the GUI. It
contains two Gaussian peaks, a small linear background and seeded random noise.
There are no experimental assignments, reference-foil features or raw-monitor
channels in these data.

### Files

All three files are under `docs/book/examples/data/` in the source checkout.

| File | Contents |
| --- | --- |
| [two-peak-spectrum.csv](data/two-peak-spectrum.csv) | 221 energy/intensity pairs, with one CSV header row |
| [two-peak-model.json](data/two-peak-model.json) | Versioned starting model that the GUI can open onto the CSV |
| [two-peak-truth.json](data/two-peak-truth.json) | Known generating values, random seed and noise level |

The energy range is 281–292 eV in 0.05 eV steps. Intensity is in arbitrary units.
Noise is independent Gaussian noise with standard deviation 0.006 and generator
seed 20261123.

| Quantity | Generating value |
| --- | ---: |
| P1 area / mu / sigma | 1.60 / 285.00 eV / 0.32 eV |
| P2 area / mu / sigma | 0.95 / 287.20 eV / 0.55 eV |
| Background level at the full-grid mean | 0.04 |
| Background slope | 0.002 intensity units/eV |

The supplied seed deliberately differs from the generating model. Peak areas,
positions, widths and background level are free; background slope is fixed.
No edge, gain or dE term is needed.

(use-either-interface)=
### Use the example in the GUI

Follow [Your first spectrum fit](../gui/getting-started.md): import the CSV,
open the model JSON, fit the full domain and inspect the residual. Save a session
and export the fitted curves/parameters.

### Things to try

- Move a starting peak slightly, refit and compare the recovered values.
- Change a bound enough to exclude the true position. Inspect the residual and
  at-bound indication rather than accepting convergence alone.
- Fix one Gaussian width, then inspect how that affects area and residuals.
- Narrow the fitting domain while keeping the full-grid background reference.
- Compare a saved successful CSV fit report with later edits to the working model.

These exercises change modelling assumptions. They are not evidence that extra
components or narrow bounds improve a scientific interpretation.

## Further examples

Continue with the [shared-parameter series walkthrough](series-walkthrough.md):
three synthetic scans, an explicit maximum and mean-based pass-two windows.
Both examples include data/model/truth and recovery checks.

The [native acquisition guide](beamline-workflow.md) supplies a complete GUI route,
with links to canonical procedures. An executed raw-SXR numerical notebook and a
validated experimental two-pass dataset are separate future documentation work;
they are not required to use the implemented GUI controls. Experimental files and
videos need established provenance and distribution permission.

## Related topics

- [Follow the first GUI fit step by step](../gui/getting-started.md).
- [Understand the definitions of fitted areas and widths](../reference/lineshapes.md).
- [Continue with the three-scan series exercise](series-walkthrough.md).
- [Apply the workflow to your own acquisition folder](beamline-workflow.md).
- [Investigate unexpected results at a particular stage](../gui/troubleshooting.md).
- [Interpret residuals and uncertainty before comparing fits](../gui/fitting-and-results.md).
