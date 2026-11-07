# SSH Authentication

Use this reference for initial setup or authentication problems. If the existing connection works, reuse it. Examples use `devbox` as the SSH host alias; substitute the actual target and selected key path.

## Verify the host before troubleshooting keys

Distinguish a connection failure from a host verification or login failure. If the host is new, have the user verify its fingerprint through a trusted source and accept it in their own terminal. If a known host's key has changed, have the user verify the change before reconnecting. Keep host verification enabled for SSH and rsync. A host verification failure gives no evidence about whether the user's key is authorized.

## Reuse existing authentication

Inspect the target's SSH configuration and available key identities, including those provided by `ssh-agent`. Do not print private-key contents. Distinguish a suitable local key from its authorization on the target: if an existing key is suitable but not yet authorized, have the user install its public key rather than generating another key. If server policy is already known to prevent public-key authentication, go directly to connection reuse.

Preserve existing key passphrases. If a key needs unlocking, have the user do so in their own terminal and use their local authentication agent. Verify access from the environment actually running your commands; a separate terminal may have different access to the agent.

## Create a key only when needed

If no suitable local key is available, create a dedicated key with an empty passphrase, as the default for this workflow. Choose a filename whose private and public key paths are both unused. Report the new paths; do not add another confirmation step just to create the key.

For example, after confirming that the following paths are unused:

```sh
ssh-keygen -t ed25519 -N '' -f ~/.ssh/remote_dev_ed25519 -C remote-dev
```

Use a different unused filename if either path already exists. Keep both key files outside the project. Never replace an existing key or remove its passphrase to make this workflow work.

## Let the user install the public key

Prepare a command with the actual target, public-key path, and connection options. Have the user run it in their own terminal and enter the server password there:

```sh
ssh-copy-id -i ~/.ssh/remote_dev_ed25519.pub devbox
```

For a direct target on a custom port, use `ssh-copy-id -i ~/.ssh/remote_dev_ed25519.pub -p 2222 user@host`. Preserve any jump host or other required connection options. If `ssh-copy-id` is unavailable, prepare an equivalent SSH command that appends only the public key while preserving existing authorized keys. Do not ask the user to paste a password into the conversation.

Make sure subsequent connections select the chosen key. Use a target-specific `IdentityFile` setting or pass the identity and any custom port explicitly to SSH and to rsync's SSH transport (`-e`). Preserve unrelated SSH configuration and tell the user about changes.

After the user finishes, verify a fresh public-key connection rather than reusing an already authenticated master connection:

```sh
ssh -S none -o BatchMode=yes -o PreferredAuthentications=publickey \
  -o StrictHostKeyChecking=yes -o ConnectTimeout=10 devbox true
```

Include the selected identity explicitly if the host configuration does not already select it. `BatchMode` disables interactive authentication prompts; it does not bypass authentication or host verification. A failed check may reflect a locked key or additional authentication requirements, not a lack of server support for public keys.

## Fall back to connection reuse

Distinguish incomplete key setup from a server policy that prevents public-key authentication. Explain the reason before falling back. Do not change server authentication policy to force key access.

When public-key authentication cannot be used, let the user authenticate a reusable master connection in their own terminal. For example, with the control socket under their private SSH directory:

```sh
ssh -M -S ~/.ssh/remote-dev-%C -o ControlPersist=30m -fN devbox
```

Use the same control path and target for subsequent commands and for rsync's SSH transport. For example:

```sh
ssh -S ~/.ssh/remote-dev-%C -o BatchMode=yes -o ConnectTimeout=10 devbox true
```

The control socket must be accessible to the local execution environment and protected from other users. `ControlPersist=30m` is an idle retention period, not a guaranteed connection lifetime. Disconnection or expiry may require the user to authenticate again. Report that need rather than collecting or storing their password.
