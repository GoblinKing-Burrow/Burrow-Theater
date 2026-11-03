# Optional Forge images

**Allow Forge images** lets roles occasionally illustrate their replies. It is off by default and saved separately for each role. A role’s setting applies wherever that role participates.

## Turn images on

1. Open **Settings → Theater → Roles**.
2. Select the role under **Saved roles**.
3. Enable **Allow Forge images**.
4. Choose **Save role**, then return to the conversation.

The checkbox is disabled when Core reports that Forge images are unavailable. Its help text carries the availability reason. Theater does not have a separate image-model selector or provider-key form. Configure Forge through Burrow. Core requires a selected image model whose catalog entry is currently available. The availability reasons can include `forge_selection_missing` or `forge_model_unavailable`. Text conversations remain usable without this feature.

## Save a reference photo

Use **Reference photo** in **Settings → Theater → Roles** for a saved role or **Settings → Theater → Operator profile** for the player. **Upload** or paste while the control is focused; **Replace** and **Remove** change the saved photo independently of other profile drafts. See [role photo controls](roles.md#add-a-canonical-reference-photo) and [photo troubleshooting](troubleshooting.md#a-reference-photo-will-not-save-or-an-illustration-fails).

## Canonical cast reference photos (server contract)

Each **saved** role and the Theater operator profile can keep one original PNG, JPEG, or WebP reference photo, at most 20 MiB. Photos are independent of role/profile avatar and SOUL/profile saves; replacing a photo rotates its reference ID. The authenticated Theater endpoints return `{ok:true,roleId,referenceImage}` for a role and `{ok:true,referenceImage}` for the operator; `referenceImage` is `null` when absent or after removal:

| Subject | Read / upload / remove |
| --- | --- |
| Role | `GET` / `PUT` / `DELETE /roles/:id/reference-image` |
| Operator | `GET` / `PUT` / `DELETE /operator-profile/reference-image` |

`PUT` accepts `{fileName, content}` with `content` as an original `data:image/png;base64,...`, `data:image/jpeg;base64,...`, or `data:image/webp;base64,...` URL. Core validates image signatures and owns bytes; the returned metadata has `{id,contentUrl,mimeType,fileName,sizeBytes,updatedAt}`. `contentUrl` is an opaque direct authenticated Core media URL, never a Mod JSON route or a public unauthenticated asset. A role must exist before reading or writing its photo. A reference photo is removed on role deletion, and deletion stops if cleanup fails. Photo edits are rejected while a referring conversation has pending text or image work; operator-photo edits are rejected while **any** conversation has pending work. This prevents an in-flight turn from silently changing its cast.

At turn admission, Theater captures the **selected group cast** and operator's saved reference IDs, keys, and names, in participant order followed by the operator, but only when a role may illustrate that turn. Illustrating roles' requests then reuse the same full ordered cast, including portraits belonging to roles without their own image permission. Theater does not use avatars as portrait references. These metadata handles are kept in private turn snapshots and image cards, not exposed in conversation outputs. Core resolves the owning Mod's exact saved photo bytes and maps image order to character names at Forge admission. If a referenced photo is missing or invalid at Forge admission (for example, after an external storage change), its pending card fails visibly rather than substituting a new identity. For unsupported image models or missing/invalid photos, image cards fail without paid dispatch. If no saved reference exists, image creation stays text-only as before; old Core versions without reference support still serve text-only Theater, while photo endpoints report unavailable. No portrait bytes or native image input enter the role text-model request.

Enabling images permits a role to request an illustration; it does not require an image on every turn. Image generation may incur usage through Burrow’s configured Forge provider.

Theater uses the normal Burrow authentication context for role settings and image reconciliation. It does not introduce a separate Forge login or per-agent tool grant.

## Per-role access

In a group, only roles with this setting enabled receive image-direction instructions and have their image requests recognized. Giving one role image access does not enable it for the other participants.

Earlier development builds used a conversation-level switch. That old conversation setting is ignored by this version; existing roles without an explicit image setting default to off. Enable each intended role in Settings.

Role edits, including this setting, are blocked while a referring conversation has pending text or image work. Once that work settles, turning the setting off prevents new recognized requests on subsequent turns; it is not a cancellation control.

## What happens next

A role can append one structured image request to its reply. Theater recognizes a valid request, removes that directive from the displayed prose, and saves a pending image card attributed to that role.

The text turn returns before Forge job creation. While the conversation is open, the UI reconciles pending image cards with Forge and shows:

- **Creating image…** while the job is pending.
- The preview when a successful job has an image preview URL.
- An error state when Forge reports a failed or otherwise unsuccessful result.

Pending cards are checked again when the conversation is opened or reloaded. Each card has a stable request identity so retrying reconciliation can reuse its Forge job.

## Important boundaries

This is a narrow, optional image-generation path supplied by the mod. Theater still sends text-model requests with no model tools.

Generated images are not fed back into the roles’ text context. An illustration is not a claim that the role inspected an image, used a shell, or performed a real-world action.

A malformed image directive can remain visible as reply text instead of generating an image. A terminal failed image has no dedicated retry button in this snapshot. Transient reconciliation errors may leave a card pending for another check.
