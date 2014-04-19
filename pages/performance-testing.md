title: "Performance Testing and Profiling: A Virtuous Cycle"

###Basics

* Generate requests against our app
    * Record and replay production
* Measure response time and error rate

###Types
      
* Stress Test
* Load Test
* Spike Test
* Soak Test


###Stress Test

* Generate excessive load
    * Lots of requests
    * Slow/difficult requests
    * Adversarial testing
* How much?
* Identify breaking point
    * Especially if you control synthetic load
* Generate specific, constant load
    * Expected conditions
    * Exaggerated conditions
* Capacity planning
* Best practices
    * Isolate testing from external influences
    * Use dedicated load testing environment
    * Scaled down copies of all components
    * Generate load consistently
    * Random considered harmful
        * No way to compare different tests well
    * Automate

###Profiling
* cProfile, pstats
* Documentation not really included

Example:

    #!python
    import cProfile
    profiler = cProfile.Profile()
    profiler.enable()

    # do stuff here …

    profiler.disable()
    profiler.dump_stats("myprogram.prof")

    import pstats

    stats = pstats.Stats("myprogram.prof")
    stats.sort_stats("Calls")
    stats.print_stats("")

    stats.print_callees("webapp.py:8(login)")



* Profiling is useful for identifying un-optimized code
    * Tight loops, recursion
    * Candidates for optimization
* Good for identifying bottlenecks
    * Distinguish between slow external resource and slow application code


###3rd Party Profilers

* [line_profiler](http://pythonhosted.org/line_profiler)
* [yappi](https://code.google.com/p/yappi/)
    * Can profile across threads
    * Can measure between "wall clock" and "CPU clock"



###Instrumentation

* Use statsd to collect time-series metrics
    * Lightweight, low-overhead, always-on profiling
* Two key instruments
    * Counters let you know how many things happened
    * Timers let you know how long they each took
* Helps learn what’s normal for your application
    * Alert when things are not normal

###The Virtuous Cycle

* Performance Testing
* Profiling
* Optimizations
* Instrumentation and Alerting
* Repeat

