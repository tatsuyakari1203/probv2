# LaTeX Development Environment with DevPod and VS Code

A reproducible LaTeX development setup using DevPod. Run a ready-to-code container with VS Code (OpenVSCode Server), TeX Live, and useful extensions.

## Prerequisites

- DevPod: https://devpod.sh/docs/getting-started/install
- Docker: https://docs.docker.com/get-docker/

## Quick Start

From this repository folder:

```bash
devpod up . --ide openvscode
```

What happens:
- Reads .devcontainer.json
- Pulls/builds the image and starts the container
- Installs VS Code extensions (LaTeX Workshop, Catppuccin, Spell Checker)
- Opens VS Code in your browser

## Manage the Workspace

Use a new terminal; get the workspace name from `devpod list`.

- List: `devpod list`
- Stop: `devpod stop <workspace-name>`
- Start: `devpod start <workspace-name>`
- Shell into container: `devpod ssh <workspace-name>`
- Delete (permanent): `devpod delete <workspace-name>`

## Access over LAN (optional)

Bind the IDE to all interfaces and choose a port (e.g., 10800):

```bash
devpod up . --ide openvscode --ide-option BIND_ADDRESS=0.0.0.0:10800
```

Open: http://<HOST_IP>:10800

Note: Exposes your IDE on the local network. Only use on trusted networks.

## Run in Background (optional)

POSIX shells (Git Bash/WSL/Linux):

```bash
nohup devpod up . --ide openvscode > devpod.log 2>&1 &
echo $! > devpod.pid
# Logs
tail -f devpod.log
# Stop
kill $(cat devpod.pid)
```

On Windows PowerShell, prefer DevPod CLI lifecycle commands (`stop`/`start`) instead of `nohup`.

## Troubleshooting

- Git: `fatal: detected dubious ownership in repository`
  - Cause: host vs container file ownership mismatch
  - Fix: handled automatically via postCreateCommand; run Git commands inside the container terminal (VS Code or `devpod ssh`).

- DevPod stops due to inactivity
  - Disable once globally: `devpod context set-options -o EXIT_AFTER_TIMEOUT=false`

- DNS errors (e.g., "server misbehaving")
  - The container uses `--dns=8.8.8.8` via runArgs in .devcontainer.json

## How it works (.devcontainer.json)

- image: `tatsuyakari/latex-coder-workspace:latest`
- runArgs: `--dns=8.8.8.8`
- postCreateCommand: trust the workspace for Git (`git config --global --add safe.directory ${containerWorkspaceFolder}`)
- VS Code customizations:
  - Extensions: `James-Yu.latex-workshop`, `Catppuccin.catppuccin-vsc`, `streetsidesoftware.code-spell-checker`
  - Settings: auto build on file change; PDF viewer in tab