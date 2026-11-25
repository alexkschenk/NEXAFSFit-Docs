# Bounds, links, sharing and fit statistics

## Four different parameter states

| State or rule | What it does |
| --- | --- |
| Free parameter with Min/Max | The optimiser can choose a value inside hard bounds |
| Fixed parameter | Held at its value; use the Free checkbox, not equal Min/Max |
| Expression-linked target | Calculated from other parameters through a supported relationship |
| Static specification | Used by the shape but not included as an optimised parameter, such as Lorentzian/Voigt gamma |

The initial value seeds an optimisation. It is not a constraint on its final
value unless the parameter is fixed or tightly bounded.
Set independent limits with the [table](../gui/parameter-table.md#values-bounds-free-and-sharing)
or [μ-bound handles](../gui/drag-controls.md#set-position-bounds-by-dragging-lines).

For graphical arrangement, [dragging an independent peak position](../gui/drag-controls.md#position-drags-carry-mu-interval)
translates that peak's interval while preserving its offsets from μ. The resulting
Min/Max remain hard limits during optimisation. Fixed states and parameter links
stay enforced, and numerical value edits do not translate limits.

### Relative peak links

In the GUI, **Peak constraints…** supports a target equal to a reference plus a
fixed or bounded fitted separation. For example, constrain P2 mu relative to P1 mu
with a separation between 1.8 and 2.2 eV. The separation has its own value/bounds;
the target is derived. Edit the source/separation, rather than the target's
inactive independent bounds.
For the dialog sequence, equality examples and removal behaviour, see
[linking peaks and edges](../gui/peak-constraints.md).

### Exact edge-position links

**Link/Unlink edge to μ…** sets any edge's position relative to a chosen peak.
Each edge can have its own exact offset/source. Linked position scope follows the
controlling peak in simultaneous fitting; these offsets do not add free parameters.
Unlink makes that edge independent again. Relationship cycles and incompatible
references are rejected. Loaded model files cannot introduce arbitrary Python or
arbitrary expression text for execution.

## Sharing across spectra

Sharing concerns a simultaneous series fit:

- `global`: one parameter value across the chosen spectra.
- `per_spectrum`: an independent value for each spectrum.

:::{important} Sharing applies to simultaneous fitting
Independent propagation strategies fit independent parameter sets even if their
models contain global sharing labels. A group name such as “surface” does not
change this rule. Linked target scopes follow their reference/separation
dependencies, so some target Sharing fields are inferred rather than editable.
:::

:::{seealso} Configure sharing and reseeding in the GUI
The GUI's [series introduction](../gui/series-fitting.md) explains reference-relative
limits and two-pass reseeding. These are separate choices from sharing.
:::

## Reference-relative and pass-two windows

Reference-relative and pass-two windows restrict allowed parameter intervals. With seed $p_0$,
absolute tolerances intersect $[p_0-b,p_0+a]$ with the existing bounds. Percentage
tolerances replace $b,a$ by the entered percentages of $|p_0|$. Mean reseeding is
an unweighted arithmetic mean over selected successful fits, not an uncertainty
estimate. See the [two-pass procedure](../gui/two-pass-fitting.md).

## Soft trends and strict constraints

### Soft penalties

A soft trend adds penalty residuals to the data residual vector. Its strength and
tolerance control a preference: a finite penalty does not mathematically guarantee
monotonicity, a chosen endpoint or a maximum step.

### Strict equalities and inequalities

Strict trends impose inequalities/equalities through a constrained optimiser.
In this GUI they use an adapter-side SLSQP path; ordinary unconstrained-by-trends
fits use the existing least-squares/TRF path. Strict trends are not a new native
lineshape or a modification of the measured spectra.

Trend rules couple selected spectra. Scan order or explicit coordinates define
their spacing. Nonuniform time/dose coordinates change derivative limits and must
be interpreted in the chosen units. An identified maximum/minimum refers to a
particular scan, not simply to whichever scan currently has the largest fitted value.

(derivative-limits-and-reseeding-windows)=

## Derivative-limit units

For ordered per-scan values $p_i$ and strictly increasing coordinates $x_i$, the
maximum-slope rule uses $s_i=(p_{i+1}-p_i)/(x_{i+1}-x_i)$. The maximum-curvature
rule uses $c_i=2(s_{i+1}-s_i)/(x_{i+2}-x_i)$. Strict rules bound their absolute
values; soft versions penalise excess over the chosen nonnegative maximum.
With equal-order spacing, the units are parameter/scan and parameter/scan².
Changing time/dose units changes numerical derivative limits.

Strict increasing/decreasing and extrema allow ties. A straight-line constraint
means zero curvature; constant-value means zero adjacent change. Neither means
"small but nonzero": use explicit derivative maxima for that. Soft log smoothing
does not imply log-space strict inequalities; strict trends use linear values.

## Read the residual before trusting a number

See [fit statistics and uncertainty](fit-statistics.md) for residuals, joint
objectives, covariance errors and strict-solver limitations.

## Optimisation controls

Use [fitting settings](../gui/fit-settings.md) for evaluation budgets and
tolerances, and [fit statistics](fit-statistics.md) for solver-specific limits.

## Related topics

- [Drag independent position bounds](../gui/drag-controls.md#set-position-bounds-by-dragging-lines).
- [Configure peak and edge relationships](../gui/peak-constraints.md).
- [Choose series constraints and soft/strict trends](../gui/series-fitting.md).
- [Configure averaged pass-two seeds and scope changes](../gui/two-pass-fitting.md).
- [Inspect optimiser outcomes and fit settings](../gui/fitting-and-results.md).
