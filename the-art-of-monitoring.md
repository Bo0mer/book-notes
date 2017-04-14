# The art of monitoring
by James Turnbull. If you like this notes, you can [buy the book](https://www.amazon.com/Art-Monitoring-James-Turnbull-ebook/dp/B01GU387MS).


**Types of monitoring across the industry**
* Manual, user-initiated or no monitoring at all
* Reactive (infrastructure centric)
* Proactive (core to managing business, application-centric)

**Customers of monitoring**
* Business - monitoring provides data to measure customer satisfaction, etc.
* IT - the IT department of the organization

**Monitoring and measurement framework**
* Provides data for the state and performance of our environment
* Event, log and metric-centered
   * Events - changes and occurrences
   * Logs - subset of events, most useful for fault diagnosis and investigation
   * Metrics - measures of properties in pieces of software or hardware

**Architecture**
Blackbox vs. Whitebox (pull vs push)
* push > pull. Why?
   * Security
   * Scalability
   * Configuration wise (e.g. no need for discovery)

**Types of metrics**
* Gauges - numbers, expected to change over time
* Counters - increase over time, never decrease, may reset, though
* Times - track how long something is took
* Metric summary - mathematical transformation applied to metric(s)
* Metric aggregation - aggregated collection of metrics

**Notifications on metrics**
* They should be actionable, clear and articulate
* They should provide as much context as possible

**Visualizations of metrics**
* Clearly show the data
* Avoid distorting the data
* Make large data sets coherent
* Allow change of granularity without impact to comprehension
* Make the viewer think about the substance, not the visuals (people are good
at finding fake patterns in a data sets that are random)
