---
name: github-nightly-sync
description: Automates pushing local changes to the GitHub repository daily at 11 p.m.
---

# Skill: github-nightly-sync

Automates pushing local changes to the GitHub repository.

## Description
This skill pushes all committed local changes to the `main` branch of the configured GitHub repository. It is intended to be run as a cron job to ensure the GitHub repository serves as a reliable daily "source of truth."

## Trigger
- Cron job: Runs daily at 23:00 (11 p.m.).

## Steps
1. Navigate to the project root directory.
2. Run `git push origin main`.

## Pitfalls
- Ensure all intended work is committed locally before the sync, as this skill only pushes *already committed* changes.
- If authentication fails, check that the `gh` CLI or `git` credentials are properly configured for the environment.
- If the remote branch has diverged, a `git pull` or `git fetch` may be required before pushing; this skill assumes a clean tracking relationship.
