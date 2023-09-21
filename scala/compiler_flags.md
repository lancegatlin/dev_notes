### Disable fatal warnings while working sbt repl


`set scalacOptions ~= { _.filterNot(Set("-Xfatal-warnings")) }` disables fatal warnings in the scala repl

this setting has to be applied for each subproject -- because of the way common settings are applied by our common sbt settings, its not possible to use `ThisBuild` , and `set every` crashes in `P-Telemetry-Alerts-Stream` (there might be another way but I'm not sure at the moment)

`set scalacOptions += "-Xfatal-warnings"` to turn it back on (or `reload` or quit and restart sbt)

`show scalacOptions` to view the current setting

