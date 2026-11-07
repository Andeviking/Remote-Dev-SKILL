# Sync and Project Records

Use this reference when establishing a project's local record, choosing transfer scope, or dealing with uncertain synchronization effects.

## Keep a small local record

Use `.remote-dev/project.md` under the local project root unless a record location is already established. Keep it as ordinary Markdown that an agent can read directly. Record known connection and project facts, not a task log or a second copy of the user's SSH configuration.

An example record is:

```markdown
# Remote development

- Local project: the project root containing .remote-dev/
- SSH target: devbox
- Remote project: /home/user/project
- Environment: /home/user/project/.venv/bin/python
- Sync notes: datasets and existing remote environments stay on the server
```

Replace the examples with known facts; omit unknown or irrelevant fields. Prefer an existing SSH host alias so ports, identities, and jump hosts remain managed by SSH. Note project-specific transfer choices when they are useful for subsequent work. Do not store passwords, private-key contents, or copied credentials.

The host handles skill discovery and selection. Once the skill is selected for a task or explicitly invoked, read the project's record to reuse its saved setup.

Apply temporary overrides to the current task without replacing persistent defaults. For example, “use the other server this time” is temporary; “use this server from now on” changes the saved default. When establishing a record, save the agreed setup rather than treating incidental commands or observed paths as new defaults.

## Exclude local state through both tools

Keep the record local-only by default. When using `.remote-dev/`, exclude that directory, including any retrieved results stored there, from both Git and outbound rsync transfers. Share its contents only when the user explicitly requests it.

- For Git, use repository-local ignore rules such as `info/exclude` in the Git directory rather than changing a team's shared ignore file unnecessarily. Resolve the actual Git directory and repository-relative path, including in worktrees or nested projects. Check whether a file is already tracked: adding an ignore rule does not untrack it or remove it from history.
- For rsync, separately exclude the record file or its dedicated local-state directory relative to the transfer root. Do not exclude a custom record's entire parent directory if it also contains project files. Git ignore rules are not automatically rsync filters. For a transfer rooted at the local project, an exclusion such as `--exclude='/.remote-dev/'` protects this default directory; adapt it when the transfer root or record location differs.

Hiding a directory is not an exclusion mechanism. If an existing record is already tracked, explain the situation before changing tracking; do not rewrite repository history as part of setup. If no Git repository exists, keep the record out of transfers and apply Git exclusion when Git is introduced.

## Check rsync availability

Rsync over SSH needs a working rsync executable on both ends. On a new target, check from the non-interactive SSH session that will run the transfer. If an existing remote executable is outside that session's `PATH`, select it with `--rsync-path`.

If rsync is missing, explain where you propose to put it and confirm that scope with the user unless they already approved the setup. When the remote has no internet access, obtain a trusted package for its system and architecture locally, including any required libraries. Check available package integrity information and transfer the files over SSH, for example with `scp`. A project-local copy can be selected with `--rsync-path` and avoids a system-wide install. Verify that it runs before syncing.

## Choose scope by file role

Sync source and configuration required by the task. Keep unrelated remote virtual environments, data, model files, caches, remote-specific configuration, and outputs intact. Transfer any of these when explicitly requested or needed, using a suitable scope rather than a universal filename blacklist. A local virtual environment is not a substitute for the remote runtime.

Exclude local configuration that contains credentials from uploads by default. If the task requires transferring any of it, confirm the exact transfer scope with the user first.

Resolve the local source and remote destination before transferring. A trailing slash on the rsync source means copying its contents; check the destination layout to avoid creating an extra project directory level. Quote paths appropriately for both local and remote command interpretation.

Use selective transfers or project-wide synchronization as the changes warrant. Consider a dry run with an itemized change list for an unfamiliar existing destination, deletions, or an uncertain transfer scope. Do not require it for every routine sync.

## Preserve independent changes

Use the available Git state, development context, and actual file differences to assess changes. A preview shows planned operations, not who changed a file. Timestamps and differing contents do not establish that the remote has independent edits, nor prove that it has none.

If you know what was last transferred, compare the affected remote files with that version before overwriting or deleting them. A content hash can be enough for a targeted check.

If an overwrite or deletion would discard independent remote edits, explain what would be lost and wait for the user's decision. If the source of relevant changes is uncertain, compare further or ask before proceeding. Treat known local edits awaiting synchronization as expected differences.

Handle deleted or renamed source files so stale code does not remain in use, while preserving remote-only content. Do not infer deletion authority from local absence alone or apply broad mirroring options without considering the affected files. Choose the mechanics for the actual change; no separate synchronization database is required.

## Retrieve results deliberately

Download experiment summaries, tables, and figures needed for analysis or reporting, plus any explicitly requested file. Read or fetch logs only as needed. Use the user's destination or a local results directory such as `.remote-dev/results/`; avoid overwriting useful files or deleting previous results as routine cleanup.

If a result belongs in version control or a shared deliverable, save it to an appropriate user-approved project location instead of leaving it only in the excluded local directory. Report actual local paths and distinguish retrieved files from files merely inspected remotely.
