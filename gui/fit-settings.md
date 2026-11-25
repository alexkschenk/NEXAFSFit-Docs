# Set the fitting budget and tolerances

GUI **0.2.30**. Open **Fit settings…** before a [single-spectrum](fitting-and-results.md) or [series fit](series-fitting.md). These numerical controls do not choose a model or domain.

(set-the-evaluation-budget-and-tolerances)=

## Set the evaluation budget and tolerances

Open **Fit settings…** in the Fitting box (under **More…** in Compact).
The three default tolerances are **1e-8**.

:::{important} The budget counts evaluations
The default maximum is **50,000 residual evaluations**, including
numerical-derivative calls; it is not an iteration count.
:::

| Control | Ordinary least-squares stopping test |
| --- | --- |
| ftol | Small relative improvement in cost |
| xtol | Small parameter-step change |
| gtol | Small bounds-scaled gradient |

Keep one common tolerance or uncheck **Use one convergence tolerance for all three
tests** to edit them separately. Smaller values demand tighter convergence;
larger values permit earlier stopping. Settings persist in sessions and are used
for explicit collection workflows too. Strict series trends use a different solver:
only their budget and ftol apply; xtol/gtol are inactive.
Use **Restore Defaults** to reset the dialog's controls. Click **OK** to
use the edited values; cancelling leaves the existing settings intact. Successful
fit records keep the settings used for that fit, independently of later edits.

:::{figure} ../media/images/fit-settings-preview.png
:label: figure-fit-settings
:alt: Fit settings dialog with maximum evaluations and common or separate ftol, xtol and gtol controls.

Evaluation budget and convergence settings; tighter tolerances do not establish
that a model is physically appropriate.
:::

(when-the-budget-is-exhausted)=

## When the budget is exhausted

Check the active channel/stage, fit range, model shape, parameter bounds and
links first. A missing edge or near-degenerate set of peaks can consume a much
larger budget without becoming identifiable. Use graphical positioning or a
simpler model to improve the starting point. Then increase maximum evaluations
if more work is justified, or adjust tolerances deliberately.

The Messages panel's **History…** button contains the full recent diagnostic
text. It is separate from the menu's model Undo/Redo history. Save a session
to retain successful work before major preparation/model changes, and use
[CSV exports](exporting-fits.md) for the retained numerical reports.

## Related topics

- [Run or stop one fit](fitting-and-results.md).
- [Review a fitted result](fit-results.md).
- [Understand solver/statistics limitations](../reference/fit-statistics.md).
