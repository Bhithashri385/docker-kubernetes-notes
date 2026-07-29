# Documentation

Welcome to the documentation folder for the Docker & Kubernetes Notes space.

This folder is prepared so you can sync this repository with your GitBook space. It contains the initial landing page and a sample index.

Suggested Table of Contents

- Introduction (index.md)
- Setup (placeholder)
- Examples (placeholder)

How to finish the GitBook integration

1. In GitBook (https://app.gitbook.com) open your Space → Settings → Integrations → GitHub → Connect.
2. When redirected to GitHub, install the GitBook app and choose "Only select repositories" then pick this repository: Bhithashri385/docker-kubernetes-notes. This ensures only this repo is synced.
3. Back in GitBook, select branch: `main` and folder path: `docs/`.
4. Enable Auto‑sync on push if you want GitHub pushes to update GitBook automatically.
5. (Optional) Enable "Create PRs from GitBook edits" if you want edits made in GitBook to open PRs back to this repo instead of pushing directly.

Notes

- Put images/files under docs/assets/ and reference them with relative paths (e.g., `./assets/image.png`).
- I added these files directly on `main` so GitBook can import them immediately once you finish the integration.
