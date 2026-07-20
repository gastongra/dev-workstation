# dev-workstation

A reproducible development environment for AI engineering work. This project defines a [Dev Container](https://containers.dev/) image with the tools, runtimes, and editor extensions needed for Python and Node.js development, GitHub workflows, and container-based tooling.

## Goal

The workstation provides a consistent, ready-to-use environment so you can start building without spending time installing dependencies locally. The container includes:

- **Base OS:** Ubuntu 24.04 (Microsoft Dev Containers base image)
- **Runtimes:** Node.js 22, Python 3.12
- **CLI tools:** Git, GitHub CLI (`gh`), `curl`, `wget`, `jq`, and common utilities
- **Docker access:** Docker-outside-of-Docker (uses the host Docker socket; Moby is not installed in the container)
- **Editor setup:** Preconfigured VS Code / Cursor extensions for Python, Docker, YAML, ESLint, Prettier, GitLens, and Playwright

The workspace is mounted at `/workspace` inside the container, with `vscode` as the default user.

## Create the container image

### Option 1: Build with Docker

From the repository root:

```bash
docker build -f .devcontainer/dockerfile -t dev-workstation .
```

This builds the base image defined in `.devcontainer/dockerfile`. Dev Container **features** from `.devcontainer/devcontainer.json` (Node, Python, GitHub CLI, etc.) are applied when the container is created through a Dev Containers–compatible client, not by this plain `docker build` step alone.

### Option 2: Build with the Dev Containers CLI (recommended)

Install the [Dev Containers CLI](https://github.com/devcontainers/cli), then from the repository root:

```bash
devcontainer build --workspace-folder .
```

This reads `.devcontainer/devcontainer.json`, builds the image, and applies all configured features.

### Option 3: Build from Cursor or VS Code

1. Open this folder in Cursor or VS Code.
2. When prompted, choose **Reopen in Container** (or run **Dev Containers: Reopen in Container** from the command palette).
3. The editor builds the image and starts the container automatically.

## Run the container

After building with Docker (Option 1), run an interactive shell:

```bash
docker run -it --rm \
  -v "$(pwd):/workspace" \
  -w /workspace \
  dev-workstation \
  bash
```

For day-to-day development, prefer Option 2 or 3 so features, mounts, and editor customizations from `devcontainer.json` are applied.

## Return to local Windows

After **Reopen in Container**, Cursor runs the terminal, extensions, and tooling inside the container. Your project files stay on disk at the same path on Windows; only the environment changes.

To switch back to local Windows development:

1. Click the **remote indicator** in the bottom-left corner (e.g. **Dev Container: …**), then choose **Reopen Folder Locally**.

   Or press `Ctrl+Shift+P` and run **Dev Containers: Reopen Folder Locally**.

Cursor reloads and opens the same folder on Windows. Your code is unchanged.

To stop the container entirely, run **Dev Containers: Stop Container** from the command palette, or:

```bash
docker ps
docker stop <container-id>
```

You can reopen the container anytime with **Dev Containers: Reopen in Container**.
