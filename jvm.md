```
/** A MetricSet that tracks GC pressure metrics including frequency, duration, and time percentage.
  * These metrics help detect memory pressure without relying on absolute heap usage thresholds.
  *
  * The following metrics are recorded for each garbage collector:
  *  - gc.{collector-name}.frequency: Number of GC collections per minute
  *  - gc.{collector-name}.time-percentage: Percentage of time spent in GC
  *  - gc.{collector-name}.avg-pause-time: Average GC pause duration in milliseconds
  *  - gc.pressure: Overall GC pressure (sum of all time percentages)
  *
  * Recommended CloudWatch alarms:
  *  - gc.pressure > 10%: Overall GC taking too much time
  *  - gc.{old-gen-collector}.frequency > 2: Too many full GCs per minute
  *  - gc.{old-gen-collector}.avg-pause-time: Increasing trend indicates memory pressure
  */
  class GCPressureMetrics( mxBeans: List[GarbageCollectorMXBean], clock: Clock ) extends MetricSet
```
