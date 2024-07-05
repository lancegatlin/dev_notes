https://stackoverflow.com/questions/10376206/what-is-the-preferred-bash-shebang


#### Running a bash startup script for even non-interactive sessions

https://unix.stackexchange.com/questions/511942/linux-redhat-7-how-to-set-a-shell-option-globally-system-wide

figured out how to force `bash -eu` flags for scripts that don't have them set by creating a bash script file and pointing env var `BASH_ENV` at that file:
~/Code/bin/bash_set_eu.sh:
```
set -eu
```
append to `~/.bash_profile`
```
export BASH_ENV=~/Code/bin/bash_set_eu.sh
```
also need to edit `~/.sdkman/candidates/sbt/current/bin/sbt` to turn off -u flag by adding following just under shebang:
```
set +u
```

https://www.gnu.org/software/bash/manual/html_node/Bash-Variables.html
