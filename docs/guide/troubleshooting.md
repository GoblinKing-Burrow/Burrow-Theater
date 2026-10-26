# Limits and troubleshooting

## Theater could not load

Use **Try again**. If it still fails, check that Theater is enabled and that Burrow and the mod are compatible.

When Burrow authentication is enabled, Theater requires the authenticated request context. With authentication disabled, its route guard does not add a separate login requirement. Theater does not provide a per-role or per-conversation access-control interface.

## No configured text model

Theater obtains its model list from Core. Configure a text model in Burrow, then reopen Theater. It does not discover models or accept credentials independently.

## Model or role no longer available

The conversation still references a model or role that Core or Theater cannot find. Select an available model where possible. Refresh the view after changing model configuration or roles.

## Effort or temperature is disabled

The selected model has not advertised support for that control. This is expected for some models. Theater validates these settings server-side as well as disabling unsupported UI controls.

## A reply failed or the conversation changed

Group assistant replies are published together only after all roles succeed. If an accepted generation later fails, the saved operator message and failed-generation card remain. A failure of the initial send request instead restores the composer text when it is empty. Reopen the conversation and inspect saved state before resending an uncertain request. For a pending card, return to that conversation to resume reconciliation and check Core’s durable generation support; there is no dedicated retry/cancel control.

The generic `internal_error` response deliberately does not include the internal exception. Theater logs server errors as **Burrow Theater request failed**. Image reconciliation warnings use **Burrow Theater could not reconcile Forge image job**. Consult the host’s available logs; this mod does not supply a dedicated log viewer.

## Conversation limits

This snapshot limits the transcript to 400 stored messages. That is not 400 operator turns.

Before generating, Theater reserves room for your message and one reply per role. It reserves one additional image message for each role allowed to use Forge, even if that role ends up producing no image. It rejects a turn that cannot fit as a whole.

Other validated limits:

- Assembled prompt: 100,000 UTF-16 code units (including SOUL/profile, instructions and transcript), reported as `generation_prompt_limit` (HTTP 413).
- Operator message: 24,000 characters.
- Generated text response: 24,000 characters per role.
- Image prompt: 4,000 characters.
- Default text output budget: 1,200 tokens per role; the API accepts 1–8,192. The current composer does not expose that budget.

String-length limits use JavaScript UTF-16 units; some symbols take two units. An oversized first prompt is rejected before saving a turn. For an already pending legacy/later-role turn, deterministic prompt or Core input-limit errors settle it as failed so the conversation and role controls can unlock; ambiguous submission errors retain the same request identity for reconciliation. The active view retries reconciliation after transient failures.

The provider’s own context window and limits still apply. Theater does not summarize a long transcript automatically.

## Deleting an in-use role

The role is protected while any active or archived conversation references it. Select those conversations in Theater and use **Delete**, confirming the permanent transcript removal, before deleting the role in Settings. Wait for pending text or Forge image work before lifecycle actions; archiving alone does not free the role.

## Features not present in this snapshot

There is no conversation file export, branching, regenerate button, or message editing. **Copy message** and **Copy conversation** provide clipboard text; images and generation-status cards are not included. The current UI creates a cast for a new group; it does not offer a participant editor for an existing conversation.

## Data handling

Roles, conversations and the Theater-specific operator profile live in Theater’s Core-managed settings namespace. They are distinct from agent sessions and Brains, but there is no Theater-specific encryption or per-conversation isolation policy in this implementation.

Selected providers receive conversation text for generation. Forge receives requested image prompts. Treat model/provider selection and access to the Burrow installation as part of your data-handling decision.

## An avatar will not save

Wait for **Processing image…** to finish and then save the role/profile; selecting an image only changes the preview. The browser rejects non-image files, unreadable images and source files over 50,000,000 bytes. It shrinks/optimizes accepted images to fit the saved 512,000-character data-URL limit. Try a smaller ordinary image if processing fails.

The API accepts an empty avatar, short plain-text fallback (up to 16 UTF-16 units, excluding `<`, `>`, `:`, `/`), or a base64 PNG/JPEG/WebP/GIF image data URL within that limit. External URLs and SVG data URLs are not accepted. Omitting `avatar` on an edit preserves it; an empty string clears it. Role edits remain blocked while a referring conversation has pending text or Forge image work. These are avatars, not character-card imports.
