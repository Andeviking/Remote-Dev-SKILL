---
name: remote-dev
description: Develop an existing local project using a remote runtime over SSH and rsync. Use for local source edits with remote execution, validation, experiments, or environment setup, including continuing an established remote development workflow.
---

# Remote Development

Keep source edits local, sync the needed files, and use the remote environment as requested. The local machine does not need the project's runtime or compute resources. Follow the user's choices of target, environment, commands, and execution method; choose the remaining details to fit the task.

## Establish or resume context

When entering this workflow, read `<project-root>/.remote-dev/project.md` if it exists, or reuse the project's already established record location. Combine it with the current request and known SSH configuration. Ask only for missing information needed to proceed.

Resolve the local project root, SSH target, remote project root, and any environment specified by the user. Prefer an existing SSH host alias. Create the remote project directory if needed; do not require a pre-existing remote checkout.

Maintain a small local project record for future conversations. Keep temporary overrides separate from saved defaults; update a default when the user makes the change persistent. Exclude the record and local-only state independently from Git and rsync uploads, and store no passwords or private-key contents. Read [Sync and project records](references/sync-and-records.md) when establishing the record or choosing synchronization boundaries.

## Work locally and execute remotely

- Edit project source locally by default. Configure the remote environment when requested, without treating local missing dependencies as evidence about the remote environment.
- Sync source and necessary configuration with rsync. Choose the scope for the current changes; preserve unrelated remote environments, data, caches, configuration, and outputs. Transfer these separately when the task calls for them.
- If remote source has independent edits, compare and explain the differences, then obtain the user's confirmation before overwriting them. Ask if you cannot establish whether an overwrite would destroy independent changes. Ordinary differences caused by the current local edits are expected and do not require confirmation.
- Handle source deletions deliberately. A file absent locally is not, by itself, permission to delete its remote counterpart. Use [Sync and project records](references/sync-and-records.md) for transfer details; do not assume a full directory mirror is appropriate.
- Execute in the intended remote directory and user-specified environment. Select commands, debugging techniques, and long-running execution methods as needed rather than imposing a fixed toolchain.
- Distinguish local edits, completed synchronization, and observed execution results. Check that relevant changes reached the remote before attributing a result to them; do not repeat a full preflight or transfer the whole project without a reason.

## Authentication

Prefer public-key authentication. For first-time setup or authentication failures, read [SSH authentication](references/ssh.md) before changing credentials. Reuse a suitable existing local key, installing its public key on the target if needed. If no suitable local key exists, create a uniquely named, passphrase-free dedicated key and report its location without overwriting existing files. Have the user install the public key from their own terminal, then verify public-key access. If public-key authentication cannot be used, help the user establish a reusable SSH connection.

Keep private keys local. Never request server passwords or private-key passphrases in the conversation. Preserve existing keys' passphrases and let the user unlock them locally when needed.

## Retrieve useful results

For experiments, proactively download summary tables, figures, and other results needed for analysis or reporting. Read or download logs selectively. Retrieve any file explicitly requested by the user.

Use the user's destination or a local directory such as `.remote-dev/results/`, avoiding overwrites of useful local files. Report where the retrieved files were saved. Inspecting a remote file is not the same as downloading it.

## Read details only when needed

| Situation | Reference |
| --- | --- |
| First authentication setup, key selection, or connection failure | [SSH authentication](references/ssh.md) |
| Creating a local record, deciding transfer boundaries, or checking a risky sync | [Sync and project records](references/sync-and-records.md) |
| Requested environment setup or downloads affected by missing internet access or slow sources | [Offline preparation and downloads](references/offline.md) |
