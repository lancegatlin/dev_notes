```
// turn on full exception trace by default
      // ensures CI logs always have full stack trace
      // see https://www.scalatest.org/user_guide/using_scalatest_with_sbt
      testOptions in Test += Tests.Argument(TestFrameworks.ScalaTest, "-oF")
```

dockerComposeTest
```
/*
        note: these settings are for dockerComposeTest command only (used in executeXXX.sh scripts for CI)
        scalatest settings for local, test and it:test are unaffected by these settings (see BaseModule)
        -oF enable full stack traces
        -oW disable color
        see https://www.scalatest.org/user_guide/using_the_runner
       */
      testExecutionArgs := "-u target/junitxml -oFW",
```