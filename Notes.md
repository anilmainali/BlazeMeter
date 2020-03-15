
# Engine Health Report

The Engine Health Report displays performance indicators received from the test engines (i.e. the infrastructure delivering the test traffic, not your system under test). EngineHleathReport00.png

The Engine Health indicates whether the test infrastructure itself could be the cause of bottlenecks or errors which are appearing in other reports. 

The Engine Health is also a great resource when deciding how many virtual users (VUs) each engine can support. The ideal ratio depends on the complexity and memory footprint of your script(s). You can read about the process for planning and calibration of test execution to optimally utilize available resources in the help topic, Calibrating a BlazeMeter test.

While running a test, make sure the CPU values are lower then 80% and memory levels are less then 70%.

Here are some actions to take when levels of CPU and/or Memory are too high:

1. Make sure that your script is resource effective, e.g with no enabled Listeners, redundant requests, or heavy samplers like WebDriver sampler that can be avoided.

2. Reduce the number of users per engine.

3. Add more engines to the test.

EngineHleathReport01.png

and here’s what each of these KPIs mean, 

CPU: Represents the Percentage usage of CPU in instance

Memory: Represents the Percentage usage of the Virtual Memory in instance

Network I/O: Represents the amount of data transferred in I/O operations (KB/s)

Connections: Represents the number of persistent connections established for each transaction throughout the test
