# Burrow Theater

![Ziggy, Burrow Theater’s purple-robed goblin](../assets/hero.png)

**New role. Same bad influence.**

Burrow Theater is a place for fictional conversations. Give a role a name and a SOUL, choose one of Burrow’s configured text models, and start talking. Bring several roles into the same conversation when one goblin is not enough.

Theater keeps the familiar chat layout without bringing along agent tools, workspace context, tasks, Brains, handoffs, or subagents. Each role receives its SOUL, a fiction boundary, your Theater-specific operator profile, the cast’s names, and the conversation’s text. Optional Forge images can illustrate the scene.

See [setup and release availability](installation.md#release-availability) for installation and daily-sync timing.

## Documentation baseline

Reviewed **2026-10-25** against source version **2026.10.25.4**, commit `7cc9169b4dba6e9a3e906537509a219a1b2fe090`. This full source/UI review covers setup, roles, profiles, lifecycle, copying and input history, durable text generation, prompt limits and optional Forge images. It does not claim application-test execution or live provider verification.

## Start here

1. [Set up Theater](installation.md).
2. [Create and edit SOUL roles](roles.md).
3. [Start a conversation or group chat](conversations.md).
4. [Let roles illustrate a scene with Forge](forge-images.md).
5. [Check limits and troubleshoot](troubleshooting.md).

## A different kind of conversation

A Theater role is a saved fictional identity, not an operational Burrow agent. It cannot run shell commands, inspect your machine, or administer Burrow through model tools. The model is explicitly instructed to stay within the fiction.

That boundary describes Theater’s conversation design. It is not a guarantee that a model will never say something inaccurate. When a role says it opened a door or repaired a machine, read that as part of the story.

Theater stores its roles and conversations in its own Core-managed settings namespace. It does not feed them into Brains. “Separate from agent conversations” does not mean encrypted, offline, or isolated from everyone who can access this Burrow installation. Text goes to the selected configured provider; optional image prompts go through Forge.

## Latest documentation update

The **2026-10-26 delta review** covers source **2026.10.25.7**, commit `527dab56e7dd10543bd7805bb1ee6379b25f48fd`: editable role/operator avatars and view-local scroll preservation. The full-review baseline above remains historical; unchanged functionality was not re-certified as a new full review. No application or live-provider tests were run for this update.
