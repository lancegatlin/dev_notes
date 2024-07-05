
https://medium.com/teads-engineering/leveraging-sbt-remote-caching-on-a-big-modular-monolith-84826f949ae8


(abandonded) https://index.scala-lang.org/elarib/partial-sbt

https://www.reddit.com/r/scala/comments/w6it6c/sbtscalatest_library_or_plugin_that_only_reruns/
- Someone did something similar at my previous work because we had a big monolith with long running tests. The granularity wasn't the test suite but the SBT module, it's easier and good enough. The gist of it is you have a SBT plugin which uses Git to identify changed modules, processing the diff between target branch and origin. Then you can use SBT API to find out the other impacted modules. The resulting SBT plugin is surprisingly short.
- sbt has `testQuick` and _"remote cache"_.

https://github.com/opt-tech/sbt-diff-project

https://github.com/sbt/sbt-git



