sbt deprecated IntegrationTest (and FT & ST) configuration migration options:

Option 1. Add all it tests to test sources & filter them
  - long term solution
  - works in sbt 2+
  - moderate effort
  - requires pipeline fixes

  1. Create a new sbt-stingray `IntegrationTest` auto plugin:
    1. add existing `src/it/scala` (and `resources`) to test classpath
    2. use test tagging OR filter tests on name (e.g. "*ITSpec")
      3. requires either tagging all existing IT tests (better) OR ensuring all names match pattern
    4. add new `itTest` task key which replaces `it:test` (which runs e.g. `sbt "testOnly -- -l IntegrationTestTag"`)
    5. see example PR: https://github.com/cat-digital-platform/P-notifications-common/pull/955

Option 2. create a new `integration` (or `it`) custom configuration (in sbt-stingray)
  - short term solution
  - eventually all custom configurations will be deprecated/removed in sbt 2+
  - very low effort
  - no pipeline fixes needed

  1. create new `IntegrationTest` auto plugin with replacement custom integration configuration
  2. everything else remains the same (e.g. it:test)

Option 3. create a new subproject for ALL integration/smoke/functional tests in all subprojects (e.g. `integration`, or `funtests`, or `st`, etc)
  - long term solution
  - "recommended" method (but it doesn't work well with builds that have many subprojects like NL)
  - works in sbt 2+
  - highest effort
  - requires pipeline fixes

  1. move entire src/it folder trees to new `integration` directory from each subproject
  2. add new subproject `integration`
  3. run `integration/test` runs tests

Option 4. Do nothing right now
  - sbt 2.0 is still pre-release as of Jan 2026
  - probably can just silence the warning
  - sbt 2 isn't ready yet and could probably be put off another year
  - almost no effort

Also, everything here that is done for currently deprecated IntegrationTest configuration will eventually have to be done for ALL custom configurations in all sbt projects (e.g. FunctionalTest, SmokeTest, IntegrationTestCustom, etc)

References:
  - https://eed3si9n.com/sbt-1.9.0
  - https://eed3si9n.com/sbt-drop-custom-config/

Note: all the `names` I've selected here are arbitrary and are still open for discussion/change
