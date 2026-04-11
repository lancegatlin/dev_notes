Note: this is the same as `lazy val CustomConfiguration = config("custom").extend(Test)` but allows more flexible
sharing of resources without re-discovering tests. See the method docs for details.

```Version 2
  implicit class SbtConfigurationExt(val self: Configuration) extends AnyVal {

    /** Make compiled code & resources from one Configuration available to this (dest) Configuration so that source Configuration helpers and fixtures can be
      * reused here.
      *
      * Uses `internalDependencyClasspath` with `exportedProducts` — the same mechanism SBT uses for inter-project dependencies — so compiled classes and
      * resources are available as a single classpath entry without re-sourcing or re-discovering tests. This avoids the need to manually manage
      * `unmanagedResourceDirectories` or `excludeFilter`.
      *
      * @param source
      *   the Configuration whose compiled output & resources are made available (e.g. Test)
      * @return
      *   a Seq of settings to include in this config's settings
      */
    def includeSourcesAndResources(source: Configuration): Seq[Setting[?]] = Seq(
      // Add source's compiled output (classes + resources) to self's internal classpath so helper/fixture code is
      // available without recompiling source's sources into self's output directory (which would cause test
      // re-discovery). exportedProducts is the same entry SBT places on the classpath for inter-project deps.
      self / internalDependencyClasspath ++= (source / exportedProducts).value,
      // Ensure source compiles before self so the products added above are populated.
      (self / compile) := (self / compile).dependsOn(source / compile).value
    )
  }
```

```Version 1
implicit class SbtConfigurationExt(val self: Configuration) extends AnyVal {

    /** Make compiled code & resources from one Configuration available to this (dest) Configuration so that source Configuration helpers and fixtures can be
      * reused here.
      *
      * The source config's compiled class directory is added to self's unmanaged classpath rather than wiring in the raw source directories. This is
      * intentional: adding source directories directly would cause the source config's test classes (e.g. unit tests) to be recompiled into self's output
      * directory, making them visible to ScalaTest's test discovery and causing them to be re-run when self's tests are executed (e.g. running `it:test` would
      * also run `Test` unit tests).
      *
      * Resources from the source config are still included via unmanagedResourceDirectories so that resource files (config, fixtures, etc.) are available at
      * runtime in self's test scope.
      *
      * @param source
      *   the Configuration whose compiled output & resources are made available (e.g. Test)
      * @return
      *   a Seq of settings to include in this config's settings
      */
    def includeSourcesAndResources(source: Configuration): Seq[Setting[?]] = Seq(
      // 1. Add source's compiled classes to self's classpath so helper/fixture code is available,
      //    without recompiling source's sources into self's output (which would cause test re-discovery).
      self / unmanagedClasspath += (source / classDirectory).value,
      // 2. Wire source resources onto self's resource classpath, ensuring:
      //   - source resource directories such as json/foo.json stays at json/foo.json on the classpath (not flattened to the root)
      self / unmanagedResourceDirectories ++= (source / unmanagedResourceDirectories).value,
      // 3. Exclude source resources that are shadowed by a resource with the same relative path in self's own resource directories,
      //    preventing duplicate mapping errors in copyResources.
      self / excludeFilter := {
        val sourceResDirs = (source / unmanagedResourceDirectories).value.toSet
        // self's unmanagedResourceDirectories includes the source dirs appended in step 2, so filter them out to get only self's own dirs
        val ownSelfResDirs = (self / unmanagedResourceDirectories).value.filterNot(sourceResDirs.contains)
        // Collect all relative paths already provided by self's own resource dirs
        val selfRelPaths: Set[String] = ownSelfResDirs.flatMap { dir =>
          (dir ** AllPassFilter).get.filterNot(_.isDirectory).flatMap(f => IO.relativize(dir, f))
        }.toSet
        // Build a filter that excludes any source resource whose relative path is already covered by self's own dirs
        val shadowedFiles: Set[File] = sourceResDirs.flatMap { dir =>
          (dir ** AllPassFilter).get.filterNot(_.isDirectory).filter { f =>
            IO.relativize(dir, f).exists(selfRelPaths.contains)
          }
        }
        new SimpleFileFilter(shadowedFiles.contains) || (self / excludeFilter).value
      },
      // 4. Ensure source compiles before self compile so the class directory added in step 1 is populated.
      (self / compile) := (self / compile).dependsOn(source / compile).value
    )
  }
```