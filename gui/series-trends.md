# Set overall series trends and scan order

GUI **0.2.30**. Open **Trends / pass-two setup… → Overall trends** from [series fitting](series-fitting.md). Coupled trend rules require simultaneous fitting.

(trends-and-ordering)=

## Trends and ordering

**Trends / pass-two setup…** supports soft penalties or strict constraints for
monotonic evolution, a straight-line trend, smoothness, endpoints, maximum
step/slope or curvature, and an identified scan as a maximum/minimum. A chosen
extremum can also have monotonic sides. These rules couple scans and require a
simultaneous fit.

Choose equal scan order or explicitly enter increasing numeric coordinates such
as time or dose. Coordinates define spacing for derivative limits; their units
therefore affect the meaning of a maximum slope/curvature. Scan order is an
explicit input, not an inferred acquisition chronology.

:::{important} Soft preferences and strict constraints differ
A soft penalty encourages a trend. It does not guarantee it. Strict constraints
use a separate constrained solver; see [constraints and statistics](../reference/constraints.md)
for the distinction and its uncertainty limitations.
:::

### Choose what a trend means

Open **Trends / pass-two setup… → Overall trends**, add a rule and select an active
independent **per_spectrum** parameter. A global parameter already has one value
across the series. For a linked target, select its controlling source/separation.
Choose whether the rules apply to pass one, pass two or both.

:::{figure} ../media/images/series-trends-controls.png
:label: figure-series-trends-controls
:alt: Overall trends tab showing a soft decreasing-area rule and a strict maximum at the middle acquisition with monotonic sides, applied to both passes.

Two example trend rules: a soft decrease and a strict maximum anchored to the
second acquisition. Strength, target scan, shape and pass selection are explicit
choices. This setup illustrates the controls rather than a recommended model.
:::

| Rule | Soft strength | Strict strength |
| --- | --- | --- |
| Increasing / decreasing | Penalise adjacent violations beyond fractional tolerance | Non-decreasing / non-increasing |
| Maximum / minimum at selected scan | Encourage the named acquisition to dominate the others | Named acquisition at least / at most every other target |
| Smooth curvature | Penalise curvature, optionally in log space | Exact straight line |
| Small steps | Penalise adjacent changes, optionally in log space | Constant value |
| Final / initial ratio | Encourage the chosen ratio | Exact ratio |
| Final value | Encourage the chosen endpoint | Exact endpoint |
| Maximum absolute slope | Penalise excess above the chosen maximum | Bound absolute slope |
| Maximum absolute curvature | Penalise excess above the chosen maximum | Bound absolute curvature |

#### Allow a nonzero step

In particular, strict “small steps” means **zero** step. Use maximum absolute
slope for a nonzero allowed step limit. Equal scan order gives slope units per
scan; numeric time/dose gives units per time/dose. A maximum curvature is a
second-derivative restriction, not an energy-width bound.

#### Read the soft-rule controls

Soft **Weight** is lambda: penalty residuals scale by its square root. **Floor**
protects relative/log calculations. **Fraction tolerance** is a fraction (0.02
means 2%), not an entered percentage; it is distinct from the percentage-tolerance
columns in Series constraints. Target is the endpoint, ratio or nonnegative maximum
for rules that use it. Inactive cells are retained but ignored.

### Pin a maximum or minimum to an acquisition

Choose the named scan in **Extremum scan**; it must also be checked as a target.
**Extremum only** compares it with all other selected targets. **Monotonic sides**
additionally imposes rise/fall for a maximum or fall/rise for a minimum in the chosen
order. Ties/plateaus are allowed, and an endpoint can be the extremum. The anchor
follows the acquisition when rows move; it is not silently changed to the largest
fitted value or implicitly checked as a target.

For a middle scan expected to have the largest P2 area, choose P2_A, Maximum at
selected scan, that middle acquisition and the appropriate strength. Choose
Monotonic sides only if intermediate scans should also follow that shape. This
does not constrain any later coverage or kinetic calculation.

### Order, coordinates and incompatible constraints

Use **Move scan up/down** to set the order explicitly. Numeric coordinates must
increase strictly for selected targets; equal/reversed values are rejected instead
of sorted. Coordinate units are chosen by the user. A limit of 0.1 area per minute
is different from 0.1 area per second. Soft nonuniform smoothness normalises mean
spacing; maximum derivative limits always use the actual coordinate units.

#### Check feasibility and solver output

Infeasible strict rules/bounds or a nonconverged joint fit produce no successful
joint stage. Strict constraints are not huge penalties and currently have no
covariance/stderr estimate; only budget/ftol apply. Soft-only fits use least squares
with all three tolerances. Joint statistics can include penalty residuals; inspect
data agreement separately. See [fitting and results](fitting-and-results.md).

## Related topics

- [Set reference-relative parameter limits](series-constraints.md).
- [Configure a second pass](two-pass-fitting.md).
- [Review fitted parameter trends](series-review.md).
- [Check derivative units and constraints](../reference/constraints.md#derivative-limits-and-reseeding-windows).
