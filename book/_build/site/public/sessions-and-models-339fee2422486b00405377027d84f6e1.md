# Saving sessions and reusing models

These instructions describe GUI **0.2.25**. A session is a checkpoint of your
collection; a model is a reusable fitting template. Numerical CSV reports are
described separately in [exporting fitted results](exporting-fits.md).

## Save and reopen a session

1. Finish or close active modal workflows. Apply any staged preparation or series
   results that you want to retain; an uncommitted dialog stage is not saved.
2. Choose **Session → Save session** (Ctrl+S) and select a JSON filename.
3. To resume it, choose **Session → Open session** and select that session JSON.
   Opening a session restores its saved workspace rather than adding a scan to
   the existing collection; respond to any current-work save/discard prompt deliberately.

Complete sessions retain the loaded spectra, native acquisition dictionaries and
source bytes, separate PD references, prepared/intermediate channels, processing
settings, active selection, per-channel models and successful fit results. They
also retain fit settings and saved browser/display choices. Native acquisitions
are embedded so reopening is not dependent on their original file paths remaining
available. Plain text/CSV imports retain their selected arrays/basic import settings,
not a complete native acquisition they never contained.

Complete sessions also retain each acquisition's [editable notes](scans-and-channels.md#attach-notes-to-a-scan),
independently of its selected detector and original comment. Older sessions without
notes reopen with empty notes. Save the session after editing; a model-only file
or fit CSV report does not preserve these annotations.

Model Undo/Redo histories and temporary Messages history are not persisted across
restarts. Saving records the current working model and retained successful results;
later model edits can mean those two models differ. [Last fit results and CSV export](exporting-fits.md)
refer to the retained successful snapshot, not an unrun trial.

GUI 0.2.27 sessions also retain [workspace arrangements](layout-and-small-screens.md#save-and-restore-an-arrangement):
panel visibility, docking/tabs, floating geometry and section expansion. These
are separate from per-channel model/display values. Large-screen standard layouts
adapt on smaller windows, and off-screen floating geometry is constrained.

## Recover interrupted work

The standalone application writes a separate recovery checkpoint approximately
every **60 seconds** while committed work is unsaved and the workspace is idle.
It also requests one before a single fit, preparation or series workflow. It skips
active drags, modal dialogs and running fits/workflows. **Session → Save recovery
checkpoint now** requests a copy immediately when eligible. Snapshot capture may
briefly pause very large collections; disk writing runs in a background worker.

Recovery copies use the same complete session representation, including raw
acquisitions, channel models/results, preparation, notes and view settings. They
exclude unapplied modal drafts, model Undo history and temporary messages. They
are written atomically to the application's local-data `recovery` folder
(normally `%LOCALAPPDATA%\NEXAFSFit GUI\recovery` on Windows), separately from
your saved session and source files. They do **not** mark the workspace saved.

After an interruption, startup offers **Restore selected**, **Keep for later**
or **Discard selected**. **Session → Recover interrupted work…** reopens this
choice later. Restore validates the checkpoint and opens it as **unsaved work**;
review it and save an ordinary session. It does not overwrite its former session.
Keep leaves the file available. Discard deletes only the chosen recovery copy.
Checkpoints belonging to another running instance are excluded using process locks.
An unreadable or unsupported checkpoint is retained unless explicitly discarded.

Each running window owns its own recovery file. A successful manual session save
or session opening clears that window's old copy. A normal close after the usual
Save/Discard decision also clears its copy; unchosen checkpoints from earlier runs
remain available. If a checkpoint is being written, let it finish and close again.
Failed writes keep the preceding complete copy and report a warning; save manually.
Changes since the last completed checkpoint can still be lost during an interruption.
Periodic recovery is enabled by the standalone launcher, not automatically by an
embedded `Workspace`.

## Save a reusable model

Choose **Model → Save model** to write the current components, parameter values,
bounds, Free/Sharing states and supported links as versioned JSON. It does not
save the loaded acquisition collection or its preparation/metadata. Use a session
when you need to reproduce the complete working dataset and its processing.

For reuse, import/select the new data first, then **Model → Open model**. The
model is applied to that active channel while its data, preparation and fit range
remain in place. A supported session file can also supply just its active model.
Other scans keep their own work. See [the model-reuse procedure](model-editor.md#reuse-a-saved-model-on-current-data).

Model import clears the active fit statistics because they do not describe a fit
of the imported model to these data. Undo can restore the previous model; run a
new fit before treating the imported model as a successful result.

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

## Related topics

- [Select the scan/channel work to retain](scans-and-channels.md).
- [Understand retained metadata and incidence mappings](metadata.md).
- [Review staged series results before applying them](series-fitting.md#review-staged-results-before-applying).
- [Export numerical curves and parameters](exporting-fits.md).
- [Find out which stage or selection produced an unexpected result](troubleshooting.md).
