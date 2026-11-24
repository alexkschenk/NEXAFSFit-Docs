# Arrange the workspace and use smaller screens

GUI **0.2.28** supports dockable panels, collapsible controls and compact layouts.
Start with [the first-fit guide](getting-started.md), then use this page to find
hidden panels or gain plotting space. These arrangements do not change spectra,
preparation, model parameters or fit limits.

## Choose a layout from View

| Command | Arrangement |
| --- | --- |
| **Compact layout** | Components and Parameters share tabs on the right; secondary controls take less space |
| **Standard layout** | Components on the left, Parameters on the right, and room for six scan rows above on a large window |
| **Plot only** | Hide the three panels and collapse secondary controls, keeping plotting and fitting actions available |
| **Reset layout for this window** | Choose a fresh compact/standard arrangement for the current window size |

Initial sizing respects the available screen in logical pixels and Windows
scaling. Smaller/shorter windows initially use Compact. Its scan browser starts
with two rows; particularly short windows start with it hidden. Larger standard
windows retain six initial rows. Drag dividers to allocate space. Presets are
explicit choices; resizing does not repeatedly undo your manual arrangement.

:::{figure} ../media/images/compact-layout-preview.png
:label: figure-compact-workspace
:alt: Compact workspace with a two-row scan browser, central data plot, Parameters and Components tabs, collapsed display controls and visible fitting actions.

Synthetic 1366 × 768 test area at 100% scaling. Controls wrap instead of widening
the window, and Components/Parameters share the right-hand panel.
:::

## Show, hide, detach or tab panels

**View → Components**, **Parameters** and **Scans / metadata** reopen their panels.
The **Scans / metadata** button above the plot also reopens the browser. Closing a
panel hides it while retaining its data and selection.
Fitting still uses the active scan/channel when its browser is hidden; reopen
the browser to verify that selection before fitting.

- Drag a title bar to another dock area; dragging onto another panel can create tabs.
- Double-click a title bar or use its native float control to detach it.
- The browser's **Detach/Attach** controls remain available on its tab bar.
- Resize floating windows by their edges. Restored windows are constrained to an
  available screen, including after a monitor change.
- Scroll a narrow parameter table horizontally for later columns, or detach it.
  The Parameter column is resizable; hover over cells for full text.

Use **Reset layout for this window** if panels become awkward to recover. Docking
moves the same widgets; it does not create another model or switch channels. See
[scan-browser behaviour](scans-and-channels.md#detach-or-hide-the-browser).

## Expand only the controls you need

Arrow headers expand **Component controls**, **Constraints / corrections**,
**Display options** and **Fit energy range**. Collapsing a section leaves its
values active: it does not disable a fitting domain or turn off edge subtraction.
The headers have a pale-blue fill, bold blue text and an outline even when closed.
Their arrows indicate open/closed state; hover and keyboard focus strengthen the
highlight. Compact **More…** and message **Details** use the same button styling.

The range header reports the limits and lock state; open it to edit them.
**Fit spectrum** and **Propagate model / fit…** stay visible. **F5** also fits the
active spectrum. In Compact, **More…** contains **Fit settings…** and **Last fit
results**; Standard keeps their separate buttons. See [fit controls](fitting-and-results.md).

Messages can collapse to severity and a one-line summary. **Details** expands the
current diagnostic and compatibility notices; **History…** keeps full recent
messages. Errors expand details automatically. This is separate from model Undo.

:::{figure} ../media/images/scaled-layout-preview.png
:label: figure-scaled-workspace
:alt: Workspace at 125 percent scaling with the scan browser hidden, tabbed component and parameter panels, collapsed secondary controls and fit buttons in view.

Synthetic 1366 × 768 test area at 125% scaling, about 1093 × 614 logical client
pixels. The browser starts hidden to preserve the plot; reopen it when choosing scans.
:::

## Preparation and series dialogs

Dialogs initially fit the available screen. Long setup content can scroll while
Apply/Cancel or staging actions remain available. Scroll wide rule tables
horizontally to reach later columns; scrolling does not edit values.

In **Prepare scans**, expand **Scans to prepare** to review target checkboxes; its
header reports the selected count. Settings scroll independently of the plots.
If **Show reference calibration** cannot fit beside the preview, it opens a
**Reference calibration** preview tab. Return to **Raw / prepared preview** for
the signals; Hide removes the optional reference tab. These view actions do not
run processing. Follow [explicit preparation](preparing-data.md#preparation-is-explicit).

In the series dialog, expand **Series setup** for strategy, constraints, trend
order/coordinates and pass-two setup. Targets and review tabs remain outside it.
Review those assumptions before staging; see [series setup](series-fitting.md#set-up-a-first-series-fit).

## Save and restore an arrangement

Sessions and recovery checkpoints retain panel visibility, docking/tabs, floating
geometry and section expansion. These settings apply to the workspace; changing
scan/channel does not switch its arrangement. Models do not carry layout settings.
Older sessions without them keep the current arrangement.

A large-screen standard arrangement adapts when restored into a small window;
secondary sections may collapse and panels may share tabs. Adjust it again through
View. See [sessions and recovery](sessions-and-models.md).

Layout checks used synthetic data and offscreen Qt at 100%, 125% and 150% scaling.
Available desktop space also depends on taskbars, borders, fonts and monitors;
these are simulated checks, rather than hands-on validation of every display.

## Related topics

- [Find controls and fit a first spectrum](getting-started.md).
- [Choose detectors and manage the browser](scans-and-channels.md).
- [Change display settings without changing fits](component-display.md).
- [Save sessions and recover work](sessions-and-models.md).
