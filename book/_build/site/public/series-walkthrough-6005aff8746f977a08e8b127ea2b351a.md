# Worked example: shared shapes and a two-pass series

This GUI **0.2.24** exercise uses three synthetic spectra. It demonstrates
propagation, sharing, named-scan trends and mean-based pass-two seeds without
assigning experimental meaning to any component. Start with an unconstrained
simultaneous fit, then explore explicit trends and a second pass below.

## Data and starting model

| File | Role |
| --- | --- |
| [series-01.csv](data/series-01.csv) | First scan; P2 generating area 0.45 |
| [series-02.csv](data/series-02.csv) | Middle scan; P2 generating area 0.95 |
| [series-03.csv](data/series-03.csv) | Final scan; P2 generating area 0.60 |
| [series-model.json](data/series-model.json) | Shared-position/width Gaussian model, independent areas and fixed background |
| [series-truth.json](data/series-truth.json) | Generating values, noise seed and illustrative coordinates |

Each CSV has 221 points from 281–292 eV, a one-row header, energy column 1 and
intensity column 2. P1 has area 1.60, μ 285.00 eV and sigma 0.32 eV in every scan;
P2 has μ 287.20 eV and sigma 0.55 eV. Noise sigma is 0.003 with seed 20261124.
The background is fixed at b0 = 0.04 and b1 = 0.002. There is no edge, gain, model
dE, raw monitor or photodiode correction. Group names bulk/surface are labels only.

## Fit the representative scan

1. Import each CSV individually through **Data import → Import**. Select comma,
   one skipped header row and columns 1/2. Load folder is for native `.asc` files.
2. Activate series-02, then **Model → Open model** and choose series-model.json.
3. Set the fitting range to 281–292 eV and **Fit spectrum**.
4. Inspect the residual and parameters. P1/P2 μ and sigma should be near the
   values above; fitted areas should be near 1.60 and 0.95.

Positions/widths have global Sharing in the template; areas are per_spectrum.
That scope matters in simultaneous fitting, not in the representative fit alone.
No preparation is needed for these already fit-ready CSVs.

## Stage a simultaneous first pass

Open **Propagate model / fit…**, check all three scans in file order, choose the
simultaneous strategy and use the source fitting range. Initially leave series
tolerance/trend rules empty. **Stage pass one / single pass** and inspect all rows.

The positions/widths are common while each scan's two areas can differ. P2's
largest fitted area should belong to series-02 because its data contain that
larger contribution. This stage alone does not impose a maximum.

## Explicitly name the maximum

For an optional constrained variant, open **Trends / pass-two setup… → Overall
trends**, add P2_A and choose **Maximum at selected scan**, series-02, **hard**
strength and **Extremum only**. Choose application to both passes for this exercise.
This changes first-pass rules, so stage pass one again.

To inspect coordinate-based limits, choose numeric coordinates in the collection
dialog and enter **0, 10, 20** in that explicit order. Those are arbitrary example
units; they are not measured times or doses. An extremum-only rule does not use
their spacing. Optional maximum-slope rules do: 0.1 area/unit across these intervals
allows area differences of 1.0, whereas a different coordinate unit changes that limit.
Changing coordinates/order invalidates staged passes; stage pass one again before
configuring pass two.

For longer series choose Monotonic sides only when all intervening values should
rise/fall on their respective sides. Strict inequalities permit ties; a soft
maximum encourages rather than guarantees the relationship.

## Configure averaged second-pass seeds

After every target has a successful pass one, open **Pass-two reseeding / bounds**.
Use the bulk group shortcut to select P1 parameters, then configure and inspect:

| Parameter | Seed / averaging pool | Bounds | Sharing |
| --- | --- | --- | --- |
| P1_mu | Mean of all three successful first-pass scans | Absolute ±0.03 eV | Per spectrum |
| P1_sigma | Mean of those same scans | Percentage ±5% | Per spectrum |
| P1_A | Leave Use unchecked | Keep its own pass-one value/bounds | Keep |

Set averaging pools explicitly and use **Review pass-two starting values and
effective bounds…**. The first pass's shared P1 shape values are identical, so
their means equal that common value. Pass two now releases them to small per-scan
adjustments inside the stated windows. This demonstrates sharing promotion; it
does not claim averaging three identical shared values adds independent information.

Apply setup, choose simultaneous Pass two and **Stage pass two**. Keep any enabled
P2_A maximum rule on the intended pass. Compare residuals, bound hits and the
selected pass before **Apply successful results**. The first pass remains available
if the second fails. See [the complete two-pass guide](../gui/two-pass-fitting.md).

## Save and compare

After applying the chosen pass, **Save session** to retain the collection and
recipe/provenance. Export curves/parameters for the selected three fits. Check
scan/channel identity, P2 areas and whether P1 positions/widths stayed inside their
pass-two windows. Joint statistics and per-scan misfit have different scopes;
strict-constrained stderr is unavailable.

This example validates software behaviour on known synthetic signals. It does
not establish appropriate constraints, component assignments or error bars for
an experimental series. No downstream coverage or kinetic analysis is included.

## Related topics

- [Choose a propagation strategy and reference-relative bounds](../gui/series-fitting.md).
- [Review all two-pass seed and averaging-pool choices](../gui/two-pass-fitting.md).
- [Save a checkpoint of the collection and its applied results](../gui/sessions-and-models.md).
- [Export and interpret the retained series reports](../gui/exporting-fits.md).
- [Apply the same review checkpoints to native acquisitions](beamline-workflow.md).
