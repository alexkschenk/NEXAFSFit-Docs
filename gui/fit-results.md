# Review a single-spectrum fit

GUI **0.2.30**. After [running a fit](fitting-and-results.md), open **Last fit results** (under **More…** in Compact). Use this page to assess the result before exporting or propagation.

(read-more-than-the-convergence-flag)=

## Read more than the convergence flag

Open **Last fit results** and inspect the Messages panel. Review:

- Residual structure: data minus model should not show unresolved systematic features.
- Parameters at/near bounds, implausible widths, or large compensating areas/gain.
- Whether fixed states, links, shared parameters and fit domain match your intent.
- Sensitivity to reasonable starting values, windows and component choices.
- Uncertainties, remembering that missing stderr is unavailable rather than zero.

:::{figure} ../media/images/last-fit-results-controls.png
:label: figure-last-fit-results-controls
:alt: Last successful fit report showing convergence message, evaluation count, statistics and a scrollable table of parameter values, standard errors and bound indicators.

An actual fit report from a deterministic synthetic example. This example varies
only one peak area; many other rows are fixed or derived. Scroll to inspect all
parameters. A fixed row's displayed zero is not an experimental uncertainty.
:::

:::{warning} Convergence does not validate a physical model
Chi-square depends on signal units and preparation. A smaller value after adding
parameters is not by itself evidence for a better physical interpretation.
:::

Joint series statistics describe all fitted scans and may include soft penalties;
per-scan misfit is a separate report. Strict trends currently have no covariance
uncertainties. Definitions and limitations are in the
[shared statistics reference](../reference/fit-statistics.md#read-the-residual-before-trusting-a-number).

## Retained fit versus working edits

Later parameter edits change the working model. CSV reports use the retained
successful fitted snapshot; see [export scope](exporting-fits.md). A failed or
stopped refit does not replace the earlier successful result.

## Related topics

- [Change numerical fit settings](fit-settings.md).
- [Review series residuals and parameter trends](series-review.md).
- [Interpret statistics and uncertainty](../reference/fit-statistics.md).
