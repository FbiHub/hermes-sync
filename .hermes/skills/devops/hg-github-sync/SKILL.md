---
name: hg-github-sync
description: Syncs critical configuration, skills, and memory to the GitHub repository for machine-agnostic survival.
---

# Skill: hg-github-sync

## Objective
Ensures system state persistence (Memory, User Config, Skills) by pushing to the GitHub repository.

## Scope of Sync
1. `/home/fabianogori/user.md`
2. `~/.hermes/skills/`
3. `~/.hermes/memories/`
4. `~/.hermes/agent_identity.md`

## Workflow
1. **Gather:** Identify changes in local configuration files.
2. **Stage:** Run `git add .` in the project root.
3. **Commit:** Execute `git commit -m "Automated System Sync: $(date -u)"`.
4. **Push:** Execute `git push origin main`.

## Integration
- Triggered by daily cron.
- Ensures recovery of the "Hg" personality and operational system after a machine swap.

## Pitfalls
- Ensure git is initialized in the target directory (check for `.git` and run `git init` if missing).
- **Verify Repository Integrity:** Before staging, confirm the repository is healthy and inside a valid work tree using `git rev-parse --is-inside-work-tree`. If this returns false despite the existence of `.git`, the index is likely corrupted; avoid destructive `reset --hard` operations without a backup.
- **Path Handling in Git:** When executing git operations from `root`, always use explicit file paths for critical configuration files (e.g., `git add /home/fabianogori/user.md /home/fabianogori/.hermes/skills/ ...`) because `git add .` often includes unwanted system files. 
- **Embedded Repository Conflict:** If a subdirectory (e.g., `.hermes/`) contains its own `.git` directory, git will flag it as an "embedded git repository" and ignore its inner content during a root commit. To recover trackability of these files, temporarily move or rename the embedded repository's `.git` folder (e.g., `mv .hermes/.git .hermes/.git-embedded-backup`) before staging.
- **Authentication/Connectivity:** If git operations fail:
    1. Verify current remote URL via `git remote -v`.
    2. Check repository existence using `gh repo list`.
    3. If authentication via SSH fails, switch to HTTPS with GitHub CLI authentication:
       - `git remote set-url origin https://github.com/<user>/<repo>.git`
       - Ensure `gh` is authenticated (`gh auth status`).
       - Use `git config --global credential.helper "!gh auth git-credential"` to persist HTTPS authentication for the repo.
    4. If the remote branch has diverged, use `git pull origin main --allow-unrelated-histories` before pushing. Note that `git pull` may be required for initial setup if the remote already contains data.
- **Ignore Local Artifacts (Crucial):** Always include a `.gitignore` to keep the synced repository focused only on critical configuration, as syncing local transient files like session logs or binary databases causes merge conflicts.
    Example `.gitignore` snippet:
    ```
    .git/
    .cache/
    *.log
    *.lock
    *.db*
    *.pid
    bin/
    pastes/
    plugins/
    sessions/
    state-snapshots/
    ```
- **Git Repository Corruption Recovery (Session-Proven):**
  If `git rev-parse --is-inside-work-tree` fails despite a `.git` directory existing:
  1. Backup existing `.git` folder (`mv .git .git-original`).
  2. Run `git init`.
  3. Re-add critical files (`git add <file> ...`).
  4. Manually re-specify remote (`git remote add origin <url>`).
  5. Push with `git branch -M main` + `git push -u origin main`.
  This is safer than complex index repair when dealing with systemic directory corruption.
- **Troubleshooting Sequence:** If `git push` fails, run `ssh -T git@github.com` first to verify the identity/key pair connectivity before attempting to change remotes or verify git configs.

- **Git Corruption:** If index or object corruption occurs, consult `references/git-corruption-recovery.md` before attempting repairs. Do NOT attempt complex repair scripts without a clear recovery plan, as it causes loss of commit history. 
- **Style/Formatting Preference:** All communications, documentation, and agent outputs must adhere to the FABI Tactical System (zero-fluff, highly structured, scannable, Houston time anchor). This must be reflected in the commit messages and any generated documentation synced to the repo.
