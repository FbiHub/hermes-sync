---
name: github-umbrella
description: "Comprehensive GitHub management: Auth, PRs, Issues, and Repo management."
version: 1.0.0
author: Hermes Agent
license: MIT
---

# GitHub Management Umbrella

This skill consolidates all GitHub-related functionality for Hermes. It is organized into the following areas:

## 1. Authentication
Handles HTTPS tokens, SSH keys, and `gh` CLI configuration. For full details, see the `github-auth` workflow.

## 2. Pull Request Workflow
Lifecycle management (branching, commits, CI, merging).

## 3. Issue Tracking
Creation, triage, labeling, and project management for issues.

## 4. Repository Operations
Creation, forking, cloning, releases, secrets management, and actions workflows.

## 7. GitHub Synchronization & Backup
Automate repository syncs, daily pushes, and machine-agnostic survival backups of configuration and skills. See [references/sync-backup.md](references/sync-backup.md).


## Troubleshooting & Pitfalls
- See [references/troubleshooting.md](references/troubleshooting.md) for handling port conflicts, OOM errors, and cross-origin authentication issues specific to the workspace.

---
*For specific tool usage patterns (gh vs curl fallbacks), refer to the consolidated reference files below.*
