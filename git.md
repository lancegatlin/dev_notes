quick rebase
```
git fetch; git rebase origin/{defaultBranch} --autostash
```


manual git squash
https://stackoverflow.com/questions/25356810/git-how-to-squash-all-commits-on-branch
```
git reset $(git merge-base master $(git rev-parse --abbrev-ref HEAD))
git commit -a -m "one commit on yourBranch"
git push --force
```


Multiple SSH Keys settings for different github account
https://gist.github.com/jexchan/2351996
What's the difference between Host and HostName in SSH Config?
https://superuser.com/questions/503687/whats-the-difference-between-host-and-hostname-in-ssh-config
Note: couldn't get multiple host nicknames for github with different keys to work


How to tell git which private key to use?
https://superuser.com/questions/232373/how-to-tell-git-which-private-key-to-use/912281#912281

Following works to set SSH key per-repo
```
git config core.sshCommand "ssh -i ~/.ssh/id_rsa_example -F /dev/null"
```

### git with PAT

https://www.learnhowtoprogram.com/introduction-to-programming/getting-started-with-intro-to-programming/creating-and-using-a-git-pat

To enable keychain storing of PAT after first login
```
git config --global credential.helper osxkeychain
```

checkout by date
```
git checkout 'master@{2024-04-18 00:00:00}'
```

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
4. Merge the history into your current branch. You must use a special flag because the repositories have no common ancestor: `git merge migration-source/main --allow-unrelated-histories`
5. Clean up: `git remote remove migration-source` `rm -rf ../temp-repo`

#### Phase 3: Finalize Source Repository

Now that the files and history are safely in the new repo, you can simply delete the original files from the source
repository using a standard git rm and commit.
