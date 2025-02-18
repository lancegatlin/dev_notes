Scripting with Scala
alexandru nedelcu
Sep 13 2022
https://alexn.org/blog/2022/09/13/scripting-with-scala/


```
I have some urgent maintenance work at my apartment so might join late for the meeting hello team, some thoughts on why we shouldn't use scala-cli:

1. It doesn't work with cats ecosystem apps  
2. It requires installing the scala-cli runtime which isn't installed on agents (and there were issues getting it installed and running on agents)  
3. It requires extra config to reuse common code  
4. It still requires compiling to run (5-6 seconds)  
5. It doesn't have a standard way to add tests  
6. It took about a month of working with scala-cli to uncover that is was a dead end. Let's not repeat this?

The existing scala scripts infrastructure:

1. Fully works with cats ecosystem  
2. Is already deployed and vetted as working in CI (and CI docker images)  
3. Has an established pattern for a CLI app and command line parsing that can be copied (though its optional)  
4. Is an sbt project `stingray-scripts/lib` which is already configured to use sbt-stingray and P-Scala-Utils (and receives regular upgrades from our upgrade process)  
5. Can write tests just like we do normally  
6. Already has the `nativeImage` plugin (requires no config) that is a longer compile (~30 seconds) but it is still possible to simply `run` from sbt console when testing  
7. Graal `nativeImage` executables have been fully vetted at this point and work great

For simple scripts, bash or python is also fine. Any kind of script where the agents will have the runtime already existing is fine. Put them in `stingray-scripts/bin` and they will be installed by the `stingray-scripts/install.sh` or `stingray-scripts/update.sh` script.

```