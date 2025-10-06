```
# todo: fix below (and remove setting in build.sbt), "set" only sets root project and "set every" throws cyclic dependency error  
# sbt -Dsbt.repository.publish.attempts=20 "set publishConfiguration := publishConfiguration.value.withOverwrite( true )" publish  
sbt -Dsbt.repository.publish.attempts=20 publish
```

