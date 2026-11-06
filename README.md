# Remote Dev Skill

A lightweight, instruction-only skill for editing an existing project locally, syncing with rsync, and using a remote environment over SSH.

The first English draft is ready for review. No scripts are included. Remote execution and automatic discovery across conversations have not yet been verified.

## Install in Codex

Install on the local machine running Codex. The remote server does not need the skill.

For a personal installation on macOS or Linux, run the following from this repository's root. It links the skill into Codex's user skill directory and leaves any existing installation unchanged:

```sh
mkdir -p "$HOME/.agents/skills"
test ! -e "$HOME/.agents/skills/remote-dev" &&
  test ! -L "$HOME/.agents/skills/remote-dev" &&
  ln -s "$PWD/skills/remote-dev" "$HOME/.agents/skills/remote-dev"
```

If that destination already exists, inspect and reuse the existing installation rather than replacing it blindly. A symlinked installation uses this checkout directly; alternatively, copy the complete `skills/remote-dev/` directory, including `references/`, to the same destination.

For project-only use, place the complete skill directory at `<project>/.agents/skills/remote-dev/` instead.

Start a new Codex chat and invoke `$remote-dev` to try the skill. See the [official Codex skill documentation](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills) for discovery rules.

## Project documents

- [Skill draft](skills/remote-dev/SKILL.md): core instructions and links to the relevant references.

The `docs/` directory is reserved for user-facing documentation.
