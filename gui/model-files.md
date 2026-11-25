# Save and reuse a model template

GUI **0.2.30**. A model JSON contains fitting components and parameter setup. Use a [session](sessions-and-models.md) to keep acquisitions, preparation and results together.

(save-a-reusable-model)=

## Save a reusable model

Choose **Model → Save model** to write the current components, parameter values,
bounds, Free/Sharing states and supported links as versioned JSON. It does not
save the loaded acquisition collection or its preparation/metadata. Use a session
when you need to reproduce the complete working dataset and its processing.

(reuse-a-saved-model-on-current-data)=

## Reuse a saved model on current data

Import the desired data first, then **Model → Open model** and choose a saved JSON.
This changes the active channel's model without replacing its data/preparation,
fit range, view or other scans. A supported session JSON can supply just its model.
The prior model is available through Undo; the prior fit statistics are cleared
because they do not describe the imported model. Fit again before exporting it.

**Session → Open session** instead restores the saved collection/workspace.
Compatibility notices explain retained inactive fields. An EMG's old gamma value,
for example, is retained but not treated as active broadening. See
[saving and exports](exporting-fits.md) for the artifact choices.

## Related topics

- [Save the complete workspace](sessions-and-models.md).
- [Construct or edit a model](model-editor.md).
- [Export successful numerical fits](exporting-fits.md).
