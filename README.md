# conductor_for_codex

Portable Conductor-style bootstrap for Codex, using transparent plain markdown skill files.

## Start Here

If you are new to Conductor, read [`what_is_conductor.md`](what_is_conductor.md) first.

## Install Once, Use Everywhere

This repo uses a two-step model:

1. Run one installer script once per machine/user (global install).
2. Run `codex_conductor_init` inside any repo you want to initialize (repo init).

The global install gives you reusable skills and a global init command.
The repo init adds only local project files needed for Conductor-style workflow.

## Transparency

This repo includes skill content as plain markdown files under `skills/`.

- No encoded/base64 skill payloads are used for installation.
- Both installers copy from `skills/<name>/SKILL.md` so users can inspect everything before running.

## What Gets Installed

Global install (user profile, offline):

- Skills in global Codex home:
  - `conductor-setup`
  - `conductor-status`
  - `conductor-implement`
  - `conductor-newTrack`
  - `conductor-revert`
  - `update-conductor`
- Global init command:
  - Windows: `%USERPROFILE%\.codex\bin\codex_conductor_init.cmd`
  - Linux: `$HOME/.local/bin/codex_conductor_init`

## What Happens in a Repo

When you run repo init, it creates/updates local files in that repo:

- `.codex/skills/<skill>/SKILL.md` (copied from global install)
- `AGENTS.md` rule line:
  - `Always run $conductor-status before doing anything else.`
- `.gitignore` line:
  - `conductor/`

## Safety (Non-Destructive by Design)

This setup is intentionally non-destructive:

- It never deletes your project files.
- It never overwrites existing repo skill folders under `.codex/skills/<skill>`.
- For `AGENTS.md`:
  - If missing, create it.
  - If present, append the required conductor-status rule only if missing.
- For `.gitignore`:
  - If missing, create it.
  - If present, append `conductor/` only if missing.

In short: append-only when needed, no destructive replacement.

## Requirements

Windows:

- PowerShell 5.1+

Linux:

- `bash`

## Install on Windows

Run from the folder containing `conductor_for_codex.ps1`:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File .\conductor_for_codex.ps1
```

What each part does:

- `powershell`: starts a new PowerShell process.
- `-NoProfile`: avoids loading profile scripts for predictable behavior.
- `-ExecutionPolicy Bypass`: avoids script policy blocking for this run.
- `-File .\conductor_for_codex.ps1`: runs the installer.

Advanced override:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File .\conductor_for_codex.ps1 `
  -CodexHome "$env:USERPROFILE\.codex" `
  -BinDir "$env:USERPROFILE\.codex\bin" `
  -SkipPathUpdate
```

Flags:

- `-CodexHome`: custom global Codex home directory.
- `-BinDir`: custom install path for `codex_conductor_init.*`.
- `-SkipPathUpdate`: skip adding the bin directory to user PATH.
  - Use this only if you intentionally manage PATH yourself.
  - Recommended default: do not pass `-SkipPathUpdate`, so `codex_conductor_init.cmd` works globally.

## Install on Linux

```bash
bash ./conductor_for_codex.sh
```

Advanced override:

```bash
CODEX_HOME="$HOME/.codex" BIN_DIR="$HOME/.local/bin" NO_PATH_HINT=0 bash ./conductor_for_codex.sh
```

Variables:

- `CODEX_HOME`: custom global Codex home directory.
- `BIN_DIR`: custom install path for `codex_conductor_init`.
- `NO_PATH_HINT=0`: print PATH hint if needed.

## Admin / sudo

- Windows: admin is not required.
  - Installs to your user profile.
  - Updates user PATH only (not machine PATH).
- Linux: sudo is not required.
  - Installs under `$HOME`.

## Use It Globally

After global install, open any repo directory and run:

- Windows: `codex_conductor_init.cmd`
- Linux: `codex_conductor_init`

If PowerShell policy blocks `.ps1` resolution, use:

```powershell
codex_conductor_init.cmd
```

or run init explicitly:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File "$env:USERPROFILE\.codex\bin\codex_conductor_init.ps1"
```

## Folder Layout

Repo-local after repo init:

```text
your-repo/
  .codex/
    skills/
      conductor-setup/
        SKILL.md
      conductor-status/
        SKILL.md
      conductor-implement/
        SKILL.md
      conductor-newTrack/
        SKILL.md
      conductor-revert/
        SKILL.md
      update-conductor/
        SKILL.md
  AGENTS.md
  .gitignore
```

Global install on Windows:

```text
%USERPROFILE%\.codex\
  skills\
    (same 6 skill folders)
  bin\
    codex_conductor_init.cmd
    codex_conductor_init.ps1
```

Global install on Linux:

```text
$HOME/.codex/
  skills/
    (same 6 skill folders)
$HOME/.local/bin/
  codex_conductor_init
```

## Re-run / Refresh

Rerunning installer or repo init is safe and idempotent.

- Existing targets are preserved.
- Missing required lines are appended once.
- Duplicate required lines are avoided.

If you want to refresh one specific repo-local skill folder, remove only that folder and rerun repo init.
