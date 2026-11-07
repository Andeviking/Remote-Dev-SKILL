# Remote Dev Skill

A lightweight, instruction-only skill for editing an existing project locally, syncing with rsync, and using a remote environment over SSH.

The workflow was tested on an Ubuntu CPU server by installing Python dependencies from locally downloaded wheels, syncing a source edit, rerunning it in the same remote venv, and retrieving the results.

## Install in Codex

Install on the local machine running Codex. The remote server does not need the skill.

### Project installation (recommended)

Copy the complete skill into the local project that will use it. On macOS or Linux, run the following from this repository's root after replacing the example path with an existing project's absolute path:

```sh
target_project="/absolute/path/to/your-project"
test -d "$target_project" &&
  mkdir -p "$target_project/.agents/skills" &&
  test ! -e "$target_project/.agents/skills/remote-dev" &&
  test ! -L "$target_project/.agents/skills/remote-dev" &&
  cp -R "$PWD/skills/remote-dev" "$target_project/.agents/skills/remote-dev"
```

These commands leave any existing installation unchanged. If the destination exists, inspect and reuse it or deliberately update it. Keep `references/` together with `SKILL.md`.

The copied skill can be committed with the target project to share the workflow and keep its version explicit. Personal connection records in `.remote-dev/` should still remain local-only, as described in the skill.

For local skill development, a project-local symlink to this checkout is also possible. Prefer a complete copy when sharing a project so it does not depend on a path on one person's machine.

Start a new Codex chat in the target project and describe the remote task. For example, ask “Run this project's tests in its existing remote Python environment.” In a fresh-chat test, Codex selected the project-installed skill from a natural-language request and reused the saved remote configuration. To invoke the skill explicitly, type `$remote-dev`.

### Personal installation (optional)

Only if you explicitly want the skill available across all projects, copy or link the complete `skills/remote-dev/` directory to `~/.agents/skills/remote-dev/`. Project installation does not require this step.

See the [official Codex skill documentation](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills) for discovery rules.

## Project documents

- [Skill instructions](skills/remote-dev/SKILL.md) and links to the relevant references.

The `docs/` directory is reserved for user-facing documentation.
