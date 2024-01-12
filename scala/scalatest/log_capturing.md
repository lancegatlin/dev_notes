Duplicate without modification:
- akka.actor.testkit.typed.internal.LogbackUtil

Duplicate with modification:
- akka.actor.testkit.typed.internal.CapturingAppender
	- add method: `def capturedLogs: Vector[ILoggingEvent] = buffer`
- akka.actor.testkit.typed.scaladsl.LogCapturing
	- add method: `def capturedLogs: Vector[ILoggingEvent] = capturingAppender.capturedLogs`