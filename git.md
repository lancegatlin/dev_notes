
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
