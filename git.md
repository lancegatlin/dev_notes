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