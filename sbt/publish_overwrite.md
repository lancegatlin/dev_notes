### verified working

add `publishConfiguration := publishConfiguration.value.withOverwrite(true)` to common settings for all subprojects
in build.sbt (note: this will only work locally and not in CI)

### other ideas that don't seem to work

```
set every publishConfiguration := publishConfiguration.value.withOverwrite(true)
```

```
set every publishConfiguration ~= (_.withOverwrite(true))
```

Both of these produce cyclic dependency error
