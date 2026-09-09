
### git fetch/pull that updates moved mutable tags and prunes deleted tags

The config
```
git config --add remote.origin.fetch '+refs/tags/*:refs/tags/*'
```
Note --add — this appends a second refspec alongside the default +refs/heads/:refs/remotes/origin/. Don't use plain git config (it would replace the branch refspec and break normal fetching).
The leading + means force. After this, a bare git fetch and git pull both update moved tags:
```
$ git fetch
t [tag update]      moving     -> moving
```
I confirmed both paths in a throwaway repo: before the config, git pull and git fetch --tags left the stale tag untouched; after it, plain git pull moved it.
Add pruning too
The refspec updates moved tags but won't delete tags removed upstream. For that:
```
git config fetch.prune true
git config fetch.pruneTags true
```
Verified: a tag deleted upstream survived locally until fetch.pruneTags=true, after which a plain git fetch removed it.
Apply to your repos
```
for d in /Users/gatlil3/Code/cat.prm/*/; do
git -C "$d" config --add remote.origin.fetch '+refs/tags/*:refs/tags/*'
git -C "$d" config fetch.prune true
git -C "$d" config fetch.pruneTags true
done
```
Guard against running it twice — --add will happily create duplicate refspecs (harmless but messy). Check with git config --get-all remote.origin.fetch.
Or set it globally so new clones inherit it:
```
git config --global fetch.prune true
git config --global fetch.pruneTags true
```
Those two are safe globally. The refspec is per-remote and can't be set globally.
Caveats
There is no fetch.forceUpdateTags option — the refspec is the only mechanism. Also worth knowing: remote.origin.tagOpt = --tags fetches all tags but does not force, so it doesn't solve this.
The real caveat is that this silently rewrites local tags on every fetch. For repos using immutable release tags that's a footgun (a tag you have checked out could move under you). For cat.prm it's exactly right — gha-vN are moving release pointers by design, and stale ones already cost us one bogus finding.
I'd keep git fetch --tags --force in the verification step of the plan regardless, since it's explicit and doesn't depend on each engineer's local config.
