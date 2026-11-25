# Fit statistics and uncertainty

GUI **0.2.30**. These definitions support [result review](../gui/fit-results.md)
and [series review](../gui/series-review.md).

(read-the-residual-before-trusting-a-number)=

## Read the residual before trusting a number

The data residual is **measured intensity minus model**. Unweighted chi-square
is the sum of squared residuals over the selected fit domain. Its numerical scale
depends on the intensity units/preparation. A smaller chi-square alone does not
prove that a more complex model is scientifically preferable.

**Last fit results** reports evaluations, chi-square, reduced chi-square and
information criteria. Check systematic residual structure, values at bounds,
sensitivity to the fit domain and whether parameters are identifiable. The current
single-spectrum GUI does not provide a general uncertainty-weight editor.

:::{warning} A standard error is a limited uncertainty estimate
A reported standard error is a local covariance-based estimate when available.
It is not a complete assessment of calibration error, preprocessing choices,
model inadequacy or correlated parameters. An unavailable error is blank in CSV,
not zero. A bound-hit indicator is a prompt to inspect the chosen limits.
:::

### Joint statistics and per-scan misfit

For simultaneous fits, joint statistics describe the entire optimisation;
per-scan chi-square and point counts are reported separately. Soft penalty terms
can contribute to the joint objective. Do not interpret that total as only a
single scan's spectral misfit.

### Strict-trend uncertainty limitations

Strict-trend fits currently have no covariance/stderr estimate. Their reduced
chi-square and information criteria are approximate because active inequalities
are not subtracted from the degrees of freedom. Only ftol and the evaluation
budget apply to this path; xtol/gtol are inactive.

## Related topics

- [Review one fitted spectrum](../gui/fit-results.md).
- [Compare series results](../gui/series-review.md).
- [Distinguish bounds, links and sharing](constraints.md).
