# Create and edit SOUL roles

A role has **Role name**, **SOUL**, an optional **Avatar**, and an optional **Allow Forge images** setting. The SOUL is Markdown describing the character, voice, relationships, and boundaries of the fiction.

## Create a role

1. Open **Settings → Theater → Roles**.
2. Choose **New role**.
3. Enter a name and SOUL.
4. Optionally enable **Allow Forge images** for this role when Forge is available.
5. Choose **Create role**.

For example:

```markdown
# SOUL — Penny, Keeper of the Lost Keys

You are Penny, a curious goblin archivist who runs a midnight lost-and-found.

Speak warmly, with dry humor and short, vivid descriptions.
Remember the details established in this conversation.
Ask what the visitor has lost before proposing an adventure.
Stay within the fictional archive.
```

This is an example identity, not a required schema. Theater adds its own fiction boundary and cast information.

## Edit a role

Select it under **Saved roles**, change its name, SOUL or avatar, and choose **Save role**.

Roles are reused by conversations. Future turns use the current saved SOUL; editing it does not rewrite old transcript messages. Theater blocks role edits while any conversation using that role has pending text or Forge image work. Wait for that work to settle; the submitted turn uses its saved role snapshot.

The image setting is off by default and follows the role across its conversations. In groups, each role keeps its own image permission. See [Forge images](forge-images.md).

## Choose or remove an avatar

In the role editor, use **Avatar → Change** to choose an image, or paste an image into the avatar control. Wait for **Processing image…** to finish, then choose **Create role** or **Save role**. The preview alone is not a saved change. **Remove**, followed by saving, clears the image and restores the fallback initial.

The browser fits the image within 512 × 512 pixels without upscaling, preserving its proportions, and optimizes it before saving. Source files larger than 50,000,000 bytes are rejected. The saved image data URL is limited to 512,000 characters. An unreadable or unprocessable image leaves an error in the avatar control; choose another image. There is no crop editor or remote-image URL field.

Role avatars appear in the role rail, group selector and transcript. They are display data, not image input to the text model. Role-avatar changes follow the same pending-work edit guard as the SOUL. For your own Theater avatar, use **Settings → Theater → Operator profile** and **Save profile**; it is separate from your normal Burrow identity.

## Remove a role

The delete button asks for confirmation. A role still referenced by any active or archived conversation cannot be deleted. In Theater Main, select each conversation that uses the role and choose **Delete**. After those transcripts are permanently removed, return to **Settings → Theater → Roles** and delete the role.

## Limits

- Role name: up to 120 characters.
- SOUL: up to 48,000 characters.
- Both fields must contain text after trimming whitespace.
- Avatar source and saved-size limits are described above.
- The current editor does not provide character-card import or a separate scenario editor.

A longer SOUL also makes every generated turn larger. Start with the details the character actually needs.



