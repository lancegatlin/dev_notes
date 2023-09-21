
Copy to `~/.sbt/1.0/plugins/fatal_warnings.scala`
```
package sbtplugins

import sbt._
import Keys._

/**
 * 
 * A lightweight plugin for enabling/disabling fatal warnings while working/troubleshooting
 * Usage:
 *   fatalWarnings off // disable fatal warnings
 *   fatalWarnings on // re-enable fatal warnings
 * 
 * note: sbt reload will also re-enable fatal warnings
 **/
object FatalWarningsPlugin extends AutoPlugin {
  val fatalWarningsAction: (State, String) => State = { (state, input) =>
    val maybeCmd =
      input.toLowerCase match {
        case "off" | "false" | "0" => Some("""set scalacOptions ~= { _.filterNot(Set("-Xfatal-warnings")) }""")
        case "on" | "true" | "1" => Some("""set scalacOptions += "-Xfatal-warnings"""")
        case _ =>
          System.err.println(s"[error] fatalWarnings argument must be one of (off, false, 0) or (on, true, 1)")
          None
      }
    maybeCmd match {
      case Some(cmd) =>
        val newState = Command.process(cmd, state)
        val (s, _) = Project.extract(newState).runTask(compile in Compile, newState)
        s
      case None => state // do nothing
    }
  }

  val fatalWarningsCommand: Command = Command.single("fatalWarnings")(fatalWarningsAction)

  override def trigger = allRequirements
  override lazy val buildSettings = Seq(commands += fatalWarningsCommand)
}


```