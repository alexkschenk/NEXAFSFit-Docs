# Recover interrupted work

GUI **0.2.30**. Recovery is a separate checkpoint mechanism in the standalone application. Use [Save session](sessions-and-models.md) for deliberate, named checkpoints.

(recover-interrupted-work)=

## Recover interrupted work

The standalone application writes a separate recovery checkpoint approximately
every **60 seconds** while committed work is unsaved and the workspace is idle.
It also requests one before a single fit, preparation or series workflow. It skips
active drags, modal dialogs and running fits/workflows. **Session → Save recovery
checkpoint now** requests a copy immediately when eligible. Snapshot capture may
briefly pause very large collections; disk writing runs in a background worker.

### Checkpoint contents and location

Recovery copies use the same complete session representation, including raw
acquisitions, channel models/results, preparation, notes and view settings. They
exclude unapplied modal drafts, model Undo history and temporary messages. They
are written atomically to the application's local-data `recovery` folder
(normally `%LOCALAPPDATA%\NEXAFSFit GUI\recovery` on Windows), separately from
your saved session and source files. They do **not** mark the workspace saved.

### Restore, keep or discard a checkpoint

After an interruption, startup offers **Restore selected**, **Keep for later**
or **Discard selected**. **Session → Recover interrupted work…** reopens this
choice later. Restore validates the checkpoint and opens it as **unsaved work**;
review it and save an ordinary session. It does not overwrite its former session.
Keep leaves the file available. Discard deletes only the chosen recovery copy.
Checkpoints belonging to another running instance are excluded using process locks.
An unreadable or unsupported checkpoint is retained unless explicitly discarded.

### Checkpoint lifetime and limits

Each running window owns its own recovery file. A successful manual session save
or session opening clears that window's old copy. A normal close after the usual
Save/Discard decision also clears its copy; unchosen checkpoints from earlier runs
remain available. If a checkpoint is being written, let it finish and close again.
Failed writes keep the preceding complete copy and report a warning; save manually.
:::{warning} Recovery is limited to the last completed checkpoint
Changes since the last completed checkpoint can still be lost during an
interruption. Unapplied dialog drafts are not part of a checkpoint. Save an
ordinary session at deliberate milestones.
:::

Periodic recovery is enabled by the standalone launcher, not automatically by an
embedded `Workspace`.

## Related topics

- [Save and reopen a normal session](sessions-and-models.md).
- [Stop a running fit safely](fitting-and-results.md#stop-a-running-fit-safely).
- [Record a useful issue report](troubleshooting.md#keep-a-useful-issue-report).
