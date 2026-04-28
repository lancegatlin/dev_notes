### move files from one repo to another while preserving history

https://kokkonisd.github.io/2021/06/23/import-git-history

Moving a subset of files between repositories while preserving their history is a multi-step process that involves isolating the files in a temporary clone and then merging that filtered history into your target repository.

#### Phase 1: Isolate Files in a Temporary Clone

Do not do this in your main local repository, as these steps rewrite history.

1. Clone the source repository to a new temporary folder: `git clone <source-repo-url> temp-repo`
2. Navigate into it: `cd temp-repo`
3. Filter the repository to keep only the files/folders you want. The modern, recommended tool is git-filter-repo (requires Python):
   `git filter-repo --path path/to/file1 --path path/to/folder2/`
   Note: If you only have access to legacy Git, use `git filter-branch --subdirectory-filter <folder-name> -- --all` instead.
4. (Optional) Move files to a subdirectory: If you want these files to live in a specific folder in the new repo, move them now and commit:
   `mkdir new-folder && mv * new-folder && git add . && git commit -m "Prepare for migration"`

#### Phase 2: Merge into the Target Repository

1. Navigate to your existing target repository: `cd ../target-repo`
2. Add the temporary repo as a remote: `git remote add migration-source ../temp-repo`
3. Fetch the history: `git fetch migration-source`
4. Merge the history into your current branch. You must use a special flag because the repositories have no common ancestor: `git merge migration-source/master --allow-unrelated-histories`
5. Clean up: `git remote remove migration-source` `rm -rf ../temp-repo`

#### Phase 3: Finalize Source Repository

Now that the files and history are safely in the new repo, you can simply delete the original files from the source
repository using a standard git rm and commit.

---

## Why Re-Running the Original Instructions Duplicates History

The original `git_move_files_preserve_history.md` instructions do a **full history migration** every time:

1. Clone nl → filter-repo → keep all history from the beginning
2. `merge --allow-unrelated-histories` into nps

Re-running this replays every commit from the beginning of predefined-mapper history on top of commits that are already in nps — resulting in duplicates.

---

## Correct Incremental Update Technique

When nl gets new `predefined-mapper/` commits in the future, use this process instead:

### Step 1 — Identify the cutoff

Find the nl commit hash that corresponds to the **last one already migrated** into nps.  
Check the nps log for the most recent predefined-mapper migration commit and match its message to nl's log:

```bash
# In nps — find the last migrated predefined-mapper commit message
cd /Users/gatlil3/Code/cat.gitm/nps
git --no-pager log --oneline -- predefined-mapper/ | head -5

# In nl — find the matching commit hash by message
cd /Users/gatlil3/Code/cat.gitm/nl
git --no-pager log --oneline -- predefined-mapper/ | head -5
```

Currently the cutoff is: **`66cdc2a08`** in nl.

### Step 2 — Confirm there are new commits to migrate

```bash
cd /Users/gatlil3/Code/cat.gitm/nl
git --no-pager log --oneline 66cdc2a08..HEAD -- predefined-mapper/
```

If this outputs nothing, nps is already up to date. If it lists commits, proceed.

### Step 3 — Create a filtered temp clone (new commits only)

```bash
# Clone nl fresh into a temp dir
git clone /Users/gatlil3/Code/cat.gitm/nl /tmp/nl-incremental-temp
cd /tmp/nl-incremental-temp

# Filter to only predefined-mapper/ files (rewrites history in the temp clone)
git filter-repo --path predefined-mapper/

# Verify: find the filtered equivalent of the cutoff commit by matching the commit message
git --no-pager log --oneline -- predefined-mapper/ | grep "AB#2138990"
# Note the hash shown — call it <CUTOFF_FILTERED_HASH>
```

### Step 4 — Cherry-pick only new commits into nps

**This is the key difference from the original instructions — use cherry-pick, not merge.**

```bash
cd /Users/gatlil3/Code/cat.gitm/nps

# Add the filtered temp clone as a remote
git remote add nl-incremental /tmp/nl-incremental-temp
git fetch nl-incremental

# List the new commits to cherry-pick (newest last so you apply oldest-first)
git --no-pager log --oneline nl-incremental/main <CUTOFF_FILTERED_HASH>..nl-incremental/main

# Cherry-pick them in order from oldest to newest
git cherry-pick <oldest-new-hash> ... <newest-new-hash>
```

If cherry-pick conflicts arise, resolve them, then `git cherry-pick --continue`.

### Step 5 — Clean up

```bash
git remote remove nl-incremental
rm -rf /tmp/nl-incremental-temp
```

---

## Quick Reference — Key Commands

```bash
# Check if nl has new predefined-mapper commits since last migration
cd /Users/gatlil3/Code/cat.gitm/nl
git fetch origin
git --no-pager log --oneline 66cdc2a08..HEAD -- predefined-mapper/

# Full incremental sync (run Steps 3–5 above when new commits exist)
```

---

## Notes

- The cutoff hash (`66cdc2a08`) must be updated each time you run a sync — it should always point to the **latest nl predefined-mapper commit** that has already been brought into nps.
- `cherry-pick` copies individual commits rather than merging an entire foreign history, so it never duplicates already-present commits.
- If `filter-repo` is not installed: `pip install git-filter-repo`

