https://perevillega.com/posts/scala-steward-automerge/

https://stackoverflow.com/questions/71623045/automatic-merge-after-tests-pass-using-actions

https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/configuring-pull-request-merges/managing-auto-merge-for-pull-requests-in-your-repository


https://engineering.avast.io/fully-automated-luxury-pipeline-for-updating-dependencies-in-scala-projects-with-missinglink/
...When Scala Steward opens a new PR updating a version of a dependency, we cannot be entirely sure it can’t break anything. Of course, in CI, we compile the project, so we can be sure that any class or method that our project directly uses is present with the expected signature (if not, the compilation will fail). But what if some other class or method in the library being updated, used only transitively by another library, changes? These issues are called “binary incompatibility” in Scala circles (even more here). It can lead to NoClassDefFoundError or NoSuchMethodError in runtime. Such issues can still be uncovered in CI by tests, but that depends on your test coverage, which is difficult to have 100%.
...Missinglink is a tool developed by Spotify, designed to solve precisely the issues outlined above. It analyses the bytecode of all classes in all libraries (JARs) that our project uses. Because it sees all the code that can be called at runtime, it can verify that indeed all the called methods and used classes are available. By the virtue of working at the level of Java bytecode, it is agnostic of the language you work with — we use it for Scala, but it should work well for Java or Kotlin too.