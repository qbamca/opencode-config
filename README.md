# opencode-config

Private, point-in-time OpenCode configuration snapshot. It contains the safe
`HEAD` tree only; it does not contain the original repository's Git history and
does not automatically synchronize with the active configuration.

## Layers

- `base/` is the shared configuration foundation.
- `work/` and `private/` are intentionally sparse profile overrides.
- `node_modules/`, runtime notifier state, provider-login stores, and secrets
  are local state and are not bundled. External skills installed globally (for
  example under `~/.agents/skills` or `~/.claude/skills`) are also dependencies
  of the local machine, not part of this repository.

## Install or refresh manually

Copy each layer into its matching active location. These commands overwrite
snapshot files but do **not** delete destination-only files; do not use a
delete/sync option. In particular, retain existing `.git/`, `secrets/`, and
`node_modules/` directories.

```zsh
mkdir -p "$HOME/.config/opencode" "$HOME/.config/opencode-work" "$HOME/.config/opencode-private"
cp -a base/. "$HOME/.config/opencode/"
cp -a work/. "$HOME/.config/opencode-work/"
cp -a private/. "$HOME/.config/opencode-private/"
```

If a layer has no local dependencies yet, run `npm install` inside that layer.
Dependencies remain local and ignored by Git.

## Local credentials

Provision credentials only after copying `base/`. The GitHub file must contain
the complete Authorization header value, including its `Bearer` prefix. The
interactive reads keep entered values out of command arguments and shell
history; never paste credentials into a command line or commit them.

```zsh
umask 077
install -d -m 700 "$HOME/.config/opencode/secrets"
read -rs 'github_authorization?GitHub Authorization header (include Bearer prefix): '
printf '\n'
printf '%s' "$github_authorization" > "$HOME/.config/opencode/secrets/github-authorization"
unset github_authorization
read -rs 'context7_api_key?Context7 API key: '
printf '\n'
printf '%s' "$context7_api_key" > "$HOME/.config/opencode/secrets/context7-api-key"
unset context7_api_key
chmod 600 "$HOME/.config/opencode/secrets/github-authorization" "$HOME/.config/opencode/secrets/context7-api-key"
```

Provider login is separate local state and remains excluded. Restart OpenCode
after changing configuration, plugins, skills, or credentials.

## Launch profiles

The current `~/.zshrc` wrapper selects `~/.config/opencode-work` for paths
under `~/igus/` and `~/mechanical-joe/igus/`, selects
`~/.config/opencode-private` for other `~/mechanical-joe/` paths, and unsets
`OPENCODE_CONFIG_DIR` elsewhere. To launch a profile explicitly, matching that
wrapper's invocation:

```zsh
OPENCODE_ENABLE_EXA=1 OPENCODE_CONFIG_DIR="$HOME/.config/opencode-work" command opencode
OPENCODE_ENABLE_EXA=1 OPENCODE_CONFIG_DIR="$HOME/.config/opencode-private" command opencode
```

`OPENCODE_CONFIG_DIR` is the documented OpenCode setting for an alternate
configuration directory. Keep the shared base and selected sparse profile in
their locations above so the established local layering remains intact.
