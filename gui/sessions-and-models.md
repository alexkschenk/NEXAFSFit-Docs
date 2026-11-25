# Save and reopen complete sessions

These instructions describe GUI **0.2.30**. A session is a checkpoint of your
collection; a model is a reusable fitting template. Numerical CSV reports are
described separately in [exporting fitted results](exporting-fits.md).

## Save and reopen a session

1. Finish or close active modal workflows. Apply any staged preparation or series
   results that you want to retain; an uncommitted dialog stage is not saved.
2. Choose **Session → Save session** (Ctrl+S) and select a JSON filename.
3. To resume it, choose **Session → Open session** and select that session JSON.
   Respond to any current-work save/discard prompt deliberately.

:::{important} Opening a session restores a workspace
**Open session** replaces the current collection with the saved workspace;
it does not add one scan. To reuse a fitting template on loaded data, use
**Open model** instead.
:::

### What a session retains

Complete sessions retain the loaded spectra, native acquisition dictionaries and
source bytes, separate PD references, prepared/intermediate channels, processing
settings, active selection, per-channel models and successful fit results. They
also retain fit settings and saved browser/display choices. Native acquisitions
are embedded so reopening is not dependent on their original file paths remaining
available. Plain text/CSV imports retain their selected arrays/basic import settings,
not a complete native acquisition they never contained.

Complete sessions also retain each acquisition's [editable notes](scan-notes.md#attach-notes-to-a-scan),
independently of its selected detector and original comment. Older sessions without
notes reopen with empty notes. Save the session after editing; a model-only file
or fit CSV report does not preserve these annotations.

Model Undo/Redo histories and temporary Messages history are not persisted across
restarts. Saving records the current working model and retained successful results;
later model edits can mean those two models differ. [Last fit results and CSV export](exporting-fits.md)
refer to the retained successful snapshot, not an unrun trial.

GUI 0.2.30 sessions also retain [workspace arrangements](layout-and-small-screens.md#save-and-restore-an-arrangement):
panel visibility, docking/tabs, floating geometry and section expansion. These
are separate from per-channel model/display values. Large-screen standard layouts
adapt on smaller windows, and off-screen floating geometry is constrained.

## Undo, checkpoints and collection changes

The **History** menu's Undo/Redo acts on the active scan/channel's model edits,
including completed drags, constraints, imported models and successful fitted
values. It is distinct from the Messages panel's **History…** record of feedback.
Switching channels restores their independent model work rather than sharing one
global Undo stack across all datasets.

Preparation and collection operations are not one collection-wide model Undo.
Save a checkpoint before replacing prepared signals or applying a large series
workflow. Preparation invalidates fits against the affected replacement data;
unavailable cached channels may be labelled previous preparation. Review the
[channel/stage guide](scans-and-channels.md) when returning to older work.

Accepted series recipes and applied result provenance can be stored in sessions,
including selected pass-two seeds/windows and first-pass provenance. Merely
staging a fit in an open dialog is not a persistent checkpoint. Apply/review the
intended pass before saving; see [two-pass result review](two-pass-fitting.md#stage-select-and-apply-the-intended-pass).

## Loading problems and compatibility notices

The application validates saved files before replacement. An unsupported version
or invalid model/session reports an error while retaining current working state.
Compatibility notices can identify preserved but inactive fields, such as unused
EMG gamma. Those fields are not evidence that a new physical parameter is active.

Do not remove fields or edit recorded results merely to suppress a warning. Keep
the original file, record the exact error and application version, and distinguish
**Open model** from **Open session** when reporting what you were trying to restore.
Preserved source records and supported migrations are handled by the session loader;
this page is not an instruction to execute content from JSON files.

## Related procedures

### Recover interrupted work

See [Recover interrupted work](recovery.md#recover-interrupted-work) for this procedure.

### Save a reusable model

See [Save a reusable model](model-files.md#save-a-reusable-model) for this procedure.

## Related topics

- [Select the scan/channel work to retain](scans-and-channels.md).
- [Understand retained metadata and incidence mappings](metadata.md).
- [Review staged series results before applying them](series-fitting.md#review-staged-results-before-applying).
- [Export numerical curves and parameters](exporting-fits.md).
- [Find out which stage or selection produced an unexpected result](troubleshooting.md).
