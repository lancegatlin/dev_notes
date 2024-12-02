Sonar

1. if <20 changed lines, passes no matter what (admin configurable)

2. "lines to cover"
  a. lines that changed that also require coverage (not all changed lines require coverage)
    1. imports, logging, others?, might be "lines changed" but don't require coverage
    2. marked in blue only in file view
  c. "lines to cover": 0
    1. file has some amount of lines changed but none that require coverage
    2. in Sonar, can't view files with 0 "lines to cover"
  d. "lines to cover": X > 0
    1. file has some amount of lines changed but only X that require coverage

3. "uncovered lines"
  a. lines that require coverage that are not covered

4. 'coverage %'
  a. only "lines to cover" in a PR are used for computing 
  a. other changed lines are ignored in 'coverage %'
  b. the overall 'coverage %' for a file isn't used in the calculation
    1. can sometimes lead to failures even when overall file 'coverage %' is acceptable
    2. can lead to accumulation of uncovered code in rare cases

5. Renames
  b. may trigger Sonar failure
    1. if a "lines to cover" which have no existing coverage are renamed

6. Fixing a PR Sonar failure
  a. add coverage for a test
  b. add `// $COVERAGE-OFF$`/`// $COVERAGE-OFF$` comments (note: permanent until coverage is added)
  c. add file to sonar per-file exceptions to build.sbt
    1. ex: `sonarProperties := excludeFromCoverage(sonarProperties.value, "**/src/st/**/*", "**/src/main/**/AdminAlertEngine.scala")`
    2. note: this setting is from our sbt-stingray plugin
    3. can later be removed in a separate PR that will always pass (changes to build.sbt itself)

