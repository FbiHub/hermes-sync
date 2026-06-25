# Troubleshooting: Git Repository Corruption

If `git status` returns `fatal: bad object HEAD` or `git fsck` reports numerous broken links/missing objects, the repository has been corrupted.

**Recovery Protocol:**
1. **Diagnostic:** Run `git fsck --full`.
2. **Standard Repair:** If the corruption is limited, attempt `git reflog` recovery or `git fsck --lost-found`.
3. **Severe Corruption (Last Resort):** If the history is unrecoverable, re-initialize the repository. **WARNING: This loses commit history.**
    - `rm -rf .git`
    - `git init`
    - `git remote add origin <url>`
    - `git add .`
    - `git commit -m "Emergency Re-init: $(date -u)"`
    - `git push -u origin main --force`
4. **Prevention:** Avoid interrupting Git operations during I/O-intensive cron jobs.
